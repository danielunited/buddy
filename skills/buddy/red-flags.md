# Buddy's Red Flags

Things to catch when Claude edits code. One line out, in character.

Format: **name it → why it bites → the principle in ~3 words.**
Audience: junior / vibe coders. Define jargon in 3 words.
Tone: warm "careful 👀" nudge, never a scold. Silence beats filler.

## Silent failures (fail loud)

| Smell | Teach |
| --- | --- |
| `except:` / `except Exception:` with no re-raise | swallows everything, even Ctrl-C. catch the specific error. |
| `as any`, `// @ts-ignore`, `// @ts-expect-error` | disables the type safety net. narrow with `unknown` + a check. |
| `eslint-disable` / `# noqa` with no reason comment | silencing the alarm ≠ fixing the fire. |
| unawaited promise / `.catch(() => {})` | async errors vanishing into the void. |

## Foot-guns

| Smell | Teach |
| --- | --- |
| `def f(items=[])` / `def f(cfg={})` | mutable default is shared across calls. use `None`. |
| `const x = obj` then mutate `x` | that's aliasing, not a copy. spread or clone. |
| `==` vs `===`, `is` vs `==` | identity ≠ equality. pick the one you mean. |
| sequential `await` in a loop that could `Promise.all` | N× slower than it needs to be. |

## Security

| Smell | Teach |
| --- | --- |
| string-interpolated SQL | that's SQL injection. use parameterized queries. |
| shell/exec with user input, `shell=True` | command injection. pass argv as a list, never a string. |
| `dangerouslySetInnerHTML`, `innerHTML =` with user data | XSS. escape or use text nodes. |
| `eval`, `Function(str)`, `exec()` | arbitrary code execution. there's always another way. |
| hardcoded token/key/secret | env vars or a vault. rotate if it touched git. |

## Over- and under-engineering

| Smell | Teach |
| --- | --- |
| a class with one method, or a factory for one caller | just use a function. abstraction earns itself. |
| config option with one value today | YAGNI. add the knob when there are two callers. |
| magic number (`86400`, `0.7`, `retries=3`) | name it as a constant. your future self is reading this. |
| no error path on an external call (fetch, DB, FS) | the network lies. handle the failure mode you can name. |

## Naming & clarity

| Smell | Teach |
| --- | --- |
| `data`, `info`, `handle`, `doIt` in a public API | names are free. be specific. |
| boolean positional params: `send(user, true, false)` | call site is unreadable. use an options object or enum. |
| single-letter vars outside tight loops | readers will thank you. |

## State smells

| Smell | Teach |
| --- | --- |
| module-level mutable state / singleton | testability pain. pass it in. |
| side effect during import | importing shouldn't *do* things. |
| deep prop drilling | there's a context / store shape hiding here. |
