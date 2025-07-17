# C-Project Hybrid Network Architecture Report

### **Introduction**

This report details the proposed hybrid and hierarchical network architecture for the C-Project ecosystem. The objective is to create a resilient, free-access, and energetically autonomous communication infrastructure. The network is structured into three layers, each utilizing the most suitable technology for its specific function, optimizing the balance between bandwidth, range, and power consumption.

---

### **Layer 1: Local Access and Mesh Network (The "Cluster")**

This is the high-density, short-range layer, designed to provide service to end-users and to allow nodes within the same area (e.g., a neighborhood, a village) to communicate with each other.

* **Technical and Software Requirements:**
    * **Hardware:** The base node is a **Raspberry Pi 4** [cite: C-Project/nodo-main/hardware/readme.md] equipped with its integrated Wi-Fi card or a quality USB adapter, connected to an omnidirectional Wi-Fi antenna.
    * **Software:**
        1.  **Operating System:** A Linux distribution (e.g., Raspberry Pi OS).
        2.  **Access Point:** Software like `hostapd` to configure the Wi-Fi interface in Access Point (AP) mode, creating the "C-Project Free Access" network for users to connect to.
        3.  **Network Services:** A DHCP server (`dnsmasq` or similar) to assign IP addresses to connecting users.
        4.  **Mesh Routing:** An Ad-Hoc routing protocol such as **OLSR (Optimized Link State Routing)** or **BATMAN-adv** to allow nodes within the cluster to automatically discover each other and route packets [cite: C-Project/network-main/README.md].

* **Estimated Power Consumption:**
    * Raspberry Pi 4 (medium load): ~5-7 Watts
    * Wi-Fi adapter in AP and mesh mode: ~2-3 Watts
    * **Total Estimated:** **8 - 10 Watts** of continuous consumption.
    * **Daily Consumption:** `10W * 24h = 240 Wh/day`.

* **Proposed Autonomy Solution:**
    * **Storage:** To guarantee 3 days of autonomy without sun, a capacity of `240 Wh/day * 3 = 720 Wh` is needed. A **12V, 75Ah LiFePO4 battery (~900 Wh)** is a robust and safe choice [cite: C-Project/nodo-main/README.md].
    * **Generation:** To replenish 240 Wh with about 4-5 hours of peak sun, a minimum of `240 Wh / 4h = 60W` is required. A **100W to 150W solar panel** is suitable to compensate for cloudy days and efficiency losses [cite: C-Project/nodo-main/hardware/readme.md].
    * **Components:** A 10A or 15A MPPT charge controller.

---

### **Layer 2: Medium/Long-Range Backbone (Inter-Cluster)**

This layer is the backbone that interconnects the different local clusters, enabling communication between nodes that are several kilometers apart.

* **Technical and Software Requirements:**
    * **Hardware:** A more robust C-Project node. In addition to its local mesh equipment, this "gateway" node needs:
        * **Option A (Practical):** A high-power Wi-Fi radio and a **directional antenna** (Yagi or parabolic) to create a Point-to-Point link with another gateway.
        * **Option B (Ideal/Future):** A **Wi-Fi HaLow (802.11ah)** adapter, which offers an ideal balance of range (1km+), bandwidth, and power consumption.
    * **Software:** Linux with advanced routing capabilities. It must be able to manage traffic between the local mesh interface and the backbone link interface.

* **Estimated Power Consumption:**
    * Raspberry Pi 4: ~5-7 Watts
    * High-power Wi-Fi radio for the backbone: ~5-10 additional Watts.
    * **Total Estimated:** **15 - 20 Watts** of continuous consumption.
    * **Daily Consumption:** `20W * 24h = 480 Wh/day`.

* **Proposed Autonomy Solution:**
    * **Storage:** `480 Wh/day * 3 = 1440 Wh`. A higher capacity battery is required, on the order of **12V, 150Ah (~1800 Wh)** [cite: C-Project/nodo-main/hardware/readme.md].
    * **Generation:** `480 Wh / 4h = 120W`. A more powerful generation system is needed. A **250W or 300W solar panel** would be appropriate [cite: C-Project/nodo-main/hardware/readme.md].
    * **Components:** A 20A or 30A MPPT charge controller.

---

### **Layer 3: Resilience Network and Isolated Nodes**

This is the layer of minimum consumption and maximum range, designed to connect the most remote nodes and to transmit critical, low-volume data, such as telemetry for Proof-of-Generation.

* **Technical and Software Requirements:**
    * **Hardware:** A C-Project node with a Raspberry Pi 4 and a **LoRaWAN module/HAT** connected to an omnidirectional LoRa antenna.
    * **Software:** The node's software must include the "translation" logic we discussed: extracting the essential content of a message (e.g., a transaction) and encapsulating it into a highly optimized, small LoRaWAN packet.

* **Estimated Power Consumption:**
    * Raspberry Pi 4: ~5-7 Watts
    * LoRaWAN module (very low power, mostly in standby): ~0.5-1 Watt average.
    * **Total Estimated:** **6 - 8 Watts** of continuous consumption.
    * **Daily Consumption:** `8W * 24h = 192 Wh/day`.

* **Proposed Autonomy Solution:**
    * **Storage:** `192 Wh/day * 3 = 576 Wh`. A **12V, 50-75Ah battery (~600-900 Wh)** is more than sufficient [cite: C-Project/nodo-main/README.md].
    * **Generation:** `192 Wh / 4h = 48W`. A **100W solar panel** provides a wide safety margin [cite: C-Project/nodo-main/hardware/readme.md].
    * **Components:** A 10A MPPT charge controller.

---

### **Summary Table of the Hybrid Architecture**

| Feature             | Layer 1: Local Mesh         | Layer 2: Backbone                 | Layer 3: Resilience (LoRaWAN) |
| :------------------ | :-------------------------- | :-------------------------------- | :---------------------------- |
| **Technology** | Wi-Fi Mesh (Ad-Hoc)         | Directional Wi-Fi / Wi-Fi HaLow   | LoRaWAN                       |
| **Range** | Short (~100-200m between nodes) | Medium-Long (1-10+ km)            | Very Long (5-15+ km)          |
| **Bandwidth** | High (Mbps)                 | Very High (Mbps)                  | Very Low (Kbps)               |
| **Primary Use Case**| User access, dense local network | Interconnecting clusters          | Isolated nodes, sensor data   |
| **Estimated Power** | ~10 W                       | ~20 W                             | ~8 W                          |
| **Suggested Autonomy** | 150W Panel / 900Wh Battery | 300W Panel / 1800Wh Battery      | 100W Panel / 600Wh Battery    |
