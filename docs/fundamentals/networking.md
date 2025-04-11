---
title: Networking & Communication
description: This document covers fundamental concepts of Networking & Communication
---

# Networking & Communication

## Network Models

### OSI Model Simplified

A simplified view of the most relevant layers for software development:

!!! info "Key Layers"

    - **Application Layer (L7)**: HTTP, HTTPS, FTP, SMTP
    - **Transport Layer (L4)**: TCP, UDP
    - **Network Layer (L3)**: IP, Routing
    - **Physical/Data Link (L1/L2)**: Hardware, MAC addresses

### TCP/IP Model

Four-layer model that combines several OSI layers into a more practical framework:

!!! info "Layers"

    1. **Link Layer**: Network interface (Ethernet)
    2. **Internet Layer**: Routing packets across networks (IP)
    3. **Transport Layer**: End-to-end data delivery (TCP, UDP)
    4. **Application Layer**: End-user services (HTTP, FTP, SMTP)

## Network Addressing

### IP Address

Numerical label assigned to devices on a network, used to identify and locate them for communication.

!!! info "Types"

    - **IPv4**: 32-bit addresses (e.g., 192.168.1.1)
    - **IPv6**: 128-bit addresses (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334)
    - **Private vs Public IPs**
    - **Localhost (127.0.0.1)**

### Port Numbers

Logical endpoints for network communication:

!!! example "Common Ports"

    - **80**: HTTP
    - **443**: HTTPS
    - **22**: SSH
    - **3306**: MySQL
    - **5432**: PostgreSQL
    - **27017**: MongoDB

### DNS (Domain Name System)

System that translates human-readable domain names into IP addresses computers use to identify each other.

!!! info "Common Record Types"

    - **A Record**: Maps hostname to IPv4 address
    - **CNAME**: Canonical name record (alias)
    - **MX**: Mail exchange record
    - **TXT**: Text record, often used for verification

!!! example "Process"

    - When you enter "example.com" in a browser, DNS resolves this to an IP address like 93.184.216.34

## Communication Protocols

### TCP vs UDP

Understanding when to use each protocol:

| Feature     | TCP                 | UDP                    |
| ----------- | ------------------- | ---------------------- |
| Connection  | Connection-oriented | Connectionless         |
| Reliability | Guaranteed delivery | Best effort delivery   |
| Order       | Maintains order     | No order guarantee     |
| Use Case    | Web, APIs, Database | Streaming, Gaming, DNS |

### HTTP/HTTPS

Protocols for transmitting web content; HTTPS adds encryption for secure data transfer.

!!! info "Key Concepts"

    - **Request/Response Model**
    - **Headers and Body**
    - **Status Codes** (2xx, 3xx, 4xx, 5xx)
    - **CORS and Security**
    - **Caching Mechanisms**

!!! example "Common Methods"

    - **GET**: Retrieve data
    - **POST**: Submit data
    - **PUT**: Update existing resources
    - **DELETE**: Remove resources

### HTTP Versions

Evolution and key improvements:

!!! info "Version Features"

    - **HTTP/1.1**: Basic persistent connections
    - **HTTP/2**: Multiplexing, Server Push
    - **HTTP/3**: QUIC protocol, improved performance

### TLS/SSL

Cryptographic protocols that provide secure communication over a computer network, commonly used in HTTPS.

!!! warning "Security Best Practice"

    - Always use TLS 1.2 or higher; older versions have known vulnerabilities

### WebSockets

Communication protocol that provides full-duplex communication channels over a single TCP connection.

!!! tip "When to Use"

    - Ideal for real-time applications like chat, live notifications, and collaborative tools

## Performance Metrics

### Latency

The time delay between a request being sent and the response being received, often measured in milliseconds.

!!! example "Typical Values"

    - **Low latency**: < 100ms
    - **Medium latency**: 100-300ms
    - **High latency**: > 300ms

### Throughput

The amount of data that can be transferred from one point to another in a given time period.

!!! info "Common Units"

    - Kbps (Kilobits per second)
    - Mbps (Megabits per second)
    - Gbps (Gigabits per second)

### Bandwidth

The maximum data transfer rate of a network or internet connection.

!!! info "Typical Values"

    - **Home broadband**: 25-1000 Mbps
    - **Business connections**: 100 Mbps to 10+ Gbps
    - **Data center connections**: Multiple 10/40/100 Gbps links

### Packet Loss

Percentage of packets that fail to reach their destination.

!!! warning "Impact"

    - **<1%**: Generally acceptable
    - **1-2.5%**: Noticeable impact on real-time applications
    - **>3%**: Significant degradation in service quality

### Jitter

Variation in the delay of received packets.

!!! info "Acceptable Values"

    - **<30ms**: Good for VoIP and video calling
    - **>30ms**: May cause quality issues in real-time communications
