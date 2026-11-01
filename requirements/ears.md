# EARS

Acceptance criteria use EARS by default. Where EARS reads unnaturally, such as for layouts or data formats, use another form that can still be checked.

| Pattern | Form |
|---------|------|
| Event | When [event], the [subject] shall [response] |
| State | While [state], the [subject] shall [response] |
| Unwanted behavior | If [trigger], then the [subject] shall [response] |
| Optional feature | Where [feature is included], the [subject] shall [response] |
| Always | The [subject] shall [response] |

Patterns combine, as in `While [state], when [event], the [subject] shall [response]`.

- Keep the keywords (`When`, `While`, `If`, `then`, `Where`, `the`, `shall`) in English, and write the bracketed parts in the document's language: `When 用户暂停传输, the 传输服务 shall 在 1 秒内停止发送数据`.
- The subject is a concrete part of the system, such as a service, a process, or a screen, named the same way throughout.
- Each criterion states one behavior.
- When a requirement has a user story, keep `As a`, `I want`, and `so that` in English as well: `As a 家庭用户, I want 只推送变化过的文件, so that 重复同步更快`.
