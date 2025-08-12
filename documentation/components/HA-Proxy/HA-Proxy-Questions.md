# HAProxy Interview & Scenario-Based Questions

## 📚 Basic Understanding
1. What is HAProxy and what are its primary use cases?
2. How does HAProxy provide high availability and scalability?
3. What are the main load balancing algorithms supported by HAProxy?
4. What is the difference between TCP and HTTP load balancing in HAProxy?

## ⚙️ Configuration & Usage
5. How do you configure HAProxy to route traffic to multiple backend servers?
6. What does the `balance roundrobin` directive do in HAProxy?
7. How can you change the port that HAProxy listens on?
8. What is the purpose of health checks in HAProxy?
9. How do you set up least-connections load balancing?
10. How do you configure SSL termination in HAProxy?

## 🚀 Deployment & Installation
11. How do you install HAProxy on a Linux server?
12. Where is the main HAProxy configuration file located?
13. How do you reload HAProxy with a new configuration without dropping connections?
14. What is a graceful reload and why is it important?

## 🧩 Advanced Routing
15. How can HAProxy route requests based on URL path or host header?
16. Can you provide an example of path-based routing in HAProxy?
17. How does HAProxy handle hundreds of microservices behind a single port?
18. How do you implement header-based routing in HAProxy?
19. How can you restrict access to certain backends based on client IP?

## ☸️ Kubernetes & Alternatives
20. What are some alternatives to HAProxy for routing in Kubernetes?
21. What features do Kubernetes ingress controllers provide that are similar to HAProxy?
22. When would you choose HAProxy over NGINX or Traefik in a cloud-native environment?

## 🛠️ Practical Considerations
23. What should you keep in mind when exposing a custom port with HAProxy?
24. How do you expose HAProxy ports when running it in Docker?
25. How do you monitor HAProxy performance and health?

## 🐞 Troubleshooting & Best Practices
26. What happens to active connections during a HAProxy reload?
27. How does HAProxy ensure no requests are lost during configuration changes?
28. What are common mistakes when configuring HAProxy for production use?
29. How do you debug routing issues in HAProxy?

## 🎯 Scenario-Based Interview Questions
30. You need to route traffic to three backend services: orders, payments, and users. Each service has two instances. How would you design the HAProxy configuration?
31. If one backend server becomes unhealthy, how does HAProxy handle incoming requests?
32. Your application experiences uneven traffic distribution. How would you troubleshoot and resolve this using HAProxy?
33. How would you set up HAProxy to support blue-green deployments?
34. Describe how you would configure HAProxy to allow only certain IP ranges to access a specific backend.
35. You need to implement canary releases for a microservice. How can HAProxy help with traffic splitting?
36. How would you secure HAProxy against DDoS attacks?
37. If you need to scale HAProxy horizontally, what architecture changes would you consider?
38. How would you integrate HAProxy with a service discovery system?
39. Your HAProxy instance is running in a Docker container. How do you persist configuration changes and logs?
40. How would you handle SSL certificate renewal and reload in a zero-downtime manner with