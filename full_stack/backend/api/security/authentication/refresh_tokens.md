<div align='center'>
    <h1> API Security & Access Control </h1>
    <h2> Refresh Tokens </h2>
</div>

# Table of Contents

- [Refresh Tokens](#refresh-tokens)
  - Refresh-Token Rotation
  - Refresh-Token Expiration
  - Token Revocation
- [References](#references)

# Refresh Tokens

Authentication tokens must be handled carefully because anyone who obtains a valid token may be able to impersonate the user until the token expires or is revoked.

A refresh token is a long-lived token issued alongside a short-lived JWT (access token). While access tokens are used to access protected resources (e.g., APIs), refresh tokens are used to obtain new access tokens when the old ones expire. This ensures continuous access without requiring users to re-enter their credentials frequently.

## Refresh-Token Rotation

With rotation, every successful refresh operation replaces the existing refresh token with a new one:

1. The client authenticates and receives an access token and a refresh token.
2. The client uses the access token to access protected APIs.
3. The access token expires.
4. The client sends the refresh token to the authorization server.
5. The authorization server validates the refresh token.
6. The authorization server issues a new access token and a new refresh token.
7. The previous refresh token is invalidated.

## Refresh-Token Expiration

Refresh tokens should have a finite lifetime. When it expires, the user must authenticate again or follow whatever reauthentication policy the application requires.

## Token Revocation

Expiration alone is not sufficient for every production system. Consider the following situations:

1. The user logs out.
2. The user changes their password.
3. The user's device is lost or stolen.
4. A refresh token is suspected of being compromised.
5. An administrator disables the user's account.
6. The user wants to terminate all active sessions.

In these situations, the authentication system may need to revoke tokens or sessions before their normal expiration time.

---

# References

[1] https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-using-the-refresh-token.html
