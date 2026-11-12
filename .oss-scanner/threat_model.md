# Threat model

## What this project does and where untrusted input enters
A Rust implementation of a Bitwarden-compatible password-manager server. Untrusted input: the
HTTP API (login, 2FA, vault CRUD, share/organization data), uploaded attachments, and
Bitwarden-protocol messages. Crypto handling (AES, PBKDF/Argon2, SOTP) is central.

## Components that matter most / least
- most: the crypto/verification code, the API request deserialization, attachment upload, and
  the organization/share permission logic.
- less: the web-vault UI (if present).

## How to exercise it
Exercise the API with crafted vault/organization payloads; test crypto paths with edge-case
keys; upload crafted attachments.

## How you rate severity
- Auth bypass, crypto weakness enabling vault decryption, or RCE: critical.
- Authorization bugs (accessing another user's vault): high.
- DoS: medium.

## Anything to leave alone
- The bundled web vault is a build artifact; focus on the Rust server.
