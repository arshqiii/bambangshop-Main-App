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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

    > Dari yang dijelaskan, subscriber dalam aplikasi BambangShop untuk saat ini hanya terdiri dari satu class, sehingga tidak perlu menambah interface/trait karena sudah cukup. Namun jika untuk kedepannya Bambangshop memiliki banyak jenis subscribers, maka alangkah baik untuk menggunakan interface karena akan bermanfaat dalam mengimplementasi subscriber yang berbeda sambil memastikan semuanya mematuhi aturan yang sama.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

    > Menurut saya penggunaan Dashmap pada implementasi tutorial ini lebih baik dibanding menggunakan Vec. Vec bisa digunakan asal data yang digunakan tidak terlalu besar namun untuk mendapat kinerja yang baik untuk manage data subscriber maka Dashmap lebih baik. Ini karena penggunaan Dashmap memiliki time complexity yang lebih baik dibanding Vec dengan O(1) dibanding O(n). Ini penting ketika nanti jumlah subscribers bertambah sehingga penggunaan Dashmap memungkinkan pencarian yang cepat dan dapat memastikan program tetap efisien.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

    > Dalam tutorial ini, singleton sudah diimplementasi melalui lazy_static, dan didalamnya digunakan Dashmap. Jadi dalam tutorial ini Dashmap dan Singleton di implementasi secara bersamaan. Singleton memiliki tujuan untuk memastikan bahwa hanya ada satu instance dari objek ketika program berjalan dan Dashmap sendiri merupakan sebuah versi dari Hashmap yang aman digunakan dalam kondisi multithreading. Jika Dashmap diganti dengan penerapan Singleton Hashmap maka tidak akan dipastikan keamanan dalam threading. Mempertahankan DashMap adalah pilihan desain yang lebih baik untuk sistem konkuren berkinerja tinggi. Jika keamanan dan kinerja thread tidak menjadi masalah, maka Singleton dengan HashMap normal sudah cukup.

#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

    > Pemisahan terhadap Service dan Repository dapat dibalikkan kepada OO principle SRP atau Single Responsibility Principle. Ini karena jika Model menangani akses data sekaligus logika bisnis, kode akan menjadi sulit dikelola dan diperluas. Dengan memisahkan Repository dan Service, setiap bagian memiliki tugas yang lebih spesifik. Repository berfokus pada interaksi dengan database, sedangkan Service menangani logika bisnis. Pemisahan ini membuat kode lebih modular, mudah diuji, dan dapat dikembangkan tanpa mengubah banyak bagian dalam sistem.


2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

    > Jika Model menangani semuanya tanpa pemisahan, kode akan semakin kompleks seiring bertambahnya fitur. Ketergantungan antar Model akan meningkat, sehingga perubahan kecil bisa berdampak besar ke seluruh sistem. Selain itu, kode yang berulang akan sulit dihindari karena setiap Model menangani logika bisnis sendiri tanpa adanya abstraksi yang jelas. Hal ini melanggar prinsip DRY, membuat proses refactoring lebih sulit, dan meningkatkan risiko kesalahan saat melakukan perubahan.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects

    > Sebelumnya saya pernah menggunakan Postman pada mata kuliah Pemrograman Berbasis Platform (PBP), Postman ini saya gunakan untuk test API endpoints secara efisien sehingga dapat menampilkan data dari endpoint tersebut dengan jelas. Fitur-fitur yang menurut saya menarik dan mungkin dapat membantu saya dalam proyek ini adalah Collections yang dapat membantu mengelompokkan request API yang berhubungan sehingga lebih mudah dikelola, dan juga fitur automated testing yang memungkinkan penambahan skrip pengujian otomatis untuk memastikan respons dari setiap endpoint sesuai dengan ekspektasi.

#### Reflection Publisher-3

1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

    > Dalam tutorial ini, Observer Pattern yang digunakan adalah Push Model, di mana publisher (sistem) secara aktif mengirimkan notifikasi ke subscribers setiap kali ada perubahan status produk. Hal ini terlihat dari implementasi pada NotificationService::notify(), di mana notifikasi dikirim secara langsung kepada subscribers melalui metode subscriber_clone.update(payload_clone).

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

    > Keuntungan dari memakai Pull Model dalam kasus ini dapat berupa mengurangi beban publisher karena tidak perlu langsung mengirimkan notifikasi ke setiap subscriber dan tiap subscriber dapat mengambil data sesuai dengan kebutuhan mereka. Namun kekurangannya terletak pada kasus dimana subscribers harus secara berkala mengambil (polling) data terbaru dari publisher untuk mengetahui perubahan status produk dan ini dapat mengurangi efisiensi karena subscribers harus terus-menerus mengecek perubahan, dibandingkan dengan Push Model yang langsung mengirim notifikasi begitu ada update.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

    > Tanpa menggunakan multithreading pada proses notifikasi, aplikasi akan mengalami kesulitan menangani kasus dimana jumlah subscribers banyak, karena semua update harus dikirim secara bergantian dalam satu thread. Dengan menggunakan multithreading (dengan thread::spawn()), dapat memungkinkan pengiriman notifikasi dilakukan secara paralel sehingga tidak menghambat eksekusi utama aplikasi dan meningkatkan kinerja sistem.
