# Proxies — Revision Notes

## 1. Proxy

A proxy server is a hardware device or software placed between a client and an application/server to provide intermediary services during communication.

### Basic idea

```
Client  →  Proxy Server  →  Server
Client  ←  Proxy Server  ←  Server
```

The proxy acts as an intermediary between the client and the server.

---

## 2. Proxy as an Intermediary

A proxy can act on behalf of another party.

For example:

```
Shyam → Ram → Other People
```

Here, Ram is acting as a proxy for Shyam.

Similarly, in networking, a proxy communicates with the destination on behalf of the client or server.

---

## 3. Proxy as a Gateway

A proxy server provides a gateway between the user and the Internet.

```
User / Client
      ↓
 Proxy Server
      ↓
   Internet
```

Instead of the client communicating directly with the Internet, the communication passes through the proxy.

---

## 4. Forward Proxy

A forward proxy is positioned between the client and the Internet/server.

```
Client  ↔  Forward Proxy  ↔  Server
```

### Working

1. The client sends a request to the forward proxy. 
2. The proxy forwards the request to the destination server. 
3. The server sends the response back to the proxy. 
4. The proxy sends the response to the client. 

### Key point

**Forward proxy hides the client.**

The destination server sees the proxy rather than directly seeing the client.

```
Client → Forward Proxy → Server
           ↑
      Client's identity
        is hidden
```

---

## 5. Why Use a Forward Proxy?

A forward proxy can be used to:

-  Hide the client's identity from the destination. 
-  Control access to Internet resources. 
-  Filter requests. 
-  Apply organizational/network policies. 
-  Act as an intermediary between users and external servers. 

---

## 6. Reverse Proxy

A reverse proxy is positioned between the client and backend servers.

```
Client  ↔  Reverse Proxy  ↔  Backend Server(s)
```

The client communicates with the reverse proxy instead of directly communicating with the backend servers.

### Key point

**Reverse proxy hides the server.**

The client does not need to know which actual backend server handles the request.

```
Client → Reverse Proxy → Server 1
                      → Server 2
                      → Server 3
```

The reverse proxy can select an appropriate backend server.

---

## 7. Reverse Proxy — Example

Suppose an application has multiple backend servers:

```
             ┌── Server 1
Client → Reverse Proxy ├── Server 2
             └── Server 3
```

The client only communicates with the reverse proxy.

Therefore:

> The server does not directly know the client.

The reverse proxy handles communication between the client and the backend servers.

---

## 8. Forward Proxy vs Reverse Proxy

| Feature | Forward Proxy | Reverse Proxy |
| --- | --- | --- |
| Hides | Client | Server |
| Located between | Client and server/Internet | Client and backend servers |
| Acts on behalf of | Client | Server |
| Main purpose | Client-side intermediary | Server-side intermediary |
| Common use | Filtering/access control | Load balancing/server abstraction |

### Easy way to remember

```
Forward Proxy  →  hides CLIENT
Reverse Proxy  →  hides SERVER
```

---

## 9. When to Use a Reverse Proxy?

Reverse proxy can be used when:

### 1. Hide the server

When you don't want to expose the actual backend server directly to clients.

### 2. Make it appear as one server

Multiple backend servers can be presented to users as if there is only one server.

```
Client
   ↓
Reverse Proxy
   ↓
┌───────┬───────┬───────┐
Server1 Server2 Server3
```

### 3. Load balancing

The reverse proxy can distribute incoming requests among multiple backend servers.

```
Client
  ↓
Reverse Proxy
  ↓
Server 1
Server 2
Server 3
```

This helps distribute the workload.

### 4. Swap backend servers

Administrators can replace or swap backend servers without disturbing client traffic because clients continue communicating with the reverse proxy.

```
Client → Reverse Proxy → Backend Server
```

The backend can be changed behind the proxy without changing the client's interaction.

### 5. Filter requests

A reverse proxy can filter out certain requests before they reach the backend servers.

---

## 10. Quick Revision

### Proxy

```
Client ↔ Proxy ↔ Server
```

A proxy is an intermediary in communication.

### Forward Proxy

```
Client → Forward Proxy → Server
```

Hides the client.

### Reverse Proxy

```
Client → Reverse Proxy → Backend Servers
```

Hides the server.

### Reverse Proxy Uses

-  Hide backend servers 
-  Present multiple servers as one 
-  Load balancing 
-  Swap backend servers without disturbing traffic 
-  Filter requests 

---

## ⭐ Exam Shortcut

Forward proxy = Client-side proxy = Hides Client

Reverse proxy = Server-side proxy = Hides Server

```
FORWARD
Client → Proxy → Internet
         ↑
    hides CLIENT


REVERSE
Client → Proxy → Servers
         ↑
    hides SERVER
```
