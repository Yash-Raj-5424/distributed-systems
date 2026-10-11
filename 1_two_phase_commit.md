**distributed traxn:** a traxn that span over multiple physical system machines or computers

> scenario: 10 min food delivery by Zomato. what it does is it orders food from resturants in bulk and keep them in their dark stores and the delivery workers are on standby. to guarantee this, it should accept the order only when food is available in the store and the delivery partner must be available.

- this is a classic distributed traxn situation - orderservice receives the order and it has to talk to store and delivery service => if either of them fails, we revert the order i.e, both steps must be atomic. 

**how we tackle this with 2PC**
- split the entire flow in 2 parts => prepare(reserve) and commit(book)
- so we reserve the food and agent first. this is kinda acquiring a lock on them so that no one else can access/acquire it. after this, we make a booking for food and the agent this is like assign the food and agent to this order_id. now once both reserving and booking have been completed, the order is placed and save the order in the orders_db and notify the user that their order has been failed.

**edge cases:**
**1. reserve phase -**
- if reserving both food and agent fails, we abort. 
- if only one succeeds, we cancel the reservation and abort or the timer will auto cancel (say food got reserved **for x time** but agent couldn't get reserved then we cancel the food reservation or a configured timer will auto cancel the food reservation if the delivery agent was not reserved)
- if both succeed, we move to commit phase

**2. Commit Phase -**
- if both succeed, the order is placed
- if any one fails, we cancel the reservation and abort
- if order_service itself fails, a configured timer cancels the reservation

**advantages**
- guarantees Atomic traxn and isolation

**disadvantages**
- slow and prone to deadlocks (separate reserve and commit phases)