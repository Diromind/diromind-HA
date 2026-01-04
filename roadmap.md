# Smart Home Migration Plan

## Phase 1: Hardware & Base OS
1. [ ] Acquire always-on PC.
2. [ ] Acquire "Zigbee 3.0 USB Dongle" and a Matter-compatible stick (or combo unit).
3. [ ] Install Home Assistant OS (HAOS) on the PC.
4. [ ] Ensure PC has a static local IP address.
5. [ ] Configure daily backups (Google Drive backup add-on is recommended).

## Phase 2: The Great Migration (Sensor Decoupling)
*Goal: Move control from Alice Cloud to Local PC.*
1. [ ] Set up Zigbee2MQTT (Z2M) or ZHA in Home Assistant.
2. [ ] Set up Matter Server add-on in Home Assistant.
3. [ ] Unpair sensors from Alice App.
4. [ ] Pair Zigbee sensors to the new USB Stick.
5. [ ] Pair Matter devices to Home Assistant using pairing codes.
6. [ ] Rename all entities in HA to a strict naming convention (e.g., `sensor.kitchen_temp_hum`, `binary_sensor.hallway_motion`). *Crucial for Python coding later.*

## Phase 3: The Bridge (Restoring Voice Control)
*Goal: Make Alice useful again.*
1. [ ] Install HACS (Home Assistant Community Store).
2. [ ] Install `YandexStation` integration via HACS.
3. [ ] Authenticate with Yandex Account.
4. [ ] "Expose" your newly paired HA sensors and switches back to Yandex Smart Home.
5. [ ] Verify: Ask Alice "What is the temperature in the kitchen?" (She should read the data from your Local PC).

## Phase 4: Logic Engine (Python)
1. [ ] Install `Pyscript` integration.
2. [ ] Connect VS Code (via SSH or Remote Add-on) to the HA config folder.
3. [ ] Create a `Hello World` python script that turns on a light.
4. [ ] Create a `Hello World` python script that forces Alice to speak (TTS).
5. [ ] Migrate logic: Translate existing scenarios into Python classes/functions.

## Phase 5: Observability (Local)
1. [ ] Install InfluxDB (or VictoriaMetrics) add-on for long-term data retention.
2. [ ] Configure `recorder` in `configuration.yaml` to exclude noise and include important sensors.
3. [ ] Build "Admin Dashboard" in HA (Lovelace) with raw history graphs for debugging.
4. [ ] Build "Tablet Dashboard" in HA for daily usage (simple buttons/gauges).

## Phase 6: Future / Advanced
1. [ ] Configure Prometheus Exporter in HA.
2. [ ] (Later) Connect Yandex Cloud Monitoring to scrape the HA Prometheus endpoint.
  
