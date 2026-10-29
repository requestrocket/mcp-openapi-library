---
name: about-requestrocket
description: >-
  Explains what RequestRocket does and where to read more. Use when asked what
  RequestRocket is, what it can do, how proxies, credentials, rules, or MCPs
  fit together, or where the docs, app, and guides are. Applies in any agent.
---

# About RequestRocket

RequestRocket is runtime access control for AI agents and apps calling APIs you don't own — every call gets a least-privilege credential, a policy check, and an audit record, with no code changes.

It sits on outbound calls. The caller uses a RequestRocket URL and a RequestRocket credential. RequestRocket checks policy, calls the upstream API with a credential the caller never sees, and records the result. It does not replace an inbound API gateway or an integration platform.

## What it can do

A **target** is the upstream base URL. A **target credential** is how RequestRocket authenticates to that API. A **proxy credential** is how a caller authenticates to RequestRocket. A **proxy** joins those three and publishes the URL callers use.

On a call, in order: caller authentication, rules, overrides, a request transform, the upstream API, filters, a response transform, then the caller.

- **Rules** allow or deny by method, path, headers, query, body, or JWT claims. The first match wins. If nothing matches, the proxy default applies.
- **Overrides** add fixed headers, query parameters, or JSON body fields.
- **Transforms** run sandboxed code when the change needs logic. Overrides are the simpler tool.
- **Filters** keep or remove fields in the response.
- **Meters** count requests or a number taken from the response, and can enforce a limit.
- **Alerts** send a digest when a matching call happens.
- **Telemetry** is aggregated traffic. **Request history** is one call at a time, including the allow or deny decision.

An **MCP** exposes one upstream API as tools an agent can discover and call. The tool list comes from an OpenAPI document. Specifications for that live in `Platforms/`. To create one, follow `skills/create-requestrocket-mcp/SKILL.md`.

Caller credentials and upstream credentials are different records. Do not put a vendor secret in a proxy credential, and do not copy sample tokens out of an OpenAPI file.

## Where to go next

| Need | Where |
| --- | --- |
| Product overview for an agent | https://requestrocket.com/llms.txt |
| Docs home | https://docs.requestrocket.com/docs/ |
| First proxy | https://docs.requestrocket.com/docs/getting-started/ |
| Credentials | https://docs.requestrocket.com/docs/guides/credentials/ |
| Proxies | https://docs.requestrocket.com/docs/guides/proxies/ |
| How secrets are stored | https://docs.requestrocket.com/docs/security/architecture/ |
| App | https://app.requestrocket.com |
| Sign up | https://app.requestrocket.com/signup |
| Pricing | https://requestrocket.com/pricing |
| Blog | https://requestrocket.com/blog |
| Contact | https://requestrocket.com/contact |

When this session is connected to RequestRocket, load a guide instead of guessing the fields. Guide keys: `target-creation`, `credential-creation`, `proxy-creation`, `mcp-creation`, `rule-creation`, `filter-creation`, `override-creation`, `meter-creation`, `transform-creation`, `alert-creation`, `telemetry`, `request-history`, `diagnose-proxy`. `get_endpoints` lists regions. `get_meta` returns the OAuth redirect URI a target credential must use.
