# Error Handling

- **Every user-facing operation must have visible error feedback.** Never silently swallow errors — if something fails (network request, file I/O, credential validation, etc.), the user must see a clear message explaining what went wrong and what they can do about it.
- **Validate inputs early and surface problems immediately.** Don't let bad data propagate silently through the system. If credentials are wrong, an API returns an error, or a file is missing, show the error at the point of failure.
- **Design for the unhappy path first.** When implementing a feature, think about what happens when the network is down, the API rate-limits you, the file doesn't exist, or the user's input is invalid. Handle these cases before considering the feature "done."
- **Timeouts must be explicit and communicated.** If an operation has a timeout, tell the user when it's taking too long. Never let the UI hang indefinitely without feedback.
