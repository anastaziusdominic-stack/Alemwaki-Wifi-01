# Alemwaki Wi-Fi backend starter

A Node.js/Express API with SQLite storage, admin JWT login, package management, voucher creation/validation, settings (including editable M-Pesa display number), and a webhook placeholder. It is a starter, not a live payment or router integration.

## Local setup
1. Install Node.js 20+.
2. Copy `.env.example` to `.env` and replace every `REPLACE...` value with strong secrets. Set `FRONTEND_ORIGIN` to your deployed frontend URL (comma-separated allowed origins).
3. Run `npm install`.
4. Run `npm run setup-admin` to create the admin account from `.env`.
5. Run `npm start`; API runs at `http://localhost:3000`.

## Endpoints
- `GET /health`
- `GET /api/packages` (public active packages)
- `GET /api/public/settings` (public business/payment display settings)
- `POST /api/admin/login` `{username,password}`
- Admin requests use `Authorization: Bearer <token>`
- `GET/POST /api/admin/packages`, `PATCH /api/admin/packages/:id`
- `POST /api/admin/vouchers` `{package_id,count}`; `GET /api/admin/vouchers`
- `POST /api/vouchers/redeem` `{code,phone?}`
- `GET/PUT /api/admin/settings`
- `POST /api/payments/webhook` (placeholder; HMAC verification, provider payload mapping not implemented)

## Alemwaki configuration
- Current display payment number is `0749734342` (Vodacom M-Pesa). It is a display destination only; automatic payment verification is not active. A Vodacom merchant/API onboarding and the official API credentials/callback configuration are required for real integration.
- Package prices and durations can be changed with the authenticated package endpoints (`GET/POST /api/admin/packages`, `PATCH /api/admin/packages/:id`). Frontend demo package data is currently local to the browser and is not yet wired to these endpoints.
- MikroTik support is planned through RouterOS API, not tied to one hardware model. Compatibility depends on RouterOS version, hotspot configuration, firewall/NAT and API reachability. A router-specific adapter, test and secure credentials are still needed before automatic access provisioning. Do not expose router API to the public internet.

## Before going live
- Choose a hosting provider and configure HTTPS, secrets and persistent disk/database backups.
- Replace the generic webhook with the official integration for the selected payment provider; never grant access based on a browser success page.
- Configure the MikroTik API integration using router-specific credentials and least privilege. This project does not yet activate users on the router.
- Use a production database and migration strategy if scaling beyond a small deployment; SQLite is included for easy testing.
- Do not expose admin credentials, JWT secret, payment secret or router credentials in frontend code or public repositories.
- Test all payment and access flows with provider sandbox and a router test environment before serving customers.
