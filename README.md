# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. The main difference between `RwLock` and `Mutex` is that `RwLock` allows multiple readers at a time, but only one writer, whereas `Mutex` only allows one thread at a time, either read or write. In this case of notification system, `RwLock` is necessary because many observers will need to read the list of notifications simultaneously. `RwLock` allows multiple threads to read the data concurrently without blocking each other, which increases performance. If `Mutex` was used instead, only one observer can have access to the data at a time (whether read or write). This would significantly decrease the performance in read-heavy scenarios (where there are many readers) like in this case.
2. In Rust, `static` variables are immutable by default, and mutation requires explicit handling for safety reasons. If you declare a static variable without `lazy_static`, the value must be determinable at compile-time. In our case, `static ref NOTIFICATIONS: RwLock<Vec<Notification>> = RwLock::new(vec![]);`, this will not work without `lazy_static` because this involves creating a new `Vec<Notification>` object, which requires runtime evaluation (and cannot be determined at compile-time). `lazy_static` allows you to define a static variable that is initialized lazily at runtime.  

   Unlike in Java, Rust's design philosophy around static variables is deeply rooted in its principles of safety, concurrency, and default immutability. Rust's philosophy states that variables should only be mutable when explicitly requested to prevent unintended side effects.  

   Java, on the other hand, allows direct mutation of objects because they use a different memory model to manage concurrency. However, this approach may lead to subtle bugs and performance issues if not carefully managed.

#### Reflection Subscriber-2
1. Yes, I have. For example, `src/lib.rs` contains the configurations and core utilities for a Rocket-based application. It includes things like global variables, app configuration management, error handling, and serialization/deserialization. `main.rs` is the entry point of the application. It indicates that it brings Rocket's procedural macros, enables global features without needing to import them in every module, declaring four modules (`controller`, `service`, `repository`, and `model`), loads environment variables, and provides an initialization function. `Cargo.toml` lists the dependencies and the versions to use (for automatically installing) and the package name information and version.
2. The Observer pattern makes it easier to plug in more subscribers because it decouples the subject from its observers (subscribers). Observers can register and unregister with the subject at runtime without modifying the subject's code, adhering to the Open-Closed Principle. This make it easier to add new subscribers without changing the core logic. Additionally, the subscribers and observers interact through a common interface, meaning that the subject doesn't need to know the concrete implementation of the observers, allowing adding new types of subscribers easily.  

   As in for spawning more than one instance of the Main app, it will be hard to add new subscribers, as in this case, the subscriber list (repository) is stored in the app itsel, meaning that adding new subscribers would lead to inconsistencies or redundancy. To fix this, a central observer list must be used and shared across all the instances of the Main app.
3. No, I haven't tried them.
