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
| C# | `enum`, records with a base type | `switch` expression (warning) |
| Python 3.10+ | `Enum`, dataclass union | `match` + `assert_never` (type checker) |
| Go | typed `const` + `iota` | linter (`exhaustive`) only |

## Rules

- Use an enum when the states carry no data. Use a sealed type or union
  when a state carries data, for example `resume(notice)`.
- Do not add a `default` branch. It hides a new state from the
  compiler.
- Keep stored formats. Map the new type to the old stored value at the
  storage boundary. Change a stored format only with the project's
  migration process.
- When the language has no exhaustiveness check, say so in the report.
  The fix then has less value.
