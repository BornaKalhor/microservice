# ftgo
### Project description 
Consumers use the FTGO website or mobile application to place food orders at local restaurants. FTGO coordinates a network of couriers who deliver the orders. It’s also responsible for paying couriers and restaurants. Restaurants use the FTGO website to edit their menus and manage orders. The application uses various web services, including Stripe for payments, Twilio for messaging, and Amazon Simple Email Service (SES) for email.
### The monolith architecture
The FTGO application has a hexagonal architecture. It consists of business logic surrounded by adapters that implement UIs and interface with external systems, such as mobile applications and cloud services for payments, messaging, and email.

<img src="smallmonolith_version.png">

In the past, when the application was relatively small, the application’s monolithic architecture had lots of benefits. Over time, though, development, testing, deployment, and scaling became much more difficult because FTGO application has grown over the years into a monstrous monolith.
The large FTGO developer team commits their changes to a single source code repository. The path from code commit to production is long and arduous and involves manual testing. The FTGO application is large, complex, unreliable, and difficult to maintain.
### The microservice architecture
The backend services include the following:
- Order Service : Manages orders
- Delivery Service : Manages delivery of orders from restaurants to consumers
- Restaurant Service : Maintains information about restaurants
- Kitchen Service : Manages the preparation of orders
- Accounting Service : Handles billing and payments

<img src="1.png">

This architecture consists of a set of loosely coupled services. Each team develops, tests, and deploys their services independently.

## Selected Components
We selected the `OrderController` class from the Order Service and `RestaurantController` from the Restaurant service as our chosen components to migrate to microservice architecture as this classes are not too trivial yet have the characteristics needed to become a service.
ther order controller service is supposed to manage the orders from the customers.
the restaurant controller service has the information about restaurants and their menus, prices, etc.

## Reusing codes for refactoring
For extracting each service from the monolith code this is a good stategy to follow below steps:

- Split the code and convert a service into a separate, loosely coupled module within the monolith
- Split the database and define a separate schema for that service.
- Define a standalone service
- Use the standalone service
- Remove the old and now unused the service functionality codes from the FTGO monolith

## Some Dependencies

> restaurant-service --> order-service:
where: ftgo-domain\src\main\java\net\chrisrichardson\ftgo\domain\Order.java,class: order
happened: E:\Fdevelop\univesity\10\Cloud\project\ftgo-monolith\ftgo-monolith\ftgo-domain\src\main\java\net\chrisrichardson\ftgo\domain\Restaurant.java, class: restaurant
code: getRestaurant()
reason: imported and used
solution: REST api

> restaurant-service --> order-service:
where: ftgo-monolith\ftgo-order-service\src\main\java\net\chrisrichardson\ftgo\orderservice\web\GetOrderResponse.java, class: GetOrderResponse
happened: E:\Fdevelop\univesity\10\Cloud\project\ftgo-monolith\ftgo-monolith\ftgo-domain\src\main\java\net\chrisrichardson\ftgo\domain\Restaurant.java, class: restaurant        code: getRestaurantName()
reason: imported and used
solution: REST api

> restaurant-service --> order-service:
where: ftgo-monolith\ftgo-order-service-api\src\main\java\net\chrisrichardson\ftgo\orderservice\api\events\OrderDetails.java class: OrderDetails
happened: E:\Fdevelop\univesity\10\Cloud\project\ftgo-monolith\ftgo-monolith\ftgo-domain\src\main\java\net\chrisrichardson\ftgo\domain\Restaurant.java, class: restaurant        code: getRestaurantName()
reason: imported and used
solution: REST api

> accounting-service --> consumer-service:
where: ftgo-monolith\ftgo-domain\src\main\java\net\chrisrichardson\ftgo\domain\Consumer.java class: Consumer
happened: ftgo-monolith\ftgo-common\src\main\java\net\chrisrichardson\ftgo\common\Money.java, class: Money        
code: import net.chrisrichardson.ftgo.common.Money
reason: imported and used
solution: REST api

> delivery-service --> order-service:
where: ftgo-monolith\ftgo-order-service\src\main\java\net\chrisrichardson\ftgo\orderservice\domain\OrderService.java class: orderService
happened: monolith\ftgo-domain\src\main\java\net\chrisrichardson\ftgo\domain\CourierRepository.java, class: CourierRepository
code:   private CourierRepository courierRepository;
reason: imported and used
solution: REST api



