---
title: Server Infrastructure
description: This document covers fundamental concepts of Server Infrastructure
---

# Server Infrastructure

We often categorize servers by their primary function rather than their underlying technology.

## Infrastructure Components

### Web Server

A server with a specialized software that **handles HTTP requests** from clients (typically browsers). It directly **serves static content** (HTML, CSS, JavaScript, images), **forwards requests** for dynamic content to backend servers for processing, and **returns appropriate responses** to the client.

!!! info "Primary Function"

    - Focus on **serving web content**, **handling HTTP protocols**, and **routing requests**

!!! example "Examples"

    - IIS, Nginx, Apache HTTP Server

### Reverse Proxy

A server with a specialized software that sits between client devices and backend servers, **intercepting client requests** and **forwarding them to appropriate backend servers** while providing security, SSL termination, and caching.

!!! info "Primary Function"

    - Focus on request **forwarding**, **content caching**, and **security**

!!! example "Examples"

    - Nginx, IIS with Application Request Routing (ARR), Apache with mod_proxy

### Load Balancer

A server with a specialized software that **distributes incoming client requests across multiple servers** to ensure high availability, avoid traffic overload, prevent single points of failure, and enable horizontal scaling.

!!! info "Primary Function"

    - Focus on **traffic distribution** and **high availability**

!!! example "Examples"

    - **Load Balancing Software**: Microsoft Network Load Balancing (NLB), Nginx, HAProxy
    - **Cloud-based Services**: AWS Elastic Load Balancing, Azure Load Balancer

### API Gateway

A server with a specialized software that **acts as a single entry point** for client applications to access multiple backend services and APIs. It **serves as a reverse proxy for API requests** while providing additional functionality such as request routing, authentication, and monitoring.

!!! info "Primary Function"

    - Focus on **API traffic management**, **security**, and **request coordination** across multiple services

!!! example "Examples"

    - **Cloud-based Services**: Azure API Management, Amazon API Gateway
    - **Software**: Kong, Apigee, Tyk

### CDN (Content Delivery Network)

**A distributed network of servers** that caches and delivers web content from the server closest to users, improving performance and user experience.

!!! note "Key Characteristic"

    - Distributed globally to optimize content delivery

!!! example "Examples"

    - CloudFlare, Akamai, Fastly

## Server Deployment Models

### Physical Servers

Traditional deployment using dedicated hardware.

!!! info "Characteristics"

    - Full control over hardware
    - Higher upfront costs
    - Limited scalability
    - Requires physical maintenance

### Virtual Machines

Software-based emulation of physical computers.

!!! info "Characteristics"

    - Resource isolation
    - Multiple OS instances
    - Better resource utilization
    - Easier management

### Containers

Lightweight, standalone executable packages of software.

!!! info "Characteristics"

    - Process isolation
    - Consistent environments
    - Fast deployment
    - Resource efficiency

### Serverless

Cloud computing execution model where the cloud provider manages the server infrastructure.

!!! info "Characteristics"

    - No server management
    - Pay-per-use pricing
    - Automatic scaling
    - Event-driven execution
