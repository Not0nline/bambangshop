# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [X] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X] Commit: `Create Subscriber model struct.`
    -   [X] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [X] Commit: `Implement add function in Subscriber repository.`
    -   [X] Commit: `Implement list_all function in Subscriber repository.`
    -   [X] Commit: `Implement delete function in Subscriber repository.`
    -   [X] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
    -   [X] Commit: `Implement unsubscribe function in Notification service.`
    -   [X] Commit: `Implement unsubscribe function in Notification controller.`
    -   [X] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [X] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [X] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [X] Commit: `Implement publish function in Program service and Program controller.`
    -   [X] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [X] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough? <br>

-  In the Observer pattern, interfaces or traits are typically used when there are multiple types of subscribers, each with its own behavior. However, in the BambangShop scenario, if all subscribers are treated uniformly, an interface or trait is not essential.


2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case? <br>

- When the identifiers for products and subscriber URLs are guaranteed to be unique, a DashMap (map/dictionary) is more suitable than a Vec (list). This is because DashMap leverages these unique identifiers as keys, which can expedite lookup, insertion, and deletion operations with an average time complexity of O(1). In contrast, using a Vec (list) would be slower due to its average time complexity of O(n).

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead? <br>

- In Rust, both DashMap and HashMap are used to store key-value pairs. The difference lies in DashMap’s thread safety feature, which allows multiple threads to access and modify it concurrently without causing data races. For this case, both DashMap and the Singleton pattern are needed as they complement each other. The Singleton pattern ensures that a class has only one instance, which is already implemented using lazy_static. Despite the Singleton implementation, the SUBSCRIBERS data will be accessed and modified by multiple threads simultaneously, necessitating a thread-safe data structure like DashMap.




#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model? <br>

- The design principle of ‘Separation of Concerns’ suggests that different aspects of a software system should be segregated based on their focus areas. This allows for isolated changes when modifications are needed. The “Repository” is dedicated to data storage and retrieval, while the “Service” handles the business logic. This separation helps in isolating data access from business rules, making the system more maintainable.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model? <br>

- If we were to use only the model to handle both data storage and business logic, it could lead to high coupling. This means each model would need to be aware of the others, and changes in one model could necessitate changes in another. Consequently, the code becomes more complex and harder to maintain.


3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects. <br>

- Postman is a tool used for testing APIs. One of the most useful features is the HTTP Request functionality, which allows you to customize the type of request (GET, POST, DELETE, etc.) and share a collection of requests simply by copying and pasting text. It’s particularly helpful for verifying that an application returns the expected response based on the requests made. Additionally, the ability to adjust methods like CRUD operations enables you to verify the accuracy of the data retrieved through Postman.



#### Reflection Publisher-3

1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?
<br>

- In this tutorial, we utilize the Push model of the Observer pattern. This is evident in the algorithm where the notification service calls a method to iterate through all subscribers to provide them with the latest updates whenever an event occurs, such as create, delete, or update actions on the object module.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)
<br>

- If we were to use the Pull model instead, each subscriber would need to determine whether the data changes are relevant to them. The advantage of this approach is that observers have the freedom to decide what data they want to retrieve and when to do so. However, the downside is that observers need to understand the structure of the data source to make these decisions, which could lead to increased complexity.


3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.
<br>

- If we decide against using multi-threading in the notification process, the notification service would face a long queue when notifying each subscriber. This could lead to a bottleneck in computational capacity, causing delays in the delivery of notifications to each subscriber and potentially degrading the user experience due to the slow notification process.
