

---

## 📦 MTK Fastboot & ADB Tool (with FRP + Bulk Flash)

A command-line utility for MediaTek-based Android devices that simplifies common Fastboot and ADB operations — including flashing partitions, unlocking bootloaders, erasing FRP, and installing APKs. Designed for power users and ROM developers working with devices like the Infinix X6731.

---

### ⚙️ Features

- 🔧 Fastboot operations: flash, boot, erase, format, reboot, unlock
- 📱 ADB operations: install APKs, push/pull files, shell access, reboot modes
- 🔓 FRP bypass via Fastboot
- 🚀 One-click bulk flashing of multiple partitions
- ✅ Auto-detects `fastboot` and `adb` from environment, local folder, or system PATH

---

### 🖥️ Requirements

- Python 3.6+
- `fastboot` and `adb` binaries (from Android SDK Platform Tools)
- Windows or Linux (tested on both)

---

### 📁 Setup

1. Place `fastboot` and `adb` in the same folder as this script, or ensure they’re in your system PATH.
2. Ensure all `.img` files for flashing are in the same directory or provide full paths when prompted.
3. Run the script:
   ```bash
   python mtk_tool.py
   ```

---

### 📋 Menu Overview

```
FASTBOOT COMMANDS:
 1) List fastboot devices
 2) Flash partition
 3) Boot image
 4) Erase partition
 5) Format partition
 6) Reboot device (fastboot)
 7) Get device info
 8) Unlock bootloader
 9) Erase FRP

ADB COMMANDS:
10) List adb devices
11) Install APK
12) Shell into the device
13) Push file to device
14) Pull file from device
15) Reboot to bootloader (adb)
16) Reboot to recovery (adb)

17) Flash full image set

 0) Exit
```

---

### 🚀 Flash Full Image Set (Option 17)


This option flashes all critical partitions in one go. The following files must be present:

```
boot.img, cam_vpu1.img, cam_vpu2.img, cam_vpu3.img,
dpm.img, dtbo.img, gz.img, lk.img, mcupm.img,
md1img.img, pi_img.img, preloader.img, scp.img,
spmfw.img, sspm.img, super.img
```
Extract from fastboot rom only for MTK devices. 
If any file is missing, it will be skipped and reported. Flashing stops if any partition fails.

---

### ⚠️ Warnings

- Unlocking the bootloader will wipe all data.
- Erasing FRP will remove Google account protection.
- Always verify your `.img` files before flashing to avoid bricking your device.
- don`t flash modify "img' like lk and others 
---

### 🧠 Tips

- You can customize the bulk flash list by editing the `FLASH_ALL` variable in the script.
- Add dry-run or logging features if you want more control or traceability.
-  Make sure you’ve unlocked the bootloader before flashing `super.img`.

---


