# fk-log

Logging for fckt, as an effect: what a program logs says what happened and what's known about it,
and its handler says where that goes.

```kotlin
import uk.m4xy.log.*
import uk.m4xy.log.Logs.*
import uk.m4xy.log.Logs.error

main() = toStandardError(Info, JsonLines) {
  withFields([("request", "7")]) {
    info("answered", [("status", "200")])
  }
}
// {"time":"2026-10-31T00:23:33.690Z","level":"info","message":"answered","request":"7","status":"200"}
```

`debug`, `info`, `warn` and `error` log an `Entry`: its `Level`, a message, and fields, pairs of a
name and a value. `error` is the core's too, which stops a program, so it's imported by name.
`withFields` puts fields before those of everything logged in its block, as a request's id is in
everything logged while it's answered.

`toStandardError` writes each entry from a level on to standard error, stamped with what the
`Clock` says, as fk-time writes a moment: as `Text`, a line for people,
`2026-10-31T09:30:00Z INFO answered request=7`, or as `JsonLines`, for a program that reads logs.
In a test, `collected` gives what a block gives and the entries it logged, so what's logged can be
checked as anything else is.

## Building

```sh
kiln build          # compiles and tests it
kiln publishLocal   # for other projects: library("uk.m4xy:fk-log:0.1.0")
```
