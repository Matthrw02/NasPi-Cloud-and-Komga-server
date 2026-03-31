# 🚀 NasPi — Self-Hosted Manga + Cloud Server on Raspberry Pi

## 🔹 Overview

**NasPi** is a self-hosted media and cloud server built on a Raspberry Pi 4 using Docker. It integrates Komga for manga management, Nextcloud for file storage, and Tailscale for secure remote access.

The system is enhanced with monitoring using Prometheus and Grafana, and container management through Portainer, creating a complete homelab environment.

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
 ├── Grafana (Visualization)
 ├── Prometheus (Metrics Collection)
 ├── Portainer (Container Management)
 └── External SSD (NAS Storage)
```

---

## ⚙️ Tech Stack

* **Hardware:** Raspberry Pi 4 (4GB RAM)
* **OS:** Raspberry Pi OS Lite (64-bit)
* **Containerization:** Docker & Docker Compose
* **Applications:**

  * Komga (media server)
  * Nextcloud (cloud storage)
  * Grafana (monitoring dashboard)
  * Prometheus (metrics collection)
  * Portainer (Docker management UI)
* **Networking:** Tailscale (secure VPN access)
* **Storage:** External SSD mounted at `/mnt/storage`
* **File Sharing:** Samba (Windows file sharing)

---

## 🚀 Features

* 📚 Self-hosted manga server accessible via web and mobile apps (Tachimanga)
* ☁️ Private cloud storage with Nextcloud
* 🔒 Secure remote access using Tailscale (no port forwarding required)
* 📊 Real-time system monitoring with Grafana dashboards
* 🧠 Metrics collection with Prometheus
* 🖥️ Container management with Portainer
* 📱 Cross-device access (phone, laptop, tablet)
* 📂 File upload and synchronization via Nextcloud

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

### 4. Deploy Core Services (Docker Compose)

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

### 5. Monitoring Stack (Prometheus + Grafana)

```yaml
services:
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"

  node-exporter:
    image: prom/node-exporter
    ports:
      - "9100:9100"

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
```

Access:

* Grafana → `http://<pi-ip>:3000`
* Prometheus → `http://<pi-ip>:9090`

Dashboard:

* Imported Node Exporter dashboard (ID: 1860)

---

### 6. Container Management (Portainer)

```bash
docker run -d \
  -p 9000:9000 \
  --name=portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce
```

---

### 7. Secure Remote Access (Tailscale)

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

Access services remotely:

* Komga → `http://<tailscale-ip>:25600`
* Nextcloud → `http://<tailscale-ip>:8080`
* Grafana → `http://<tailscale-ip>:3000`

---

### 8. Nextcloud Trusted Domains Fix

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

* Integrated Komga with Tachimanga for remote manga reading
* Enabled secure remote access via Tailscale VPN
* Configured Nextcloud for file uploads and synchronization

---

## ⚠️ Challenges & Solutions

* **PDF compatibility issues in Tachimanga**
  → Converted files to `.cbz` format

* **Docker daemon not running**
  → Reinstalled Docker and enabled the service

* **Permission issues with mounted storage**
  → Fixed using `chown`

* **Nextcloud "untrusted domain" error**
  → Added IPs to trusted domains using `occ`

* **Container access and visibility challenges**
  → Implemented Portainer for management

---

## 📸 Screenshots

*(Add your screenshots here)*

* Komga UI
* Tachimanga mobile view
* Nextcloud dashboard
* Grafana dashboard
* Portainer interface

---

## 💼 Key Learnings

* Linux server deployment and troubleshooting
* Docker container orchestration and management
* VPN networking with Tailscale
* Monitoring and observability using Prometheus and Grafana
* File system and storage management
* Real-world troubleshooting and system debugging

---

## 🔮 Future Improvements

* Add HTTPS with reverse proxy (NGINX)
* Implement automated backups
* Add alerting (CPU/disk thresholds)
* Upgrade to more powerful hardware (mini PC / Proxmox)

---

## 📌 Summary

**NasPi** is a lightweight yet powerful self-hosted infrastructure project demonstrating real-world skills in system administration, networking, and containerized application deployment.

---
