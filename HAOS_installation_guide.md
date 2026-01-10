# Home Assistant OS (HAOS) Deployment

## 1. Create the Virtual Machine
*We use a helper script to automate the complex BIOS/EFI/Disk settings required for HAOS.*

1.  Open the Proxmox **Shell** (Node -> >_ Shell).
2.  Download and run the installer script:
    ```bash
    wget https://raw.githubusercontent.com/tteck/Proxmox/main/vm/haos-vm.sh
    ```
    *(Note: If the script fails with "Version not supported", verify `sed` fix is applied to regex matches).*

3.  **Run with Version Bypass (If required for newer Proxmox versions):**
    ```bash
    sed -i 's/8\.$$1-3$$/8.[1-9]/' haos-vm.sh
    bash haos-vm.sh
    ```

4.  **VM Wizard Settings:**
    *   **Create VM?** Yes.
    *   **Settings:** Default settings are usually sufficient (2 Cores, 4GB RAM).
    *   **Start when finished?** Yes.

## 2. Network Configuration (Static IP)
*Once HAOS is running, we must lock its IP address.*

1.  Find the dynamic IP in the Proxmox Web UI -> VM 100 -> **Console**.
2.  Open `http://<DYNAMIC_IP>:8123` in a browser. Create an Admin account.
3.  Navigate to **Settings -> System -> Network**.
4.  Under **IPv4**, switch from Automatic to **Static**.
5.  **IP:** Set a dedicated IP (e.g., `192.168.1.240/24`). **Note:** Do not include `/24` in the input field if the UI has a separate Netmask field.

## 3. Hardware Passthrough (Zigbee/Matter)
*Home Assistant needs direct access to the USB Radio Sticks.*

1.  Plug your Zigbee/Matter sticks into the Server.
2.  In Proxmox, select the **HAOS VM (100)**.
3.  Go to **Hardware -> Add -> USB Device**.
4.  Select **"Use USB Vendor/Device ID"**.
5.  Choose the stick (e.g., `Sonoff Zigbee 3.0` or `Silicon Labs`).
6.  **Reboot the VM** via the Proxmox UI for changes to take effect.

## 4. Matter Device Commissioning (The Manual Flow)
*If devices cannot be exported from proprietary hubs (like Alice), use the Reverse Flow.*

1.  **Reset** the Matter device (Toggle power 5 times).
2.  **Commission via Mobile:**
    *   Ensure phone is on **2.4GHz WiFi**.
    *   Open **HA Companion App** -> Settings -> Devices -> User Phone Bluetooth Commissioning.
    *   Scan QR code on bulb.
3.  **Share to Alice:**
    *   In HA: Device Page -> Share/Matter -> **Copy Pairing Code**.
    *   In Alice App: Add Device -> Matter -> **Enter Code**.
