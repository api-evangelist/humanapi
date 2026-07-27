---
name: Manage report subscriptions for a user
description: Subscribe a recipient to a user's reports, list and inspect subscriptions, and remove them.
api: openapi/humanapi-admin-openapi.yml
operations: [createSubscription, getUsersSubscriptions, getSubscriptionDetails, deleteSubscription]
---

# Manage report subscriptions

Subscriptions attach recipients (by email/role) to a user's reports.

## Auth
Bearer JWT from `POST https://auth.humanapi.co/v1/admin/token`.

## Steps
1. **`createSubscription`** — `POST /api/v1/subscriptions` with `clientUserId`,
   `subscriberEmail`, and `role` (optional `subscriberFirstName`/`subscriberLastName`/
   `subscriberOffice`). Returns `201`; `409` if the subscription already exists.
2. **`getUsersSubscriptions`** — `GET /api/v1/subscriptions` to find subscriptions
   (filter by `subscriberEmail`, `role`, `clientUserId`; paginate with `offset`/`limit`).
3. **`getSubscriptionDetails`** — `GET /api/v1/subscriptions/{subscriptionId}`.
4. **`deleteSubscription`** — `DELETE /api/v1/subscriptions/{subscriptionId}`.

## Notes
- Error envelope and `x-humanapi-request-id` tracing per
  `errors/humanapi-problem-types.yml`.
