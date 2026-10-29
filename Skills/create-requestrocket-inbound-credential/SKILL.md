---
name: create-requestrocket-inbound-credential
description: >-
  Creates a RequestRocket inbound credential, the proxy credential callers use
  to authenticate to a proxy or MCP. Use when asked to add caller auth, a proxy
  credential, or an inbound credential. Applies in any agent.
---

# Create a RequestRocket inbound credential

An inbound credential is how a caller authenticates to RequestRocket. The record is `credentialType=proxy`. It is not the vendor secret. If a RequestRocket guide is available, load `credential-creation` and follow the proxy direction.

## Allowed auth types

`key`, `bearer`, `basic`, `jwt`, `jwtVerify`.

`oauth2`, `custom`, and `none` are rejected for a proxy credential. `jwt` checks that the token string matches. Use `jwtVerify` when the caller JWT must be verified.

For per-user OAuth, do not create a proxy credential. Follow `skills/create-requestrocket-consumer/SKILL.md` and use `consumer_pool`.

## Secret shape

Ask the user for the real secret. Do not copy a sample token out of an OpenAPI file.

- `key`: `key`, `value`, `addToHeader`. For a proxy credential, `key` is `Authorization`.
- `bearer`: `token`, `addToHeader` true. `prefix` is optional.
- `basic`: `username`, `password`. `prefix` is optional.
- `jwt`: `token`, `addToHeader` true. `prefix` is optional. Equality only.
- `jwtVerify`: `algorithms`, plus `jwksUri` or `sharedSecret`. Optional: `issuer`, `audience`, `subject`, `clockSkewSeconds`, `failPolicy`, `jtiValidation`, `requiredClaims`.

## Create it

`create_credential`:

- `clientId`
- `credentialName`
- `credentialType` — `proxy`
- `credentialAuthType` — one of the types above
- `credentialRegion` — the same region as the proxy or MCP
- `credentialSecret` — the shape above
- `credentialNotes` — optional

Save `credentialId`. Create a separate inbound credential for each proxy. Do not reuse one across proxies.

Show the name, auth type, and region, and wait for confirmation before creating. Do not deactivate or delete a credential unless asked.
