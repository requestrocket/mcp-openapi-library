---
name: create-requestrocket-target
description: >-
  Creates a RequestRocket target, the upstream API base URL a proxy or MCP
  calls. Use when asked to add, create, or choose a target, or to set
  targetBaseURL from an OpenAPI server URL. Applies in any agent.
---

# Create a RequestRocket target

A target is the upstream API. It is not a credential and it is not the URL callers use. If a RequestRocket guide is available, load `target-creation` and follow it.

## Create it

`create_target`:

- `clientId` — the RequestRocket client
- `targetName` — a human-readable label, such as the API name
- `targetBaseURL` — full URL, including `https://`, with no trailing slash
- `targetRegion` — the AWS region the user names. When connected to RequestRocket, `get_endpoints` lists the regions that are deployed. Do not invent one.
- `targetTestPath` — optional health-check path, such as `/v1/models`
- `targetNotes` — optional

Save `targetId`. The next record is a target credential, which is how RequestRocket authenticates to this URL. Follow `skills/create-requestrocket-target-credential/SKILL.md`.

Use a region close to the upstream API. Show the name, base URL, and region, and wait for confirmation before creating. Do not deactivate or delete a target unless asked.

## Base URL from a specification in this library

When the URL comes from a file under `Platforms/`:

- OpenAPI 3: `servers[0].url`, with a trailing slash removed.
- Swagger 2, when there is no `servers` array: `{scheme}://{host}{basePath}`, with a trailing slash removed. Prefer `https` when `schemes` lists it.
- If the URL contains `{placeholders}`, stop and ask for the real host. Do not create a target with an unresolved template.

That URL is what tool and proxy paths are appended to. A shorter host, or a leftover trailing slash, sends calls to the wrong place.
