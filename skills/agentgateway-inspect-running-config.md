---
name: agentgateway-inspect-running-config
description: Read what an agentgateway proxy is actually running - binds, listeners, routes, backends and policies - from its loopback admin interface, and validate a candidate config against the first-party JSON Schema before changing anything.
api: agentgateway
generated: '2026-08-30'
method: generated
source: openapi/agentgateway-config-api-openapi.yml + https://agentgateway.dev/docs/standalone/latest/operations/debug/ + https://agentgateway.dev/schema/config
operations:
  - GET /config_dump
  - GET /logging
license: CC-BY-NC-SA-4.0
---

# Inspect a running agentgateway

Use this when you need to know what an agentgateway process is actually serving, as opposed to what
a configuration file in a repository says it should serve.

## Before you start

The admin interface binds to `127.0.0.1:15000` by default (configurable via `adminAddr`). It is
**loopback-only and unauthenticated**. You must run from the same host, pod, or container network
namespace as the proxy. If you get a connection error rather than an HTTP status, you are off-host —
that is the design, not a fault. Do not "fix" it by exposing the port.

The specs in `openapi/` were written by API Evangelist from the provider's documented endpoint table.
Agentgateway publishes no OpenAPI of its own, so treat operation names as ours and the paths and
methods as the provider's.

## Steps

1. **Dump the runtime configuration.**

   ```sh
   curl -s http://127.0.0.1:15000/config_dump > /tmp/agw-dump.json
   ```

   `GET /config_dump` returns binds, listeners, routes, backends, workloads, services and policies.
   Treat the output as sensitive: it is the full running configuration, and it comes back to any
   caller that can open a connection.

2. **Render it.** Raw JSON is hard to scan; the first-party CLI formats it.

   ```sh
   agctl proxy config all --file /tmp/agw-dump.json -o yaml
   ```

   `agctl proxy config backends` narrows to backend endpoint status. `agctl` is marked experimental
   by the provider — fine for inspection, not something to build automation on.

3. **Check the current log level** before you change it.

   ```sh
   curl -s http://127.0.0.1:15000/logging
   ```

   Capture the exact filter string it returns. See `agentgateway-change-log-level` for the reversal
   discipline.

4. **Validate a candidate configuration** against the provider's own schema rather than by
   restarting the proxy and watching it fail.

   Add the editor directive to the top of the file:

   ```yaml
   # yaml-language-server: $schema=https://agentgateway.dev/schema/config
   ```

   That URL floats to `main`. For a reproducible check, pin the release:

   ```
   https://raw.githubusercontent.com/agentgateway/agentgateway/refs/tags/v1.5.0/schema/config.json
   ```

   A local verbatim copy is in `json-schema/agentgateway-config-schema.json` (draft 2020-12, title
   `LocalConfig`, 302 definitions).

## What you can rely on

- Configuration reloads are fail-safe: a reload that fails validation keeps the last good
  configuration rather than stopping the process.
- Startup is fail-fast as of 1.5.0: a listener that cannot bind exits non-zero instead of logging a
  warning. Under a supervisor that restarts on exit, a port conflict becomes a crash loop.

## Errors

Neither is in the spec, so handle both:

- No HTTP status at all → you are calling from off-host.
- `404` on `/debug/pprof/profile` → profiling is registered on Linux builds only.
