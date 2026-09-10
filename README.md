# 🚀 D-Link DWR-116 (A1) - Unbrick, OpenWrt 22.03.7 Flashing, Wi-Fi Repeater & Backup Guide

A comprehensive, step-by-step guide to recovering an inaccessible (*bricked*) **D-Link DWR-116 (Hardware Revision A1)** router, installing **OpenWrt 22.03.7**, connecting to an ISP main Wi-Fi network, creating a new local repeated Wi-Fi network, passing internet access through to the physical LAN ports, and saving a post-configuration system backup.

---

## 🛠️ Requirements & Official Downloads

- **Hardware:** D-Link DWR-116 (HW Ver: A1)
- **OpenWrt Release:** `22.03.7`
- **Factory Firmware (For Emergency / First Flash):**  
  👉 [`openwrt-22.03.7-ramips-rt305x-dlink_dwr-116-a1-squashfs-factory.bin`](https://downloads.openwrt.org/releases/22.03.7/targets/ramips/rt305x/openwrt-22.03.7-ramips-rt305x-dlink_dwr-116-a1-squashfs-factory.bin)
- **Ethernet Cable** connected from your PC to the **LAN 1** port on the router.

---

## 📂 Repository Image Structure

Create an `img/` directory at the root of your repository to store the screenshot files using these exact names:

```text
.
├── README.md
└── img/
    ├── 01-windows-ip.png       # Windows Static IP Configuration
    ├── 02-emergency-room.png   # Emergency Room Recovery Web Page (192.168.0.1)
    ├── 03-luci-login.png       # OpenWrt LuCI First Login (192.168.1.1)
    ├── 04-wireless-overview.png# Wireless Overview Dashboard (LuCI)
    ├── 05-ping-test.png        # Ping Diagnostic Test Output (SSH)
    └── 06-backup-luci.png      # Backup Generation Screen (LuCI)
