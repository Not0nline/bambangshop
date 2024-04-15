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
-   [v] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [v] Commit: `Create Subscriber model struct.`
    -   [v] Commit: `Create Notification model struct.`
    -   [v] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [v] Commit: `Implement add function in Subscriber repository.`
    -   [v] Commit: `Implement list_all function in Subscriber repository.`
    -   [v] Commit: `Implement delete function in Subscriber repository.`
    -   [v] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [v] Commit: `Create Notification service struct skeleton.`
    -   [v] Commit: `Implement subscribe function in Notification service.`
    -   [v] Commit: `Implement subscribe function in Notification controller.`
    -   [v] Commit: `Implement unsubscribe function in Notification service.`
    -   [v] Commit: `Implement unsubscribe function in Notification controller.`
    -   [v] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough? <br>
<br>

-  In the Observer pattern, interfaces or traits are typically used when there are multiple types of subscribers, each with its own behavior. However, in the BambangShop scenario, if all subscribers are treated uniformly, an interface or trait is not essential.


2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case? <br>
<br>

- When the identifiers for products and subscriber URLs are guaranteed to be unique, a DashMap (map/dictionary) is more suitable than a Vec (list). This is because DashMap leverages these unique identifiers as keys, which can expedite lookup, insertion, and deletion operations with an average time complexity of O(1). In contrast, using a Vec (list) would be slower due to its average time complexity of O(n).

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead? <br>
<br>

- In Rust, both DashMap and HashMap are used to store key-value pairs. The difference lies in DashMap’s thread safety feature, which allows multiple threads to access and modify it concurrently without causing data races. For this case, both DashMap and the Singleton pattern are needed as they complement each other. The Singleton pattern ensures that a class has only one instance, which is already implemented using lazy_static. Despite the Singleton implementation, the SUBSCRIBERS data will be accessed and modified by multiple threads simultaneously, necessitating a thread-safe data structure like DashMap.




#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model? <br>
<br>

- The design principle of ‘Separation of Concerns’ suggests that different aspects of a software system should be segregated based on their focus areas. This allows for isolated changes when modifications are needed. The “Repository” is dedicated to data storage and retrieval, while the “Service” handles the business logic. This separation helps in isolating data access from business rules, making the system more maintainable.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model? <br>
<br>

- If we were to use only the model to handle both data storage and business logic, it could lead to high coupling. This means each model would need to be aware of the others, and changes in one model could necessitate changes in another. Consequently, the code becomes more complex and harder to maintain.


3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects. <br>
<br>

- Postman is a tool used for testing APIs. One of the most useful features is the HTTP Request functionality, which allows you to customize the type of request (GET, POST, DELETE, etc.) and share a collection of requests simply by copying and pasting text. It’s particularly helpful for verifying that an application returns the expected response based on the requests made. Additionally, the ability to adjust methods like CRUD operations enables you to verify the accuracy of the data retrieved through Postman.



#### Reflection Publisher-3
