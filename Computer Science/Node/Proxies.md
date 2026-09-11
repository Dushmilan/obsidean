# Proxies

A proxy is any server that sits between a client and another server, forwarding requests and responses — it's the middleman that can filter, cache, load-balance, or encrypt traffic depending on which side it's on.

**The Intuition:** Think of a proxy like a restaurant receptionist. A **forward proxy** (client-side) is your personal assistant who picks up your order and takes it to the restaurant — the restaurant never sees your face, only the assistant's. A **reverse proxy** (server-side) is the restaurant's host who greets all customers, decides which kitchen station handles your order, and brings the food back — you never see the kitchen. Both are intermediaries; the difference is who they represent.

## Two types

| Type | Sits in front of | Purpose | Example |
|------|------------------|---------|---------|
| **Forward proxy** | The client | Hides client identity, filters content, bypasses restrictions | Corporate proxy, VPN, ad blocker |
| **Reverse proxy** | The server | Hides server identity, load balances, terminates TLS, caches | Nginx, Traefik, Caddy, Cloudflare |

## Forward proxy

```text
Client → Forward Proxy → Internet → Server
         (filters, masks IP, caches)

Server sees: proxy's IP, not the client's
Client sees: proxy's response, not the origin server directly
```

**Use cases:**
- **Privacy/anonymity** — server sees proxy IP, not yours (VPN, Tor)
- **Access control** — corporate networks block certain sites
- **Caching** — proxy stores frequently requested resources locally
- **Bypass geo-restrictions** — appear to be in a different location

**In Node.js — using a proxy for outbound requests:**
```js
import { ProxyAgent } from 'undici';

const agent = new ProxyAgent('http://proxy.corp.example.com:8080');

const res = await fetch('https://api.external.com/data', {
  dispatcher: agent,  // route through forward proxy
});
```

## Reverse proxy

```text
Client → Internet → Reverse Proxy → Backend servers
                     (load balances, terminates TLS, caches)

Client sees: proxy (the public endpoint)
Backend sees: proxy's request, not the client's IP directly
```

**Use cases:**
- **TLS termination** — proxy holds the cert, backends talk plain HTTP internally
- **Load balancing** — distribute traffic across multiple app servers
- **Caching** — serve static assets directly, only forward dynamic requests
- **Rate limiting / WAF** — block bad actors before they hit the app
- **SSL offloading** — backends don't need their own certs

## Reverse proxy in Node.js

```js
import httpProxy from 'http-proxy';

const proxy = httpProxy.createProxyServer({ target: 'http://localhost:3000' });

const server = http.createServer((req, res) => {
  proxy.web(req, res);  // forward to backend
});

server.listen(80);
```

**Production — Nginx as reverse proxy:**
```nginx
upstream app_servers {
    server 127.0.0.1:3000;
    server 127.0.0.1:3001;
}

server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate     /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    location / {
        proxy_pass http://app_servers;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;      // pass client IP
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /static/ {
        alias /var/www/static/;    // serve directly, don't hit backend
    }
}
```

**FastAPI behind a reverse proxy:**
```python
from fastapi import FastAPI
from fastapi.middleware.trustedhost import TrustedHostMiddleware
from fastapi.middleware.httpsredirect import HTTPSRedirectMiddleware

app = FastAPI()

# Trust proxy headers
app.add_middleware(TrustedHostMiddleware, allowed_hosts=["example.com"])

@app.get("/whoami")
def whoami(request: Request):
    # Without proxy headers: sees proxy IP
    # With X-Forwarded-For: sees real client IP
    real_ip = request.headers.get("X-Forwarded-For", request.client.host)
    return {"ip": real_ip}
```

## Load balancing strategies

| Strategy | How it works | Best for |
|----------|-------------|----------|
| Round-robin | Requests rotate: server1 → server2 → server3 → ... | Equal-capacity servers |
| Least connections | Route to server with fewest active connections | Long-lived connections |
| IP hash | Same client IP always goes to same server | Session stickiness |
| Weighted | Servers get traffic proportional to their weight | Mixed hardware |

**Nginx weighted example:**
```nginx
upstream backend {
    server 127.0.0.1:3000 weight=3;   # 3x more traffic
    server 127.0.0.1:3001 weight=1;
}
```

## Headers proxies set

| Header | Set by | Purpose |
|--------|--------|---------|
| `X-Forwarded-For` | Reverse proxy | Original client IP (chain of IPs if multiple proxies) |
| `X-Forwarded-Proto` | Reverse proxy | Original protocol (`http` or `https`) |
| `X-Real-IP` | Reverse proxy | Simplified — just the client IP |
| `X-Forwarded-Host` | Reverse proxy | Original `Host` header from client |
| `Via` | Any proxy | Indicates request passed through a proxy |

**Always trust these headers in your app** — without them, every request appears to come from `127.0.0.1` (the proxy).

## Security considerations

```text
Forward proxy risks:
  - Proxy operator can see all traffic (mitigate: HTTPS end-to-end)
  - Can be used to bypass firewalls/filters
  - "Anonymous" proxies may log everything

Reverse proxy risks:
  - Single point of failure (mitigate: HA, multiple proxies)
  - Header injection if not configured carefully
  - IP spoofing via X-Forwarded-For (mitigate: trust only known proxy IPs)
  - TLS termination means plain HTTP between proxy and backend (mitigate: internal network or mTLS)
```

## Proxy vs CDN vs Load Balancer

| Concept | What it is | Relationship |
|---------|-----------|-------------|
| Proxy | Any middleman server | Superset — both LBs and CDNs can be proxies |
| Load Balancer | Distributes traffic across servers | Usually a reverse proxy with balancing logic |
| CDN | Caches static content at edge locations | Forward + reverse proxy hybrid with geographic distribution |

Cloudflare is all three: reverse proxy (for your domain), CDN (caches static assets globally), and load balancer (distributes across origin servers).

---

## Practice (try before peeking)

1. What's the difference between a forward proxy and a reverse proxy — who are they hiding?
2. Why does a reverse proxy need to set `X-Forwarded-For` and `X-Forwarded-Proto`?
3. If you terminate TLS at Nginx, what happens to traffic between Nginx and your Node backend?

<details><summary>Answers</summary>

1. A forward proxy hides the **client** from the server (server sees proxy IP). A reverse proxy hides the **server** from the client (client sees proxy IP).
2. Without these headers, the backend sees all requests coming from `127.0.0.1` (the proxy) and can't determine the real client IP or whether the original request was HTTPS. Apps need these for logging, rate limiting, and redirects.
3. Traffic is plain HTTP between Nginx and the backend — anyone on the internal network can read it. Use an internal-only network, or mTLS between proxy and backend for zero-trust setups.

</details>

---

**Common traps:**
- Running Node directly on port 80/443 in production — use a reverse proxy for TLS termination, static serving, and rate limiting
- Trusting `X-Forwarded-For` without validating — clients can spoof it; only trust from known proxy IPs
- Forgetting to pass proxy headers — app thinks every request is from `127.0.0.1` and can't do IP-based rate limiting or logging
- TLS termination at proxy + plain HTTP internally without securing that hop — MITM on your own network
- Confusing forward and reverse proxies — remember: forward = client's agent, reverse = server's agent

---

A reverse proxy is your production deployment's front door — TLS termination, load balancing, static serving, and security filtering all happen there, keeping your app servers simple and internal.

**Up:** [[Node_Index]]
