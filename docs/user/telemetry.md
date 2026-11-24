# Telemetry

This fork sends no telemetry. Product usage events, hashed account and install identifiers, relay
connection traces, and mobile crash reports have all been removed from the code; there is no
setting to turn them back on.

- **Product usage data.** The server does not send events to `ingest.t3.codes` or PostHog, and no
  longer reads your Codex or Claude sign-in files to derive an identifier.
- **Relay connection diagnostics.** The server, web app, and mobile app do not export connection
  traces to Axiom.
- **Mobile crash reports.** The mobile app does not include a crash reporter. Startup crashes are
  still recorded on the device and shown in **Settings > About > Diagnostics**, where you can copy
  them yourself.
- **Agent feedback.** Reports are saved on your machine and never sent. `t3 feedback list` shows
  them.

`T3CODE_TELEMETRY_ENABLED` and the managed policy's telemetry limit are still accepted, and have
nothing left to control.

## What still uses the network

These are features, not telemetry, and each one talks only to the service it names:

- Your coding agents and providers (Codex, Claude Code, and so on) talk to their own services.
- T3 Connect, if you sign in, routes remote connections through the T3 Connect relay.
  **Publish agent activity** under **Settings > Sharing & access** sends project and thread titles,
  model names, and activity status to that relay for
  [mobile notifications](./mobile-notifications.md); leave it off to keep those on your machine.
- Update checks, and source control or issue tracker integrations you connect.

## Export diagnostics to your own receiver

To send traces, metrics, or logs to an OpenTelemetry receiver you run, open
**Settings > Maintenance > Diagnostics**, select an environment, and enter each signal's OTLP HTTP
endpoint under **OpenTelemetry export** (for example `http://localhost:4318/v1/traces`). Save, then
restart that environment's server. Nothing is exported unless you set an endpoint.

`T3CODE_OTLP_*_URL` and standard `OTEL_EXPORTER_OTLP_*` environment variables override the saved
settings. Set `T3CODE_OTLP_HEADERS` for receivers that need authentication and
`T3CODE_OTLP_PROTOCOL=http/protobuf` for receivers that do not accept JSON.
