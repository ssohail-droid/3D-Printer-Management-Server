# Bambu Printer Farm on Linux

A step-by-step guide to setting up a networked Bambu 3D printer farm running on Linux Mint. 
This guide covers everything from hardware setup to configuring multiple printers in LAN Only 
Mode using Bambu Studio — no cloud required.

---

## Prerequisites

Before getting started, make sure you have the following:

- **Linux Mint** 21.x or newer installed on your PC/Server
- **Bambu Studio** (latest version) — [Download here](https://bambulab.com/en/download/studio)
- All printers and the PC/Server connected to the **same local network**
- Ports **1883** and **990** open on your firewall for LAN communication
- Each printer **connected via Ethernet** to the network switch (no WiFi recommended for stability)

> **Tip:** Disable any VPN on the server while setting up — it can block local printer discovery.

---

## Required Components & Tools

- Printers (using 4 for this example)
- PC (Anything that can run Linux) — *Dell OptiPlex 7050, Intel i3, 8GB RAM, 256GB SSD*
- [Network Switch](https://www.amazon.com/Gigabit-Managed-Ethernet-Splitter-Snooping/dp/B0D4TP93SH/ref=sr_1_5_sspa?sr=8-5-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9udGY)
- [WiFi Router](https://www.amazon.com/WiFi-6-Router-Gigabit-Wireless/dp/B08H8ZLKKK/ref=sr_1_3?sr=8-3)
- Ethernet cables
- Zip ties

---

## OS

We'll be using [Linux Mint](https://www.linuxmint.com/) for this build. It's easy to use, 
beginner-friendly, and just works out of the box.

---

## Slicer Software

After setting up Linux Mint, you'll need a slicer. This guide uses [Bambu Studio](https://bambulab.com/en/download/studio), 
but any slicer with a network printing plugin will work (e.g., PrusaSlicer, OrcaSlicer, Cura).

> **Note:** Make sure your slicer of choice has network printing support enabled before proceeding.

---

## Bambu Studio Installation (Linux Mint)

1. Download the latest `.deb` package from [bambulab.com/en/download/studio](https://bambulab.com/en/download/studio)
2. Open a terminal and navigate to your Downloads folder:
```bash
   cd ~/Downloads
```
3. Install the package:
```bash
   sudo dpkg -i BambuStudio_linux_*.deb
```
4. If you get dependency errors, run:
```bash
   sudo apt --fix-broken install
```
5. Launch Bambu Studio from the application menu or by running:
```bash
   bambu-studio
```

---

## Network Setup

All printers connect to the network switch via Ethernet, with a single Ethernet cable running 
from the switch to the PC/Server.

<p align="center">
  <img src="doc/images/NetDiagram.png" width="45%" alt="Network Diagram">
  &nbsp;&nbsp;
  <img src="doc/images/physical-setup.jpeg" width="45%" alt="Physical Hardware Setup">
</p>

---

## Static IP Setup

Assigning static IPs to each printer ensures Bambu Studio can always find them, even after a 
reboot or power cycle.

### On Your Router

1. Log into your router's admin page (typically `192.168.1.1` or `192.168.0.1`)
2. Navigate to **DHCP Reservations** or **Static Leases**
3. Find each printer by its MAC address (found in the printer's **Settings → Network** menu)
4. Assign a fixed IP to each printer, for example:

   | Printer   | MAC Address           | Assigned IP     |
   |-----------|-----------------------|-----------------|
   | Printer 1 | `xx:xx:xx:xx:xx:01`  | `192.168.1.101` |
   | Printer 2 | `xx:xx:xx:xx:xx:02`  | `192.168.1.102` |
   | Printer 3 | `xx:xx:xx:xx:xx:03`  | `192.168.1.103` |
   | Printer 4 | `xx:xx:xx:xx:xx:04`  | `192.168.1.104` |

5. Save and reboot the router

> **Note:** Steps may vary slightly depending on your router's firmware.

---

## Printer Setup — LAN Only Mode

To keep all printers on the local network and off Bambu's cloud, we need to enable LAN Only 
Mode on each printer.

---

### Bambu X1E / H2D — LAN Only Mode

#### Step 1: Enable LAN Only Mode on the printer

- In Settings, click **Settings → LAN Only** to enter the LAN Only page.

<p align="center">
  <img src="doc/images/X1-HD/setting1.jpg" width="600" alt="X1E Settings Menu">
</p>

<p align="center">
  <img src="doc/images/X1-HD/lanonly2.jpg" width="600" alt="X1E LAN Only Page">
</p>

- Turn on LAN Only Mode.

<p align="center">
  <img src="doc/images/X1-HD/lanen2.png" width="600" alt="X1E LAN Only Toggle">
</p>

- Choose whether to enable LAN mode liveview according to your needs.

<p align="center">
  <img src="doc/images/X1-HD/lanen.png" width="600" alt="X1E Liveview Option">
</p>

#### Step 2: Bind the printer in LAN mode to Bambu Studio

- Open the printer list popup under the **Device** page and find the printer that has been 
switched to LAN Only Mode. (The printer will have a lock icon in front of its name as shown below)

- This can take between 20–60 seconds, or longer in some rare cases. If your printer is still 
not displayed, check that the printer and Bambu Studio are on the same local network and that 
communication is not blocked. (This can happen on some Guest networks)

<p align="center">
  <img src="doc/images/X1-HD/other_device.png" width="600" alt="X1E Device List in Bambu Studio">
</p>

- Input the **Printer Access Code** and click **Confirm**.

<p align="center">
  <img src="doc/images/X1-HD/access_code.png" width="600" alt="X1E Access Code Entry">
</p>

- After confirming the connection, you can use the printer in LAN mode.

<p align="center">
  <img src="doc/images/X1-HD/connection.png" width="600" alt="X1E Connection Confirmed">
</p>

---

### Bambu P1P / P1S — LAN Only Mode

#### Step 1: Enable LAN Only Mode on the printer

- Navigate to **Settings → WLAN** and select it. Here you can find the LAN Only Mode option.

<p align="center">
  <img src="doc/images/P1S/pap_lan_mode1.png" width="600" alt="P1S WLAN Settings">
</p>

- The LAN Only Mode option is initially set to OFF. Turn it on here.

<p align="center">
  <img src="doc/images/P1S/pap_lan_mode2.png" width="600" alt="P1S LAN Only Mode Toggle">
</p>

- Confirm the selection by selecting **Yes**.

<p align="center">
  <img src="doc/images/P1S/pap_lan_mode3.png" width="600" alt="P1S LAN Only Mode Confirmation">
</p>

- When LAN Only Mode changes to **ON**, the switch was successful. Take note of the **Access Code**.

<p align="center">
  <img src="doc/images/P1S/pap_lan_mode4.png" width="600" alt="P1S LAN Only Mode Active with Access Code">
</p>

#### Step 2: Bind the printer in LAN mode to Bambu Studio

- Open the printer list popup under the **Device** page and find the printer that has been 
switched to LAN Only Mode. (The printer will have a lock icon in front of its name as shown below)

- This can take between 20–60 seconds, or longer in some rare cases. If your printer is still 
not displayed, check that the printer and Bambu Studio are on the same local network and that 
communication is not blocked. (This can happen on some Guest networks)

<p align="center">
  <img src="doc/images/P1S/x1_lan_mode4.png" width="600" alt="P1S Device List in Bambu Studio">
</p>

- Input the **Printer Access Code** and click **Confirm**. After the connection is confirmed, 
you can use the printer just like you normally would.

<p align="center">
  <img src="doc/images/P1S/x1_lan_mode5.png" width="600" alt="P1S Access Code Entry">
</p>

---

## Adding Multiple Printers

Repeat the **LAN Only Mode** and **Bambu Studio binding** steps for each printer in your farm. 
Each printer will need its own:

- LAN Only Mode enabled on the touchscreen
- Access Code noted down
- Individual binding in Bambu Studio under the **Device** page

Once all printers are bound, they will all appear in the Bambu Studio device list and can be 
managed independently from the same PC/Server.

> **Tip:** Label your printers physically (e.g. Printer 1–4) and match them to their static IPs 
> in a note or spreadsheet — this saves a lot of confusion when managing multiple machines.
