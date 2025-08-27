📦 MTK Fastboot & ADB Tool (with FRP + Bulk Flash) 

A command-line utility for MediaTek-based Android devices that simplifies common Fastboot and ADB operations — including flashing partitions, unlocking bootloaders, erasing FRP, and installing APKs. Designed for power users and ROM developers working with devices like the Infinix X6731.

⚙️ Features 

 • 🔧 Fastboot operations: flash, boot, erase, format, reboot, unlock 
 • 📱 ADB operations: install APKs, push/pull files, shell access, reboot modes 
 • 🔓 FRP bypass via Fastboot 
 • 🚀 One-click bulk flashing of multiple partitions 
 • ✅ Auto-detects from environment, local folder, or system PATH

🖥️ Requirements 

 • Python 3.6+ https://www.python.org/ 
 • and binaries (from Android SDK Platform       Tools) 
 • Windows or Linux (tested on win11)

📁 Setup

   Place fastboot,adb and mtk fastboot tool  the same folder as this script(dist), 
 ensure they’re in your system PATH.
  
 if you confused or it not work jus copy pest all platforms tool binaries in dist 
   
  Ensure all fils for flashing are in the same directory or provide full paths when prompted.
  Run the script:python mtkflash.py
 
 Or 

 just download https://t.me/ghokielrom/5694


📋 Menu Overview

=========================================
   MTK Fastboot & ADB Tool (with FRP)
=========================================
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
12) Shell into device
13) Push file to device
14) Pull file from device
15) Reboot to bootloader (adb)
16) Reboot to recovery (adb)

17) Flash full image set

 0) Exit


🚀 Flash Full Image Set (Option 17) 
    
    This option flashes all critical partitions simultaneously. The following files must be present:

Put all .img in dist folder. or the same folder that you puted mtk fastboot tool.exe and fastboot and adb 

If any file is missing, it will be skipped and reported as such. Flashing stops if any partition fails.

⚠️ Warnings
 
  • Unlocking the bootloader will wipe all data.
  • Erasing FRP will remove Google account protection.
  • Always verify your files before flashing to avoid bricking your device.

🧠 Tips
 • You can customize the bulk flash list by editing the variable in the script. 
 • Add dry-run or logging features if you want more control or traceability.
 • make sure you’ve unlocked the bootloader before flashing
 
 
 
