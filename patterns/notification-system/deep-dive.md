# Notification System

## Problem

Need to reliably deliver notifications to millions of users across multiple channels without slowing down the application.

---

## High-Level Flow

```
Application
      ↓
 Notification API
      ↓
 Message Queue
      ↓
 Notification Workers
      ↓
Push / Email / SMS Providers
      ↓
      User
```

The application should never wait for the notification to be delivered.

---

## Step 1 - Receive Request

Example:

```
User placed an order.

↓

Send notification
```

The API validates the request and stores a notification job.

---

## Step 2 - Queue

Push the job into a message queue.

Benefits:

- Decouples services
- Handles traffic spikes
- Enables retries

---

## Step 3 - Worker

Workers consume jobs independently.

Responsibilities:

- Build notification payload
- Select delivery channel
- Call provider API
- Record delivery status

Workers can scale horizontally.

---

## Step 4 - Provider

Examples:

```
Push

↓

APNs
FCM

Email

↓

SMTP
Email Provider

SMS

↓

SMS Gateway
```

Each provider has different APIs and rate limits.

---

## Step 5 - Retry

Failures happen:

- Timeout
- Network failure
- Provider unavailable

Retry using:

- Exponential backoff
- Maximum retry count
- Dead-letter queue

---

## Step 6 - User Preferences

Before sending:

```
Is this channel enabled?

↓

Yes → Send

No → Skip
```

Users may disable email while keeping push notifications enabled.

---

## Step 7 - Delivery Tracking

Store:

- Pending
- Sent
- Delivered
- Failed

Useful for analytics and debugging.

---

## Scaling

Scale independently:

- API Servers
- Queue
- Workers
- Providers

Workers are usually the first component to scale.

---

## Common Challenges

- Duplicate notifications
- Provider failures
- Rate limits
- Retry storms
- Notification ordering
