---
title: Load Balancer Basic
date: 2019-03-27 00:00:00 Z
---

- **Basic Concept**

  - Load balancing  is all about **aggregating multiple components** in order to achieve a total processing capacity above each component's individual capacity, without any intervention from the end user and in a scalable way. Load balancing improves application responsiveness and availability. 
  - The mechanism or the component which performs load balancing operation is called a load balancer.
  - Load balancing results in more operations being performed simultaneously by the time it takes a component to perform only one. **Metaphorically**, this is equivalent of having a number of lanes on a highway that allows as many cars to pass during the same time frame *without increasing their individual speed*.

- **Load Balancing @**

  - link level : balances load while interfering between a WAN and 1 or more LANs, choosing the network link to send a packet to;
  - network level : balances load while routing traffic across multiple network routes; choosing **what route** a series of packets will follow; this sort of load balancing happens at Layer 4 of OSI layers
  - server level : balances load while routing traffic across multiple servers deciding **what server** will process a connection or request.

- **Categories** 

  - **Packet level load balancing** processes packets more or less individually. There is a 1-to-1 relation between input and output packets, so it is possible to follow the traffic on both sides of the load balancer using a regular network sniffer. This technology can be **very cheap and extremely fast**. Usually stateless, it can also be stateful, may support DSR (direct server return, without passing through the LB again) if the packets were not modified, but provides almost no content awareness. This technology is very well suited to network-level load balancing, though it is sometimes used
    for very basic server load balancing at high speed.
    - Packet-based load balancers are generally **deployed in cut-through mode, so they**
      **are installed on the normal path of the traffic and divert it according to the**
      **configuration**. The return traffic doesn't necessarily pass through the load
      balancer. 
  - **Session content based load balancing** processes **session contents**. It requires that the input streams is reassembled and processed as a whole. The contents may be modified, and the
    **output stream is segmented into new packets**. For this reason it is generally
    performed by proxies and they're often called layer 7 load balancers or L7. This implies that there are two distinct connections on each side, and that there is no relation between input and output packets sizes nor counts. Clients and servers are not required to use the same protocol (for example IPv4 vs IPv6, clear vs SSL). The operations are always stateful, and the return traffic
    must pass through the load balancer. The sheme offers wide possibilities and is generally achieved by pure software. This technology is very well suited for server load balancing.
    - Proxy-based load balancers are deployed **as a server with their own IP addresses**
      **and ports, without architecture changes**. 

- **Traffic Routing Algorithm**

  - Load Balancer uses one of the following algorithms to route the traffic to one of the target servers
    - Least Connection Method — directs traffic to the server with the fewest active connections. Most useful when there are a large number of persistent connections in the traffic unevenly distributed between the servers.
    - Least Response Time Method — directs traffic to the server with the fewest active connections and the lowest average response time.
    - Round Robin Method — rotates servers by directing traffic to the first available server and then moves that server to the bottom of the queue. Most useful when servers are of equal specification and there are not many persistent connections.
    - IP Hash — the IP address of the client determines which server receives the request.

- **Health Check** 

  - Presence of number of servers that can serve a given request equally well, increases number of possible paths for the traffic. This in turn increases the risk of failure; in very large environments. For this reason, load balancers verifies that the components it intends to deliver the traffic to are still alive and reachable, and it will stop delivering traffic to faulty ones. This is usually achieved using one of the following methods
    - In the first method, periodically a probe is being sent to ensure the component is still operational. These probes are called "health checks". They must be representative of the type of failure to address. For example a ping-based check will not detect that a web server has crashed and doesn't listen to a port anymore, while a connection to the port will verify this. The period between checks must be small enough to ensure the faulty component is not used for too long after an error occurs.
    - In the second method, production traffic is sampled and sent to a destination to observe if it is processed correctly or not, and to evict the components which return inappropriate responses. However this requires to sacrifice a part of the production traffic and this is not always acceptable. 
  - A central monitoring agent shows the outcome of health check 

- **Session Stickiness**

  - Load balancers attempt to solve an important problem named as Session Stickiness. Session Stickiness is mechanism that helps Load Balancers to direct multiple subsequent requests or connections from a same origin (such as an end user) to the same target. The best known example is the shopping cart on an online store. If each click leads to a new connection, the user must always be sent to the server which holds his shopping cart. The solution against this issue consists in memorizing the chosen target so that each time the same visitor is seen, he's directed to the same server regardless of the number of available servers. The information may be stored in the load balancer's memory, in which case it may have to be replicated to other load balancers if it's not alone, or it may be stored in the client's memory. 

- **Encryption & Decryption**

  - Secure Sockets Layer (SSL) is the standard security technology for establishing an encrypted link between a web server and a browser. SSL traffic is often decrypted at the load balancer. When a load balancer decrypts traffic before passing the request on, it is called SSL termination. The load balancer saves the web servers from having to expend the extra CPU cycles required for decryption. This improves application performance.

  - However, SSL termination comes with a security concern. The traffic between the load balancers and the web servers is no longer encrypted. This can expose the application to possible attack. However, the risk is lessened when the load balancer is within the same data center as the web servers.

  - Another solution is the SSL pass-through. The load balancer merely passes an encrypted request to the web server. Then the web server does the decryption. This uses more CPU power on the web server. But organizations that require extra security may find the extra overhead worthwhile.

    

