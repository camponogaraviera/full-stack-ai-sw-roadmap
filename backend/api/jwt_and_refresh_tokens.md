<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.2 API Development and Communication</h2>
    <h3> JSON Web Tokens and Refresh Tokens </h3>
</div>

# Table of Contents

- [JSON Web Token](#json-web-token)
- [Secure Token Storage](#secure-token-storage)
- [Refresh Tokens](#refresh-tokens)
  - Refresh-Token Rotation
  - Refresh-Token Expiration
  - Token Revocation
- [Connection with REST APIs](#connection-with-rest-apis)
- [Connection with AWS Cognito](#connection-with-aws-cognito)

---

# JSON Web Token

A JSON Web Token (JWT) is a compact, URL-safe way of representing claims to be transferred between two parties. It is commonly used for stateless authentication (identity) and authorization (permission) in web applications. Not every authentication token is a JWT.

- A JWT contains three parts:
  - `Header`: The header typically consists of two parts.
    - The type of token (which is always JWT).
    - The signing algorithm being used (e.g., HS256 or RS256).
  - `Payload`: The payload contains the claims, which are statements about an entity (typically, the user) and additional metadata. It could be an **ID Token Payload** or an **Access Token Payload**.
  - `Signature`: Ensures the token's integrity and authenticity.

---

# Secure Token Storage

Tokens should be protected from unauthorized access.

For browser-based applications, avoid storing long-lived authentication credentials such as refresh tokens in `localStorage` or `sessionStorage`. If an attacker can execute malicious JavaScript through a Cross-Site Scripting (XSS) vulnerability, JavaScript can potentially read tokens stored there and send them to an attacker.

A common approach is to store a session/refresh credential in a cookie configured with security attributes such as:

- `HttpOnly`: Prevents JavaScript from directly reading the cookie. It reduces the impact of token theft through XSS, although it doesn't prevent XSS itself.
- `Secure`: Tells the browser to send the cookie only over encrypted HTTPS connections.
- `SameSite`: Helps reduce cross-site request forgery (CSRF) by controlling whether the browser sends the cookie on cross-site requests.

An alternative architecture is Backend-for-Frontend (BFF), where the browser/frontend communicates with a backend that handles and stores sensitive OAuth tokens. The frontend does not receive or directly access those OAuth access or refresh tokens. Instead, the frontend receives only a session cookie configured with security attributes.

---

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

# Connection with REST APIs

- JWTs can be used for stateless authentication in [REST APIs](./restfull_api.md). The JWT is stored in the frontend in a secure cookie and may contain identity and authorization claims such as the user's ID, roles, or scopes. The API server can then authenticate a request by validating the JWT's signature and claims without maintaining a server-side session.
- A client includes the JWT in the Authorization header of each HTTP request for each protected CRUD operation to access secured endpoints.

- When the access token expires, the client sends the refresh token to a designated endpoint to obtain a new access token and a new refresh token.

---

# Connection with AWS Cognito

When a user signs in through AWS Cognito, it issues three tokens: the ID token, the access token, and the refresh token.

- The ID token contains information about the authenticated user (e.g., username, email) and is used for client-side operations.
- The access token is used to authorize access to protected APIs and AWS services. It includes OAuth scopes (e.g., read and write) `cognito:groups` (e.g., admin).
- The refresh token is used to obtain a new access token.

Where are the tokens saved? Cognito issues the tokens, but your application/client is responsible for storing them.
