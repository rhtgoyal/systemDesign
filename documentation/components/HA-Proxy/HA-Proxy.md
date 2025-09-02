# HAProxy for Scalable Microservice Routing

## 📌 What Is HAProxy?

**HAProxy** (High Availability Proxy) is a fast, reliable, and flexible TCP/HTTP load balancer and proxy server. It is widely used to distribute incoming traffic across multiple backend servers, ensuring high availability, fault tolerance, and scalability.

---

## 🔁 Routing Traffic to Multiple Instances

HAProxy can route traffic to multiple instances of the same application using various strategies:

- **Round-robin**: Distributes requests evenly across instances.
- **Least connections**: Sends traffic to the instance with the fewest active connections.
- **Health checks**: Automatically removes unhealthy instances from rotation.

### 🧩 Example: Two Instances of a Web App

```haproxy
frontend http_front
    bind *:80
    default_backend web_back

backend web_back
    balance roundrobin
    server app1 127.0.0.1:9094 check
    server app2 127.0.0.1:9095 check
```

### Decoding the config file: ### 
- HAProxy listens on port 80
- Routes traffic to two instances running on ports 9094 and 9095
- If you want HA-proxy to expose port `8081` instead of default 80, simply change the bind:
```
frontend http_front
    bind *:8081
    default_backend web_back
```
- Now request to `http://yourdomain.com:8081` will be routed to your backend servers.
- things to keep in mind:
    - Make sure port 8081 is open in your firewall or security group.
    - If you're using Docker, expose port 8081 in your container config:
    ```
    docker run -p 8081:8081 haproxy
    ```
---
    
## 🚀 Deployment Overview
HAProxy is deployed as a standalone service, typically on a Linux server or container. It runs as a system-level process and listens on configured ports.

### 🔧 Install HA Proxy
```bash
sudo apt-get update
sudo apt-get install haproxy
```

### 🔧 Configuration File
The main configuration file is usually located at /etc/haproxy/haproxy.cfg. It defines:
- Frontend listeners
- Backend server pools
- Routing rules
- Health checks

---
## 🔄 Runtime Configuration Changes
HAProxy supports runtime reconfiguration using:
- **Reloading**: You can reload HAProxy with a new config using `systemctl reload haproxy` or `service haproxy reload`.
- **Graceful reloads**: Existing connections are preserved, and no requests are lost.
-  **Runtime API**: Advanced setups can use the HAProxy Runtime API for dynamic updates.

✅ No requests are lost during a proper reload —> HAProxy handles this gracefully.

---

## 🌐 Exposing Hundreds of Microservices
If you have 100s of microservices, each with 2 instances, HAProxy can expose them all through a single external port (e.g., port 80 or 443) using smart routing.

### ✅ Strategies for Routing
| Strategy | Example URL | Routing Logic |
|-----------|-----------|-----------|
| Path-based | `api.example.com/orders` | Match URL path |
| Host-based | `orders.example.com` | 	Match Host header
| Header-based | Custom headers | Match request headers |

#### 🧩 Example: Path-Based Routing
```bash
frontend api_gateway
    bind *:80

    acl is_orders path_beg /orders
    acl is_auth path_beg /auth

    use_backend orders_backend if is_orders
    use_backend auth_backend if is_auth

backend orders_backend
    server orders1 127.0.0.1:9001 check
    server orders2 127.0.0.1:9002 check

backend auth_backend
    server auth1 127.0.0.1:9011 check
    server auth2 127.0.0.1:9012 check
```

---

## 🔄 Alternatives to HAProxy in Kubernetes
| Tool              | Description                                 |
|-------------------|---------------------------------------------|
| Ingress Controller| Manages external access to services via HTTP/HTTPS |
| NGINX Ingress     | Most popular ingress controller             |
| Traefik           | Dynamic, Kubernetes-native routing          |
| Istio / Linkerd   | Service mesh with advanced traffic control  |

These tools integrate deeply with Kubernetes and support:
- Automatic service discovery
- TLS termination
- Path and host-based routing
- Canary deployments and traffic shaping

---

## 🛡️ what are purpose of HA-Proxy
1. High Availability
2. Scalability
3. Fault Tolerance

## How HAProxy Achieves Fault Tolerance

HAProxy ensures fault tolerance through intelligent traffic management, health monitoring, and redundancy strategies.

### 1. Health Checks
- Continuously monitors backend servers.
- Automatically removes unhealthy servers from rotation.
- It prevents request failure by preventing traffic to route to failed instance.

### 2. Load Balancing Algorithms
- Distributes traffic using round-robin, least connections, or source hashing.
- Prevents overload and supports graceful degradation.

### 3. Failover with Keepalived
- Paired with Keepalived to manage a Virtual IP (VIP).
- Automatically transfers VIP to backup HAProxy if primary fails.

### 4. Graceful Reloads
- Supports configuration reloads without dropping connections.
- Preserves existing sessions during updates.

### 5. Redundant Architecture
- Deploy multiple HAProxy nodes in active-passive or active-active mode.
- Ensures traffic rerouting in case of node failure.

### 🧪 Real-World Setup Example
A typical fault-tolerant HAProxy setup includes:
- Two HAProxy instances (primary and backup)
- Keepalived managing the VIP
- Health checks on backend services
- Monitoring tools like Prometheus or Datadog

🔗 [HAProxy + Keepalived Setup Guide](https://sysadmins.co.za/achieving-high-availability-with-haproxy-and-keepalived-building-a-redundant-load-balancer/)

---

## 🧠 What Is Keepalived?

Keepalived is a Linux-based service that provides high availability and failover capabilities, often used with HAProxy.

### 1. Failover with VRRP
- Implements Virtual Router Redundancy Protocol (VRRP).
- Shares a Virtual IP (VIP) between multiple servers.
- Automatically transfers VIP to backup node on failure.

### 2. Health Monitoring
- Monitors services or servers.
- Triggers failover when issues are detected.

### 3. Optional Load Balancing
- Can perform Layer 4 load balancing using IPVS.

### ⚙️ Typical Use Case with HAProxy

| Component       | Role                                  |
|----------------|----------------------------------------|
| HAProxy         | Load balances traffic across backend servers |
| Keepalived      | Monitors HAProxy and manages VIP failover  |
| Virtual IP (VIP)| Shared IP that floats between nodes   |

### 4. What Is a VIP?
A **VIP (Virtual IP)** is a shared IP address that floats between two servers:
- One server is **active** and handles traffic.
- The other is **passive** and waits in standby.
- If the active server fails, the VIP moves to the passive server.

Clients always connect to the **VIP**, not the actual server IPs.

| Component         | Role                                                   |
|------------------|--------------------------------------------------------|
| **HAProxy (Primary)** | Handles traffic when healthy.                      |
| **HAProxy (Backup)**  | Takes over if primary fails.                       |
| **Keepalived**        | Manages which HAProxy is active by controlling the VIP. |
| **VIP**               | The IP address exposed to users/clients.  

### 5. What Happens During Failover

1. Primary HAProxy is healthy → VIP is assigned to it → it serves traffic.
2. Primary fails → Keepalived detects failure.
3. VIP moves to backup HAProxy → it starts serving traffic.
4. Clients continue using the same VIP — no change needed on their side.

### 6. 👀 What Clients See

Clients only see and connect to the **VIP**:
```plaintext
http://123.45.67.89 (VIP)
```
This ensures zero downtime and seamless failover.

---

## ⚠️ HAProxy: Limitations and Drawbacks

HAProxy is a powerful and reliable load balancer, but it’s not perfect. Here are some of its key limitations:


### 🧩 Configuration Complexity

- Steep learning curve for beginners.
- Configuration syntax can be hard to understand and debug.
- Initial setup may feel overwhelming without good documentation.


### 📊 Monitoring & Logging

- Weak built-in logging and monitoring tools.
- Requires external tools like **Prometheus**, **Grafana**, or **Datadog** for full observability.
- Dashboards and admin interfaces are not very user-friendly.


### 🔄 Dynamic Configuration

- Many config changes require restarting HAProxy, which can cause brief downtime.
- No native support for full dynamic reloading (though newer versions and external tools help).


### 📦 Limited Built-in Features

- No built-in SSL certificate management UI.
- No native service discovery — needs integration with tools like **Consul**, **etcd**, or DNS-based setups.


### 💸 Enterprise Features

- Some advanced features (like real-time metrics, GUI dashboards, and WAF) are only available in **HAProxy Enterprise**, which is not free.


### 🧠 Summary Table

| Limitation               | Description                                      |
|--------------------------|--------------------------------------------------|
| Complex Configuration    | Hard for beginners, requires careful setup       |
| Weak Monitoring          | Needs external tools for full visibility         |
| Static Reloads           | Config changes often need restart                |
| Limited GUI              | No built-in dashboard or admin panel             |
| No Service Discovery     | Must integrate with external systems             |
| Enterprise Lock-in       | Some features behind a paywall                   |

---

### ✅ Tip

If you're building a production-grade system, consider pairing HAProxy with:
- **Keepalived** for high availability
- **Prometheus + Grafana** for monitoring
- **Consul or DNS** for service discovery

---

## 🚦 HAProxy: Routing Traffic Based on Region

You can use HAProxy to send specific traffic to different servers. For example:

- 📱 North mobile requests → Server 1
- 📱 South mobile requests → Server 2


### 🧠 How It Works

HAProxy uses **ACLs (Access Control Lists)** to inspect request details like:
- Headers (e.g. `X-Region`)
- IP address
- User-Agent
- Cookies or query strings

Then it routes traffic to the right backend.


### 🛠️ Example Config

```haproxy

frontend http-in
    bind *:80

    acl is_north req.header(X-Region) -i North
    acl is_south req.header(X-Region) -i South

    use_backend server_north if is_north
    use_backend server_south if is_south

backend server_north
    server s1 192.168.1.10:80 check

backend server_south
    server s2 192.168.1.20:80 check
```

---
