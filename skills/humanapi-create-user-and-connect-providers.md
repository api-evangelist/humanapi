---
name: Create a user and connect their health-data providers
description: Register a user, obtain their connect link, and read back the providers linked to them.
api: openapi/humanapi-admin-openapi.yml
operations: [createUser, getUserDetails, userProviders, performActionForUser]
---

# Create a user and connect their providers

## Auth
Bearer JWT from `POST https://auth.humanapi.co/v1/admin/token`
(`client_id`/`client_secret`). Send `Authorization: Bearer <jwt>`.

## Steps
1. **`createUser`** — `POST /api/v1/users` with `clientUserEmail`. Capture `humanId` and
   the `inviteLink` from the response so the member can connect their accounts.
2. **`performActionForUser`** — `POST /api/v1/users/actions` to trigger user-scoped
   actions (e.g. re-send invite / initiate a connection) by `humanId` or `clientUserId`.
3. **`getUserDetails`** — `GET /api/v1/users/{humanId}` to read the user's `status`,
   `suggestedSources`, and `inviteLink`.
4. **`userProviders`** — `GET /api/v1/users/providers` to retrieve the list of health-data
   providers (providers/labs/pharmacies/devices) linked to the user.

## Notes
- For end-user connection UIs use a Connect token from
  `POST https://auth.humanapi.co/v1/connect/token` (per-user, supports `extra_scopes`).
- See `conventions/humanapi-conventions.yml` for pagination and request-id tracing.
