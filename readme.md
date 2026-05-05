# Encrypted Video Call App

A browser-based, peer-to-peer video calling application with end-to-end encryption using WebRTC's built-in DTLS-SRTP security.

---

## Features

- **End-to-End Encrypted Video & Audio** — All media is encrypted via DTLS-SRTP before leaving your device
- **Peer-to-Peer Connection** — Direct browser-to-browser calls; no media passes through a server
- **Encryption Verification Panel** — Live status of DTLS handshake and SRTP encryption during calls
- **Camera & Microphone Controls** — Toggle video and audio on/off during a call
- **Auto-generated Peer IDs** — Unique IDs are generated on load; share yours to receive calls
- **Dark Mode Support** — Automatically adapts to system theme
- **Responsive Layout** — Works on desktop and mobile browsers

---

## How It Works

### Encryption (DTLS-SRTP)

WebRTC enforces encryption on all media streams by default:

1. **DTLS (Datagram Transport Layer Security)** — Performs a cryptographic handshake between peers to establish a shared secret key.
2. **SRTP (Secure Real-time Transport Protocol)** — Uses that key to encrypt all audio and video packets (AES-128) before transmission.
3. Your media is decryptable **only** by the other party — not by any relay server or third party.

### Signaling (PeerJS)

This app uses [PeerJS](https://peerjs.com/) to exchange connection metadata (SDP offers/answers and ICE candidates) between peers. Only connection setup information passes through PeerJS servers — **never any media**.

---

## Getting Started

### Prerequisites

- A modern browser with WebRTC support (Chrome, Firefox, Edge, Safari)
- Camera and microphone access
- An internet connection (for STUN server access and PeerJS signaling)

### Running the App

The app is a single self-contained HTML file — no build step or server required.

```bash
# Option 1: Open directly in your browser
open index.html

# Option 2: Serve locally (recommended to avoid camera/mic permission issues)
npx serve .
# or
python3 -m http.server 8080
```

Then open `http://localhost:8080` in your browser.

---

## Making a Call

1. Open the app — your **Peer ID** is auto-generated and displayed.
2. Share your Peer ID with the person you want to call (copy with the **Copy** button).
3. Enter their Peer ID in the **"Call peer ID"** field and click **📞 Video Call**.
4. The recipient will be prompted to allow camera/microphone access, then the call connects automatically.

---

## Verifying Encryption

During an active call, the **Encryption Verification** panel shows:

| Check | What It Means |
|-------|--------------|
| **DTLS Handshake: ✓ Verified** | Cryptographic key exchange succeeded |
| **SRTP Encryption: ✓ Active** | Media is being encrypted in transit |
| **Connection: connected** | DTLS transport state is live |

### Additional Verification Methods

- **Browser DevTools** — Open F12 → Console. Look for `DTLS` and `SRTP` log entries.
- **Chrome WebRTC Internals** — Visit `chrome://webrtc-internals` for full connection stats.
- **Firefox** — Visit `about:webrtc` for equivalent diagnostics.
- **Wireshark** — Capture network traffic and confirm packets are encrypted SRTP, not plaintext RTP.

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Video/Audio | WebRTC (browser native) |
| Encryption | DTLS 1.2 + SRTP (AES-128) |
| Signaling | PeerJS v1.5.2 |
| NAT Traversal | STUN (Google public servers) |
| UI | Vanilla HTML/CSS/JavaScript |

---

## Project Structure

```
index.html        # Entire application (single file)
README.md         # This file
```

---

## Configuration

### STUN Servers

Default STUN servers are Google's public endpoints:

```javascript
iceServers: [
  { urls: 'stun:stun.l.google.com:19302' },
  { urls: 'stun:stun1.l.google.com:19302' }
]
```

For production use, consider adding **TURN servers** to support calls behind strict NATs/firewalls.

### PeerJS Server

The app uses PeerJS's hosted cloud server by default. To self-host:

```javascript
const peer = new Peer({
  host: 'your-peerjs-server.com',
  port: 9000,
  path: '/myapp'
});
```

---

## Limitations

- **No TURN server** — Calls may fail between peers on highly restrictive corporate/NAT networks.
- **No persistent IDs** — Peer IDs are regenerated each page load.
- **No multi-party calls** — Supports 1-to-1 calls only.
- **No text chat** — Audio/video only.

---

## Browser Compatibility

| Browser | Support |
|---------|---------|
| Chrome 74+ | ✅ Full |
| Firefox 66+ | ✅ Full |
| Edge 79+ | ✅ Full |
| Safari 14.1+ | ✅ Full |
| IE / Legacy | ❌ Not supported |

---

## License
 free to use, modify, and distribute.
