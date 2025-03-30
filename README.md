# Technical Document: Infrastructure and Services for the C-Project Network

## 1. Introduction

The Carbon Project Foundation (C-Project) aims to build a decentralized network independent of centralized ISPs. This network will serve as a platform for developing our own blockchain—and will also allow other blockchains to utilize it. The design is intended to ensure:

- **Security and Decentralization:** Utilizing routing and connectivity protocols (CJDNS, BATMAN-adv, or Babel) to secure communications through encryption and self-organization.
- **Resilience and Self-Sustainability:** All nodes must operate on 100% renewable energy, promoting an environmentally responsible technology ecosystem.
- **Interoperability:** The infrastructure will act as the underlying layer (or “substrate”) for blockchains, offering low latency, high security, and scalability.

This document describes the technical architecture and the philosophy behind the network, focusing on connectivity/routing and essential services, while also serving as the foundation for our blockchain development.

---

## 2. Philosophy and Objectives of the Network

### 2.1. Sustainable and Self-Sustaining Vision

- **100% Renewable Energy:** Every node in the network will run on renewable energy sources (such as solar or wind power), ensuring minimal carbon footprint and promoting environmental sustainability.
- **Economic Self-Sufficiency:** Nodes managed directly by C-Project will not generate direct profit; their operational yields will be reinvested in network maintenance and upgrades. In contrast, individual users running their own nodes can earn tokens as rewards, thereby fostering a circular economy.
- **Neutrality and Censorship Resistance:** The network is designed to eliminate single points of failure and rely solely on user collaboration, guaranteeing free and secure access to information.

### 2.2. Interoperability with Blockchains

- **Platform as a Substrate:** The network is designed so that various blockchains can be deployed and operated on top of it, offering low latency, security, and scalability.
- **Support for External Projects:** In addition to our internal blockchain, other chains can be integrated by leveraging standardized protocols and adaptive gateways.

---

## 3. Connectivity Infrastructure and Routing

### 3.1. Mesh Connectivity and Secure Routing Protocols

#### **CJDNS**
- **Description:** A routing protocol based on IPv6 that uses cryptography (Curve25519) to create self-assigned, encrypted addresses.
- **Functionality:** Facilitates direct and secure communication between nodes without relying on centralized infrastructure.
- **Advantages:**
  - Built-in encryption for enhanced anonymity and security.
  - Auto-configuration of nodes, ideal for decentralized networks.

#### **BATMAN-adv / Babel**
- **BATMAN-adv:**
  - **Usage:** A widely used mesh routing implementation that runs on standard hardware (e.g., routers with OpenWRT).
  - **Benefits:** Enables dynamic route detection and has been proven in community network projects.
- **Babel:**
  - **Usage:** A routing protocol known for its fast convergence in heterogeneous and dynamic network topologies.
  - **Benefits:** Provides stability in networks with frequently connecting and disconnecting nodes.

> The choice among these protocols will depend on the deployed hardware and the technical expertise of the node operators, ensuring flexibility and adaptability in the network rollout.

### 3.2. Hardware and Optional Gateway Integration

- **Recommended Devices:**
  - **Raspberry Pi and routers with OpenWRT firmware:** These low-cost, energy-efficient solutions can easily integrate with the aforementioned protocols.
- **Optional Internet Gateways:**
  - Although the aim is an independent network, optional gateways using solutions like **WireGuard** may be set up to allow secure interoperability with the traditional Internet when needed, without compromising the network's autonomy.

---

## 4. Essential Services: Decentralized DNS and Distributed Hosting

### 4.1. Decentralized Domain Name System

#### **Options Considered: Handshake, Namecoin, and DHT/IPNS**
- **Handshake:**
  - **Description:** A decentralized naming system built on blockchain technology that allows TLD registration and management without intermediaries.
  - **Benefits:** Highly resistant to censorship and enables direct name management.
- **Namecoin:**
  - **Description:** A pioneering cryptocurrency that supports domain registration (e.g., .bit), offering a proven decentralized DNS solution.
  - **Considerations:** While it has some scalability challenges, its maturity and use cases make it a viable option.
- **DHT/IPNS (integrated with IPFS):**
  - **Description:** Leveraging a Distributed Hash Table (DHT) combined with the IPFS naming system (IPNS) to resolve names and associate them with content hosted on the network.
  - **Benefits:** Provides close integration with distributed hosting and reduced operational complexity.

> The final choice will depend on scalability requirements and integration ease with other services. A modular approach is recommended to allow for evolution or even combining these systems based on needs.

### 4.2. Distributed Hosting with IPFS

#### **IPFS (InterPlanetary File System)**
- **Description:** A peer-to-peer protocol for storing and distributing content by addressing it through content hashes.
- **Functionality:**
  - Enables content replication and distribution without a central server.
  - Ensures data integrity through cryptographic verification of content hashes.
- **Integration:**
  - IPFS will serve as the primary hosting service for websites and applications within the network.
  - It seamlessly integrates with the decentralized DNS system, allowing resolved names to point directly to content stored on IPFS.

> IPFS's robustness and widespread adoption in decentralized projects make it an ideal solution for ensuring content availability and censorship resistance.

---

## 5. Integration with Blockchain and Future Expansions

- **Substrate for Blockchain Deployment:**
  - The network will be designed to support the deployment of both our internal blockchain and third-party chains, offering low latency, security, and scalability.
  - We will define APIs and integration protocols that enable other projects to connect their blockchains to the C-Project infrastructure, leveraging its connectivity and distributed services.

- **Interoperability:**
  - Standards and gateways will be established to facilitate communication between the decentralized network and external blockchains, fostering a multi-chain ecosystem.

---

## 6. Conclusion

The proposed infrastructure for C-Project is built on a decentralized and self-sustaining network where every node operates on 100% renewable energy. It leverages robust technologies—such as CJDNS for routing and IPFS for distributed hosting—to provide a secure, censorship-resistant platform. Coupled with a decentralized DNS system (using options like Handshake, Namecoin, or DHT/IPNS), the network not only ensures security and resistance to censorship but also serves as a foundational “substrate” for deploying blockchains.

This comprehensive solution promotes an ecological, self-sufficient, and highly scalable model, aligned with the vision of a free, decentralized Internet. It lays the groundwork for an ecosystem where community participation and sustainability are key.

---
