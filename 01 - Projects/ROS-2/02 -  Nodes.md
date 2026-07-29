---
resource: project
created: 2026-07-28
project: ROS-2
module: Nodes
task: Write a minimal heartbeat node
language: python cpp
framework: ROS 2 Jazzy
repository: github.com/ryalaan/ros-2-jazzy/
tags:
  - project
  - documentation
  - ROS2-Jazzy
  - python
  - nodes
  - "#cpp"
status: todo
---
# Write a minimal heartbeat node

> **Objective**
> 
> 1. Write a minimal Python node called `heartbeat_node` that just prints "alive" once on startup, and run it with `ros2 run`.  
> 2. Modify it to print "alive" every 1 second using a timer instead of a one-off print.  
> 3. Rename the node at runtime using `ros2 run my_robot_basics heartbeat_node --ros-args -r __node:=heartbeat_node_2` and confirm with `ros2 node list`.

---

# 📌 Background

What is node

What is pub/sub

---

# 🎯 Requirements

- [x] create a node `heartbeat_node` that print "alive" 
- [x] Set print "alive" every 1 second using timer
- [ ] Rename node while runtime using `--ros-args -r __node:=...`
- [ ] Verifikasi dengan `ros2 node list`

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

```bash
cd src/my_robot_basic_py/my_robot_basic_py # navigate to our targeted packages

touch heartbeat_node.py # create new file called heartbeat_node.py
```

inside heartbeat_node.py, write : 

```python
import rclpy
from rclpy.node import Node

class HeartbeatNode(Node) :
    def __init__(self):
        super().__init__('heartbeat_node')
        self.timer = self.create_timer(1.0, self.timer_callback)

    def timer_callback(self):
        self.get_logger().info('alive')


def main(args=None):
    rclpy.init(args=args)
    node = HeartbeatNode()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

```bash
cd ros2_ws

colcon build --packages-select my_robot_basic_py

source install/setup.bash

ros2 run my_robot_basic_py heartbeat_node

ros2 run my_robot_basic_py heartbeat_node --ros-args -r __node:=heartbeat_node_2 

ros2 node list
```

---

# 🧠 Why It Works



---

# 📚 References

- 

---

# 📌 Takeaways 

- 