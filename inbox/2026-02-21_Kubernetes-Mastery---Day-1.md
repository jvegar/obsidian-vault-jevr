---
date: 2026-02-21
tags:
  - #containers
hubs:
  - "[[kubernetes]]"
urls:
  -
---
# 2026-02-21_Kubernetes-Mastery---Day-1

# Key concepts

## Linux namespaces
They are a Kernel feature that provides **process-level** isolation, allowing containers to have their own distinct view of systems resources. By restricting what a process can see and interact with (e.g. networking, users, filesystems), they enable secure, isolated environments.
### Key types of Linux Namespaces:
    - Mount (mnt): Isolates file system mount points.
    - Process ID (PID): Isolates process IDs, allowing independent PID allocation.
    - Network (net): Isolates network devices, stacks and ports.
    - User (user): Isolates user and group IDs.
    - Interprocess Communication (IPC): Isolates shared memory, semaphores, and message queues.
    - UTS (Unix Timesharing System): Isolates hostnames and domain names.
    - Control Group (cgroup): Isolates cgroup root directories.
    - Time: Isolates system clocks, added in 2020.

### Core functionality and benefits
    - Isolation and Security: Prevents processes in one namespace fro accessing resources in another, minimizing attack surfaces.
    - Containerization: Provides the foundation for technologies like Docker to virtualize system resources.
    - Resource Management: Facilitates dividing resources such as CPU, memory, and I/O efficiently.
    - Flexibility: Enables complex networking setups, such as using `ip netns` to create separate network stacks.

### Common Tools for Managing Namespaces
    - lsns: Lists information about all currently accessible namespaces.
    - unshare: Runs a program with some namespaces unshared from the parent.
    - nsenter: Enters the namespaces of one or more other processes.
    - ip netns: Manages network namespaces.



# Control Groups (cgroups)
Control groups are a Linux kernel feature that organizes processes into hierarchical groups to limit, prioritize, and monitor system resources like CPU, memory, disk I/O, and network bandwidth. They prevent single application from consuming all resources, ensuring system stability. They are foundational to modern containerization (Docker/Podman) and systemd services.
