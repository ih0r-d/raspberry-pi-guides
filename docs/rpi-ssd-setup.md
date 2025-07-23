# Raspberry Pi 5 + SSD Setup Guide (Headless, Ubuntu Server)

This guide describes how to configure a Raspberry Pi 5 with an NVMe SSD for data and storage, using Ubuntu Server. Includes partitioning, formatting, auto-mounting, and optional Wi-Fi setup.

---

## 📦 Requirements

- Raspberry Pi 5 (tested on 16GB version)
- microSD card (16GB+)
- NVMe M.2 SSD + PCIe adapter (e.g. 256–512GB, Gen3 x4)
- USB keyboard, micro-HDMI cable
- Ethernet or Wi-Fi connection
- Ubuntu Server 25.10 for Raspberry Pi

---

## 🖥️ Flash Ubuntu Server

1. Download image: https://ubuntu.com/download/raspberry-pi
2. Flash to SD card using Raspberry Pi Imager or balenaEtcher
3. Insert SD, plug in HDMI, keyboard, power.

---

## 🌐 Connect to Internet

### Option A: Ethernet

- Plug in cable — DHCP works by default

### Option B: Wi-Fi (edit netplan before boot)

On the SD card (boot partition), edit/create:

```yaml
# /etc/netplan/50-cloud-init.yaml
network:
  version: 2
  wifis:
    wlan0:
      optional: true
      access-points:
        "YourSSID":
          password: "YourPassword"
      dhcp4: true
```

Then apply after boot:

```bash
sudo netplan apply
```

---

## 🔍 Detect SSD

```bash
lsblk
```

Should show `/dev/nvme0n1`

---

## 🛠️ Partition SSD

```bash
sudo fdisk /dev/nvme0n1
```

Inside `fdisk`:

```
g   # create GPT
n   # new partition
(Enter through defaults)
w   # write and exit
```

---

## 💽 Format SSD

```bash
sudo mkfs.ext4 /dev/nvme0n1p1
```

---

## 📂 Mount SSD

```bash
sudo mkdir -p /mnt/ssd
sudo mount /dev/nvme0n1p1 /mnt/ssd
df -h | grep /mnt/ssd
```

---

## 🔁 Auto-mount on boot (fstab)

1. Get UUID:

```bash
sudo blkid /dev/nvme0n1p1
```

2. Edit fstab:

```bash
sudo nano /etc/fstab
```

Add line:

```
UUID=xxxx-xxxx  /mnt/ssd  ext4  defaults,nofail  0  2
```

Replace with your real UUID.

3. Test:

```bash
sudo mount -a
```

---

## 📌 Optional: Enable Swap (Recommended)

```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

---

## ✅ Done

Your SSD is now auto-mounted and ready for projects, Docker, databases, etc.
