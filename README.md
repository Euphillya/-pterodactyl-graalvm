# Pterodactyl GraalVM Docker Images

![Build GraalVM](https://github.com/Euphillya/pterodactyl-graalvm/actions/workflows/docker-image.yml/badge.svg)
![License MIT](https://img.shields.io/badge/license-MIT-green)

This repository provides **ready-to-use GraalVM Docker images for Pterodactyl Panel**, built for performance-oriented Java workloads such as **Minecraft (Paper / Folia)** and other JVM applications.

GraalVM is a high-performance runtime that offers improved startup time, lower memory usage, and better throughput compared to traditional JVMs.  
More information: https://www.graalvm.org/

---

## ✨ Features

- ✅ Multiple **Java versions** (21 → 25)
- ✅ **NUMA-enabled images** for Folia
- ✅ Multi-architecture support (`amd64`, `arm64`)
- ✅ Optimized for **Pterodactyl Panel**
- ✅ Minimal and clean Ubuntu base
- ✅ Automatic builds via GitHub Actions

---

## 🐳 Available Docker Images

All images are published on **GitHub Container Registry**:

`ghcr.io/euphillya/pterodactyl-graalvm`

### 📦 Image Tags

| Java Version | Tag |
|-------------|-----|
| 21 | `ghcr.io/euphillya/pterodactyl-graalvm:21-JDK` |
| 21 (NUMA) | `ghcr.io/euphillya/pterodactyl-graalvm:21-NUMA-JDK` |
| 22 | `ghcr.io/euphillya/pterodactyl-graalvm:22-JDK` |
| 23 | `ghcr.io/euphillya/pterodactyl-graalvm:23-JDK` |
| 24 | `ghcr.io/euphillya/pterodactyl-graalvm:24-JDK` |
| 25 | `ghcr.io/euphillya/pterodactyl-graalvm:25-JDK` |
| 25 (NUMA) | `ghcr.io/euphillya/pterodactyl-graalvm:25-NUMA-JDK` |

---

## 🧠 Folia NUMA Scheduling (How to enable)

Folia introduced a new scheduler with Linux NUMA-aware scheduling support in this commit:  
https://github.com/PaperMC/Folia/commit/eee7128bc810195ad758ea5ace1b72c600896d3b

To enable NUMA scheduling, you must:

### 1) Use the `WORK_STEALING` scheduler
In Paper global config, select the new scheduler (the default is EDF).

### 2) Add the JVM startup flag
Add this JVM flag to your Pterodactyl **Startup Command / JVM arguments**:

`-DPaper.NumaScheduling=true`

### 3) Use the NUMA image (recommended)

Use one of the `*-NUMA-JDK` images, which include the required Linux packages:

* `numactl`
* `libnuma1`

Example:

`ghcr.io/euphillya/pterodactyl-graalvm:21-NUMA-JDK`

> Notes:
>
> * NUMA scheduling is **Linux-only**.
> * It is only useful on machines that actually have **NUMA nodes** (common on dual-socket servers).

## ⚙️ Pterodactyl Usage

In your **Pterodactyl Egg**, set the Docker image to one of the tags above, for example:

`ghcr.io/euphillya/pterodactyl-graalvm:21-NUMA-JDK`

Then add JVM flags in your startup (example):

`java -DPaper.NumaScheduling=true -Xms16G -Xmx16G -jar server.jar nogui`

The container:

* Runs as a non-root `container` user
* Uses `/home/container` as the working directory
* Relies on `entrypoint.sh` for startup (compatible with standard Java eggs)

---

## 🏗️ Base Image Details

* **OS**: Ubuntu 22.04 / 24.04 (depending on Java version)
* **Locale**: UTF-8 (`en_US.UTF-8`)
* **Java**: GraalVM Community Edition
* **Architectures**:

  * `linux/amd64`
  * `linux/arm64`

---

## 📜 License

This project is licensed under the **MIT License**.

> If you run Folia on NUMA hardware: prefer `WORK_STEALING` + `-DPaper.NumaScheduling=true` + `*-NUMA-JDK`.