## 📌 What Is a Load Balancer?

A **load balancer** is a system component that distributes incoming network traffic across multiple backend servers. Its goal is to:
- Improve performance
- Achieve high availability and reliability
- Prevent overload
- Enable fault tolerance

---

## 🧱 Types of Load Balancers

### 1. **Layer 4 (Transport Layer)**
- Operates at TCP/UDP level.
- Routes traffic based on IP address and port.
- Examples: AWS Network Load Balancer, IPVS, LVS.

### 2. **Layer 7 (Application Layer)**
- Operates at HTTP/HTTPS level.
- Routes traffic based on URL path, headers, cookies.
- Examples: NGINX, Envoy, AWS Application Load Balancer, Traefik.

---

## 🚀 Popular Load Balancer Tools (Excluding HAProxy)

| Tool        | Type     | Key Features                                 |
|-------------|----------|----------------------------------------------|
| **NGINX**   | Layer 7  | Reverse proxy, SSL termination, caching      |
| **Envoy**   | Layer 7  | Service mesh, observability, gRPC support    |
| **Traefik** | Layer 7  | Dynamic config, Kubernetes-native, ACME SSL  |
| **AWS ELB** | Layer 4/7| Managed, integrates with EC2 and ECS         |
| **F5 BIG-IP**| Layer 4/7| Enterprise-grade, advanced traffic control  |
| **Cloudflare Load Balancer** | Layer 7 | Global routing, DNS-based failover |

---

## 🧠 Key Concepts in Load Balancing

### 🔁 Load Balancing Algorithms
- **Round Robin**: Even distribution.
- **Least Connections**: Server with fewest active connections.
- **IP Hash**: Sticky sessions based on client IP.
- **Weighted Round Robin**: Prioritize stronger servers.

### 🛡️ Health Checks
- Periodic checks to ensure backend servers are alive.
- Unhealthy servers are removed from rotation.

### 🔄 Session Persistence
- Also called "sticky sessions".
- Ensures a user is routed to the same server across requests.

### 🔐 SSL Termination
- Load balancer handles HTTPS decryption.
- Offloads CPU-intensive SSL work from backend servers.

### 🌍 Global Load Balancing
- Routes traffic across regions or data centers.
- Often DNS-based or uses Anycast IPs.

---

## 🧪 Load Balancing in Kubernetes

Kubernetes uses several components for load balancing:

### 1. **Service**
- Internal load balancing across pods.
- Uses kube-proxy and iptables/IPVS.

### 2. **Ingress Controller**
- Layer 7 routing for HTTP/HTTPS.
- Examples: NGINX Ingress, Traefik, Istio Gateway.

### 3. **Service Mesh**
- Fine-grained traffic control between services.
- Examples: Istio, Linkerd, Consul Connect.

---

## 📚 Advanced Topics

### 🔄 Blue-Green & Canary Deployments
- Use load balancers to route traffic to new versions gradually.
- Enables safe rollouts and instant rollback.

### 🧠 Load Balancer as API Gateway
- Handles routing, authentication, rate limiting.
- Often used in microservice architectures.

### 🧰 Load Balancer in CI/CD
- Used to drain traffic from nodes during deployment.
- Ensures zero-downtime upgrades.

---

## 🎯 Interview Questions on Load Balancers

### ✅ Fundamentals
- What is a load balancer and why is it needed?
- Difference between Layer 4 and Layer 7 load balancing.
- How does round-robin differ from least-connections?

### ✅ Design & Architecture
- How would you design a fault-tolerant load balancer?
- How do you handle session persistence?
- How do you scale a load balancer horizontally?

### ✅ Real-World Scenarios
- How would you implement blue-green deployment using a load balancer?
- How do you route traffic based on headers or user location?
- What happens if the load balancer itself fails?

### ✅ Kubernetes-Specific
- How does Kubernetes handle internal load balancing?
- What is the role of an Ingress Controller?
- How do you expose multiple microservices via a single domain?

---

## 🧠 Summary

Load balancers are critical for:
- Distributing traffic
- Ensuring uptime
- Scaling applications
- Enabling safe deployments

Whether you're working with cloud-native systems, microservices, or traditional web apps, understanding load balancing is essential for system design and reliability engineering.

