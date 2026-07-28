---
resource: article
title: ROS 2 Basic Concept
author:
  - Open Robotics
source: "ROS 2 Documentation: Jazzy"
url: https://docs.ros.org/en/jazzy/Concepts/Basic/
published:
created: 2026-07-23
tags:
  - resources/article
  - robotics
  - ROS2-Jazzy
---
# ROS 2 Basic Concept

# 📖 Summary

#### Interface

ROS 2 menyediakan tiga jenis interface komunikasi, yaitu **Message**, **Service**, dan **Action**. Ketiga interface tersebut ditulis menggunakan **Interface Definition Language (IDL)**. Saat package dibangun menggunakan `colcon`, ROS 2 secara otomatis menghasilkan kode dalam berbagai bahasa pemrograman seperti C++, Python, dan bahasa lain yang didukung

#### Message or Topic

**Message (.msg)** merupakan struktur data yang digunakan untuk mengirim informasi antar node melalui **Topic**. Sebuah node dapat mempublikasikan (_publish_) message, sedangkan node lain dapat menerima (_subscribe_) message tersebut. Biasanya, data yang dikirimkan adalah data yang berubah secara terus menerus seperti data sensor, data letak robot, etc

```c
int_32 sendInt 42 // fieldType1 fieldName1 defautlValue
string someString "pria sigma" // fieldType2 fieldName2 defaultValue
```

Klik [Field type](https://docs.ros.org/en/jazzy/Concepts/Basic/About-Interfaces.html#field-types) untuk melihat daftar tipe data.

#### Service

**Service (.srv)** merupakan suatu file yang memiliki dua bagian yaitu **request** dan **response**. Service cocok digunakan ketika dibutuhkan respons langsung terhadap suatu permintaan. Berikut adalah contoh sederhana dari service :

```python
# request constants
int8 FOO=1
int8 BAR=2
# request fields
int8 foobar
another_pkg/AnotherMessage msg
---
# response constants
uint32 SECRET=123456
# response fields
another_pkg/YetAnotherMessage val
CustomMessageDefinedInThisPackage value
uint32 an_integer
```

#### Action

Berbeda dengan Service, **Action (.act)** digunakan untuk pekerjaan yang membutuhkan waktu lebih lama. Selama server menjalankan Goal yang diberikan oleh client, server dapat mengirimkan **Feedback** secara berkala mengenai progres pekerjaan. Setelah pekerjaan selesai, server akan mengirimkan **Result** kepada client.. Misalnya :

``` python
float32 distance 2.5 # server (responder)
---
bool success # client (requester)
---
float32 current_distance ... # feedback
```

Selama client menunggu server, maka bagian action akan terus menerus dijalankan hingga server tercapai.

#### Nodes

![[Nodes-TopicandService.gif]]

Node adalah komponen pembentuk graph pada ROS 2. Node dapat berkomunikasi dengan node yang lain baik di dalam fungsi yang sama mau pun berbeda, juga di device yang berbeda (laptop - jetson - miniPC - RasPi - etc). Setiap satu node memiliki maksimal satu proses logika.

---

# 💡 Key Ideas

- **ROS 2 menggunakan graph architecture**, di mana setiap aplikasi dibangun dari kumpulan **node** yang saling berkomunikasi.

- **Node** adalah unit eksekusi yang menjalankan satu tanggung jawab atau logika tertentu (misalnya membaca sensor IMU, mengendalikan motor, atau memproses kamera).

- ROS 2 menyediakan tiga jenis interface komunikasi:
    - **Message (.msg)** → komunikasi satu arah melalui **Topic** (Publisher → Subscriber).
    - **Service (.srv)** → komunikasi **Request → Response** untuk operasi yang membutuhkan balasan langsung.
    - **Action (.action)** → komunikasi **Goal → Feedback → Result** untuk tugas yang memerlukan waktu penyelesaian.

- **Topic** cocok untuk data yang dikirim secara terus-menerus (_streaming_), seperti data sensor, posisi robot, atau kecepatan roda.

- **Service** cocok untuk operasi yang bersifat instan, seperti mengaktifkan motor, mereset encoder, atau mengubah parameter.

- **Action** cocok untuk tugas berdurasi panjang, seperti navigasi menuju tujuan, mengikuti lintasan, atau memindahkan lengan robot.

- Satu executable dapat berisi **satu atau lebih node**, sehingga node dapat berjalan **dalam proses yang sama (intra-process)** atau **proses yang berbeda (inter-process)**.

- ROS 2 menggunakan **DDS (Data Distribution Service)** sebagai middleware, sehingga node dapat berkomunikasi tanpa perlu mengetahui apakah node lain berada di proses atau mesin yang sama.

- Node dapat berjalan pada **komputer yang berbeda** (misalnya Laptop, Jetson, Raspberry Pi, atau Mini PC) selama berada pada jaringan ROS 2 yang sama.

- Setiap interface menghasilkan kode secara otomatis ketika package dibangun menggunakan **colcon**, sehingga dapat digunakan langsung dalam C++, Python, maupun bahasa lain yang didukung


---

# 🔗 Related Notes

- [[ROS 2 Environment]]