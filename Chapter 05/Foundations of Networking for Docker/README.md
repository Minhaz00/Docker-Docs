# Foundations of Networking for Docker

##   Introduction 
Docker networking is a crucial aspect of containerization that enables communication between Docker containers, as well as between containers and the outside world. It allows us to create isolated environments for our applications while providing flexibility and control over how they communicate. This documentation covers essential aspects of Docker networking, including protocols, interfaces, ports, Network Address Translation (NAT), and port forwarding, with practical demonstrations.

**Core Concepts of Networking**

#### Protocols

Docker networking primarily uses standard networking protocols such as TCP/IP, UDP, and ICMP. These protocols facilitate communication between containers and between containers and external networks.

#### Network Interfaces

![Network Interfaces](image_link)

Each container has its own network namespace, including a loopback interface (`lo`) and one or more Ethernet interfaces (`eth0`, `eth1`, etc.).

##### Loopback Interface (lo)

- **Purpose**: Internal communication within the container.

##### Ethernet Interfaces (eth0, eth1, etc.)

- **Purpose**: Facilitate communication with external networks and other containers.

#### Ports

![Ports](image_link)

Ports play a crucial role in networking by providing unique addresses for individual processes. They allow containers to expose their services to other containers or external networks. Docker supports publishing container ports to the host.

#### Network Address Translation (NAT)

NAT serves as a bridge between internal container networks and external networks by dynamically mapping container IP addresses to host IP addresses. This translation enables seamless communication between containers and external resources, facilitating connectivity and resource access.

#### Port Forwarding

Port forwarding maps host ports to container ports, enabling access to container services from outside the Docker host.

## Docker Networking

Docker treats networks as first-class entities with their own lifecycle. Containers attached to a Docker network receive unique IP addresses routable from other containers on the same network. This section will explore Docker's default networks and user-defined networks.

### Default Docker Networks

By default, Docker provides three types of networks:

1. **Bridge**: This is the default network mode in Docker. It creates a virtual bridge network that allows containers to communicate with each other on the same host. Each container connected to the bridge network gets its own IP address, and containers can communicate with each other using these IP addresses. The bridge network also provides external connectivity, allowing containers to access resources outside of the host.

2. **Host**: In this mode, containers share the network namespace with the host. This means they use the host's network stack directly, including its IP address and network interfaces.

3. **None**:  This mode disables networking for containers entirely. Containers in this mode cannot access the network, including external resources and other containers.

Run `docker network ls` to list all networks:

```shell
docker network ls
```

Example output:

//image

