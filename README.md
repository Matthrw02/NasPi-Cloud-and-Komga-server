# 🚀 NasPi — Self-Hosted Manga + Cloud Server on Raspberry Pi

## 🔹 Overview

**NasPi** is a self-hosted media and cloud server built on a Raspberry Pi 4 using Docker. It combines Komga for manga management, Nextcloud for file storage, and Tailscale for secure remote access.

This project demonstrates hands-on experience with Linux systems, containerization, networking, and storage management in a real-world homelab environment.

---

## 🧠 Architecture

```text
Phone / Laptop
     ↓
 Tailscale VPN
     ↓
Raspberry Pi 4 (NasPi)
 ├── Komga (Manga Server)
 ├── Nextcloud (Cloud Storage)
 └── External SSD (NAS Storage)
```

---

## ⚙️ Tech Stack

* **Hardware:** Raspberry Pi 4 (4GB RAM)
* **OS:** Raspberry Pi OS Lite (64-bit)
* **Containerization:** Docker & Docker Compose
* **Applications:**

  * Komga (self-hosted manga server)
  * Nextcloud (self-hosted cloud platform)
* **Networking:** Tailscale (VPN for secure remote access)
* **Storage:** External SSD mounted at `/mnt/storage`
* **File Sharing:** Samba (optional)

---

## 🚀 Features

* 📚 Self-hosted manga library accessible via web and mobile (Tachimanga)
* ☁️ Private cloud storage with Nextcloud
* 🔒 Secure remote access using Tailscale VPN (no port forwarding required)
* 📱 Cross-device access (phone, laptop, tablet)
* 🧱 Organized and scalable storage structure
* ⚡ Lightweight and efficient setup on low-power hardware

---

## 🛠️ Setup Process

### 1. System Preparation

* Installed Raspberry Pi OS Lite (64-bit)
* Enabled SSH for headless setup
* Updated system packages

---

### 2. Storage Configuration

Mounted external SSD:

```bash
/mnt/storage
```

---

### 3. Install Docker

```bash
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
```

---

### 4. Deploy Services with Docker Compose

```yaml
services:
  komga:
    image: gotson/komga
    container_name: komga
    ports:
      - "25600:25600"
    volumes:
      - /mnt/storage/docker/komga-config:/config
      - /mnt/storage/komga:/data
    restart: unless-stopped

  nextcloud:
    image: nextcloud
    container_name: nextcloud
    ports:
      - "8080:80"
    volumes:
      - /mnt/storage/nextcloud:/var/www/html
    restart: unless-stopped
```

Start services:

```bash
docker compose up -d
```

---

### 5. Secure Remote Access (Tailscale)

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

Access services remotely:

* Komga → `http://<tailscale-ip>:25600`
* Nextcloud → `http://<tailscale-ip>:8080`

---

## 📁 File Structure

```bash
/mnt/storage/
  ├── komga/
  │     └── Manga/
  │           └── SeriesName/
  │                 └── Volume.cbz
  ├── nextcloud/
  └── docker/
```

---

## ⚠️ Challenges & Solutions

* **PDF compatibility issues in Tachimanga**
  → Converted files to `.cbz` format for proper rendering

* **Docker daemon not running**
  → Reinstalled Docker and enabled the service

* **File permission errors**
  → Fixed ownership using `chown`

* **Remote access limitations**
  → Implemented Tailscale VPN for secure connectivity

---

## 📸 Screenshots


* Komga interface
<img width="2555" height="1313" alt="Screenshot 2026-03-26 173501" src="https://github.com/user-attachments/assets/14ca1378-dddd-4e66-a317-066a60a351a3" />
* Tachimanga mobile access
<img width="827" height="1792" alt="IMG_8332" src="https://github.com/user-attachments/assets/c5513795-3195-44df-bf9e-4e12cc030770" />

* Nextcloud dashboard
<img width="2558" height="1307" alt="Screenshot 2026-03-26 180041" src="https://github.com/user-attachments/assets/93b0810a-0e99-407d-8f7b-9bf3e7d83247" />
* Docker containers running
<img width="1093" height="128" alt="Screenshot 2026-03-26 173721" src="https://github.com/user-attachments/assets/f598ab91-ce75-438a-a03a-dcc17f18d099" />

---

## 💼 Key Learnings

* Linux server deployment and management
* Docker containerization and orchestration
* Network configuration and VPN setup
* Storage mounting and file system organization
* Troubleshooting real-world infrastructure issues

---

## 🔮 Future Improvements

* Add HTTPS with reverse proxy (NGINX)
* Implement automated backups
* Migrate to higher-performance hardware (mini PC or server)
* Add monitoring tools (Grafana, Prometheus)

---

## 📌 Summary

**NasPi** is a lightweight, scalable self-hosted solution that demonstrates the ability to design, deploy, and manage a full-stack home server using modern tools and best practices.

---
