# Target System Architecture: Local-First Observability & Control

1. Infrastructure Requirements
   - [ ] Host Machine: x86-64 PC/MiniPC (Always-on, wired Ethernet preferred)
   - [ ] Zigbee Radio: USB Coordinator (Zigbee 3.0 compatible)
   - [ ] Matter Radio: Matter-compatible USB stick or Multi-Protocol Stick

2. Core Software Stack
   - [ ] OS Layer: Home Assistant Operating System (HAOS) OR Containerized HA Supervised
   - [ ] Zigbee Service: Zigbee2MQTT (preferred for decoupling) OR ZHA
   - [ ] Matter Service: Python Matter Server (Official HA Add-on)
   - [ ] Database Engine: InfluxDB or VictoriaMetrics (Long-term metrics storage)

3. Integration & Connectivity
   - [ ] Smart Assistant Bridge: Custom Component (e.g., YandexStation) to expose HA entities to Alice
   - [ ] Voice Feedback: TTS (Text-to-Speech) service enabled on all Alice stations for notifications
   - [ ] External Access: Reverse Proxy / Nabu Casa / VPN (for remote access)

4. Logic & Automation (Infrastructure as Code)
   - [ ] Python Engine: Pyscript or AppDaemon installed
   - [ ] Version Control: Automations folder linked to private Git repository
   - [ ] Logic Structure: 100% of complex logic handled via .py files (no UI-based Node-RED/YAML automations)

5. Observability & UI
   - [ ] Dashboard: Home Assistant Lovelace (Vanilla or Mushroom Cards)
   - [ ] Metrics Exporter: Prometheus Exporter integration (configured for future external scraping)
   - [ ] Real-time Visualization: Standard History Graph cards using local DB source

6. Target Data Flow
   [Sensors] --(Zigbee/Matter)--> [Local USB Stick] 
         |
         v
   [Home Assistant Core] <--(Bi-Directional Sync)--> [Yandex Alice Cloud]
         |
         +--> [InfluxDB] (Historical Data)
         +--> [Pyscript] (Python Logic Evaluation)
         +--> [Prometheus Exporter] (Future: Yandex Cloud Monitoring)
         +--> [Lovelace UI] (Local Dashboard)
