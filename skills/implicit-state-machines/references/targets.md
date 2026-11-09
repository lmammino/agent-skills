# Target types

Replace an implicit state machine with one value of a type that lists
every state. An exhaustive switch makes the compiler report each
missing case.

| Language | Target type | Exhaustiveness check |
|---|---|---|
| Dart 3 | `enum`, `sealed class` | `switch` with no `default` |
| TypeScript | discriminated union | `never` in the default branch |
| Kotlin | `enum class`, `sealed interface` | `when` as an expression |
| Swift | `enum` with associated values | `switch` (compiler) |
| Rust | `enum` | `match` (compiler) |
| Java 21+ | `sealed interface` + records | pattern `switch` |
| C# | `enum`, records with a base type | Enum `switch` expression: CS8509 for missing members, CS8524 for unnamed values; base records do not form a compiler-checked closed union |
| Python 3.10+ | `Enum`, dataclass union | `match` + `assert_never` (type checker); import from `typing` on 3.11+, or `typing_extensions` 4.1+ on 3.10 |
| Go | typed `const` + `iota` | linter (`exhaustive`) only |

## Rules

- Use an enum when the states carry no data. Use a sealed type or union
  when a state carries data, for example `resume(notice)`.
- Do not add a fallback branch that hides missing states. A branch
  that enforces exhaustiveness is allowed, such as a TypeScript
  `default` assigning to `never`, or a Python wildcard case calling
  `assert_never`.
- For C# enums, keep CS8509 enabled so new members produce a warning.
  A switch covering every named member can still produce CS8524 for
  unnamed enum values. If these are excluded by validation, document
  that boundary and suppress only CS8524 for the affected switch (for
  example with a scoped `#pragma warning disable CS8524` and restore).
  Otherwise, flag the runtime-value handling decision before refactoring.
  A discard arm such as `_ => throw` also hides newly added named members.
- Keep stored formats. Map the new type to the old stored value at the
  storage boundary. Change a stored format only with the project's
  migration process.
- When the language has no exhaustiveness check, say so in the report.
  The fix then has less value.
