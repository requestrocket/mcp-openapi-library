---
name: create-requestrocket-proxy
description: >-
  Creates a RequestRocket proxy that joins a target and two credentials into
  a caller URL. Use when asked to create a proxy, publish an upstream API, or
  attach an MCP to a proxy. Applies in any agent.
---

# Create a RequestRocket proxy

A proxy is the live forwarding endpoint. It needs three records first. If a RequestRocket guide is available, load `proxy-creation` and follow it.

1. A target. Follow `skills/create-requestrocket-target/SKILL.md`. Save `targetId`.
2. An inbound credential (`credentialType=proxy`). Follow `skills/create-requestrocket-inbound-credential/SKILL.md`. Save that `credentialId`. For per-user sign-in, skip that skill and set `proxyProxyCredentialId` to `consumer_pool`, as `skills/create-requestrocket-consumer/SKILL.md` describes.
3. A target credential (`credentialType=target`). Follow `skills/create-requestrocket-target-credential/SKILL.md`. Save that `credentialId`.

Use the same region on all three.

## Create it

`create_proxy`:

- `clientId`
- `proxyName`
- `proxyRegion` — the same region as the target and credentials
- `proxyProxyCredentialId` — the inbound credential id
- `proxyTargetId`
- `proxyTargetCredentialId`
- `proxyDefaultRuleEffect` — optional, `allow` or `deny`. `allow` is the default. Use `deny` when only explicit allow rules should pass.
- `proxyActive` — optional, default true

Save `proxyId`. Then `get_proxy_url` with that id and the same region. That URL is what callers use.

To check it, `execute_proxy_read_request` with the proxy URL and the inbound credential's token. Do not set `proxyActive` to false unless asked. `proxyMaxRetries` retries upstream failures, up to 10.

Show the name, target, both credentials, region, and default rule effect, and wait for confirmation before creating. Do not delete a proxy unless asked.
