<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.3 API Development and Communication</h2>
    <h3> CORS </h3>
</div>

# Table of Contents

- [About](#about)
- [How does CORS work?](#how-does-cors-work)
- [Example 1: setting up CORS in a Node.js Express Server (RESTful API)](#example-1)
- [Example 2: setting up CORS using the Node.js cors package](#example-2)

---

# About

[Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) is a security feature/mechanism implemented by web browsers to restrict cross-origin HTTP requests.

When an [AJAX](<https://en.wikipedia.org/wiki/Ajax_(programming)>) request ([XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)) from a particular URL (protocol://domain1:port) is made to a different URL (protocol://domain2:port), the browser enforces CORS policies to ensure that the server allows such a request made between different origins. Recall that changing either the protocol, domain, or port also changes the [URL](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/infrastructure/01_url_dns.md):

- protocol1://domain1:port1.
- protocol2://domain1:port1 (protocol changed).
- protocol1://domain2:port1 (domain changed).
- protocol1://domain1:port2 (port changed).

Even though CORS is implemented by the browser, it needs to be implemented in the backend of the application by including specific HTTP headers in the response of the RESTful API verb/operation (GET, POST, etc.). When using AWS as the hosting provider, one can enable CORS via AWS Lambda functions.

---

# How does CORS work?

Before CORS, AJAX requests could only be made to servers belonging to the same origin (domain) as the webpage/browser making the request. This was known as "same origin restriction". Ansatz solutions for this problem at the time were Proxy or JSONP (limited to only GET requests). CORS emerges as a new standard.

Suppose that your website, named `my_domain1.com` has an image tag `<img>` or a script tag `<script>` to load contents from a website named `my_domain2.com` via AJAX requests.

The browser will do a preflight request, i.e., an OPTIONS request that has the following headers:

- Origin header, which is the domain with the scheme of the webpage making the request.
- Access-Control-Request-Method: The GET, POST, and PUT requests that the browser can make.
- Access-Control-Request-Headers: The headers of the browser.

While the response in the backend of your server may have the following headers:

- Access-Control-Allow-Origin: Specifies which origin is allowed to access the resource. It can be a specific origin, such as `https://my_domain1.com/`, or `*`, which allows requests from any origin when credentials are not involved.
- Access-Control-Allow-Methods: Specifies the HTTP methods that the server allows for cross-origin requests.
- Access-Control-Allow-Headers: Specifies which request headers the server allows the browser to send in cross-origin requests.
- Access-Control-Max-Age: Specifies how long the browser may cache the result of a CORS preflight (OPTIONS) request.
- Access-Control-Allow-Credentials: Indicates whether the browser may expose the response to frontend JavaScript when the cross-origin request includes credentials, such as cookies or HTTP authentication. When credentials are used, `Access-Control-Allow-Origin` must specify an explicit origin rather than `*`.

---

# Example 1

Setting up CORS in a Node.js Express Server (RESTful API):

```javascript
const express = require("express");
const app = express();

// Global middleware function to enable CORS:
app.use((req, res, next) => {
  // Allow requests from specific origin:
  res.setHeader("Access-Control-Allow-Origin", "https://www.my_domain.com");

  // HTTP methods allowed:
  res.setHeader(
    "Access-Control-Allow-Methods",
    "GET, POST, PUT, PATCH, DELETE",
  );

  // Request headers allowed by the server in cross-origin requests:
  res.setHeader("Access-Control-Allow-Headers", "Content-Type, Authorization");

  // Handle CORS preflight requests:
  if (req.method === "OPTIONS") {
    return res.sendStatus(204);
  }

  // Continue to the next middleware:
  next();
});

// The port is dynamically assigned by the host environment:
const PORT = process.env.PORT || 3000;
// Start the server:
app.listen(PORT, () => {
  console.log(
    `Running server in ${process.env.NODE_ENV} mode and listening on port ${PORT}...\n`,
  );
});
```

---

# Example 2

Setting up CORS using the Node.js [cors package](https://www.npmjs.com/package/cors):

```javascript
const express = require("express");
const cors = require("cors");
const app = express();

// Use CORS middleware (callback function):
const corsOptions = {
  origin: process.env.FRONTEND_DOMAIN || "http://localhost:3000",
  methods: "GET, POST, PUT, PATCH, DELETE", // HTTP methods allowed.
  allowedHeaders: ["Content-Type", "Authorization"], // Request headers allowed by the server in cross-origin requests.
  optionsSuccessStatus: 200, // The status code returned for successful preflight requests. Legacy browsers might not handle status 204.
};

// Handle CORS preflight requests automatically:
app.use(cors(corsOptions));

// The port is dynamically assigned by the host environment:
const PORT = process.env.PORT || 3000;
// Start the server:
app.listen(PORT, () => {
  console.log(
    `Running server in ${process.env.NODE_ENV} mode and listening on port ${PORT}...\n`,
  );
});
```
