---
name: create-requestrocket-mcp
description: >-
  Creates a RequestRocket MCP from an OpenAPI specification in this library.
  Use when asked to create, wire, or publish an MCP for a platform whose spec
  is under Platforms/, or to turn one of these OpenAPI files into RequestRocket
  tools. Applies in any agent, including Claude and Cursor.
---

# Create a RequestRocket MCP from this library

If a RequestRocket guide is available, load `mcp-creation` and follow it. Carry the steps out in the RequestRocket app, or through the RequestRocket API, whichever this session can use.

Choose one connection mode before creating records. Do not send both sets of ids.

**Direct mode.** The MCP calls the upstream API itself. It needs a target, a target credential, and an inbound credential. There is no proxy. Follow, in order:

1. `skills/create-requestrocket-target/SKILL.md`
2. `skills/create-requestrocket-target-credential/SKILL.md`
3. `skills/create-requestrocket-inbound-credential/SKILL.md` for a shared secret, or `skills/create-requestrocket-consumer/SKILL.md` for per-user sign-in

`create_mcp` then takes `mcpTargetId`, `mcpTargetCredentialId`, and `mcpProxyCredentialId`. For the consumer pool, `mcpProxyCredentialId` is the literal `consumer_pool`. Leave `mcpProxyId` unset.

**Proxy-backed mode.** The MCP uses a proxy that already forwards to the upstream API. Follow `skills/create-requestrocket-proxy/SKILL.md` when that proxy does not exist yet. That skill creates the target and both credentials, then the proxy. For per-user sign-in, the proxy uses `consumer_pool` instead of an inbound credential. Follow `skills/create-requestrocket-consumer/SKILL.md`. `create_mcp` then takes `mcpProxyId` only. Do not send `mcpTargetId`, `mcpTargetCredentialId`, or `mcpProxyCredentialId` with it.

## Consumer pool

The consumer pool is per-user OAuth. Each caller is invited and entitled separately. It is a different identity pool from the RequestRocket app, and the client needs the `mcp_user_pool` feature.

Follow `skills/create-requestrocket-consumer/SKILL.md` whenever `consumer_pool` is the inbound id.

- Direct mode: `mcpProxyCredentialId` is `consumer_pool`. After the MCP exists, invite the person and entitle them to that MCP.
- Proxy-backed mode: the backing proxy's `proxyProxyCredentialId` is `consumer_pool`. Entitle the person to that proxy. The MCP does not get a separate entitlement.

## Pick the specification

1. Find the platform folder under `Platforms/`. The specification is `{kebab-name}-openapi.json`, `.yaml`, or `.yml`. `microsoft` is shortened to `ms` (`Platforms/Microsoft Graph/ms-graph-openapi.yaml`).
2. Use that file. Do not search the web for a second copy.
3. Read `info.title`, the server URL, and `security` / `securitySchemes` before creating records.
4. If the folder has sibling files that the specification `$ref`s with a relative path, bundle those files into one document first. RequestRocket resolves only `#/` references. It does not fetch `./` or `../` files.
5. Count operations (`get`, `post`, `put`, `patch`, `delete`, `head`, `options`). More than 200 operations is rejected. Ask which operations to keep, then upload a document that stays at or under 200. Do not drop operations silently.

## Create the MCP

`create_mcp`:

- `clientId` of the RequestRocket client
- `mcpName`
- `mcpRegion` — the same region used for the target and credentials. Use the region the user names.
- Direct mode: `mcpTargetId`, `mcpTargetCredentialId`, `mcpProxyCredentialId`
- Proxy-backed mode: `mcpProxyId` only

`mcpAlias` is optional. Lowercase and URL-safe. Confirm the slug first.

Publish the specification as the `openapi` field: the raw JSON or YAML string. On create, if `tools` is omitted, RequestRocket builds the catalog from that document. Each operation becomes one tool. Deprecated operations are created disabled. Local `#/` refs are inlined. The tool name comes from `operationId`, sanitized to letters, numbers, `_`, and `-`, and truncated to 64 characters.

In the app, paste the document on the MCP Tools tab, preview, then save. On a later update, send `openapi` together with the `tools` catalog. An update that sends only `openapi` stores the document and does not rebuild tools.

Then read the MCP URL (`get_mcp_url`) for that id and region. It looks like `https://{region}.requestrocket.com/mcp/{mcpId}`, with the stage prefix when the account is not production.

## Confirm before writing

Creating a target, a credential, or an MCP changes the account. Show the platform, base URL, auth types, and operation count, and wait for confirmation before the first create. Do not deactivate the MCP unless asked. Do not delete an MCP.

## After it exists

Read the MCP and check `referenceStatus`. `ok` means the target and credentials are still there. If the target base URL and the specification server URL differ, tool paths call the wrong host. Fix the target. Do not rewrite the specification to hide that.
