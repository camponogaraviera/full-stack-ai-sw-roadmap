<div align='center'>
    <h1> API Security & Access Control </h1>
    <h2> JSON Web Tokens </h2>
</div>

# Table of Contents

- [JSON Web Tokens](#json-web-tokens)
- [Secure Token Storage](#secure-token-storage)
- [Connection with REST APIs](#connection-with-rest-apis)
- [Connection with AWS Cognito](#connection-with-aws-cognito)
- [References](#references)

# JSON Web Tokens

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

# Connection with REST APIs

- JWTs can be used for stateless authentication in [REST APIs](../../arch_styles/restfull_api.md). The JWT is stored in the frontend in a secure cookie and may contain identity and authorization claims such as the user's ID, roles, or scopes. The API server can then authenticate a request by validating the JWT's signature and claims without maintaining a server-side session.
- A client includes the JWT in the Authorization header of each HTTP request for each protected CRUD operation to access secured endpoints.

- When the access token expires, the client sends the refresh token to a designated endpoint to obtain a new access token and a new refresh token.

---

# Connection with AWS Cognito

When a user signs in through AWS Cognito, it issues three tokens: the ID token, the access token, and the refresh token.

- The ID token contains information about the authenticated user (e.g., username, email) and is used for client-side operations.
- The access token is used to authorize access to protected APIs and AWS services. It includes OAuth scopes (e.g., read and write) `cognito:groups` (e.g., admin).
- The refresh token is used to obtain a new access token.

Where are the tokens saved? Cognito issues the tokens, but your application/client is responsible for storing them.

---

# References

[1] https://www.jwt.io/

[2] https://en.wikipedia.org/wiki/JSON_Web_Token
