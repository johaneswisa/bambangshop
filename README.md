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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
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
1. Tergantung pada skala dan kompleksitas, dalam kasus bambangshop, karena hanya ada satu class subscriber, maka satu model struct sudah cukup. 

2. Karena id dan url unik, maka penggunaan DashMap menjadi penting karena kemampuannya dalam mengelola data dengan kunci identifikasi yang unik secara efisien, serta memastikan keunikan data tanpa duplikat. 

3. Penggunaan DashMap tidak wajib dan dapat digantikan oleh implementasi Pola Singleton. Namun, DashMap sangat membantu dengan menyediakan HashMap yang aman untuk akses konkuren. Hal ini bisa dicapai dengan menggunakan Pola Singleton, tapi lebih sulit untuk membuat implementasi yang sepenuhnya aman untuk penggunaan multi-thread dan berpotensi menciptakan masalah baru dalam program.
#### Reflection Publisher-2
1. Dalam pola Model-View-Controller (MVC), Model mencakup penyimpanan data dan logika bisnis. Namun, pemisahan "Service" dan "Repository" dari Model diperlukan untuk memisahkan tanggung jawab dan mempromosikan prinsip isolasi dan kohesi dalam desain perangkat lunak. "Service" bertanggung jawab untuk logika aplikasi tingkat tinggi dan memfasilitasi interaksi antara Model dan Controller, sementara "Repository" bertanggung jawab untuk interaksi dengan sumber data, memungkinkan Model untuk tetap fokus pada representasi logis data. Dengan memisahkan "Service" dan "Repository" dari Model, kita dapat mencapai desain yang lebih modular, mudah diuji, dan lebih mudah dipelihara.

2. Yang terjadi apabila kita hanya menggunakan Model adalah code-nya akan menjadi lebih kompleks. Code-nya tidak mengimplementasikan prinsip SRP dan SoC dan tidak adanya pemisahan service dan repository membuat setiap model memiliki coupling yang tinggi satu sama lain dan menurunkan software maintainability.

3. Postman dapat membantu saya dengan menyediakan lingkungan yang terpisah untuk menguji API dan mengelola koleksi requests. Fitur yang akan berguna bagi saya adalah fitur organisasi requests dan variables API, menulis test scripts, API security testing, dll. 
#### Reflection Publisher-3
1. Kita menggunakan push model yang mana publisher mem-push data ke subscribers. Push tersebut terjadi setiap suatu pembaruan (CRUD Product) terjadi. 

2. Jika kita menggunakan model pull, maka subscribers hanya akan mendapatkan notification saat mereka melakukan request baru secara manual. 

Advantages: 

Kontrol yang lebih baik: Pelanggan memiliki kontrol penuh atas kapan dan bagaimana mereka mengambil data dari publisher.

Efisiensi: Pelanggan hanya mengambil data ketika dibutuhkan, mengurangi penggunaan sumber daya.

Disadvantages:

Kompleksitas tambahan: Implementasi model Pull bisa menjadi lebih rumit karena pelanggan harus mengelola proses penarikan data secara aktif.

Responsivitas rendah: Dalam model Pull, pelanggan harus secara teratur memeriksa publisher untuk pembaruan, yang dapat mengakibatkan keterlambatan dalam respons terhadap perubahan data.

3. Jika kita memutuskan untuk tidak menggunakan multi-threading dalam proses pemberitahuan, program akan menjalankan proses pemberitahuan secara berurutan atau secara sekuensial. Hal ini berarti bahwa setiap notifikasi akan diproses satu per satu secara berturut-turut, yang dapat mengakibatkan penundaan dalam pengiriman notifikasi dan berpotensi memengaruhi kinerja aplikasi secara keseluruhan, terutama jika ada banyak notifikasi yang harus diproses dalam waktu singkat.