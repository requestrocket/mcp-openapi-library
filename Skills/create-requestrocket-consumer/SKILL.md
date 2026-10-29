---
name: create-requestrocket-consumer
description: >-
  Invites a RequestRocket MCP consumer and entitles them to an MCP or a proxy
  that uses the consumer pool. Use when asked to add a caller, invite a user
  to an MCP, or turn on per-user OAuth with consumer_pool. Applies in any agent.
---

# Create a RequestRocket consumer

A consumer is a person who signs in to call an MCP or a proxy. The account is in the MCP consumer pool. It is not a RequestRocket app user, and it is not a proxy credential. Do not look up or copy a `sub` from the RequestRocket app user pool.

The client needs the `mcp_user_pool` feature. If it is missing, stop. Consumer pool authentication is not available on that plan.

`consumer_pool` is the literal inbound id. It replaces a proxy credential.

- Direct MCP: set `mcpProxyCredentialId` to `consumer_pool`. Still set `mcpTargetId` and `mcpTargetCredentialId`. Leave `mcpProxyId` unset.
- Proxy, including a proxy that backs an MCP: set `proxyProxyCredentialId` to `consumer_pool`. Still set the target and the target credential. Do not create an inbound credential for that proxy.

Confirm before using `consumer_pool`.

## Invite the person

In the app, invite them from the MCP page. Through the API:

`POST clients/{clientId}/consumers`

- `email`
- `givenName`
- `familyName`

Save `sKey` as the consumer id, and `cognitoSub`. That `cognitoSub` is the consumer-pool subject. The invitation email contains a temporary password. They choose their own password the first time they sign in, and they reset a forgotten password from that same page.

One email is one consumer for that client. If the invite says the email already exists, use that consumer. The same person can be entitled to more than one MCP or proxy, including other regions, and keeps one password.

Show the name, email, and which MCP or proxy they will be able to call, and wait for confirmation before inviting.

## Entitle them

An invite does not grant access. Assign the consumer in a second step. Send `mcpId` or `proxyId`, not both. `expiresAt` is optional and must be a future ISO timestamp.

`POST clients/{clientId}/consumers/{consumerId}/entitlements`

- Direct MCP: `{ "mcpId": "<mcpId>" }`. Access is checked against that MCP.
- Proxy, or an MCP backed by that proxy: `{ "proxyId": "<proxyId>" }`. The proxy entitlement is the source of truth. A proxy-backed MCP does not get its own entitlement.

The caller then uses the MCP or proxy URL and signs in as that consumer. There is no shared token to paste into the client.

Do not remove a consumer or an entitlement unless asked. Removing an entitlement stops that person calling that MCP or proxy. Removing the consumer deletes every entitlement they have with this client.
