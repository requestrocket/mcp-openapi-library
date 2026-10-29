---
name: create-requestrocket-target-credential
description: >-
  Creates a RequestRocket target credential, the secret RequestRocket uses to
  call an upstream API. Use when asked to add upstream auth, a vendor key, or
  a target credential. Applies in any agent.
---

# Create a RequestRocket target credential

A target credential is how RequestRocket authenticates to the upstream API. The record is `credentialType=target`. Callers never see this secret. If a RequestRocket guide is available, load `credential-creation` and follow the target direction.

## Allowed auth types

`key`, `bearer`, `basic`, `oauth2`, `customToken`, `custom`, `passthrough`, `none`.

Match the vendor's security scheme. When the scheme comes from a file under `Platforms/`, read `security` and `securitySchemes` in that file. Do not copy example keys or tokens out of the file. Ask for the real secret, or stop.

## Secret shape

- `key`: `key`, `value`, `addToHeader`. `key` is the header name the vendor expects. It does not have to be `Authorization`.
- `bearer`: `token`, `addToHeader` true. `prefix` is optional.
- `basic`: `username`, `password`. `prefix` is optional.
- `oauth2` client credentials: `appClientId`, `grantType` `client_credentials`, `clientSecret`, `accessTokenURL`, `addToHeader`.
- `oauth2` authorization code: `appClientId`, `grantType` `authorization_code`, `clientSecret`, `authURL`, `accessTokenURL`, `redirectUri`, `addToHeader`. `redirectUri` is the OAuth callback from `get_meta`, not a URI taken from the vendor specification. Use `get_meta` for PKCE as well.
- `customToken`: `tokenLocation`, `tokenPath`, `addToHeader`, `key`. Optional: `tokenURL`, `tokenMethod`, `tokenHeaders`, `tokenQueryParams`, `tokenData`, `tokenExpiryPath`, `tokenExpiryMethod`, `prefix`.
- `custom`: optional `additionalHeaders`, `additionalQueryParams`, `additionalData`.
- `passthrough`: secret is `{}`. Forwards the caller's `Authorization` header, and `X-API-Key` and `Proxy-Authorization` when the caller sent them.
- `none`: secret is `{}`.

## Create it

`create_credential`:

- `clientId`
- `credentialName`
- `credentialType` — `target`
- `credentialAuthType` — one of the types above
- `credentialRegion` — the same region as the target
- `credentialSecret` — the shape above
- `credentialNotes` — optional

The target must already exist. Follow `skills/create-requestrocket-target/SKILL.md` first. Save `credentialId`.

Show the name, auth type, and which target it is for, and wait for confirmation before creating. Do not deactivate or delete a credential unless asked.
