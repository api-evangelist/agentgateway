---
name: agentgateway-change-log-level
description: Raise agentgateway's log level at runtime to diagnose a problem, then put it back - the one reversible write on the admin API, with the read-before-write step that makes the reversal possible.
api: agentgateway
generated: '2026-08-30'
method: generated
source: openapi/agentgateway-logging-api-openapi.yml + https://agentgateway.dev/docs/standalone/latest/operations/debug/
operations:
  - GET /logging
  - POST /logging
license: CC-BY-NC-SA-4.0
---

# Change the agentgateway log level at runtime

`POST /logging` is the only routine write on agentgateway's admin API, and it is fully reversible —
but only if you read the current value first. Nothing on the API will tell you what the level was
after you have overwritten it.

## Steps

1. **Read and save the current level. Do not skip this.**

   ```sh
   curl -s http://127.0.0.1:15000/logging
   ```

   The response is a `RUST_LOG`-style filter string, and in a real deployment it is usually not a
   single word. A live example from the provider's docs:

   ```
   typespec_client_core::http::policies::logging=warn,hickory_server::server::server_future=off,rmcp=warn,debug
   ```

   Keep that whole string. It is your undo.

2. **Raise the level.**

   ```sh
   curl -X POST "http://127.0.0.1:15000/logging?level=debug"
   ```

   Levels are `error`, `warn`, `info`, `debug`, `trace`. Per-module filters use the same syntax, so
   prefer the narrowest scope that answers your question:

   ```sh
   curl -X POST "http://127.0.0.1:15000/logging?level=info,proxy::httpproxy=trace"
   ```

3. **Reproduce the problem**, reading stdout. Access logs are written per request; they can also be
   exported over OTLP or persisted to a database without losing the stdout stream.

4. **Put it back.** Re-POST the exact string from step 1.

   ```sh
   curl -X POST "http://127.0.0.1:15000/logging?level=<the string you saved>"
   ```

## Reversibility

- **Reversal:** `POST /logging` with the previous level. No time window — the change is runtime-only
  and non-persistent.
- **Second reversal:** a restart also reverts it, because the level comes from `config.logging.level`
  in the configuration file at startup.
- Setting the level permanently is a config-file change, not an API call:

  ```yaml
  config:
    logging:
      level: debug
      format: json
  ```

## Cautions

- `trace` on a busy proxy is expensive and will log request detail. Scope it per module and put it
  back promptly.
- This endpoint is unauthenticated on loopback. Anything that can reach the admin port can change
  your log level.
