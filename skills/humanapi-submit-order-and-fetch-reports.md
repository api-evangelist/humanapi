---
name: Submit a health-data order and fetch the resulting reports
description: Create a user, submit a data-retrieval order for them, then poll for and download the generated reports.
api: openapi/humanapi-admin-openapi.yml
operations: [getOrderTypes, createUser, performActionForUser, getUserReports, getReportById]
---

# Submit a health-data order and fetch reports

Human API's Admin API (`https://admin.humanapi.co`) turns a user + an order into
downloadable health reports. Authenticate first, then run the flow.

## Auth
Exchange your `client_id`/`client_secret` at `POST https://auth.humanapi.co/v1/admin/token`
for a JWT, then send `Authorization: Bearer <jwt>` on every Admin API call.

## Steps
1. **`getOrderTypes`** — `GET /api/v1/order-types` to list the order types configured
   for your client app. Pick the `orderType` (or `orderTypeAlias`) you need.
2. **`createUser`** — `POST /api/v1/users` with the member's `clientUserEmail` (and
   optional name/date-of-birth). Capture the returned `humanId`. A `409` means the user
   already exists — reuse the existing `humanId`.
3. **`performActionForUser`** — `POST /api/v1/users/actions` to submit the order action
   for that user (identify the user by `humanId` or `clientUserId`). Returns `202`
   (accepted, async).
4. **Wait for the webhook** — the `orders.OrderSummaryUpdated` notification fires when the
   order reaches a terminal state (`completed`/`cancelled`) and carries
   `reportsAvailable`. See `asyncapi/humanapi-notifications-webhooks.yml`.
5. **`getUserReports`** — `GET /api/v1/user/reports` to list the user's available reports.
6. **`getReportById`** — `GET /api/v1/user/reports/{reportId}` to fetch report content.

## Conventions
- Collections paginate with `offset` (default 0) and `limit` (default 50).
- Errors return `application/json` `{ statusCode, error, message, x-humanapi-request-id }`;
  log `x-humanapi-request-id` for support (see `errors/humanapi-problem-types.yml`).
- No idempotency-key header exists — guard `createUser` against duplicates via `409`.
