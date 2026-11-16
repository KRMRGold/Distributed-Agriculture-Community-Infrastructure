# Existing Projects & Research

Nov 16, 2026 · v0.2.0 · exploratory research

## Purpose

This document collects examples of things people have already built, deployed, tested, or standardized that relate to distributed community infrastructure.

The subject is broader than farming. Agriculture is one useful environment for testing networks across fields, buildings, sensors, machines, and remote locations. The wider interest is how communities can operate and connect their own infrastructure using a mixture of wireless and wired technologies, local computing, and systems that tolerate unreliable internet access.

This is a readable research collection, not a proposed architecture or implementation plan. The summaries are starting points; the linked sources are the authority for technical details.

## How to read the entries

- **Deployed example** — a network or system used in a real place by real participants.
- **Open-source project** — software or hardware that can be inspected, adapted, or self-hosted.
- **Standard / protocol** — a documented way for systems to communicate.
- **Research experiment** — a prototype or field trial with reported measurements. Results apply to the tested setup, not automatically to other deployments.

A project can fit more than one category. A working project is not automatically a proven fit for every scale or environment.

## Quick map

| Area | Examples | What they help us understand |
|---|---|---|
| Community-owned connectivity | guifi.net, Freifunk, Zenzeleni, Wave 7 | Local ownership, governance, maintenance, rural connectivity |
| LoRa messaging and sensors | Meshtastic, The Things Network, ChirpStack | Long-range low-power devices, gateways, private sensor networks |
| Mixed communication media | Reticulum, private agricultural network trial | Connecting different radio and wired links |
| Local device mesh | Bluetooth Mesh | Short-range many-to-many sensor and building networks |
| Disconnected communication | Delay-Tolerant Networking, Bundle Protocol v7 | Store-and-forward between places that are not always connected |
| Local/offline computing | Internet-in-a-Box, Kiwix, Portable Network Kits | Useful local services without internet access |\n| Disconnected data synchronization | Syncthing, Automerge, local-first software | Moving and merging changes after devices reconnect |\n| Power-resilient nodes | Portable Network Kits, TakNet 2, Zenzeleni | Solar/battery operation and low-power design examples |\n| Research background | 2019 mesh survey, 2022 tractor trial, 2026 LoRaMESH/HaLow trial | Technology comparisons and measured field examples |

---

## 1. Community-owned and locally operated networks

These examples are important because they show that a network is not just radios and routers. People also need ways to share costs, document equipment, assign responsibilities, maintain infrastructure, and make decisions.

### guifi.net and the Network Commons platform

**Type:** Deployed community network + infrastructure/governance platform

guifi.net is a long-running community network in Catalonia, Spain. Its Network Commons platform is designed to help communities build, document, and operate shared connectivity infrastructure.

The Commons platform brings together:
- Participants, organizations, and service providers.
- Infrastructure records, including networks, locations, devices, and connections.
- Identity, roles, and access permissions.
- Responsibilities, participation, and collective governance.
- Tools for coordinating the deployment and operation of shared infrastructure.

**Why it matters:** This is a useful example of treating network infrastructure as a shared community resource, with governance and documentation as part of the technical system.

**What it is not:** The Commons platform is not itself a radio mesh protocol. It addresses the organizational and infrastructure-management side of community networking.

Sources:
- [Network Commons platform overview](https://commons.guifi.net/)
- [guifi.net](https://guifi.net/)

### Freifunk

**Type:** Deployed community wireless networking movement

Freifunk is a non-commercial community wireless networking initiative. People install and maintain wireless routers that connect with nearby participants, forming local mesh networks. Communities maintain their own firmware variants and organize their own local networks.

People can use the local network for services hosted by participants, and some communities also share internet access.

**Why it matters:** Freifunk demonstrates incremental network growth: individual participants contribute nodes, and local groups adapt the network to their circumstances.

**Limitations to keep in mind:** Freifunk is primarily about community Wi-Fi networking. It does not by itself solve long-range low-power sensor communication, offline synchronization, or community governance for every kind of shared resource.

Source: [What is Freifunk about?](https://freifunk.net/en/what-is-it-about/)

### Zenzeleni Networks — South Africa

**Type:** Deployed community-owned rural connectivity

Zenzeleni is a locally owned, non-profit internet service provider working with rural communities in South Africa. A documented case study describes community Wi-Fi hotspots powered by solar, battery, or mains electricity. Hotspots are identified, owned, and managed through a local cooperative, with local hosts helping operate them.

The case study also describes revenue being put back into network maintenance and related community projects, including solar-powered computer access and digital-skills support.

**Why it matters:** It shows the importance of local ownership, local operators, power resilience, training, and reinvesting in the infrastructure.

**What it is not:** This is a community connectivity and service model, not evidence of a LoRa or multi-radio mesh deployment.

Source: [ILO case study: Zenzeleni Community Networks (PDF)](https://www.anses.org.za/wp-content/uploads/2022/11/K-16351-ILO-Case-study_Zenzeleni_DevV3.pdf)

### Wave 7 Communications — Enfield, North Carolina

**Type:** Deployed rural fixed-wireless internet service

Wave 7 is a rural internet provider that expanded service in Enfield, North Carolina. A 2022 report describes the town library providing a location for the network hub and a fiber connection, while town buildings and water towers provided rooftop locations for wireless relay equipment. The provider also hired local technicians and partnered on digital-literacy support.

The article describes an early subscriber base and funding intended to expand the network to more than 400 households; that figure was an expansion target in the report, not a claim that all those households were already connected.

**Why it matters:** This is a nearby rural example of using existing community buildings and local partnerships to make fixed-wireless connectivity viable.

**Limitations:** It is a locally focused wireless ISP, not necessarily a community-owned mesh network. It is useful as a deployment and partnership example rather than a direct model for every part of this idea.

Source: [Wave of Wireless Connectivity Crests in Enfield, North Carolina](https://www.communitynetworks.org/content/wave-wireless-connectivity-crests-enfield-north-carolina)

---

## 2. LoRa networks: messaging, sensors, and gateways

LoRa is a low-power, long-range radio technology. It is useful when small messages need to travel farther or devices need to run for a long time on limited power. It is not intended to carry ordinary broadband traffic.

A useful distinction:

- **LoRa** is the radio modulation technology.
- **LoRaWAN** is a networking protocol commonly used for sensor and IoT deployments. Its usual topology is a star-of-stars: end devices send to gateways, and gateways forward messages to a network server.
- **LoRa mesh** uses nodes that relay messages through other nodes. This is a different network arrangement from standard LoRaWAN.

### Meshtastic

**Type:** Open-source project; working LoRa mesh messaging system

Meshtastic uses LoRa radios to create a mesh for text messages and small data. Phones and computers can connect to a radio over Bluetooth, Wi-Fi, or USB, then communicate with other radios in range of the mesh.

A typical arrangement is a phone connected by Bluetooth to a small radio. The radio handles the LoRa communication, and other radios relay messages across the network.

**Why it matters:** It is a concrete example of combining a familiar local interface (Bluetooth to a phone) with long-range radio between independent nodes. It can support messaging when cellular and internet service are unavailable.

**Limitations:** It is designed for messaging and small data, not as a general-purpose internet connection or a high-bandwidth network for images, video, or continuous machine control. Actual range and reliability depend heavily on terrain, antennas, radio settings, and local regulations.

Source: [Meshtastic — Getting Started / How it works](https://meshtastic.org/docs/getting-started/)

### The Things Network

**Type:** Community LoRaWAN ecosystem

The Things Network is a community-driven LoRaWAN ecosystem. People and organizations can install gateways that receive messages from nearby LoRaWAN devices and forward them to network services.

The important architecture detail is that LoRaWAN end devices do not normally relay each other's messages. They communicate with gateways. Gateways then use a backhaul connection such as Wi-Fi, Ethernet, or cellular to reach the network server.

**Why it matters:** It is an example of communities contributing coverage for low-power sensors, while using ordinary IP networking between gateways and servers.

**Limitations:** The usual LoRaWAN topology is not a peer-to-peer LoRa mesh. The Things Network's public community infrastructure and a privately operated local LoRaWAN deployment are also different operating models.

Sources:
- [The Things Network — LoRaWAN architecture](https://thethingsnetwork.github.io/docs/lorawan/architecture/)
- [The Things Network](https://www.thethingsnetwork.org/)

### ChirpStack

**Type:** Open-source, self-hostable LoRaWAN network server

ChirpStack is software for operating public or private LoRaWAN networks. It manages gateways, devices, and tenants, and provides interfaces for sending sensor data to other applications and databases.

Its components include gateway software and message forwarding through MQTT. The project also documents a Gateway Mesh option using relay gateways to extend LoRaWAN gateway coverage.

**Why it matters:** It provides an existing foundation for operating a local LoRaWAN system without building the network server and device-management layer from scratch.

**Limitations:** ChirpStack manages LoRaWAN networks; it is not a universal bridge for every radio protocol or a general-purpose community mesh.

Sources:
- [ChirpStack documentation](https://www.chirpstack.io/docs/)
- [ChirpStack architecture](https://www.chirpstack.io/docs/architecture.html)
- [ChirpStack Gateway Mesh](https://www.chirpstack.io/docs/chirpstack-gateway-mesh/)

---

## 3. Connecting different communication media

### Reticulum Network Stack

**Type:** Open-source networking software

Reticulum is a networking stack designed to work across different kinds of communication links. Its documented interfaces include Ethernet, Wi-Fi, LoRa through supported RNode hardware, packet radio, serial connections, and IP networks.

A small computer such as a Raspberry Pi can connect to more than one medium at once. For example, it could have a LoRa radio and a Wi-Fi or Ethernet connection. Reticulum can then carry network traffic between nodes reached through those different interfaces.

**Why it matters:** This is one of the closest existing software projects to the idea of a common network spanning several different physical media.

**Limitations and questions:** Reticulum's support for a medium depends on its documented interface and compatible hardware; do not assume every Bluetooth device is a native Reticulum interface. Its suitability for a particular community deployment, scale, and application still needs to be evaluated rather than assumed from the feature list.

Sources:
- [What is Reticulum?](https://reticulum.network/manual/whatis.html)
- [Supported interfaces and hardware](https://reticulum.network/hardware.html)
- [Interface configuration](https://reticulum.network/manual/interfaces.html)

### Private mobile networks and agricultural machinery — Finland, 2021 field trial

**Type:** Research field trial

A 2022 paper reports a smart-farm field trial in Finland that combined several networking technologies into interconnected local network areas, described as private network bubbles.

The tested setup included:
- A tactical wireless IP mesh network used as a backbone.
- Local private 4G and 5G access points.
- LoRaWAN-connected environmental sensors.
- Satellite connectivity as an additional communication path.
- A remotely controlled agricultural tractor, plus cameras and other farm monitoring equipment.

The trial is especially useful because it did not assume one wireless technology should do everything. Different links served different roles: sensor telemetry, local broadband, network backhaul, and backup connectivity.

**Why it matters:** It is a real field example of heterogeneous networking in an agricultural environment, including a mesh backbone and multiple other communication technologies.

**Limitations:** This was a research trial with specialized equipment and a remote-control use case. It is not a ready-made low-cost community network design. The paper is useful for understanding integration, performance requirements, and security concerns.

Source: [Unmanned Agricultural Tractors in Private Mobile Networks (2022)](https://www.mdpi.com/2673-8732/2/1/1)

### LoRaMESH + Wi-Fi HaLow — 2026 agricultural field experiment

**Type:** Research field deployment with measured results

A 2026 paper describes a two-level network for agricultural sensing:

1. LoRaMESH nodes form local multi-hop sensor clusters.
2. Cluster gateways use Wi-Fi HaLow (IEEE 802.11ah) as an IP backhaul to a central access point.

The researchers tested the system over an agricultural area approximately 2.5 km by 1 km. They reported more than 90% packet delivery across one to three LoRa mesh hops, with average end-to-end delays of about 10.6–13.3 seconds. In the tested Wi-Fi HaLow backhaul, packet delivery was 99.5% at 50 metres and 89.68% at 200 metres.

**Why it matters:** This is a direct example of combining a low-power sensor mesh with a longer-range IP-capable backhaul. It also provides measured performance rather than only a proposed architecture.

**Important caveat:** The paper did not conduct a same-testbed comparison against LoRaWAN or NB-IoT. The results validate the tested arrangement; they do not prove that it is universally better than alternatives. The reported delays also make it unsuitable to assume this network could carry time-critical control traffic.

Source: [Design and Experimental Evaluation of a Hierarchical LoRaMESH-Based Sensor Network with Wi-Fi HaLow Backhaul for Smart Agriculture (2026)](https://www.mdpi.com/1424-8220/26/9/2645)

---

## 4. Bluetooth Mesh for local devices

### Bluetooth Mesh

**Type:** Published standard / protocol

Bluetooth Mesh uses Bluetooth Low Energy to support many-to-many communication between devices. It is intended for local networks containing devices such as building controls, lights, sensors, and asset-tracking equipment.

Instead of only connecting one phone to one nearby device, mesh nodes can relay messages across a local network.

**Why it matters:** It could be useful inside a building, greenhouse, workshop, equipment shed, or other area with many nearby low-power devices.

**Limitations:** Bluetooth Mesh is a local device-network technology, not a replacement for farm-wide long-range networking. Its role would be different from LoRa, Wi-Fi, or Ethernet.

Sources:
- [Bluetooth SIG — Topology options](https://www.bluetooth.com/learn-about-bluetooth/topology-options/)
- [Bluetooth Mesh primer](https://www.bluetooth.com/bluetooth-mesh-networking-primer/)
- [Bluetooth Mesh FAQ](https://www.bluetooth.com/learn-about-bluetooth/topology-options/le-mesh/mesh-faq/)

---

## 5. Communicating when links are unavailable

### Delay-Tolerant Networking (DTN) and Bundle Protocol v7

**Type:** Networking architecture and standardized protocol

Delay-Tolerant Networking is designed for environments where links may be intermittent, delayed, disrupted, or unavailable for long periods.

Ordinary internet communication often assumes the sender and receiver can establish a usable end-to-end path at the same time. DTN instead allows a node to store a message and forward it later when a suitable connection becomes available. This is often called **store-carry-forward**.

Bundle Protocol version 7 (BPv7) is standardized in RFC 9171. It provides a way to carry bundles across networks that may have very different characteristics.

**Why it matters:** Imagine two communities with no continuous connection between them. Each can keep operating locally. When a radio link, scheduled connection, or physical data transfer becomes available, selected information can be forwarded.

**Limitations:** DTN is a communication architecture, not a complete application or automatic data-conflict solution. Applications still need policies for message expiry, duplicate handling, permissions, synchronization, and conflicts.

Sources:
- [RFC 9171 — Bundle Protocol Version 7](https://www.rfc-editor.org/rfc/rfc9171.html)
- [RFC Editor — RFC 9171 overview](https://www.rfc-editor.org/info/rfc9171/)

---

## 6. Broad research background

### Wireless Mesh Networking: An IoT-Oriented Perspective Survey on Relevant Technologies (2019)

**Type:** Research survey

This paper compares wireless technologies relevant to mesh networking, including Wi-Fi, Bluetooth, IEEE 802.15.4-family technologies, and sub-GHz LoRa.

It discusses how different technologies can support mesh networking, either natively or through adaptations, and considers heterogeneous Internet of Things scenarios such as smart cities and smart agriculture.

**Why it matters:** It provides a broad technical map of the radio technologies rather than focusing on one product. It is useful background for understanding why different parts of a network may need different radios.

**Limitations:** It was published in 2019, so it should be treated as foundational background, not a current hardware or product comparison.

Source: [Wireless Mesh Networking: An IoT-Oriented Perspective Survey on Relevant Technologies (2019)](https://www.mdpi.com/1999-5903/11/4/99)

---

---

## 7. Local computing and offline services

Connectivity is only one part of a useful community network. A local server can provide services to nearby devices even when there is no route to the wider internet.

### Internet-in-a-Box (IIAB)

**Type:** Open-source offline server project; used in education and community settings

Internet-in-a-Box provides an offline collection of educational and reference resources from a small computer, commonly a Raspberry Pi. Its documented content options include Wikipedia, books, Khan Academy videos, OpenStreetMap, and local publishing or learning tools. Connected users access the material through a local network and web browser.

**Why it matters:** It is a concrete example of a community or school having useful local information services without depending on an external website being reachable.

**Limitations:** The content must be selected, downloaded, stored, and updated. A local server also needs power, storage, backups, and someone responsible for maintaining it. The available content depends on what has been installed.

Sources:
- [Internet-in-a-Box project wiki](https://wiki.iiab.io/go/Main_Page)
- [Internet-in-a-Box FAQ and hardware guidance](https://wiki.iiab.io/go/FAQ)

### Kiwix and Kiwix Server / Hotspot

**Type:** Open-source offline knowledge software; hotspot deployment

Kiwix makes downloadable collections of web content available offline. Its ZIM archives can include resources such as Wikipedia and other reference collections. Kiwix Server can host those archives on a local network so that phones, laptops, and desktops can access them through an ordinary browser. Kiwix Hotspot packages this approach as a local Wi-Fi network with its own content server.

**Why it matters:** This is a relatively narrow, understandable local service that could be installed on a community server or small computer. It demonstrates that local information access does not require a custom network protocol.

**Limitations:** Kiwix is primarily a content distribution and reading system, not a general-purpose community collaboration platform. Content freshness depends on downloading and distributing updated archives.

Sources:
- [Kiwix Server](https://get.kiwix.org/en/solutions/applications/kiwix-server/)
- [Kiwix Hotspots](https://get.kiwix.org/en/solutions/hotspots/)
- [Kiwix FAQ: serving content on a local network](https://get.kiwix.org/en/faq/)

### Portable Network Kits (PNK) — Community Tech NY

**Type:** Community-built portable network kit; training and emergency standalone network

Community Tech NY describes a Portable Network Kit as a wireless network in a waterproof, battery-powered, solar-enabled case. It uses off-the-shelf hardware and open-source software to create a self-contained local network with a server, chat service, and document editor. It can also connect to an existing internet connection when one is available.

The kits were developed for community network education and have also been repurposed as standalone emergency networks. They are used as a hands-on teaching tool rather than sold as a standard off-the-shelf product.

**Why it matters:** This combines local connectivity, local applications, portable hardware, backup power, and community training in one real project. It is a particularly useful example of treating a network as something people can build and govern, not just consume.

**Limitations:** The project description establishes the kit's design and use, but does not provide a universal runtime, solar yield, or cost figure. Those would need to be measured for a particular build and location.

Sources:
- [Community Tech NY — Portable Network Kits](https://www.communitytechny.org/portable-network-kits)
- [PNK hardware and software documentation](https://community-tech-lab.github.io/PNK/)

---

## 8. Offline synchronization and local-first software

A local service can keep working while disconnected, but shared data creates another problem: two or more devices may change the same information before they can exchange updates.

"Synchronization" can mean several different things. Copying files, merging structured records, delivering queued messages, and reconciling edits to a shared document are not interchangeable operations.

### Syncthing

**Type:** Open-source peer-to-peer file synchronization

Syncthing synchronizes selected folders between devices. It exchanges file indexes and transfers changed file blocks between connected devices. It does not require a central cloud storage account; devices can synchronize directly when they can reach one another.

When two devices modify the same file independently, Syncthing detects the conflict and preserves a conflicting copy rather than silently treating the two edits as a semantic merge.

**Why it matters:** It is an existing solution for distributing documents, manuals, exports, and other files across devices that may not always be online together.

**Limitations:** Syncthing synchronizes files, not the meaning of application records. It cannot safely merge arbitrary database rows or understand whether two edits to an equipment record are compatible. Conflicting files may need human review. Synchronization is also not a substitute for independent backups.

Sources:
- [Syncthing — Understanding Synchronization](https://docs.syncthing.net/users/syncing.html)
- [Syncthing FAQ](https://docs.syncthing.net/users/faq.html)

### Automerge

**Type:** Open-source local-first data and synchronization library

Automerge provides data structures for applications that keep local copies of shared data on multiple devices. Each device can make changes independently, including while offline. When devices reconnect, Automerge exchanges changes and merges concurrent edits using conflict-free replicated data types (CRDTs).

Its data model is independent of a specific network transport. The project documents peer-to-peer and client/server synchronization, as well as exchanging data through one-way methods such as files on removable media.

**Why it matters:** This is closer to the shared-record problem than file synchronization. It provides an existing implementation to study before designing custom merge logic for things such as equipment registries, task claims, or shared plans.

**Limitations:** A CRDT does not automatically make every application-level conflict meaningful or desirable. The data model must be designed carefully, and authorization, identity, retention, backups, and application-specific validation still need to be handled. Automerge is a library for application developers, not a complete ready-to-deploy community information system.

Sources:
- [Automerge documentation](https://automerge.org/docs/)
- [Automerge network synchronization tutorial](https://automerge.org/docs/tutorial/network-sync/)
- [Automerge project overview](https://automerge.org/)

### Local-first software principles

**Type:** Research paper and design principles

The 2019 essay by Martin Kleppmann, Adam Wiggins, Peter van Hardenberg, and Mark McGranaghan describes software that treats users' local copies of data as primary, supports offline work, and enables collaboration across devices. It discusses the trade-offs of different storage and synchronization approaches, including CRDTs.

**Why it matters:** It provides a useful design lens for the community infrastructure idea: applications should not become unusable merely because a central server or internet connection disappears.

**Limitations:** The essay is a set of principles and technical discussion, not a deployment recipe. A local-first application still needs a carefully designed data model, permissions, synchronization behavior, and backup strategy.

Source: [Local-first software: You own your data, in spite of the cloud (2019)](https://www.inkandswitch.com/essay/local-first/)

---

## 9. Power-resilient local network examples

Power is not an accessory concern for offline infrastructure. It determines which nodes stay available, for how long, and which services need to be reduced or shut down during an outage.

### TakNet 2 — rural school wireless mesh, Thailand

**Type:** Deployed rural school mesh-network research project

The Asian Institute of Technology's intERLab describes a wireless mesh network deployed at Ban Mae Kued Luang School in Thailand in January 2014. The network used low-cost Wi-Fi routers running a self-configuring mesh protocol, with local USB storage used to cache educational video and ebook content.

The project page reports a maximum router power consumption of 5 W, optional solar power, and an optional internal battery described as providing more than four hours of operation. The deployment included an educational video-on-demand service.

**Why it matters:** This is a useful earlier example of combining a local content service with a rural Wi-Fi mesh, while documenting a concrete power figure and battery option.

**Limitations:** It is a 2014 deployment and should not be treated as a current hardware recommendation. The stated power and runtime describe the project's equipment and configuration, not a universal mesh-node budget.

Source: [TakNet 2 — A rural school wireless mesh network](https://interlab.ait.ac.th/cwmn/tak2.php)

### Portable Network Kits — solar and battery-enabled network

The Portable Network Kit described above is also relevant here: its documented design combines a battery-powered case with solar capability and a local server. The public project description does not specify a standard runtime or power budget, so it is evidence of a working design approach, not a quantified energy benchmark.

Source: [Community Tech NY — Portable Network Kits](https://www.communitytechny.org/portable-network-kits)

### Zenzeleni Networks — community-owned solar-powered access

The Zenzeleni case study already listed in §1 describes community Wi-Fi hotspots using solar, battery, or mains power, with local cooperative ownership and operation. It is useful as an example of power choices being connected to maintenance and local responsibility, rather than treated as a hardware-only decision.

Source: [ILO case study: Zenzeleni Community Networks (PDF)](https://www.anses.org.za/wp-content/uploads/2022/11/K-16351-ILO-Case-study_Zenzeleni_DevV3.pdf)

**What these examples do not settle:** A reliable design still needs a measured energy budget for each node type, including radio duty cycle, compute load, battery reserve, seasonal solar availability, weather, temperature, and the desired outage duration. A published wattage for one router is not enough to size a community network.

---

## 10. What these examples collectively show


There is no need to assume that one project already provides every part of a distributed community infrastructure system. The examples above tend to solve different layers of the problem.

| Need | Existing work worth examining |
|---|---|
| Community ownership and network governance | guifi.net / Network Commons, Zenzeleni |
| Volunteer-operated local mesh networks | Freifunk |
| Rural wireless deployment and local partnerships | Wave 7 |
| Long-range off-grid messaging | Meshtastic |
| Community-operated sensor coverage | The Things Network |
| Self-hosted LoRaWAN management | ChirpStack |
| One network across different physical links | Reticulum |
| Local many-to-many device networks | Bluetooth Mesh |
| Store-and-forward between disconnected places | DTN / Bundle Protocol |
| Multi-radio farm networking | 2022 private-network field trial |
| LoRa mesh with IP backhaul | 2026 LoRaMESH + Wi-Fi HaLow field trial |\n| Local offline information services | Internet-in-a-Box, Kiwix, Portable Network Kits | Local content and applications without internet |\n| File synchronization | Syncthing | Peer-to-peer file distribution and conflict preservation |\n| Structured offline collaboration | Automerge, local-first software | Replicated records and concurrent change merging |\n| Power-aware network deployments | TakNet 2, Portable Network Kits, Zenzeleni | Low-power nodes, battery/solar operation, local maintenance |

### Important distinctions

A few technologies can sound similar while solving different problems:

- A **mesh** lets nodes relay traffic for other nodes. Not every wireless network is a mesh.
- **LoRaWAN** normally uses end devices talking to gateways; it is not the same thing as a LoRa mesh.
- A **gateway** connects network segments, but it does not automatically translate every protocol or make their data mutually understandable.
- **Reticulum** is an example of a networking layer that can use different underlying media; it is not the same thing as a LoRaWAN server.
- **DTN** addresses delayed and intermittent delivery. It does not by itself decide how two applications reconcile conflicting records.
- **Community ownership** is an organizational and governance question, not something a mesh protocol provides automatically.

### Early observations for this project

1. **The community-network examples are as important as the radio examples.** Long-lived infrastructure needs local operators, documentation, funding, equipment records, and succession plans.
2. **A mixed network is a realistic pattern.** The research includes examples where low-power sensor links, local wireless networks, wired backbones, and other backhaul links each have a distinct role.
3. **Local services and internet access are separate concerns.** A community can operate useful local applications even when it cannot reach the wider internet; the examples vary in how much they address this.
4. **Disconnected operation needs its own design.** A network that can relay a message is not automatically a system that can synchronize databases, resolve conflicts, or manage identities while offline.
5. **Performance depends on the job.** A sensor reading that arrives after ten seconds may be useful; a safety-critical machine-control message has very different requirements.
6. **There is substantial prior art to reuse.** Community-network projects, LoRaWAN servers, messaging systems, mesh protocols, and DTN standards should be investigated before designing custom replacements.\n7. **A local network can be a useful environment, not just a transport.** Internet-in-a-Box, Kiwix, and Portable Network Kits show that local knowledge, chat, and collaboration services can run without internet access.\n8. **File sync and structured-data merging are different jobs.** Syncthing is useful for files; Automerge is an example of a CRDT-based application data layer. Neither removes the need to design permissions, identity, backups, and conflict semantics.\n9. **Power examples need to be read at the right scale.** TakNet 2 documents a specific low-power deployment, while PNK and Zenzeleni show solar/battery approaches. None provides a universal node power budget.

These are research observations, not settled architectural decisions.

## 11. Useful next research questions

- Which community networks have published practical operating, maintenance, funding, and governance models?
- What are the real costs, power requirements, and maintenance burdens of LoRa, Wi-Fi mesh, Wi-Fi HaLow, Bluetooth Mesh, and wired links?
- How do projects bridge different network technologies without making the gateway a fragile single point of failure?
- Which existing systems support local services and store-and-forward operation without requiring a cloud account?
- How do offline systems handle identity, revocation, synchronization, and conflicting records?
- Which projects have been maintained successfully by non-specialists over several years?
- What can be learned from community networks outside agriculture, including rural broadband, disaster resilience, environmental monitoring, and remote research stations?

## Research status

This is an initial desk-research collection, not an exhaustive survey. The summaries are based on the linked project documentation, standards, case studies, and papers. No hardware was tested and no independent performance replication was performed.

The next pass should deepen the most relevant examples, check their current maintenance and deployment status, and compare practical costs and limitations before any implementation decisions are made.
