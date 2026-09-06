<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.3 API Development and Communication</h2>
    <h3> Real-Time Communication: Polling vs. WebSockets </h3>
</div>

# Table of Contents

- [Sockets](#sockets)
- [Polling](#polling)
- [WebSockets](#websockets)
- [Scaling WebSockets Horizontally](#scaling-websockets-horizontally)
- [Applications of WebSockets](#applications-of-websockets)
- [Implementation](#implementation)
- [References](#references)

---

# Sockets

A socket is a general communication endpoint that enables applications to exchange data over a network connection.

---

# Polling

Polling is a technique in which the client periodically sends HTTP requests (POST, PUT, GET, DELETE) to the server in one direction (client to server). This makes polling more resource-intensive than WebSockets because of the repeated requests. There are two main types:

- Regular Polling: the client sends a request at fixed intervals (e.g., every few seconds) in a loop.
- Long Polling: the client sends a request, and the server holds the connection open until there is new data. This reduces the number of requests but can still be less efficient than sockets for real-time applications.
- Example of Polling:

```javascript
const pollRate = 500;

const pollMessages = setInterval(async () => {
  try {
    const response = await fetch(
      "https://api.github.com/users/camponogaraviera",
    );
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const messages = await response.json();
    console.log(messages);
  } catch (error) {
    console.error("Error fetching messages:", error);
  }
}, pollRate);

// clearInterval(pollMessages); // To stop polling.
```

---

# WebSockets

WebSocket is a web communication protocol that provides full-duplex (two-way or bidirectional) communication between a client (usually a web browser) and a server over a single TCP connection. Full-duplex communication means that once the connection is established, either side (client or server) can send data at any time. When using WebSockets, the server can push messages to the client instantly without the client needing to poll repeatedly.

Unlike Server-Sent Events (SSE), which are unidirectional, and traditional HTTP communication, which is inherently request-response-based and can be slow due to the overhead of repeatedly establishing connections, WebSockets enable real-time, low-latency communication between a client and server without requiring periodic HTTP requests.

WebSockets are supported by all modern web browsers and many backend frameworks, including Node.js, Python's Tornado, and Java's Spring Framework.

---

# Scaling WebSockets Horizontally

Unlike a stateless [REST request](./restfull_api.md), a WebSocket connection is a long-lived TCP connection held in memory by the specific server process that accepted it (see [Stateful Servers](../../system_design/servers.md#stateful-servers)). Once a WebSocket tier is scaled out to more than one instance, this has two direct consequences:

- A client's existing connection cannot be transparently moved to another instance. If the instance it is connected to goes down or is removed during a scale-in event, the connection is lost, and the client must reconnect.
- A message produced by application logic running on one instance is not automatically visible to clients connected to a different instance.

Two mechanisms are used together in production to address this:

- `Sticky sessions (session affinity)`: the load balancer (e.g., an Application Load Balancer using a cookie, or Nginx's `ip_hash`) routes all requests/reconnects from a given client to the same backend instance for the lifetime of its session. This keeps a client's connection attempts landing on a server that already holds its state, but does not, by itself, let one instance broadcast a message to clients connected to other instances.

- `Shared pub/sub backplane`: to broadcast a message to every relevant client regardless of which instance they are connected to, each WebSocket server instance subscribes to a shared messaging layer (e.g., Redis Pub/Sub, or a broker such as NATS or Amazon MQ). When any instance needs to send a message, it publishes it to the backplane, and every subscribed instance forwards it to its own locally connected clients. Socket.IO provides this out of the box via the [Redis adapter](https://socket.io/docs/v4/redis-adapter/), which lets multiple Socket.IO instances behave as one logical cluster.

In practice, both are combined: sticky sessions for connection routing, and a pub/sub backplane for cross-instance message delivery.

---

# Applications of WebSockets

WebSockets are used in applications that require real-time, persistent connections.

- Real-time collaboration tools: Applications similar to Google Docs can use WebSockets to provide instant feedback and synchronization among multiple users editing or viewing the same document or chat.

- Real-time location tracking to send and receive updates on the driver's location (e.g., Uber).

- Real-time Chat applications.

- Online Gaming: Multiplayer online games can use WebSockets for real-time communication between players and game servers, enabling quick updates on player actions and game state.

- Financial Market Data Feeds: Stock trading platforms and financial services can use WebSockets to deliver real-time updates of stock prices, exchange rates, and market events to traders and investors.

- IoT Communication: WebSockets provide a lightweight and efficient communication protocol for Internet of Things (IoT) devices that need to send and receive data in real-time.

---

# Implementation

It is possible to implement a plain WebSocket API using [AWS AppSync Events](https://docs.aws.amazon.com/appsync/latest/eventapi/event-api-welcome.html) or Node.js libraries, such as [ws](https://github.com/websockets/ws) or [µWebSockets.js](https://github.com/uNetworking/uWebSockets.js).

Libraries such as [Socket.IO](https://socket.io/docs/v4/) do not implement plain WebSockets. The Socket.IO library is built on Engine.IO, which provides the underlying transport layer. It typically uses WebSockets for low-latency communication, with HTTP long-polling as a fallback when a WebSocket connection cannot be established. Current versions also support WebTransport. As of this writing, a Socket.IO client cannot simply connect to a plain WebSocket server, and a WebSocket client cannot connect to a Socket.IO server. Also, the Socket.IO client is not meant to be used in a background service for mobile applications. Maintaining an open TCP connection consumes battery, so Socket.IO recommends a dedicated messaging/push mechanism for background use. For example:

- Foreground: Socket.IO gives real-time bidirectional communication.
- Background: Firebase Cloud Messaging (FCM)/Apple Push Notification (APNs) wakes or notifies the Android/iOS OS running the frontend application when appropriate.

---

# References

[1] https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API

[2] https://socket.io/

[3] https://socket.io/docs/v4/redis-adapter/
