<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.3 API Development and Communication</h2>
    <h3> WebRTC </h3>
</div>

# Table of Contents

- [WebRTC](#webrtc)
- [WebSockets vs WebRTC](#websockets-vs-webrtc)

---

# WebRTC

WebRTC is a set of web APIs and protocols for real-time peer-to-peer audio, video, and data communication. It uses ICE, with STUN and TURN, to establish connectivity between peers. WebRTC does not define a signaling protocol, so applications need a separate signaling mechanism, commonly WebSockets, to exchange SDP offers/answers and ICE candidates.

---

# WebSockets vs WebRTC

WebSockets are commonly used for WebRTC signaling, but they are not the transport for the WebRTC media itself.

- **Protocol Limitation**:
  - WebSockets use **TCP**, which ensures reliability but introduces **latency and jitter** (bad for real-time audio/video).
  - WebRTC generally prefers **UDP** because it provides lower latency and allows real-time applications to tolerate some packet loss, but it can fall back to TCP/TLS-based connectivity when necessary.

- **Bandwidth & Performance Issues**:
  - WebSockets transmit data as **binary blobs or text**, but encoding/decoding media (e.g., H.264/Opus) adds overhead.
  - WebSockets do not provide real-time media-specific mechanisms such as adaptive bitrate control and media-oriented packet-loss handling. Although TCP provides congestion control and reliable retransmission, these mechanisms can introduce latency and head-of-line blocking, which are undesirable for real-time audio/video.
