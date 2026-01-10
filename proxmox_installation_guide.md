# Proxmox VE Hardware Installation Guide

## 1. Preparation
1.  **Download:** Get the latest Proxmox VE ISO (Version 8.x) from [proxmox.com](https://www.proxmox.com).
2.  **Flash:** Use BalenaEtcher to burn the ISO to a USB stick.

## 2. BIOS Configuration
*Before booting, ensure these settings are applied in the BIOS:*
*   **Secure Boot:** DISABLED (Crucial: Proxmox will not boot if enabled).
*   **Virtualization (VT-x / Vt-d):** ENABLED.
*   **Power State:** Set "Restore on AC Power Loss" to **Power On** (Ensures server reboots after outages).

## 3. The Installer Wizard
1.  Boot from USB. Select **"Install Proxmox VE (Graphical)"**.
2.  **EULA:** Click Agree.
3.  **Target Disk:** Select target disk. (Warning: This wipes the drive).
4.  **Country/Timezone:** Select your location (Critical for Smart Home automation timing).
5.  **Password:** Set a strong root password and valid email.
6.  **Network Configuration:**
    *   **Management Interface:** Select the primary Ethernet port (will boot via Wi-Fi, but must have LAN to use HA).
    *   **Hostname:** `pve.local` (or similar FQDN - must have dot inside).
    *   **IP Address:** Choose a **Static IP** outside your DHCP range (e.g., `192.168.1.250/24`).
    *   **Gateway/DNS:** Your Router IP (e.g., `192.168.1.1`).
7.  **Finish:** Click Install. Remove USB when the system reboots.

## 4. First Login & Post-Install Initialization
1.  Navigate to `https://<YOUR_STATIC_IP>:8006` in a browser.
2.  Login as `root` with your password.
3.  **Fix Update Repositories (The "Magic Script"):**
    *   Proxmox defaults to "Enterprise" (Paid) update channels. We must switch to "Community".
    *   Select your Node on the left (in Datacenter folder) -> **Shell**.
    *   Run the community initialization script:
    ```bash
    bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/misc/post-pve-install.sh)"
    ```
    *   Script may incorrectly check versions higher-than-expected (e.g. 8.4 is "below minimal") - in that case you can fix validation with smth like that
    ```bash
    sed -i 's/8\.\[1-3\]/8.[1-9]/' post-pve-install.sh
    ```
    *   **Wizard Choices:** Select `Yes` to all prompts (Disable Enterprise, Enable Community, Update Now).
    *   **Reboot:** Allow the system to reboot to apply the new Linux Kernel.
