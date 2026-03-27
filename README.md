# 🚀 NasPi — Self-Hosted Manga + Cloud Server on Raspberry Pi

## 🔹 Overview

**NasPi** is a self-hosted media and cloud server built on a Raspberry Pi 4 using Docker. It combines Komga for manga management, Nextcloud for file storage, and Tailscale for secure remote access across devices.

This project demonstrates hands-on experience with Linux system administration, containerization, networking, and storage management in a real-world homelab environment.

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
* **Networking:** Tailscale (secure VPN access)
* **Storage:** External SSD mounted at `/mnt/storage`
* **File Sharing:** Samba (Windows file sharing)

---

## 🚀 Features

* 📚 Self-hosted manga server accessible via web and mobile apps (Tachimanga)
* ☁️ Private cloud storage with Nextcloud
* 🔒 Secure remote access using Tailscale (no port forwarding required)
* 📱 Cross-device access (phone, laptop, tablet)
* 📂 File upload and synchronization via Nextcloud
* 🧱 Organized storage system for scalability

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

### 5. Configure Remote Access (Tailscale)

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

Access services remotely:

* Komga → `http://<tailscale-ip>:25600`
* Nextcloud → `http://<tailscale-ip>:8080`

---

### 6. Fix Nextcloud Trusted Domains

To allow access via local and Tailscale IPs:

```bash
docker exec -u www-data nextcloud php occ config:system:set trusted_domains 1 --value=<local-ip>
docker exec -u www-data nextcloud php occ config:system:set trusted_domains 2 --value=<tailscale-ip>
docker restart nextcloud
```

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

## 📱 Mobile Integration

* Integrated Komga with **Tachimanga** for mobile reading
* Enabled remote access via Tailscale VPN
* Optimized media format to `.cbz` for compatibility

---

## ⚠️ Challenges & Solutions

* **PDF compatibility issues in Tachimanga**
  → Converted files to `.cbz` format

* **Docker daemon not running**
  → Reinstalled Docker and enabled service

* **Permission issues with mounted storage**
  → Fixed using `chown`

* **Nextcloud "untrusted domain" error**
  → Added IPs to trusted domains using `occ`

---

## 📸 Screenshots

*(Add your screenshots here)*

* Komga UI
<img width="2555" height="1313" alt="Screenshot 2026-03-26 173501" src="https://github.com/user-attachments/assets/f1f17a0e-f099-4559-a996-7f4b92d72479" />

* Tachimanga mobile view
<img width="827" height="1792" alt="IMG_8332" src="https://github.com/user-attachments/assets/ff21cbe3-55ee-489d-9ea8-41d13aecae75" />

* Nextcloud dashboard
<img width="2558" height="1307" alt="Screenshot 2026-03-26 180041" src="https://github.com/user-attachments/assets/b86d7394-1245-41b5-a02f-066fdf955c36" />

* Docker containers (`docker ps`)
<img width="1093" height="128" alt="Screenshot 2026-03-26 173721" src="https://github.com/user-attachments/assets/a6154434-6baa-4796-b1f4-dba006a6ef38" />


---

## 💼 Key Learnings

* Linux server deployment and troubleshooting
* Docker container management and orchestration
* VPN networking with Tailscale
* File system mounting and storage organization
* Cross-platform application integration

---

## 🔮 Future Improvements

* Add HTTPS with reverse proxy (NGINX)
* Automate backups
* Implement monitoring (Grafana/Prometheus)
* Upgrade hardware (mini PC / Proxmox server)

---

## 📌 Summary

**NasPi** is a lightweight, scalable self-hosted server solution demonstrating real-world infrastructure skills using modern tools and best practices.

---

---
