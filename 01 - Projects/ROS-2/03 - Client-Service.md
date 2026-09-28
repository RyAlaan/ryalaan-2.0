---
resource: project
created: 2026-08-04
project: ROS-2
module: Client-Service
task: Implementating Client-Service in Real-World-Shaped Service
language: python
framework: ROS 2 Jazzy
repository: github.com/ryalaan/ros-2-jazzy/
tags:
  - project
  - documentation
  - ROS2-Jazzy
---
# Implementating Client-Service in Real-World-Shaped Service

> **Objective**
> 
>1. Design a service `SetLEDColor.srv` where the request has `string color` and the response has `bool success` and `string message`. Make the server reject invalid color names and return `success: false` with a reason.  
>2. Add basic validation logic in the server (e.g. reject empty strings, reject unknown colors) and test both success and failure paths from the CLI.
>3. Build a node that is _both_ a publisher and a service server: it publishes sensor data on a topic continuously, but also exposes a service like `ResetSensor` that resets an internal counter/state.  
>4. Build a node that is a subscriber _and_ a service client: it listens to a topic, and when a value crosses a threshold, it calls a service on another node (chain reaction between two node types).

---

# 📌 Background

Mengapa task ini perlu dikerjakan?

- ...
- ...

---

# 🎯 Requirements

- [ ]
- [ ]
- [ ]

---

# 🧩 Problem

Masalah yang ditemukan.

Contoh:

- API mengembalikan data kosong.
- ROS Node tidak menerima message.
- Query GORM terlalu lambat.

---

# 🔍 Investigation

Hal-hal yang sudah dicek.

## Hipotesis

- ...
- ...
- ...

## Yang Sudah Dicoba

| Percobaan | Hasil |
|-----------|-------|
| | |
| | |

---

# 🏗 Solution

Jelaskan solusi akhirnya.

## Langkah

1.
2.
3.

---

# 💻 Code

```bash
$ ros2 pkg create --build-type ament_python --license Apache-2.0 led_color

$ cd ./led_color && mkdir srv

$ cd ./srv && touch SetRobotMode.srv

$ cd ../led_color

$ touch robot_mode_service.py robot_mode_client.py
```

robot_mode_service.py :

```python
import rclpy

from rclpy.node import Node
from led_color.srv import SetRobotMode

class RobotModeService(Node):
    def __init__(self):
        super().__init__('robot_mode_service')
        self.subscription = self.create_subscription(
            SetRobotMode,
            '/robot_mode',
            self.handle_robot_mode,
            10
        )

    def handle_robot_mode(self, request, response):
        if request.mode in ['idle', 'active', 'charging']:
            response.success = True
            response.message = f"Mode set to {request.mode}."
        else:
            response.success = False
            response.message = f"Invalid mode: {request.mode}. Allowed: idle, active, charging."

        return response


def main(args=None):
    rclpy.init(args=args)
    robot_mode_service = RobotModeService()
    rclpy.spin(robot_mode_service)
    robot_mode_service.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()

```

robot_mode_client.py : 

```python

```

---

# ⚠️ Important Notes

- Source ros jazzy workspace dulu
- Masuk ke direktori src sebelum membuat package

---

# 🧠 Why It Works

Bukan hanya "apa" solusinya.

Tetapi mengapa solusi tersebut berhasil.

---

# 📖 Concepts Learned

Konsep baru yang dipelajari.

- ...
- ...
- ...

---

# 🐛 Bugs Encountered

| Bug | Penyebab | Solusi |
|------|----------|---------|
| | | |

---

# 🚀 Improvement

Kalau nanti dikerjakan lagi.

- ...
- ...
- ...

---

# 🔗 Related Files

```text
backend/
 ├── ...
 ├── ...
```

---

# 🔗 Related Notes

- [[...]]
- [[...]]

---

# 📚 References

-
-

---

# ✅ Result

Apa hasil akhirnya?

Contoh:

- API berhasil berjalan.
- Robot dapat subscribe topic.
- Build berhasil tanpa warning.

---

# 📌 Takeaways

Ringkasan yang ingin diingat.

- ...
- ...
- ...