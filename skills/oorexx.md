---
name: oorexx
description: >
  Pitfalls, best practices and traps for ooRexx code. Read this
  whenever writing, reading, editing, or debugging ooRexx code.
  Does NOT cover coding style — see your own style skill (if
  any) and conventions.md for that.
---

# ooRexx — Pitfalls & Best Practices

This is not a language reference — Claude already knows ooRexx from
training. This document covers what makes Claude trip: special
variables, non-obvious semantics, and common mistakes in generated
code.

---

## Special variables — NEVER use as variable names

`RESULT`, `RC`, and `SIGL` are silently overwritten by the interpreter.
**Never use any of them as a variable name.** Use `response`, `res`,
`outcome`, `ret`, etc. instead.

- `RESULT` — set by unassigned message sends and `CALL` that return a
  value. Assigned sends (`x = obj~method`) do NOT update it, but
  unassigned sends (`obj~method`) DO. A bare `RETURN` drops it.
- `RC` — set by `ADDRESS` commands (command instructions). Equally
  dangerous if the program uses command instructions.
- `SIGL` — set by `CALL` and `SIGNAL` to the source line number.
- `SELF` — the receiver inside a method (safe to read, not to assign).
- `SUPER` — the superclass scope inside a method.

```rexx
-- WRONG: result gets overwritten by the ~setEntry send
result = .Directory~new
result~setEntry("status", "200")   -- RESULT is now "200"!

-- RIGHT: use a different name
response = .Directory~new
response~setEntry("status", "200")
-- response is still the Directory
```

---

## Common pitfalls

### 1. Logical operators are NOT short-circuit

`&`, `|`, and `&&` evaluate **both** operands unconditionally. Use
commas (short-circuit AND) in `If` and `When` when a condition guards
a subsequent one:

```rexx
-- WRONG: crashes if i == 1
If ch == "#" & i > 1 & text[i - 1] == " " Then ...

-- RIGHT: commas as short-circuit AND
If ch == "#", i > 1, text[i - 1] == " " Then ...
```

### 2. There is no `main` routine

The prolog (code before the first `::directive`) is the entry point.

### 3. Loop Until evaluates at the end

Variables assigned in the loop body are available when `Until` is
first evaluated. This is by design, not a bug. `While` evaluates
at the start.

### 4. Uninitialised variables evaluate to their own uppercase name

```rexx
Say foo        -- prints "FOO" (not an error)
```

Use `Signal On Novalue` to trap this during development.

### 5. EXPOSE and variable scoping in methods

Methods do not see the caller's variables. Instance variables must
be explicitly exposed:

```rexx
::Method increment
  Expose count
  count = count + 1
```

Without `Expose count`, the body starts a fresh local `count` (which
evaluates to `"COUNT"`, see pitfall 4) and the assignment never
touches the instance variable.

Don't write a plain getter or setter this way: `::Method name; Expose
name; Return name` is exactly what `::Attribute name Get` generates
for you. Reach for `Expose` when the method does more than relay an
attribute — like the increment above.

### 6. Assignment operators are pure syntactic expansion

`x[f(n)] += 1` becomes `x[f(n)] = x[f(n)] + 1` — evaluates `f(n)`
twice. Be aware when the left-hand side has side effects.

### 7. Use `=` for equality, reserve `==` for identity

`=` is non-strict (strips blanks, compares numerically). `==` is
strict (character-by-character for strings, object identity for
non-strings).

This matters with objects that have `makeString` (e.g., sentinel
classes like `YamlBoolean`): `=` compares string representations,
`==` compares identity.

```rexx
.YamlBoolean~true =  1    -- 1 (true: equality)
.YamlBoolean~true == 1    -- 0 (false: different objects)
```

**Rule of thumb**: default to `=`. Use `==` for strict string
comparison or identity checks (`.nil` tests). In test assertions,
always use `=`.

### 8. `stem. = value` resets the entire stem

It drops all existing tails and installs a new default. After
`x.1 = 12; x. = 3`, `x.1` evaluates to `3`, not `12`.

### 9. `stem.tail` is NOT a message send

It is variable name resolution by the interpreter. The dot is a
tail separator, not the `~` operator.

### 10. A label inside a block causes error 47.2

A label may not appear inside a `DO`/`LOOP` block (including the
`do...end` that hangs off a `Select` `When` or an `If`). The
interpreter rejects it: *"Error 47.2: Labels are not allowed within
a DO/LOOP block."* The culprit is the **label**, not `Signal`.

`Signal` itself jumps *out* of a loop perfectly well — it terminates
the active control structures and transfers control to the target
label, which lives at prolog level (outside any block):

```rexx
do i = 1 to 3
  if i = 2 then signal done   -- fine: jumps out of the loop
end
done:
```

What fails is putting the `label:` *inside* the block. If you need a
jump target near loop logic, hoist it out of the block — or extract
the logic into a separate routine.

---

## Best practices

### Prefer `text[i]` over `text~substr(i, 1)`

More legible and more performant for single characters. Use `substr`
only for multiple characters.

### Use `select case` for same variable or expression

```rexx
select case value
  when "null", "Null", "NULL" then ...
  when "true", "True", "TRUE" then ...
  otherwise nop
end
```

### Use MutableBuffer for string accumulation

```rexx
mb = .mutableBuffer~new
do item over collection
  mb~append(item, "0A"x)
end
out = mb~string
```

`append` accepts multiple arguments:
`mb~append("prefix", value, "0A"x)`.

### StringTable iteration order is undefined

Do not rely on key order. For sorted keys: `~allIndexes~sort`.

### Use `makeString('L', separator)` for array-to-string

```rexx
-- WRONG: first argument is treated as type
arr~makeString("0A"x)

-- RIGHT
arr~makeString('L', "0A"x)
```

### Line endings: native vs. forced LF

For ordinary text output, use `Say` or `LineOut`. They emit the
platform's **native** line terminator — `LF` on Unix/Linux, `CRLF`
on Windows — which is what the rest of that platform's tools expect.
That is the portable, do-the-right-thing default.

```rexx
do line over arr
  s~lineOut(line)        -- native terminator on each platform
end
```

Only reach for `charOut` with an explicit `"0A"x` when you
deliberately want `LF` everywhere regardless of platform — e.g.
emitting a file destined for a Unix system, or a format that
mandates `LF`:

```rexx
s~charOut(arr~makeString('L', "0A"x))   -- forces LF on every platform
```

Note this is the *opposite* of platform independence: it pins
Unix-style endings even on Windows. Don't use it as a generic way to
"avoid CRLF" — use `LineOut` for native endings, and `charOut`/`"0A"x`
only when a fixed `LF` is the actual requirement. (Also mind that
`makeString('L', sep)` places the separator *between* elements, so
there is no trailing terminator after the last line.)

---

## Resources

- **ooRexx on SourceForge**: <https://sourceforge.net/projects/oorexx/>
- **RexxLA (Rexx Language Association)**: <https://www.rexxla.org>
- **ooRexx Reference (rexxref)**: distributed with every installation
