# Alves Lab – Jetson Nano Environment Setup Guide

![Tux svg](https://github.com/user-attachments/assets/7f2b103f-fa43-4f78-ba2c-d38969a5a187)

This guide walks you through preparing a Jetson Nano for camera-based data collection and remote uploads.

---

## 🔧 1. System Update & Python Setup

### Update system packages:
```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install curl
```

### Install pip for Python 3
```bash
sudo apt-get install python3-pip
sudo python3 -m pip install --upgrade pip
```

### 🐍 2. Install Required Python Libraries

```bash
python3 -m pip install PyDrive2 opencv-python numpy
```

### 📷 3. Install Intel RealSense SDK

```bash
git clone https://github.com/JetsonHacksNano/installLibrealsense
cd installLibrealsense
./installLibrealsense.sh
```

### 📁 4. Download Camera Code Repositories

```bash
git clone https://github.com/plf-uga/capture_IPCAM.git
git clone https://github.com/plf-uga/rs_capture.git
git clone https://github.com/plf-uga/upload_py.git
git clone https://github.com/plf-uga/get_thermal_data.git
```

### 🔓 5. Make Scripts Executable (Examples)

```bash
cd capture_rs
chmod +x run.sh
cd ../upload_py
chmod +x upload.py down_token.sh
```

### ⚙️ 6. Compile the RealSense Capture Program

```bash
cd rs_capture
g++ -std=c++11 rs_capture.cpp -lrealsense2 -o rs-shot.exe
```

### 🌐 7. Remote Access via Dataplicity

Create a Gmail account for the Jetson Nano.

Go to: https://www.dataplicity.com/

Enter the email and follow the on-screen instructions to link the device.

### ⏰ 8. Schedule Tasks via crontab

```bash
crontab -e
```



```bash
SHELL=/bin/bash
PATH=/bin:/sbin:/usr/bin:/usr/sbin
# Daily captures
45 02 * * * /home/alveslab/capture_cam1/run.sh
46 02 * * * /home/alveslab/capture_cam2/run.sh
45 13 * * * /home/alveslab/capture_cam1/run.sh
46 13 * * * /home/alveslab/capture_cam2/run.sh

# Token refresh and upload
45 23 * * * /home/alveslab/upload_py/down_token.sh
05 00 * * * /usr/bin/python3.6 /home/alveslab/upload_py/upload.py >> /home/alveslab/upload.log 2>&1
```

### 📦 9. Special Instructions for Sub-Versions (16GB Storage)

Some Jetson Nano units have a pre-installed OS and limited space. Follow these extra steps to enable external storage.

### 9.1 Add a New User

```bash
sudo adduser <username>
sudo usermod -aG sudo <username>
```

### 9.2 Permanently Mount an SD Card

a. Install nano (if not present):
```bash
sudo apt-get install nano
```

b. Identify your SD card:
```bash
lsblk
```
Look for something like mmcblk0p1 or sdb1.

c. Create a mount directory:

```bash
sudo mkdir -p /mnt/external
```

d. Edit /etc/fstab:  
```bash
sudo nano /etc/fstab
```

Add one of the following lines based on the file system:

* ext4 format:

```bash
/dev/mmcblk0p1  /mnt/external  ext4  defaults,exec  0  0
```

* FAT32 format:

```bash
/dev/sdb1  /mnt/external  vfat  defaults,exec,uid=1000,gid=1000  0  0
```

* exFAT format:

```bash
/dev/sdb1  /mnt/external  exfat  defaults,exec,uid=1000,gid=1000  0  0
```

🔍 You can verify the device and UUID with:

```bash

lsblk -f
sudo blkid

```
e. Mount the drive:

```bash
sudo mount -a
```

f. Give executable permissions if needed:

```bash
sudo chmod -R +x /mnt/external
```

g. Verify the mount:

```bash
mount | grep external
# or
findmnt /mnt/external
```

