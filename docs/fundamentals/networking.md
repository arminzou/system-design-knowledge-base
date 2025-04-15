---
title: Networking & Communication
description: This document covers fundamental concepts of Networking & Communication
---

# Networking & Communication

## Network Models

Describes how computer networks are organized and how their components interact.

=== "TCP/IP Model"

    **TCP/IP (Transmission Control Protocol/Internet Protocol)** model is a conceptual and practical framework that organizes network communications into four layers:

    !!! info annotate "Layers"

        - **Application Layer**(1): Where applications and services operate (HTTP, FTP, SMTP)
        - **Transport Layer**(2): Handles data transmission between hosts (TCP, UDP)
        - **Internet Layer**(3): Manages addressing and routing of data packets
        - **Network Access/Link Layer**(4): Handles physical connection to the network (Ethernet, WiFi)

    1. End-user services
    2. End-to-end data delivery
    3. Routing packets across networks
    4. Network interface

=== "OSI Model"

    **OSI (Open Systems Interconnection) model** is another conceptual framework that describes and breaks down network communication in seven layers:

    !!! info "Layers"

        - **Application Layer (L7)**: User interface and services (HTTP, HTTPS, FTP, SMTP, DNS)
        - **Presentation Layer (L6)**: Data translation, encryption, compression (SSL/TLS, JPEG, MPEG)
        - **Session Layer (L5)**: Connection management and synchronization (NetBIOS, RPC)
        - **Transport Layer (L4)**: End-to-end communication and error recovery (TCP, UDP)
        - **Network Layer (L3)**: Logical addressing and routing (IP, ICMP, OSPF)
        - **Data Link Layer (L2)**: Physical addressing and error detection (Ethernet, MAC, PPP)
        - **Physical Layer (L1)**: Physical transmission of raw bits (Cables, Hubs, Repeaters)

!!! tip "Common Protocols"

    - **Web**: HTTP/HTTPS (Application)
    - **Email**: SMTP (Application)
    - **File Transfer**: FTP (Application)
    - **Reliable Data**: TCP (Transport)
    - **Fast Data**: UDP (Transport)
    - **Routing**: IP (Network)

## Network Addressing & Identification

### IP Addressing

Numerical label assigned to devices on a network using the Internet Protocol (IP), used to identify and locate them for communication.

#### IPv4 and IPv6

IPv4 and IPv6 are two versions of the Internet Protocol (IP) used to identify devices on a network.

!!! Note "IPv4 Key Characteristics"

    -   32-bit address format (4 bytes): Provides over 4.3 billion unique addresses (2^32)
    -   Dotted decimal notation: Addresses written as four 8-bit numbers separated by dots (e.g., 192.168.1.1), divided into network and host portions
    -   Address classes: Traditionally divided into classes A, B, C, D, and E (though modern networks use CIDR instead)
    -   Limited supply: The finite address space has led to IPv4 address exhaustion
    -   NAT dependency: Network Address Translation became necessary to extend the usable life of IPv4

!!! Note "IPv6 Key Characteristics"

    -   128-bit address format (16 bytes): Provides virtually unlimited addresses (2^128) to solve IPv4 address exhaustion
    -   Hexadecimal notation: Addresses written as eight 16-bit groups separated by colons (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334)
    -   Built-in security: Integrated IPsec support for authentication and encryption
    -   Auto-configuration: Supports stateless address auto-configuration (SLAAC)
    -   No NAT requirement: Designed to provide direct end-to-end connectivity without address translation

#### Subnets and CIDR notation

**Subnets** and **CIDR notation** is about **how networks are segmented**.
A **subnet** is a logical partition to divide a large network into smaller netowkr segments for improved security and performance, while **CIDR notation** uses format `IP_address/prefix_length` to represent subnet masks in IP addresses (e.g., 192.168.1.0/24).

!!! example

    **192.168.1.0/24**

    - 192.168.1.0 is the network address and /24 is the subnet mask
    - Subnet mask 24 means the first 24 bits are used for network identification, while the remaining 8 bits are used for host identification within the network
    - Contains 254 usable addresses (192.168.1.1 to 192.168.1.254)

#### Public vs Private addresses

**Public IP addresses** are globally routable and unique across the internet.

**Private IP addresses** (10.x.x.x, 172.16-31.x.x, 192.168.x.x) are used within local networks and cannot be accessed directly from the internet without NAT (Network Address Translation).

!!! Note

    - Every device has to have an IP address for communication purposes.
    - Public IP address is unique and assigned by Internet Service Provides to give you access to the World Wide Web.
    - Private IP address is only used internally (Home, business, etc) and needs to be conveted to public IP address to access the internet.

#### Loopback addresses

**127.0.0.1 (IPv4)** and **::1 (IPv6)** are **localhost** IP addresses for local development.

Special addresses (**127.0.0.1** in IPv4 and **::1** in IPv6) that allow device to send network packets to itself. Essential for testing network services locally without using the physical network interface.

### DNS (Domain Name System)

A system that translates domain names into IP addresses computers use to identify each other.

!!! example

    - When you enter "example.com" in a browser, DNS resolves this to an IP address like 93.184.216.34

!!! info "Common Record Types"

    - **A Record**: Maps hostname to IPv4 address
    - **CNAME**: Canonical name record (alias)
    - **MX**: Mail exchange record
    - **TXT**: Text record, often used for verification

#### Resolution process

DNS resolution typically involves multiple steps to find the IP address for the domain.

!!! info "How does it work"

    1. Browser first checks local DNS cache
    2. If not found, it queries a recursive DNS resolver (Provided by ISP)
    3. Resolver queries root servers, TLD nameservers, authoritative nameservers to find the IP address for the domain
    4. Caches the response and return the IP address to client

    This process is often cached at various levels to improve performance, based on the TTL.

*[Resolution process]: How your computer converts a human-friendly domain name (like "example.com")

#### TTL and propagation

TTL is a value assigned to each DNS record that determines how long that record can be cached before they must be refreshed.

DNS propagation refers to the time required for updated DNS records to spread across all DNS servers globally, which can take from minutes to 48 hours depending on TTL settings.

!!! note annotate "DNS propagation"

    When you make DNS changes (like pointing your domain to a different server), the updates won't be fully propagated until the TTL expires. (1)

1. :notepad_spiral: When planning major DNS changes (like changing web hosts), it's common practice to:
    1. Lower your TTL values to something small (like 300 seconds/5 minutes) a day or two before the change
    2. Make the DNS change
    3. Wait for the short TTL to expire globally
    4. Then increase the TTL back to a longer value (like 3600 or 86400 seconds)

#### Local host files

Operating systems maintain a **hosts file** (1) that maps hostnames to IP addresses locally, bypassing DNS resolution. This file can be modified for testing, development, or blocking specific websites, and takes precedence over DNS lookups.
{ .annotate }

1. `/etc/hosts` on Linux/Mac or `C:\Windows\System32\drivers\etc\hosts` on Windows

### Port Numbers

Logical endpoints for network communication that help computers distinguish between different services or applications running on the same IP address. (1)
{ .annotate }

1. Think of an **IP address** as the address of a building, while **port numbers** identify specific rooms or offices within that building.

!!! example "Common Ports"

    - **Web server (80/443)**: 192.168.1.10:80 (HTTP) or 192.168.1.10:443 (HTTPS)
    - **FTP server (21)**: 64.233.160.0:21
    - **SSH connection (22)**: 52.45.187.132:22
    - **SMTP mail server (25)**: smtp.example.com:25 (same as 192.168.2.5:25)
    - **DNS server (53)**: 8.8.8.8:53 (Google's DNS)
    - **Remote desktop (3389)**: 10.10.5.12:3389 (RDP)
    - **MySQL database (3306)**: 10.0.0.5:3306
    - **PostgreSQL server (5432)**: 172.16.254.1:5432
    - **MongoDB instance (27017)**: 192.168.0.100:27017

### Socket

A two-way communication endpoint that allows programs to exchange data over the network. A socket is bound to a port number so that the TCP layer can identify the application that data is destined to be sent to.

!!! tip

    A **socket address** is the combination of **IP address** and **port number**: 192.168.1.5:80

!!! info "So How Does It Work Exacly?"

    A typical example will be when you're using a web browser to visit a website:

    1. **Application Initiates Connection**: When you type "www.example.com" in your browser, your browser (the client) needs to create a network connection.
    2. **Socket Creation**: Your browser creates a socket - think of this as opening a communcation channel
    3. **Client Port Assignment**: Your OS automatically assigns your browser a random high-numbered port (like 49152-65535) as the source port.
    4. **DNS Resolution**: Your computer figures out the IP address for "example.com".
    5. **Connection Establishment**: Your browser uses its socket (Your IP address + Source Port) to connect to the web server at example.com on port 80/443 (standard HTTP/HTTPS port).
    6. **On the Server Side**: The web server at "example.com" already has a socket bound to port 80/443, activaly listening for incoming connections.
    7. **Data Exchange**: Your browser sends an HTTP request for the webpage, and the server sends back the content.
    8. **Connection Termination**: After receiving the webpage, the connection might be kept open for a while or closed, depending on settings.

    !!! note annotate "Port Biding"

        For client applications:

           - Typically don't bind to a specific port explictly
           - The OS assigns them a random port during socket creation process

        For server applications (1):

           - Explicitly bind to a specific, well-known port (like port 80 for HTTP or 443 for HTTPS) 
           - Choice of port depends on the type of application so the clients know where to connect
  
    1. :information_source: For web applications: Actually the **web server (like Apache, Nginx, or IIS)** binds to port 80, not the individual web applications. When requests come in, the web server routes them to different applications.

#### Socket Types and States

!!! info "Socket Types"

    === "Stream Socket"

        Connection-oriented sockets that use TCP for reliable, sequenced communication.
            
        Example: Web servers, file transfers, where data integrity is crucial

    === "Datagram Socket"

        Connectionless sockets that use UDP and don't guarantee reliable delivery.
            
        Example: DNS queries, streaming video, online gaming, where speed is more important

!!! info "Socket States"

    Socket states can be defined as the different phases a network socket goes through during its lifecycle.

    === "Stream Socket States (TCP-Specific)"

        TCP sockets follow a [complex state]() transition diagram throughout their connection lifecycle.


    === "Datagram Socket States (UDP-Specific)"

        UDP sockets have a much [simpler state]() model:

        - Bound: Socket has a local address and port assigned
        - Connected: Socket filters to receive only from a specific address (not a true connection)
        - Closed: Socket is no longer valid for communication

### Data Packet

A unit of data that is transferred over a network. Data sent over network is divided into packets, they are then reassembed by the device that received them.

#### Headers and encapsulation

**Encapsulation** is the process of wrapping data chunks in layers of **headers** as it moves down the protocol stack.

**Headers** are the information (protocol, source address, destination address,etc) added to the packet.

!!! note annotate

    If the orginal data is too large to  fit into a single packet, the data will be divided into multiple smaller chuncks. (1) Each of these chuncks will then go through the encapsulation process to become a data packet.

    !!! info "How does it work?"

        When you send information across a network:

        1. The original data (like text from a webpage) starts as raw content.
        2. The transport layer (TCP) divides large data into appropriate segments if needed
        3. As this data moves down through the network protocol stack, each layer adds its own header information (encapsulation):
           - The application layer might format it as HTTP
           - The transport layer (TCP/UDP) adds port information and sequence numbers
           - The network layer (IP) adds source and destination IP addresses
           - The link layer adds MAC addresses
        4. After complete encapsulation, if the packet exceeds the network's MTU, IP fragmentation occurs.

1. This division typically happens at different layers: At the **transport layer** (TCP), large data is segmented into smaller pieces that fit within the Maximum Segment Size (MSS). After encapsulation, if packets are still too large, the **network layer** (IP) will perform further fragmentation.

#### MTU and Fragmentation

**MTU**: The largest packet size that can be sent over a network connection.

**Fragmentation**: When a packet is larger than the MTU, it gets split into smaller fragments:

- Each fragment gets its own set of headers
- Fragments are reassembled at the destination
- The IP header contains information to help with reassembly

!!! tip "Standard MTU Sizes"

    - Ethernet: 1500 bytes
    - Wi-Fi: 1500 bytes

!!! info "Relationship between Encapsulation and Fragmentation"

    1. **MTU Constraints**: Each network link has an MTU limit. If after encapsulation a packet exceeds this limit, fragmentation becomes necessary.

    2. **Header Overhead**: Each layer's headers (added during encapsulation) reduce the amount of space available for actual data. For example, if MTU is 1500 bytes and headers take 40 bytes, only 1460 bytes remain for data (the MSS).

    3. **Performance Impact**: Fragmentation impacts network performance because:

          - It increases overall overhead (each fragment needs its own complete headers)
          - All fragments must arrive for reassembly (if one is lost, everything must be retransmitted)
          - Reassembly requires extra processing and memory

    4. **Path MTU Discovery**: Modern networks use this technique to determine the smallest MTU along the entire path and limit packet sizes accordingly to avoid fragmentation.

## Transport Protocols

**Transport protocols** enable communication between applications across networks, handling data delivery between endpoints. They manage how data is packaged, transmitted, received, and verified.

TCP and UDP represent two fundamentally different approaches to network communication:

- TCP: Prioritizes reliability and order at the cost of speed and overhead

- UDP: Prioritizes speed and simplicity at the cost of reliability guarantees

### TCP

A connection-oriented transport protocol that provides a reliable communication channel between devices over the internet with guaranteed delivery and ordered data transmission.

#### Connection-Oriented Communication

TCP establishes a dedicated connection through *Three-way Handshake* before exchanging data and ensures all packets arrive correctly.

!!! info "Three-Way Handshake"

    A fundamental process to establish a reliable connection between a client and a server before data transmission begins. It's like a formal greeting ritual that happens in three steps:

    ```mermaid
    sequenceDiagram
        participant Client
        participant Server

        Note over Client,Server: Connection Establishment
        Client->>Server: SYN (seq=x)
        Note right of Client: Client sends SYN packet with initial sequence number
        Server->>Client: SYN-ACK (seq=y, ack=x+1)
        Note left of Server: Server acknowledges client's SYN and sends its own SYN
        Client->>Server: ACK (seq=x+1, ack=y+1)
        Note right of Client: Client acknowledges server's SYN
        Note over Client,Server: Connection Established
        
    ```

    1. **SYN**: The client initiates the connection by sending an SYN packet with client's sequence number (x)
    2. **SYNC-ACK**: The server responds with a SYN-ACK message to acknowledge the client's request and synchronize sequence numbers (server's sequence number (y) and client's sequence number + 1 (x+1))
    3. **ACK**: The client sends an ACK packet with server's sequence number + 1 (y+1) to establish the connection.

!!! question "Why Do We Need Three-Way Handshake?"

    The three-way handshake is necessary because both parties need to confirm that they can send and receive messages properly.

    <figure markdown>
        ![Three-way Handshake Diagram](../assets/images/three-way-handshake.png){ width="650" }
        <figcaption>TCP Three-way Handshake Process</figcaption>
    </figure>

#### TCP Socket States

TCP connections go through different states throughout their lifecycle.

| State        | Description                                                                                     |
|--------------|-------------------------------------------------------------------------------------------------|
| CLOSED       | The default state when no connection exists                                                     |
| LISTEN       | The server is waiting for connection requests (only servers enter this state)                   |
| SYN_SENT     | The client has sent a SYN packet and is waiting for a response                                  |
| SYN_RECEIVED | The server has received a SYN and sent a SYN-ACK, waiting for the final ACK                     |
| ESTABLISHED  | The three-way handshake is complete and data transfer can begin                                 |
| FIN_WAIT_1   | The application has finished sending data and sent a FIN packet to start closing the connection |
| FIN_WAIT_2   | The local end has received acknowledgment of its FIN packet                                     |
| CLOSE_WAIT   | The remote end has initiated connection termination, and the local end needs to close too       |
| LAST_ACK     | The local end has sent its own FIN after receiving a FIN from the remote end                    |
| TIME_WAIT    | Waiting to ensure the remote end received the acknowledgment of its FIN                         |
| CLOSING      | Both sides initiated closing simultaneously                                                     |

!!! note "Key Characteristics"

    - Reliability: Acknowledgment and retransmission of lost packets
    - Ordered Delivery: Sequence numbers ensure correct ordering
    - Flow Control: Prevents overwhelming receiver with too much data
    - Congestion Control: Adjusts transmission rate based on network conditions
    - Error Detection: Checksum verification ensures data integrity

!!! example "Common TCP Use Cases"

    - Web browsing (HTTP/HTTPS)
    - Email (SMTP, IMAP, POP3)
    - File transfers (FTP, SFTP)
    - Remote terminal access (SSH)
    - Database connections

### UDP

A connectionless transport protocol that sends datagrams between applications without guaranteeing delivery, used for real-time applications where speed matters more than perfect reliability.

#### Connectionless Communication

UDP sends datagrams without establishing a dedicated connection, prioritizing speed over reliability.

!!! info "UDP Communication Flow"

    In contrast to TCP's connection-oriented approach, UDP operates without formal connection between endpoints:

    ```mermaid
    sequenceDiagram
        participant Client
        participant Server

        Note over Client,Server: Connectionless Communication
        Client->>Server: Datagram 1
        Client->>Server: Datagram 2
        Client->>Server: Datagram 3
        Note right of Client: Datagrams sent without waiting for acknowledgment
        Note left of Server: May arrive in any order, or not at all
        
    ```

    1. **No Handshake**: Packets are sent immediately without connection establishment
    2. **Stateless Communication**: No connection state is maintained between endpoints
    3. **Independent Packets**: Each datagram handled independently ("fire and forget")
    4. **No Order Guarantee**: Packets may arrive in a different order than sent, with no mechanism to restore sequence

#### UDP Socket States

UDP sockets have a simplified state model compared to TCP:

`Simple Lifecycle: Create → Bind → Send/Receive → Close`

| State     | Description                                                               |
|-----------|---------------------------------------------------------------------------|
| UNBOUND   | Socket created but not yet assigned to an address or port                 |
| BOUND     | Socket assigned to a local address and port, ready for communication      |
| CONNECTED | Socket filtered to receive only from a specific address (not true connection) |
| CLOSED    | Socket is no longer valid and cannot be used for communication            |

!!! note "Key Characteristics"

    - Message-Oriented: Preserves message boundaries
    - Unreliable: No guaranteed delivery or acknowledgments
    - Unordered: No sequence numbers or packet ordering
    - Lightweight: Minimal header overhead (8 bytes)
    - Broadcast/Multicast: Supports one-to-many communication patterns
    - No Flow Control: Can send at any rate regardless of receiver capacity

!!! example "Common UDP Use Cases"

    - Domain Name System (DNS)
    - Streaming media (audio/video)
    - Voice over IP (VoIP)
    - Online gaming
    - IoT sensor data
    - Network monitoring (SNMP)

### TCP vs UDP

| Feature            | TCP                             | UDP                                     |
|--------------------|---------------------------------|-----------------------------------------|
| Connection         | Connection-oriented             | Connectionless                          |
| Reliability        | Guaranteed delivery             | Best-effort delivery                    |
| Ordering           | Maintains packet order          | No ordering guarantees                  |
| Error checking     | Extensive                       | Basic checksumming                      |
| Speed              | Slower due to overhead          | Faster, minimal overhead                |
| Header size        | 20-60 bytes                     | 8 bytes                                 |
| Congestion control | Yes                             | No                                      |
| Flow control       | Yes                             | No                                      |
| Packet boundaries  | Stream-oriented (no boundaries) | Message-oriented (preserves boundaries) |
| Use cases          | Web, email, file transfer       | Streaming, gaming, DNS                  |

!!! abstract "TCP vs UDP: Stream-Oriented vs Message-Oriented"

    - **TCP**: You get a continuous flow of bytes, requiring you to add your own message boundaries
    - **UDP**: You get distinct, separate messages with natural boundaries already preserved

    !!! example "TCP (Stream-Oriented)"

        TCP works like a continuous phone call where all words flow together:

        - When you send three messages: "Hello", "How are you?", "What's new?"
        - TCP delivers them as one continuous stream: "HelloHow are you?What's new?"
        - Your app must add its own framing mechanism to know where messages begin and end
        - Like adding delimiter characters after each message: `Hello\nHow are you?\nWhat's new?\n`

    !!! example "UDP (Message-Oriented)"

        UDP works like sending separate text messages:

        - Send three messages: "Hello", "How are you?", "What's new?"
        - UDP delivers them as three separate packages, boundaries intact
        - Each message arrives independently, with clear start and end points
        - No need to add extra markers - the packaging itself separates messages

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

#### HTTP Versions

Evolution and key improvements:

!!! info "Version Features"

    - **HTTP/1.1**: Basic persistent connections
    - **HTTP/2**: Multiplexing, Server Push
    - **HTTP/3**: QUIC protocol, improved performance

### WebSockets

Communication protocol that provides full-duplex communication channels over a single TCP connection.

!!! tip "When to Use"

    - Ideal for real-time applications like chat, live notifications, and collaborative tools

### API Protocols

#### REST

#### GraphQL

#### gRPC

### TLS/SSL

Cryptographic protocols that provide secure communication over a computer network, commonly used in HTTPS.

!!! warning "Security Best Practice"

    - Always use TLS 1.2 or higher; older versions have known vulnerabilities

## Network Services & Infrastructure

### DHCP

#### Automatic IP configuration

#### Lease process

### NAT (Network Address Translation)

#### How private networks connect to the internet

#### NAT types and challenges

### Routing

#### Routing tables and protocols

#### Default gateways

#### Autonomous systems

#### Routing protocols: How routers learn paths (BGP, OSPF, etc.)

### Proxies & Intermediaries

#### Forward proxies

#### Reverse proxies

#### Load balancers

### Firewalls & Network Security

#### Traffic filtering

#### Stateful vs stateless inspection

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

## Network Troubleshooting

### Common diagnostic tools

### Typical network issues

### Debugging approaches
