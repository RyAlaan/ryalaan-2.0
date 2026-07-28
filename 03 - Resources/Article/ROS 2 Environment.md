---
resource: article
title: Configure ROS 2 Environment
author:
  - Open Robotics
source: "ROS 2 Documentation: Jazzy"
url: https://docs.ros.org/en/jazzy/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html
published:
created: 2026-07-23
tags:
  - resources/article
  - ROS2-Jazzy
  - robotics
---
# ROS 2 Environment

# 📖 Summary

ROS 2 memiliki suatu konsep yang dapat menggabungkan beberapa workspace dalam satu shell environment. Term > Workspace adalah direktori tempat kita menyimpan, membangun (_build_), dan mengembangkan package ROS 2.. Pada saat setup ROS 2, core ROS 2 akan berperan menjadi lapisan bawahnya (underlay). Ketika suatu workspace sudah di source, workspace tersebut akan menumpuk di atas core ROS 2 (overlay). 

Contoh penerapannya, misal kita memiliki workspace seperti berikut :

```bash
~/ros2_ws       # workspace ROS
~/arm_ws        # workspace robot arm
~/vision_ws     # workspace komputasi visual
```

Supaya ROS 2 dapat mengenali package yang ada dalam setiap workspace tersebut, kita harus menjalankan perintah source terlebih dahulu : 

```bash
source /opt/ros/jazzy/setup.bash # underlay atau fondasi utama
source ~/ros2_ws/install/setup.bash # overlay 1
source ~/robot_ws/install/setup.bash # overlay 2
source ~/vision_ws/install/setup.bash # overlay 3
```  

Perintah `source` digunakan untuk memperbarui shell environment sehingga ROS 2 mengetahui lokasi package, executable, library, launch file, message, dan service yang berada pada workspace tersebut. Dengan menjalankan perintah source, maka ROS 2 akan mencari package secara berurutan dimulai dari overlay paling baru atau paling atas. 

Tujuannya, beberapa workspace tersebut saling terintegrasi satu sama lain dan dapat digunakan secara bersamaan dalam satu environment. 

---

# 💡 Key Ideas

- **Underlay** = fondasi, biasanya instalasi ROS 2 di `/opt/ros/<distro>`.
- **Overlay** = workspace yang udah dibuat sendiri.
- underlay tersebut haruslah di-_source_ terlebih dahulu sebelum menimpa dengan overlay workspace selanjutnya
- Jika ada package dengan nama yang sama di overlay dan underlay, **versi di overlay yang akan diprioritaskan**. Ini memungkinkan kamu mengembangkan atau memodifikasi package tanpa mengubah instalasi ROS 2 yang asli.

---

# 🔗 Related Notes

- [[ROS 2 Basic Concept]]
