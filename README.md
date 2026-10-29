# mcp-openapi-library

Vendor OpenAPI specifications used to create MCPs in RequestRocket.

## RequestRocket

RequestRocket is runtime access control for AI agents and apps calling APIs you don't own — every call gets a least-privilege credential, a policy check, and an audit record, with no code changes.

A caller uses a RequestRocket URL. A shared credential, or a sign-in through the consumer pool, identifies them. RequestRocket checks policy, calls the upstream API with a credential the caller never sees, and records the result. An MCP turns one of the specifications in this library into tools an agent can discover and call.

More on the product is in `skills/about-requestrocket/SKILL.md`, and at [requestrocket.com/llms.txt](https://requestrocket.com/llms.txt) and [docs.requestrocket.com](https://docs.requestrocket.com/docs/).

## Specifications

Each platform has its own folder under `Platforms/`. The folder name is the platform name, with a trailing “API” removed and characters that are invalid in Windows paths replaced by hyphens. The specification file is the kebab-case form of that name, with `microsoft` shortened to `ms`, plus `-openapi.json`, `-openapi.yaml`, or `-openapi.yml`.

```text
Platforms/Nimble/nimble-openapi.json
Platforms/Microsoft Graph/ms-graph-openapi.yaml
```

When a vendor publishes one document, that document is stored here. When a vendor publishes many APIs, the folder holds a primary specification for the product rather than every file they publish. A few vendors split a specification across files; those related files stay in the same folder so relative references still resolve.

Not every RequestRocket platform is represented. A platform is omitted when no JSON or YAML OpenAPI document could be retrieved.

## Skills

An agent creating a RequestRocket MCP from one of these files should follow `skills/create-requestrocket-mcp/SKILL.md`. That skill chooses direct mode or proxy-backed mode and uses the skills below.

| Skill | Use it to |
| --- | --- |
| `skills/about-requestrocket/SKILL.md` | Read what RequestRocket does and where the docs are |
| `skills/create-requestrocket-target/SKILL.md` | Create the upstream base URL |
| `skills/create-requestrocket-target-credential/SKILL.md` | Store how RequestRocket calls the vendor |
| `skills/create-requestrocket-inbound-credential/SKILL.md` | Store a shared credential callers present |
| `skills/create-requestrocket-consumer/SKILL.md` | Invite a person and entitle them when the inbound id is `consumer_pool` |
| `skills/create-requestrocket-proxy/SKILL.md` | Join a target and credentials into a caller URL |
| `skills/create-requestrocket-mcp/SKILL.md` | Publish one of these specifications as an MCP |

## Why RequestRocket

These specifications describe APIs you do not own. You cannot add authentication, a path rule, or an audit log to the vendor. A vendor key pasted into an agent or an app is a secret that caller holds, and a call that should have been refused still reaches the upstream.

RequestRocket sits on that outbound call. The caller presents a RequestRocket credential, or signs in through the consumer pool. The vendor secret stays in the target credential. Every call is checked against policy and written to an audit record. The agent and the upstream API stay as they are.

An MCP from this library is that same control, published as tools. The agent discovers the operations in the specification and calls the RequestRocket URL. It never receives the vendor key.

## Disclaimer

These files are copies of specifications published by the vendors, or complete community copies of those specifications when the vendor file itself could not be downloaded. RequestRocket does not author, own, or warrant them.

A file can be incomplete, outdated, or limited to one product area. Titles, hosts, and example values are whatever the source document contained. Sample keys, tokens, and account identifiers are examples only and are not credentials. Check the vendor’s current documentation before relying on a specification, and respect the license that came with the original document.
