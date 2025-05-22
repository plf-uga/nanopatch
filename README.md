# 🚀 Alves Lab – Jetson Nano Environment Setup Guide

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







