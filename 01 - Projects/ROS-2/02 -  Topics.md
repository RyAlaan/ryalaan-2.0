---
resource: project
created: 2026-07-28
project: ROS-2
module: Topics
task: Write a publisher and subscriber
language: python cpp
framework: ROS 2 Jazzy
repository: github.com/ryalaan/ros-2-jazzy/
tags:
  - project
  - documentation
  - ROS2-Jazzy
  - python
  - "#topics"
---
# Write a simple publisher and subscriber

> **Objective**
> 
> 1. Create a publisher node `temperature_publisher` that publishes a `std_msgs/msg/Float32` on topic `/temperature` every 2 seconds, with a fake value that slowly drifts (e.g. random walk).  
> 2. Inspect it live: use `ros2 topic echo /temperature`, `ros2 topic hz /temperature`, and `ros2 topic info /temperature -v`.
> 3. Create a subscriber node `temperature_logger` that subscribes to `/temperature` and prints a warning if the value goes above some threshold.  
> 4. Run publisher and subscriber in separate terminals, confirm data flows, then kill the publisher and observe what the subscriber does (nothing crashes, it just stops receiving).

---

# 📌 Background

What is node

What is topic

What is pub/sub

---

# 🎯 Requirements

### Publisher Node

- [x] Membuat node bernama `temperature_publisher`.
- [x] Menggunakan message type `std_msgs/msg/Float32`.
- [x] Mempublikasikan data ke topic `/temperature`.
- [x] Mengirim data setiap 2 detik menggunakan timer.
- [x] Nilai temperatur berubah secara perlahan (random walk), bukan nilai acak yang berubah drastis.
### Topic Inspection

- [x] Memastikan topic `/temperature` berhasil dibuat.
- [x] Memverifikasi data menggunakan `ros2 topic echo /temperature`.
- [x] Memverifikasi frekuensi publish menggunakan `ros2 topic hz /temperature`.
- [x] Memeriksa informasi topic menggunakan `ros2 topic info /temperature -v`.

### Subscriber Node

- [x] Membuat node bernama `temperature_logger`.
- [x] Subscriber berhasil subscribe ke topic `/temperature`.
- [x] Menampilkan nilai temperatur yang diterima.
- [x] Memberikan peringatan ketika temperatur melebihi nilai threshold yang ditentukan.

### Integration Testing

- [x] Menjalankan publisher dan subscriber pada terminal yang berbeda.
- [x] Memastikan subscriber menerima seluruh data yang dipublikasikan.
- [x] Menghentikan publisher dan memastikan subscriber tetap berjalan tanpa error.
- [x] Mengonfirmasi bahwa subscriber hanya berhenti menerima data setelah publisher dihentikan.

---

# 🧩 Problem

- Colcon build tidak mendeteksi package my_robot_basic_py

```bash
$ colcon build --packages-select my_robot_basic_py [0.232s] WARNING:colcon.colcon_core.package_selection:ignoring unknown package 'my_robot_basic_py' in --packages-select

Summary: 0 packages finished [0.22s]
```

- Colcon build berhasil tapi tidak berhasil menjalankan node heartbeat_node

```bash
$ ros2 run my_robot_basic_py heartbeat_node
No executable found
```

---

# 🔍 Investigation

## Problem 1

### Hipotesis

- ROS Jazzy belum di source
- Typo pada command
- Kesalahan tempat pada saat menjalankan command
### Yang Sudah Dicoba

| Percobaan                                          | Hasil      |
| -------------------------------------------------- | ---------- |
| Mengecek apakah ros sudah di source                | Sudah      |
| Mengecek nama package                              | Benar      |
| Melakukan colcon build di root workspace directory | ✅ Berhasil |

## Problem 2

### Hipotesis

- install/setup.bash pada root workspace belum di source
- Node belum didaftarkan sebagai executable

### Yang Sudah Dicoba

| Percobaan                                            | Hasil                       |
| ---------------------------------------------------- | --------------------------- |
| Sourcing install/setup.bash                          | Sudah                       |
| Mengecek setup.py di dalam package my_robot_basic_py | ✖️ Entry point masih kosong |
 
---

# 🏗 Solution

Kedua masalah tersebut terjadi karena entry point pada setup.py masih kosong.

```python
entry_points={
    'console_scripts': [
        'heartbeat_node = my_robot_basic_py.heartbeat_node:main',
    ],
},
```

Format console_scripts :  `'nama_command = nama_package.nama_file:nama_fungsi'`.

Kemudian jalankan colcon pada root workspace (ros2_ws)

```bash
$ colcon build
```

Perintah tersebut akan memperbarui folder `build`, `install`, dan `log` yang ada pada root workspace. Lalu jangan lupa untuk sourcing `install/setup.bash` sebagai Overlay.

---

# 💻 Code

### Preparation

```bash
cd src/my_robot_basic_py/my_robot_basic_py # navigate to our targeted packages

touch temperature.py # create new file called heartbeat_node.py
```

### Publisher Node

inside temperature.py, write : 

```python
import rlcpy
import std_msgs

from rlcpy.node import Node
from std_msgs.msg import Float32

class TemperaturePublisher(Node) :
    def __init__(self):
        super().__init__('temperature_publisher')
        self.temperature = 25.0
        self.publisher = self.create_publisher(Float32, "/temperature", 10)
        self.timer = self.create_timer(2, self.timer_callback)

    def timer_callback(self) -> None:
        self.temperature += random.uniform(-2.0, 2.0)
        
        msg = Float32(data=self.temperature)
        
        self.publisher.publish(msg)
        self.get_logger().info(f"Temperature: {self.temperature}°C")


def pubFunc(args=None):
    rclpy.init(args=args)
    tempPublisher = TemperaturePublisher
    rclpy.spin(tempPublisher)
    node.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

in terminal 1, run this command

```bash
source /opt/ros/jazzy/setup.bash

cd ros2_ws

colcon build --packages-select my_robot_basic_py

source install/setup.bash

ros2 run my_robot_basic_py temperature_publisher
```

### Topic Inspection

open other terminal and run this commands

```bash
source /opt/ros/jazzy/setup.bash

cd ros2_ws

source install/setup.bash

$ ros2 topic echo /temperature
	data: 28.47776985168457
---

$ ros2 topic hz /temperature
average rate: 0.500
	min: 1.999s max: 2.000s std dev: 0.00044s window: 2

$ ros2 topic info /temperature
Type: std_msgs/msg/Float32
Publisher count: 1
Subscription count: 1
```

### Subscriber Node 

Inside the same temperature.py file, create node called temperature_subscriber

```python
class TemperatureSubscriber(Node) :
    def __init__(self) -> None:
        super().__init__("temperature_subscriber")
        self.subcsription = self.create_subscription(Float32, '/temperature', self.temperature_subscriber_callback, 10)
        # self.subcsription

    def temperature_subscriber_callback(self, msg: Float32) -> None:
        if msg.data > 26.0 :
            self.get_logger().warning(f"Temperature: {msg.data} °C")
        else :
            self.get_logger().info(f"Temperature: {msg.data} °C")

def subFunc(args=None) -> None:
    rclpy.init(args=args)
    tempSubscriber = TemperatureSubscriber()
    rclpy.spin(tempSubscriber)
    rclpy.shutdown()
```

while terminal 1 running, open a new terminal and run temperature_subscriber

```bash
source /opt/ros/jazzy/setup.bash

cd ros2_ws

colcon build --packages-select my_robot_basic_py

source install/setup.bash

ros2 run my_robot_basic_py temperature_publisher
```

kill terminal 1 process, and let terminal 3 (temperature subscriber) alive. 

---

# 🧠 Why It Works



---

# 📚 References

- 

---

# 📌 Takeaways 

- 