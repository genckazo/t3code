# Product analytics

This fork has none, and must not grow any.

- [`AnalyticsService`](../../apps/server/src/telemetry/AnalyticsService.ts) is a no-op kept only so
  upstream call sites merge cleanly: `record` drops the event and returns `false`. Do not give it
  an HTTP client, an endpoint, or an identity.
- [`relayTracing.layer`](../../packages/shared/src/relayTracing.ts) never installs a relay tracer,
  so `withRelayClientTracing` leaves spans in the local trace.
- The mobile app has no crash reporter (`expo-observe` is not a dependency).
- Clients do not send account or device hashes when they connect.

When merging from upstream, check new code for outbound requests that report usage, identity, or
errors to a T3-operated or third-party endpoint, and remove them. User-configured OTLP export to
the user's own receiver is fine.
