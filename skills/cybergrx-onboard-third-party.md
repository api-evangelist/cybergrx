---
name: Onboard and profile a third party
description: Add a vendor to the CyberGRX / ProcessUnity Global Risk Exchange, answer its inherent-risk scoping questions, and tag it.
api: https://api.cybergrx.com/v1/swagger/
method: generated
source: https://github.com/CyberGRX/api-examples
operations:
  - "GET /v1/third-parties"
  - "POST /v1/third-parties"
  - "PUT /v1/third-parties/{id}/scoping"
  - "PUT /v1/third-parties/{id}/tagging"
---

# Onboard and profile a third party

Operating instructions for adding a vendor to the CyberGRX / ProcessUnity
Global Risk Exchange (GRX) and setting up its inherent-risk profile.

## Prerequisites
- An account API token. Generate it in the GRX UI under **Manage my company
  user accounts -> Access Tokens -> Add a new token** (the secret is shown only
  once). Send it verbatim in the `Authorization` header on every request.
- Base URL: `https://api.cybergrx.com` (use `https://demo-api.cybergrx.com` to
  test).

## Steps
1. **Check whether the vendor already exists.**
   `GET /v1/third-parties?limit=1&name=<company_name>` with header
   `Authorization: <token>`. If a match is returned, capture its id and skip
   creation.
2. **Create the third party** if it does not exist.
   `POST /v1/third-parties` with a JSON body containing at least `name`, and
   ideally `url` and `address` (`city`, `country`) — records with a URL and full
   address profile more reliably. Capture the returned third-party id.
3. **Answer the inherent-risk scoping questions.**
   `PUT /v1/third-parties/{id}/scoping` with the profile answers. Answer values
   are one of `Least`, `Minimal`, `Moderate`, `Significant`.
4. **Tag the vendor** for your own segmentation.
   `PUT /v1/third-parties/{id}/tagging` with body `{"tags": ["..."]}`.

## Rules
- Auth: API token in the `Authorization` header (see
  `authentication/cybergrx-authentication.yml`).
- No documented idempotency key — do not blindly retry `POST`; look the vendor
  up by name first (step 1) to avoid duplicates.
- Conventions: `conventions/cybergrx-conventions.yml`.
