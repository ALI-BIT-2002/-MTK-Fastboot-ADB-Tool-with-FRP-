import subprocess
import sys
import os
from pathlib import Path
import shutil

def find_executable(name):
    # Check env var, local folder, then PATH
    env = os.environ.get(name.upper())
    if env and Path(env).exists():
        return str(Path(env))
    local = Path(__file__).parent / (f"{name}.exe" if os.name == "nt" else name)
    if local.exists():
        return str(local)
    which = shutil.which(name)
    return which

FASTBOOT = find_executable("fastboot")
ADB      = find_executable("adb")

def run_cmd(cmd):
    try:
        proc = subprocess.run(cmd, stdout=subprocess.PIPE,
                              stderr=subprocess.PIPE, text=True)
        if proc.stdout.strip():
            print(proc.stdout.strip())
        if proc.stderr.strip():
            print(proc.stderr.strip())
        return proc.returncode
    except FileNotFoundError:
        print(f"❌ {cmd[0]} not found.")
        return 1

# Full‐flash manifest
FLASH_ALL = [
    ("boot",       "boot.img"),
    ("cam_vpu1",   "cam_vpu1.img"),
    ("cam_vpu2",   "cam_vpu2.img"),
    ("cam_vpu3",   "cam_vpu3.img"),
    ("dpm",        "dpm.img"),
    ("dtbo",       "dtbo.img"),
    ("gz",         "gz.img"),
    ("lk",         "lk.img"),
    ("mcupm",      "mcupm.img"),
    ("md1img",     "md1img.img"),
    ("pi_img",     "pi_img.img"),
    ("preloader",  "preloader.img"),
    ("scp",        "scp.img"),
    ("spmfw",      "spmfw.img"),
    ("sspm",       "sspm.img"),
    ("super",      "super.img"),
]

def menu():
    print("""
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
""")

def erase_frp():
    confirm = input("⚠️ This will erase FRP lock. Type ERASE to continue: ").strip().upper()
    if confirm == "ERASE":
        run_cmd([FASTBOOT, "erase", "frp"])
    else:
        print("Cancelled.")

def flash_all_images():
    print("🚀 Flashing full image set…")
    missing = []
    for part, img in FLASH_ALL:
        if not Path(img).exists():
            missing.append(img)
            continue

        print(f"→ Flashing {part} from {img}")
        ret = run_cmd([FASTBOOT, "flash", part, img])
        if ret != 0:
            print(f"❌ Failed on {part}. Aborting.")
            return

    if missing:
        print("⚠️ The following files were not found, skipped:")
        for f in missing:
            print(f"   • {f}")
    else:
        print("✅ All partitions flashed successfully.")

def main():
    if not FASTBOOT:
        print("❌ fastboot not found! Place in platform-tools or add to PATH.")
        sys.exit(1)
    if not ADB:
        print("❌ adb not found! Place in platform-tools or add to PATH.")
        # We continue so fastboot still works

    while True:
        menu()
        choice = input("👉 Select option: ").strip()

        # -- FASTBOOT --
        if choice == "1":
            run_cmd([FASTBOOT, "devices"])
        elif choice == "2":
            part = input("Partition name: ").strip()
            img  = input("Image file path: ").strip()
            if Path(img).exists():
                run_cmd([FASTBOOT, "flash", part, img])
            else:
                print("❌ File not found.")
        elif choice == "3":
            img = input("Image file path: ").strip()
            if Path(img).exists():
                run_cmd([FASTBOOT, "boot", img])
            else:
                print("❌ File not found.")
        elif choice == "4":
            part = input("Partition to erase: ").strip()
            run_cmd([FASTBOOT, "erase", part])
        elif choice == "5":
            part = input("Partition to format: ").strip()
            run_cmd([FASTBOOT, "format", part])
        elif choice == "6":
            run_cmd([FASTBOOT, "reboot"])
        elif choice == "7":
            run_cmd([FASTBOOT, "getvar", "all"])
        elif choice == "8":
            confirm = input("⚠️ Unlocking will wipe all data. Type YES to continue: ").strip().upper()
            if confirm == "YES":
                run_cmd([FASTBOOT, "oem", "unlock"])
            else:
                print("Cancelled.")
        elif choice == "9":
            erase_frp()

        # -- Bulk Flash --
        elif choice == "17":
            flash_all_images()

        # -- ADB --
        elif choice == "10":
            run_cmd([ADB, "devices"])
        elif choice == "11":
            apk = input("APK file path: ").strip()
            if Path(apk).exists():
                run_cmd([ADB, "install", apk])
            else:
                print("❌ File not found.")
        elif choice == "12":
            run_cmd([ADB, "shell"])
        elif choice == "13":
            src = input("Local file path: ").strip()
            dst = input("Device path: ").strip()
            if Path(src).exists():
                run_cmd([ADB, "push", src, dst])
            else:
                print("❌ File not found.")
        elif choice == "14":
            src = input("Device file path: ").strip()
            dst = input("Local path: ").strip()
            run_cmd([ADB, "pull", src, dst])
        elif choice == "15":
            run_cmd([ADB, "reboot", "bootloader"])
        elif choice == "16":
            run_cmd([ADB, "reboot", "recovery"])

        # -- Exit / Fallback --
        elif choice == "0":
            print("👋 Exiting tool...")
            sys.exit()
        else:
            print("❌ Invalid choice.")

if __name__ == "__main__":
    main()
