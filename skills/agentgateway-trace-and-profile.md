---
name: agentgateway-trace-and-profile
description: Find out why a request took the route it took, and where an agentgateway process is spending CPU or memory, using the tap-style trace stream and the pprof endpoints - with the platform limits that make half of it silently useless on macOS.
api: agentgateway
generated: '2026-08-30'
method: generated
source: openapi/agentgateway-debug-api-openapi.yml + openapi/agentgateway-profiling-api-openapi.yml + openapi/agentgateway-memory-api-openapi.yml + https://agentgateway.dev/docs/standalone/latest/operations/debug/
operations:
  - GET /debug/trace
  - GET /debug/tasks
  - GET /debug/pprof/profile
  - GET /debug/pprof/heap
  - GET /memory
license: CC-BY-NC-SA-4.0
---

# Trace and profile agentgateway

Two different questions, two different tools. "Why did this request behave that way?" is a trace.
"Why is this process slow or fat?" is a profile.

## Trace one request

`GET /debug/trace` streams JSON-over-SSE for the **next** request the proxy handles. It shows the
route that was selected, the policies that were applied, the backend that was chosen and the response
status — which is how you answer why a request matched, or failed to match, a route.

Use the CLI rather than the raw stream:

```sh
# terminal 1
agctl proxy trace --local

# terminal 2
curl http://127.0.0.1:3000/headers
```

`agctl` opens a TUI walking the request/response lifecycle. `--raw` prints JSON Lines instead, which
is what you want when piping to another tool.

This is a long-lived stream, not a request/response call. If you are driving it programmatically,
treat it as SSE and set your own read deadline — nothing on the server side will close it for you.

## Inspect the task tree

```sh
curl -s http://127.0.0.1:15000/debug/tasks
```

Returns the live tokio task tree. Useful when the proxy is up but wedged.

## Memory statistics

```sh
curl -s http://127.0.0.1:15000/memory
```

Allocator and process memory statistics. Cheap; a reasonable first look before reaching for a heap
profile.

## Profiles — read this before you spend an hour

**Profiling data is available only when agentgateway runs on Linux.** On macOS and Windows builds:

- the CPU profile endpoint is not registered, so `agctl proxy profile cpu` fails with `404 Not Found`;
- `agctl proxy profile heap` succeeds and writes a profile that **contains no allocation samples**.

The heap case is the trap: you get a file, it looks like a result, and it is empty. Check the
platform first.

On Linux:

```sh
agctl proxy profile cpu --local --seconds 30 -o ./cpu.pprof
agctl proxy profile heap --local -o ./heap.pprof
go tool pprof ./cpu.pprof
```

Send traffic through the gateway **while the CPU profile runs**, or it contains no samples for a
second, unrelated reason. Install Graphviz if you want the visual output.

The raw endpoints behind those commands are `GET /debug/pprof/profile` (`?seconds=` 1–300, default
10; `?frequency=` 1–1000 Hz, default 100) and `GET /debug/pprof/heap`. Note the endpoint default of
10 seconds differs from `agctl`'s documented 30-second default.

## Where the real telemetry lives

The admin endpoints are for debugging one process on one host. Steady-state observability is
elsewhere and does not require the admin port:

- Prometheus-compatible metrics on port `15020`, with a published metrics reference.
- OTLP traces, with a published span-attribute reference.
- Structured access logs on stdout, exportable over OTLP or to a database.
