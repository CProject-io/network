Claro, aquí tienes el informe completo en formato Markdown estándar.

# Informe de Arquitectura de Red Híbrida de C-Project

### **Introducción**

Este informe detalla la arquitectura de red híbrida y jerárquica propuesta para el ecosistema C-Project. El objetivo es crear una infraestructura de comunicación resiliente, de acceso libre y energéticamente autónoma. La red se estructura en tres capas, cada una utilizando la tecnología más adecuada para su función específica, optimizando el balance entre ancho de banda, alcance y consumo energético.

---

### **Capa 1: Red de Acceso y Malla Local (El "Cluster")**

Esta es la capa de alta densidad y corto alcance, diseñada para dar servicio a usuarios finales y para que los nodos de una misma zona (ej. un barrio, un pueblo) se comuniquen entre sí.

* **Requisitos Técnicos y de Software:**
    * **Hardware:** El nodo base es una **Raspberry Pi 4** equipada con su tarjeta Wi-Fi integrada o un adaptador USB de buena calidad, conectado a una antena Wi-Fi omnidireccional.
    * **Software:**
        1.  **Sistema Operativo:** Una distribución de Linux (ej. Raspberry Pi OS).
        2.  **Punto de Acceso:** Software como `hostapd` para configurar la interfaz Wi-Fi en modo Access Point (AP), creando la red "C-Project Free Access" a la que se conectan los usuarios.
        3.  **Servicios de Red:** Un servidor DHCP (`dnsmasq` o similar) para asignar IPs a los usuarios que se conectan.
        4.  **Enrutamiento Mesh:** Un protocolo de enrutamiento Ad-Hoc como **OLSR (Optimized Link State Routing)** o **BATMAN-adv** para que los nodos del cluster se descubran y enruten paquetes entre sí de forma automática.

* **Consumo Eléctrico Estimado:**
    * Raspberry Pi 4 (carga media): \~5-7 Watts
    * Adaptador Wi-Fi en modo AP y mesh: \~2-3 Watts
    * **Total Estimado:** **8 - 10 Watts** de consumo continuo.
    * **Consumo Diario:** `10W * 24h = 240 Wh/día`.

* **Solución de Autonomía Propuesta:**
    * **Almacenamiento:** Para garantizar 3 días de autonomía sin sol, se necesita una capacidad de `240 Wh/día * 3 = 720 Wh`. Una batería de LiFePO4 de **12V y 75Ah (\~900 Wh)** es una elección robusta y segura.
    * **Generación:** Para reponer 240 Wh con unas 4-5 horas de sol pico, se necesita un mínimo de `240 Wh / 4h = 60W`. Un **panel solar de 100W a 150W** es adecuado para compensar días nublados y pérdidas de eficiencia.
    * **Componentes:** Un controlador de carga MPPT de 10A o 15A.

---

### **Capa 2: Backbone de Mediano y Largo Alcance (Inter-Cluster)**

Esta capa es la columna vertebral que interconecta los diferentes clusters locales, permitiendo la comunicación entre nodos que están a varios kilómetros de distancia.

* **Requisitos Técnicos y de Software:**
    * **Hardware:** Un nodo C-Project más robusto. Además de su equipo para la malla local, este nodo "gateway" necesita:
        * **Opción A (Práctica):** Un radio Wi-Fi de alta potencia y una **antena direccional** (Yagi o parabólica) para crear un enlace Punto-a-Punto con otro gateway.
        * **Opción B (Ideal/Futura):** Un adaptador **Wi-Fi HaLow (802.11ah)**, que ofrece un balance ideal entre rango (1km+), ancho de banda y consumo.
    * **Software:** Linux con capacidades de enrutamiento avanzadas. Debe ser capaz de gestionar el tráfico entre la interfaz de la red de malla local y la interfaz del enlace backbone.

* **Consumo Eléctrico Estimado:**
    * Raspberry Pi 4: \~5-7 Watts
    * Radio Wi-Fi de alta potencia para el backbone: \~5-10 Watts adicionales.
    * **Total Estimado:** **15 - 20 Watts** de consumo continuo.
    * **Consumo Diario:** `20W * 24h = 480 Wh/día`.

* **Solución de Autonomía Propuesta:**
    * **Almacenamiento:** `480 Wh/día * 3 = 1440 Wh`. Se requiere una batería de mayor capacidad, del orden de **12V y 150Ah (\~1800 Wh)**.
    * **Generación:** `480 Wh / 4h = 120W`. Se necesita un sistema de generación más potente. Un **panel solar de 250W o 300W** sería apropiado.
    * **Componentes:** Un controlador de carga MPPT de 20A o 30A.

---

### **Capa 3: Red de Resiliencia y Nodos Aislados**

Esta es la capa de mínimo consumo y máximo alcance, diseñada para conectar los nodos más remotos y para transmitir datos críticos de bajo volumen, como la telemetría para el Proof-of-Generation.

* **Requisitos Técnicos y de Software:**
    * **Hardware:** Un nodo C-Project con una Raspberry Pi 4 y un **módulo/HAT LoRaWAN** conectado a una antena LoRa omnidireccional.
    * **Software:** El software del nodo debe incluir la lógica de "traducción" que discutimos: extraer el contenido esencial de un mensaje (ej. una transacción) y encapsularlo en un paquete LoRaWAN optimizado y muy pequeño.

* **Consumo Eléctrico Estimado:**
    * Raspberry Pi 4: \~5-7 Watts
    * Módulo LoRaWAN (muy bajo consumo, principalmente en espera): \~0.5-1 Watt promedio.
    * **Total Estimado:** **6 - 8 Watts** de consumo continuo.
    * **Consumo Diario:** `8W * 24h = 192 Wh/día`.

* **Solución de Autonomía Propuesta:**
    * **Almacenamiento:** `192 Wh/día * 3 = 576 Wh`. Una batería de **12V y 50-75Ah (\~600-900 Wh)** es más que suficiente.
    * **Generación:** `192 Wh / 4h = 48W`. Un **panel solar de 100W** proporciona un amplio margen de seguridad.
    * **Componentes:** Un controlador de carga MPPT de 10A.

---

### **Tabla Resumen de la Arquitectura Híbrida**

| Característica | Capa 1: Malla Local | Capa 2: Backbone | Capa 3: Resiliencia (LoRaWAN) |
| :--- | :--- | :--- | :--- |
| **Tecnología** | Wi-Fi Mesh (Ad-Hoc) | Wi-Fi Direccional / Wi-Fi HaLow | LoRaWAN |
| **Alcance** | Corto (\~100-200m entre nodos) | Mediano-Largo (1-10+ km) | Muy Largo (5-15+ km) |
| **Ancho de Banda** | Alto (Mbps) | Muy Alto (Mbps) | Muy Bajo (Kbps) |
| **Caso de Uso Principal**| Acceso de usuarios, red local densa | Interconexión de clusters | Nodos aislados, datos de sensores |
| **Consumo Estimado** | \~10 W | \~20 W | \~8 W |
| **Autonomía Sugerida** | Panel 150W / Batería 900Wh | Panel 300W / Batería 1800Wh | Panel 100W / Batería 600Wh |
