# Repository instructions

Go proxy server providing OpenAI/Gemini/Claude/Codex-compatible APIs. Use this fork for requested changes; upstream publication must be part of the authorized task.

## Verification and configuration

- For Go changes, format affected files with `gofmt`, run affected package tests with `go test ./path/to/pkg`, and verify the server builds with `go build -o /dev/null ./cmd/server`. Use `go test ./...` when the change warrants full coverage.
- Configuration defaults and supported options live in `config.example.yaml`. Auth storage follows `auth-dir`; `.env` is loaded from the working directory. Keep credentials and tokens out of commits and logs.

## Runtime constraints

- Preserve the thinking pipeline: suffix parsing overrides the body, normalization and validation produce canonical `ThinkingConfig`, and `ProviderApplier` translates it per provider. See `internal/thinking/` when changing reasoning behavior.
- `internal/runtime/executor/` contains executors and their unit tests; put supporting helpers in `internal/runtime/executor/helps/`.
- Keep translator fixes scoped to the fault; unrelated changes elsewhere are not required to make a translator fix acceptable in this fork.
- Return errors instead of calling `log.Fatal`/`log.Fatalf` or panicking in HTTP handlers; use logrus for structured logging and handle deferred close errors.
- After an upstream connection is established, preserve streaming without new network timeouts. Existing exceptions are Codex websocket liveness deadlines (`internal/runtime/executor/codex_websockets_executor.go`), wsrelay deadlines (`internal/wsrelay/session.go`), management APICall (`internal/api/handlers/management/api_tools.go`), and `cmd/fetch_antigravity_models`. Credential-acquisition timeouts remain allowed.
- Write new comments in English; preserve the existing language of user-visible strings and language-specific Markdown. Translate existing comments only when editing their content.
