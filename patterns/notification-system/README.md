# Notification System

> Deliver notifications reliably across multiple channels (Push, Email, SMS, In-App).

## Functional Requirements

- Send notifications
- Support multiple channels
- Retry failed deliveries
- User notification preferences
- Scheduling (optional)

## Non-Functional Requirements

- High availability
- Low latency
- Scalable
- Reliable delivery
- Fault tolerant

## Core Components

- API
- Notification Service
- Message Queue
- Workers
- Channel Providers
- User Preferences
- Retry Queue

## Delivery Channels

- Push
- Email
- SMS
- In-App

## Design Decisions

- Queue strategy
- Retry policy
- Provider integration
- Idempotency
- Delivery guarantees

## Interview Recognition

Use when designing:

- Order updates
- Password reset
- Marketing campaigns
- Chat notifications
- Social media alerts

## Key Takeaways

- Never send notifications synchronously.
- Use queues to decouple producers from consumers.
- Workers handle delivery.
- Retries are mandatory.
