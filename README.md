# NetworkManager Interface Configuration

This guide explains how to hand over control of your network interfaces to **NetworkManager** after a minimal desktop installation (like `gnome-core`), preventing conflicts with legacy networking services.

---

## Prerequisites
Ensure **NetworkManager** is installed on your system. If not, install it using your package manager:
```bash
sudo apt update && sudo apt install network-manager
```

---

## Setup Steps

### 1. Enable and Start NetworkManager
Ensure the NetworkManager service is active and set to start automatically on boot:
```bash
sudo systemctl enable --now NetworkManager
```

### 2. Clear Legacy Interface Configurations
NetworkManager ignores interfaces explicitly defined in traditional configuration files. You must clear them out.

1. Open the network interfaces configuration file:
   ```bash
   sudo nano /etc/network/interfaces
   ```
2. Comment out or remove lines configuring your specific hardware interfaces. **Keep only the loopback (`lo`) interface.**

   *Example of what it should look like:*
   ```text
   auto lo
   iface lo inet loopback

   # Comment out or delete everything below this line:
   # auto eth0
   # iface eth0 inet dhcp
   ```

### 3. Disable Conflicting Services
Disable the traditional networking backend service to prevent it from fighting NetworkManager for control:
```bash
sudo systemctl disable --now networking.service
```

### 4. Restart NetworkManager
Force NetworkManager to rescan the available hardware devices:
```bash
sudo systemctl restart NetworkManager
```

---

## Verification

Check the operational state of your network adapters:
```bash
nmcli device status
```

Your physical interfaces (like `eth0`, `enp3s0`, or `wlan0`) should now show as **connected** or **disconnected** (managed), rather than **unmanaged**.
