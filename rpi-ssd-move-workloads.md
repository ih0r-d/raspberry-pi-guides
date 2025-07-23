# Raspberry Pi 5: Move Data & Workloads to SSD (Keep OS on SD Card)

This guide moves all important data, logs, and workloads to the SSD, while keeping the base OS on the SD card. Ideal setup for performance, SD-card longevity, and clarity.

---

## ✅ Assumptions

- Ubuntu Server is installed on the SD card
- SSD is mounted at `/mnt/ssd`
- You want to keep only system files on SD
- Everything else (data, projects, logs, docker, swap) goes to SSD

---

## 1️⃣ Create SSD Directory Structure

```bash
sudo mkdir -p /mnt/ssd/{projects,data,logs,tmp,venv,docker,swap}
sudo chown -R $USER:$USER /mnt/ssd
```

---

## 2️⃣ Symlink Useful Paths (Optional)

```bash
ln -s /mnt/ssd/projects ~/projects
ln -s /mnt/ssd/data ~/data
ln -s /mnt/ssd/logs ~/logs
ln -s /mnt/ssd/tmp ~/tmp
```

---

## 3️⃣ Move Docker to SSD

1. Stop Docker:

```bash
sudo systemctl stop docker
```

2. Move Docker directory:

```bash
sudo mv /var/lib/docker /mnt/ssd/docker
```

3. Create config:

```bash
echo '{ "data-root": "/mnt/ssd/docker" }' | sudo tee /etc/docker/daemon.json
```

4. Restart Docker:

```bash
sudo systemctl daemon-reexec
sudo systemctl start docker
```

---

## 4️⃣ Move /var/log to SSD

> You can use bind-mount or rsync + fstab.

### One-time move:

```bash
sudo rsync -aAXv /var/log/ /mnt/ssd/logs/
sudo mv /var/log /var/log.bak
sudo ln -s /mnt/ssd/logs /var/log
```

### (Optional) Or make it persistent with fstab:

```bash
sudo nano /etc/fstab
```

Add:

```
/mnt/ssd/logs  /var/log  none  bind  0  0
```

---

## 5️⃣ Setup Swap on SSD

```bash
sudo fallocate -l 2G /mnt/ssd/swap/swapfile
sudo chmod 600 /mnt/ssd/swap/swapfile
sudo mkswap /mnt/ssd/swap/swapfile
sudo swapon /mnt/ssd/swap/swapfile
echo '/mnt/ssd/swap/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

---

## ✅ Done

- All working data, logs, docker, swap → SSD  
- SD card only holds base Ubuntu system  
- You gain speed + reliability
