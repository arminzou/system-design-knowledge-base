---
title: API Development & Management
description: This document covers fundamental concepts of API Development & Management
---

# API Development & Management

## API Architecture

### REST API

Architectural style for designing networked applications using HTTP methods and
stateless operations.

!!! info "Key Principles"

    - Stateless
    - Resource-based
    - Uses standard HTTP methods
    - Representation-oriented

### GraphQL

Query language and runtime for APIs that allows clients to request exactly the
data they need.

!!! tip "Advantages"

    - Single endpoint for all operations
    - Clients specify exactly what data they need
    - Strongly typed schema
    - Reduces over-fetching and under-fetching of data

### gRPC

High-performance, open-source universal RPC framework using Protocol Buffers.

!!! info "Key Features"

    - Uses HTTP/2 for transport
    - Binary serialization with Protocol Buffers
    - Strongly typed contracts
    - Supports streaming: unary, server, client, and bidirectional

## API Communication Mechanisms

### Polling

Client regularly checks the server for new data at fixed intervals.

!!! warning "Considerations"

    - Simple but inefficient for real-time data; generates unnecessary requests

### Long Polling

Client makes a request to the server, and the server holds the connection open until new data is available.

!!! tip "Best For"

    - Simple real-time applications where WebSockets might be overkill

### Webhooks

HTTP callbacks that deliver data to other applications in real-time when
specific events occur.

!!! example "Common Use Cases"

    - Payment processing notifications
    - CI/CD pipeline events
    - CRM updates

### Circuit Breaker

Pattern that prevents cascading failures by stopping requests to failing services.

!!! info "States"

    - **Closed**: Requests flow normally
    - **Open**: Requests immediately fail without attempting to call the service
    - **Half-Open**: Limited requests allowed to test if service has recovered

## API Reliability & Protection

### Rate Limiting

Strategy to restrict the number of API requests a user can make within a
specified time period.

!!! example "Common Approaches"

    - Fixed window
    - Sliding window
    - Token bucket
    - Leaky bucket

### Throttling

Mechanism to control the usage of APIs by limiting the rate at which consumers can make calls.

!!! tip "Implementation"

    - Should return 429 (Too Many Requests) status code with Retry-After header

### Idempotency

Property where multiple identical requests have the same effect as a single
request.

!!! example "HTTP Methods"

    - **Naturally idempotent**: GET, PUT, DELETE
    - **Not naturally idempotent**: POST (requires idempotency keys)

### Backoff Strategies

Technique where a client progressively increases the delay between retries after failed requests.

!!! example "Common Types"

    - **Exponential backoff**: Wait times increase exponentially (1s, 2s, 4s, 8s...)
    - **Jittered backoff**: Adds randomness to avoid thundering herd problems
