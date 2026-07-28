---
resource: project
created: 2026-07-27
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

- [ ] create a node `heartbeat_node` that print "alive" 
- [ ] Set print "alive" every 1 second using timer
- [ ] Rename node while runtime using `--ros-args -r __node:=...`
- [ ] Verifikasi dengan `ros2 node list`

---

# 💻 Code


```bash
ryalaan:
```

---

# 🧠 Why It Works



---

# 📚 References

- 

---

# 📌 Takeaways 

- 