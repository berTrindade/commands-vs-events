# Commands vs Events

The confusion usually comes from mixing up **RabbitMQ** (a tool) with **architectural patterns** (how you use the tool).

Think of RabbitMQ as the postal service. It doesn't decide what kind of communication you're doing. It just moves messages.

## Case 1 — sending a command (orchestration)

```
Order Service
      |
      | "Charge this customer"
      v
Payment Queue
      |
      v
Payment Service
```

The Order Service is telling the Payment Service exactly what to do. That's a **command**.

## Case 2 — publishing an event (choreography)

```
Order Service
      |
      | "An order was created"
      v
   RabbitMQ
   /       \
Inventory   Email
Service     Service
```

The Order Service isn't asking anyone to do anything. It's announcing that something happened. Any service interested in that event can react. That's **choreography**.

## If you install RabbitMQ today, you get neither

Out of the box RabbitMQ only gives you this:

```
Producer ---> RabbitMQ ---> Consumer
```

It has no idea whether the message means "charge the customer", "OrderCreated", or "hello world". That meaning is your application's decision, not the broker's.

## What people mean by "Event-Driven Architecture"

They usually mean messages that describe something that **already happened**.

```
OrderCreated
PaymentSucceeded
UserRegistered
InvoiceGenerated
```

Other services subscribe and react on their own terms.

## Rule of thumb

Ask yourself one question.

> Am I telling another service what to do, or am I announcing something that happened?

- "Charge the customer." is a **command**, and usually points to orchestration.
- "CustomerCharged." is an **event**, and usually points to choreography.

That's the core distinction. RabbitMQ can carry both kinds of message. The pattern lives in your intent, not in the broker.
