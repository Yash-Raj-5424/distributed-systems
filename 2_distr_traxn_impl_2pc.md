**designing schemas :**
-  for **agent** : id, is_reserved, order_id
- for **store service:**  food -> id, name and packets(individual food items in the store) -> id, food_id, is_reserved, order_id
**API endpoints**
- delivery_service => /delivery/agent/reserve and /delivery/agent/book (assign delivery agent to an order)
- store_service => /store/food/reserve and /store/food/book

**ARCHITECTURE** 
```
user places order using order_service => order_service talks to store_service and delivery_service (each of order_service and delivery_service have their own dbs)
```

**FLOW**

```
reserve the food => reserve the agent => assign the food to the order_id => assign the agent to the order_id
```

> all these four work should happen atomically. if any of these four fails, we notify the user that we couldn't place their order