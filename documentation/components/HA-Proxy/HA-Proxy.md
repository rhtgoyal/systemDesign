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