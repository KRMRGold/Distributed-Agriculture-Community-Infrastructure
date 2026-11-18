Distributed Agriculture & Community Infrastructure
Nov 16, 2026 · @Roxy · v0.5.2 · exploratory ideation

1. What this document is (and isn't)
This is an ideation document. It exists to think through an idea before it becomes a design doc. It is a place to park decisions, open questions, and the reasoning behind them while the idea is still forming. It is not a specification, not a build plan, and not a commitment to any particular implementation.

The idea it explores: an open, modular ecosystem of agricultural robots, distributed sensors, local computing, and resilient networks for farms and geographically isolated communities, built so a community can keep farming, share resources, monitor infrastructure, and exchange information without continuous internet access or a central service provider.

How to read it. Each section says what is decided, what is still open, and what the decision criteria are. The language distinguishes three things:

What we believe — design choices we've made and why.

What we require — things an implementation would need to do if it claims to follow this idea. These are marked with "shall" and are internal to this document, not claims of conformance to any external standard.

What we don't know yet — questions in §12, tagged with the phase that needs an answer.

What this is for. The goal is to get the idea to a state where a real design doc can be written: one that answers the questions in §12, resolves the open decisions in §8.1, and commits to specific choices instead of listing candidates. This document is the scaffolding for that.

Relationship to ITU-T Y.2256 (09/2024). ITU-T Y.2256 is a standards Recommendation from the International Telecommunication Union (ITU) on network-based unmanned smart farms. It's a useful reference point because it describes a similar system from the outside. But Y.2256 assumes a network that is there, and this idea assumes a network that might not be there. So we note where the idea aligns with Y.2256 and where it deliberately departs — not because conformance matters yet, but because the comparison sharpens what's different about this idea. How far that alignment goes is itself an open question (§12); so far we've only read Y.2256's summary and table of contents.

AI-assisted provenance and intended use

This document was developed through a human-directed, AI-assisted process. Research was gathered, summarized, compared, drafted, edited, and iterated with AI. The document records a developing line of thought; it is not an independently validated expert assessment, formal specification, or final design. AI-assisted summaries and interpretations may be incomplete or mistaken, and linked sources should be consulted for consequential technical claims.

This is also a handoff document. A reader can use it to get the basic idea, or give it to their own AI assistant and ask for a plain-language summary, a critique, an explanation of unfamiliar areas, or help exploring whichever branch interests them. The reader is not expected to agree with the current framing or follow every direction. The intention is to make the accumulated context useful and portable so someone else can take it forward in their own way.

1.1 Requirement convention
"Shall" marks a requirement that an implementation following this idea must meet. "Should" marks a recommended practice. "May" marks an option. A heading marked "(a requirement)" or "(requirements)" contains at least one "shall". These terms are internal to this document and do not imply conformance to any external standard. Where a requirement cannot be met, the deviation and its rationale shall be recorded in the deployment's decision record (§13, phase 1).

2. Design principles
Seven principles guide the idea. Each has a test: something you could actually do to check whether a proposed implementation honors it.

Local first. Every service a farm or community depends on runs locally and keeps working when the internet is gone. Test: cut the wide-area network (WAN) link; monitoring, control, and records still function.

Degrade, don't fail. Losing a link, a node, or a power source reduces capability in steps; it never stops the whole system. Test: every tier has a documented degraded mode and a way back.

Safety stays on the machine. Robots make their own safety and stopping decisions, independent of any network. Test: a network compromise cannot command unsafe motion.

Repairable and affordable. Parts and designs can be sourced, fixed, and replaced locally. Test: a non-specialist can restore a node from backup using written instructions.

Incremental growth. One sensor and one gateway is already a working system; scale comes by adding nodes. Test: the minimum viable deployment is defined and functional.

Community-controlled data. Communities decide what leaves their network, to whom, and on what terms. Test: every egress path is logged and permissioned.

Zero implicit trust. No device, node, or community is trusted just because it's on the network. Test: every session is authenticated and every update is verified.

3. Architecture (working shape)
Five layers. The power layer sits underneath the other four because it limits what they can do during an outage.

Layer	What's in it	The main constraint
Power	Solar, batteries, charge controllers, load shedding, duty-cycle schedules	Sets how long everything above it stays up
Device	Robots, soil and weather sensors, greenhouse, water and energy monitors, community computers	Must run standalone and report a standard status
Communication	Long-range radio (LoRa and LoRaWAN), LoRa mesh, Bluetooth Low Energy (BLE) and BLE Mesh, Wi-Fi and Wi-Fi mesh, Wi-Fi HaLow, Ethernet, joined by gateways	No single technology is required
Computing	Device firmware, robot-local compute, field gateways, farm servers, community servers, optional cloud	Each tier works without the one above it
Application	Monitoring, robot coordination, crop and soil data, resources, messaging, maintenance, energy and water, shared documentation	Built only on shared interfaces, never on a specific radio
3.1 How this relates to Y.2256
Y.2256's table of contents lists three functions: data processing, environmental control, and smart machines control. (Basis: table of contents only; we haven't read the full text. See §11.1 and §12.) The idea here spreads those functions across tiers instead of putting them in single nodes:

Data processing is spread across device firmware, field gateway, farm server, and community server. The "lowest tier that can answer" rule (§3.3) applies.

Environmental control runs on sensors and control devices at the device and field-gateway tiers; the communication layer carries the traffic.

Smart machines control runs on the robot itself, bounded by the safety boundary in §4.1.

Y.2256 also has a section of service requirements for network-based unmanned smart farms. We treat it as a reference to check against the full text later, and add an offline-tolerant operating profile on top. Whether Y.2256 already deals with disconnected operation is open (§12): its framing assumes a network, while this idea assumes the network might be absent.

3.2 Power as a design input
Power budgets get written down per node type before radios and sensors are chosen. Each node declares:

sleep current,

how often it samples,

radio duty cycle,

battery reserve,

how long it should survive an outage.

Gateways and community servers publish a power state so applications can drop non-essential work (big syncs, video) when batteries are low. Power state is a first-class field in the standard status report (§4.2).

3.3 Computing tiers
A request is served at the lowest tier that can answer it.

Request	Served at
Soil alarm	Sensor or field gateway
Field plan	Farm server
Cross-farm query	Community server
Cloud	Optional mirror and backup, never a dependency
Each tier has to work without the one above it. The farm server must serve all farm-local requests during a community-backbone outage; the field gateway must serve alarms and control during a farm-server outage.

4. Robotics and safety
The idea targets a family of small, specialized machines that share a common interface — not one universal robot. Low-risk, low-speed platforms come first.

4.1 Safety boundary (a requirement)
Safety logic lives on the robot and is never delegated to the network. Every platform shall have:

a hardware emergency stop;

a watchdog that halts motion when the controller stalls;

geofencing enforced on board;

a defined safe state for lost communication, low battery, or sensor failure.

Remote commands may ask for a task but cannot override these limits. Any smart-machine control command — including anything Y.2256 defines — is treated as a task request bounded by this boundary.

Still open: supervised transport vehicles probably fall under ISO 18497 (a standard for highly automated agricultural machinery), so the modular and interchangeable goal needs checking against certification and liability early. Lighter scout robots probably face fewer requirements, but that needs confirming, not assuming.

4.2 Shared interface (a requirement)
Every platform shall provide:

a unique device identifier (ID);

a standard status report: position (with a declared accuracy class — see §10), battery, faults, current task, power state;

swappable sensor mounts;

local operation during outages;

remote diagnostics;

a way to claim shared work.

4.3 Platforms, in priority order
Scout rovers for crop and environment observation.

Greenhouse monitoring robots.

Field maintenance robots.

Modular sensor and equipment carriers.

Supervised transport vehicles — last, highest risk, gated on the §4.1 standards review.

4.4 Coordinating without connectivity (a requirement)
Robots coordinate through simple, loosely coupled rules:

tasks are posted with an area, a deadline, and a priority;

a robot claims one and publishes its claim when it can;

conflicts resolve by a deterministic rule (e.g., lowest ID wins) once robots reconnect.

No live leader is needed. Claim records are a conflict-free replicated data type (CRDT) compatible shared record (§6). A claim record shall carry a monotonic claim counter and a deterministic tie-break key (robot ID), so merged records converge on the same winner without central arbitration.

5. Communications
Different links do different jobs. The network is several technologies joined by gateways; no single technology is required.

Technology	Best for	Limits to check per region
Long-range radio (LoRa) and LoRaWAN	Sensor telemetry, alarms, long range	Tiny payloads, duty-cycle limits, low throughput
LoRa mesh	Experimental multi-hop sensor and short-message relay	Scaling and reliability under load unproven
Bluetooth Low Energy (BLE) and BLE Mesh	Short-range pairing, provisioning, local sensors	Tens of meters; mesh adds latency
Wi-Fi and Wi-Fi mesh	Robots, cameras, field and farm backbone	Range and power draw; coverage planning needed
Wi-Fi HaLow	Sub-GHz internet-protocol (IP) backhaul between buildings or fields	Newer, fewer hardware options, cost to confirm
Ethernet	Farm backbone, servers, gateways	Cable runs and trenching
5.1 Traffic classes (a requirement)
Every message shall carry a priority, and constrained links shall serve the most important first:

Safety and alarms — frost, flood, fault, emergency-stop (e-stop) events.

Control and robot task messages.

Routine telemetry.

Bulk data — logs, images, updates, documents.

5.2 Required behaviors (requirements)
Local device discovery and gateway bridging between radio domains.

Store-and-forward queues on every gateway, with expiry rules so stale data doesn't clog links.

Prioritization, and recovery with resynchronization after an outage.

Multiple paths shall be used, with automatic fallback, where two independent links exist between the same tiers. Where only one link exists, that tier's single-path operation shall be recorded in the power budget (§3.2) as its degraded mode.

Topology: device → local network → gateway → farm network → community backbone, with community gateways linking farm networks.

6. Data model and synchronization
Data has to move and merge across links that are slow, intermittent, and sometimes one-way. This is the hardest choice in the idea, and it should be settled before hardware. Each data class gets a declared sync model.

Class	Examples	Likely sync model
Telemetry	Soil moisture, temperature, tank levels	Append-only time series, deduplicated by device ID and timestamp
Events and alarms	Frost warning, pump fault	Priority messages with acknowledgment
Shared records	Equipment registry, resource requests, task claims	Eventually consistent, mergeable (e.g., CRDTs)
Documents	Repair guides, crop notes	Versioned files, synced when bandwidth allows
Software and firmware	Updates	Signed bundles, verified before install
6.1 Candidate transports (none chosen yet)
MQTT with bridging — Message Queuing Telemetry Transport (MQTT) is familiar and well supported, but assumes reasonably stable connections and needs care with offline queues.

Delay-tolerant networking (Bundle Protocol) — designed for intermittent links, but tooling is less mature for small farms.

Eventually consistent replicated data (CRDTs or similar) — suits shared records edited at several sites; adds complexity and storage overhead.

Hybrid — MQTT locally, bundle or sync-based transport between farms and communities. This is the working hypothesis to test first.

6.2 Time (a requirement)
Devices without internet need a trusted time source. Every deployment shall define:

Global Navigation Satellite System (GNSS) time at gateways as the primary source;

a clock-drift policy for sensors;

logical ordering (sequence numbers) where timestamps can't be trusted. Sequence numbers shall be per-device and persist across reboots; a replaced device starts a new stream under a new device ID, so reordering never conflates the two.

7. Identity, security, and trust
Trust has to be established and revoked without internet access. Each community runs its own root of trust rather than relying on a public certificate authority.

7.1 Offline identity (requirements)
Each Community Node runs a local certificate authority (or equivalent key hierarchy) that enrolls devices.

Devices get a unique key pair at provisioning, ideally in a secure element; provisioning happens locally (BLE or cable).

Revocation lists are distributed through the same store-and-forward paths as other data. Revocation is delayed on isolated links, so short-lived credentials limit exposure. Credential lifetime shall be no shorter than the longest expected isolation of a device from its certificate authority (otherwise offline devices lock themselves out) and no longer than the exposure window the community accepts. Where no lifetime satisfies both, the gap shall be recorded as a deviation (§1.1).

Key recovery has a documented plan: an offline backup of the root key, held by more than one trusted holder.

7.2 Baseline requirements
Every deployment shall provide the following. Each is a deployment-level requirement; the phase that first tests it is noted in §13.

Encrypted communications wherever the link supports it; on LoRa, payload encryption with per-device keys.

Role-based access control with least privilege for people, devices, and applications.

Network segmentation: robot control, sensors, and community services on separate segments.

Signed software updates, verified on the device before install, with rollback.

Local backups and tested restores, including recovery after power or network loss.

Explicit, logged permissions for any information leaving the community.

7.3 Threats this is designed against
A compromised or stolen sensor; a rogue gateway; replayed or spoofed robot commands; a malicious update; a neighboring community node that's no longer trustworthy. Robot-local safety limits (§4.1) mean a network compromise can't by itself command unsafe motion.

8. Community Nodes and governance
A Community Node is the shared local hub. Communities own their nodes and their data.

What a node provides: local storage and backup; device registration and the local certificate authority; gateway services; local applications; equipment and resource registries; agricultural information; shared documentation; optional synchronization with other communities.

8.1 Governance decisions (must be settled)
Question	Why it matters	Who decides	Needed by phase
Who operates a node, and who can replace the operator?	A node with one volunteer is a single point of failure	Community	4
Who may register devices and issue credentials?	Determines the root of trust	Community	4
What happens if the operator leaves or a dispute arises?	Needs key handover, data export, fallback node	Community	4
Who pays for and owns shared infrastructure?	Affects repair, upgrades, access rules	Community	4 (may settle during)
How are shared equipment and resources booked, and disputes resolved?	Drives what the resource registry needs	Community	4 (may settle during)
What data is private, farm-shared, community-shared, or public?	Drives access control and inter-community sharing	Community	4 (may settle during)
The operator, credential-issuance, and key-handover questions are blocking for phase 4; the other three may be settled during phase 4.

8.2 Node resilience (a requirement)
Each community shall run at least two nodes. Where only one node is operated, that shall be recorded as a deviation (§1.1), with the single point of failure and its mitigation described. Phase 4 (§13) tests this: an operator other than the builder restores a node from backup using written instructions.

9. Inter-community connectivity
Communities operate independently and connect only by choice. Each connection is optional, can be cut without breaking either side, and carries only what its owner has explicitly allowed.

What can be shared: agricultural and environmental observations; equipment availability; repair documentation; resource requests; signed software updates.

How it works (requirements):

Peering is bilateral and explicit: each community decides which data classes it sends and accepts, per partner.

Shared data is labeled with origin and permissions, so a receiver can verify and honor them.

Links may be intermittent or even physical (a vehicle carrying a signed data bundle); delay-tolerant transfer is a first-class case, not an edge case.

Reputation and trust between communities are handled locally; no global registry is required.

Peering shall be revocable unilaterally. Revocation takes effect immediately on the revoking side; the partner is notified at the next successful link contact, and the revoking community shall treat data received after its revocation notice as untrusted. The partner shall stop sending the revoked data classes on receipt of the notice and shall acknowledge revocation at the next opportunity. Data the revoking community received before the partner's acknowledgment may be kept but shall be marked "pre-revocation" and excluded from any onward sharing.

10. Shared positioning (RTK GNSS)
Centimeter-level positioning is what makes autonomous rovers practical, and it's a natural shared community service. A community can host a fixed base station and broadcast corrections to every robot and survey tool over local Wi-Fi or LoRa — spreading cost and working offline. Real-Time Kinematic (RTK) is the technique that makes the corrections precise.

Open questions: how corrections reach fields beyond Wi-Fi range; who maintains and surveys the base station; and what each robot does when corrections drop. Each robot's fallback behavior shall be declared in its status report and shall be one of: degrade to standard GNSS, with the accuracy loss declared as a position-accuracy class and without claiming precision-dependent tasks (seeding, precision spraying) while degraded; or execute the §4.1 safe state.

11. Prior art and reusable projects

For readable summaries of existing community networks, communication projects, standards, and field research, see [Existing Projects & Research](research-landscape.md). This is a separate research collection; the summaries inform exploration but do not create design decisions.
None of the sources below have been reviewed in depth yet. The first research task is to confirm what each one actually provides. Y.2256 has been checked only at the level of its summary and table of contents.

11.1 Papers and standards
Unmanned Agricultural Tractors in Private Mobile Networks (2022)

Design and Experimental Evaluation of a Hierarchical LoRaMESH-Based Sensor Network with Wi-Fi HaLow Backhaul for Smart Agriculture (2026)

Wireless Mesh Networking: An IoT-Oriented Perspective Survey on Relevant Technologies (2019). IoT here means the Internet of Things.

ITU-T Y.2256 (09/2024), Overview of unmanned smart farms based on networks — in force; summary and table of contents reviewed 2026-11-16.

11.2 Candidate open-source projects
Area	Projects	Question to answer
Long-range networking	Meshtastic, ChirpStack, Reticulum	Which handles small, encrypted, store-and-forward messages reliably at community scale?
Robotics	Robot Operating System 2 (ROS 2), AgOpenGPS	What can be reused for navigation, status reporting, and safety layers?
Open agricultural hardware	FarmBot, Open Source Ecology	Which designs are repairable and documented well enough to build locally?
Community networks	Freifunk, guifi.net	What governance and operating models have lasted?
Edge applications	Home Assistant, Node-RED	Can they serve as the local application and automation layer?
11.3 Fields to survey
Wireless sensor networks, delay-tolerant networking, edge computing, distributed systems, precision agriculture, agricultural robotics, open hardware, community networks, federated infrastructure.

12. Open research questions
Each question has a disposition: blocking phase N means that phase can't exit without an answer; informational means it shapes the design but doesn't gate a phase. Phase 1's exit criterion (§13) includes sorting every question below into one of these, or "out of scope."

Carried over from the original draft

Question	Disposition
Which networking technologies best balance cost, range, power, and maintainability?	Blocking phase 2
How do heterogeneous networks discover and talk to one another?	Blocking phase 3
How can robots coordinate without continuous connectivity?	Blocking phase 3
How should community computing be organized?	Blocking phase 4
How can communities share resources and keep local control?	Blocking phase 5
What hardware can realistically be repaired and maintained locally?	Blocking phase 2
How should the ecosystem handle different scales of deployment?	Informational
Which existing open-source projects can be reused?	Blocking phase 2
Added in v0.2.0

Question	Disposition
What is the power budget per node type, and how long must each tier survive an outage?	Blocking phase 1
Which sync model (MQTT, bundles, CRDTs, or hybrid) fits each data class?	Blocking phase 1
How are identities issued, revoked, and recovered with no internet?	Blocking phase 1
Which safety standards and liability rules apply to each robot class, and where?	Blocking phase 3
Who governs a Community Node, and what happens when an operator leaves?	Blocking phase 4
How are RTK corrections shared, and what happens when they're lost?	Blocking phase 3
What can LoRa realistically carry at the node counts a community would deploy?	Blocking phase 2
Added in v0.3.0

Question	Disposition
Does Y.2256 support a formal conformance claim, and if so, how does a deployment show alignment with its service requirements when network interactions are served through store-and-forward paths?	Informational
Does Y.2256 define authentication, authorization, or service-registration procedures, and if so, what's the offline-tolerant variant?	Informational
Do the entity names and service requirements in the full text of Y.2256 match the function-level mapping in §3.1?	Blocking phase 1
Where does Y.2256's network-centric reference architecture conflict with an offline-first design?	Informational
13. Development phases
The idea stays in ideation and research until phase 1's exit criteria are met. Each phase ends with a decision, not just a deliverable.

Phase	What happens	How we know it's done
1. Research and decisions	Review the papers and candidate projects; estimate power budgets; choose the sync model, identity scheme, and governance approach; read Y.2256's full text against §3.1	A short decision record for each of those four choices, plus a triage of every §12 question into "resolved," "blocking phase N," or "out of scope," and confirmation that no "blocking phase 1" question remains unresolved
2. Bench prototype	2–3 sensors, a LoRa gateway, a farm server, and a local dashboard, running offline for a week on battery or solar	Measured range, battery life, and message loss against the power budget
3. Single-farm pilot	Store-and-forward gateway, signed update path, offline certificate authority, one low-risk scout rover with on-board safety stops	Recovery drill: cut power and internet, restore, confirm no data loss
4. Community node	Second farm, shared registries and documentation, Wi-Fi or HaLow backbone	An operator other than the builder restores a node from backup using written instructions
5. Inter-community pilot	Two communities with explicit, logged sharing rules	Either side can disconnect with no loss of local function
Transport vehicles and other higher-risk robots wait until the §4.1 safety standards review is complete.

14. Change log
From v0.2.0 to v0.3.0
Promoted principles to testable form (§2).

Added a function-level mapping to ITU-T Y.2256 (§3.1), worded as alignment rather than conformance.

Made power budgets and power-state reporting requirements (§3.2, §4.2).

Promoted the safety boundary and shared robot interface to requirements with "shall" language (§4.1–4.2).

Linked task-claiming to the CRDT shared-record class (§4.4, §6).

Made store-and-forward, prioritization, and time policy requirements (§5.2, §6.2).

Made the offline certificate authority, short-lived credentials, and root-key recovery requirements (§7.1).

Added a governance decision-owner column and the two-node resilience rule, tied to the phase 4 exit test (§8).

Added the RTK fallback declaration requirement (§10).

Added four Y.2256 research questions and labeled open questions by version (§12).

Converted development phases to a table with explicit exit criteria (§13).

Corrections made while reviewing v0.3.0
Removed Y.2256 entity names and service-requirement details that couldn't be confirmed from its summary and table of contents.

Reworded "conformance baseline" to "reference baseline to be reviewed" (§1, §3.1).

Fixed a cross-reference in §4.4 that pointed to the traffic classes (§5.1) instead of the data model (§6).

Resolved the conflict between §8.2 and the phase 4 exit criterion.

Added a note on Y.2256's network-centric framing versus this idea's offline-first assumption (§3.1, §12).

Changes in v0.4.0
Added §1.1, defining "shall," "should," "may," and "(normative)" as internal terms that don't imply external conformance.

Added a basis note to the §3.1 claim about Y.2256's three functions, corrected to cite the table of contents (the summary doesn't list them); full text not yet reviewed.

Added a CRDT convergence rule for claim records in §4.4 (monotonic counter plus deterministic tie-break key).

Replaced "multiple paths where practical" in §5.2 with a decision rule: use multiple paths where two independent links exist, otherwise document the single-path degraded mode.

Added a per-device, reboot-persistent sequence-number rule in §6.2; a replaced device starts a new stream under a new ID.

Bounded credential lifetime in §7.1 from both sides: no shorter than the longest expected isolation from the certificate authority, no longer than the accepted exposure window, with any gap recorded as a deviation. This corrected the draft rule, which required lifetime to exceed revocation delay and wouldn't have limited exposure.

Added a "Needed by phase" column to the §8.1 governance table.

Added unilateral peering revocation to §9: immediate on the revoking side, partner notified at next contact, later data untrusted.

Limited §10 RTK fallback to two outcomes: degrade to standard GNSS with documented accuracy loss, or enter the §4.1 safe state.

Restructured §12 as disposition tables and added the §12 triage and the Y.2256 full-text review to phase 1 in §13.

Changes in v0.5.0
Reframed §1 as an ideation document: what it is, what it's for, how to read it, and how the Y.2256 comparison is being used (as a sharpening tool, not a conformance target).

Added a position-accuracy class to the §4.2 status report and tied the §10 RTK fallback to it; degraded robots may not claim precision-dependent tasks.

Rewrote the §5.2 multiple-paths rule to remove the circular "documented as that tier's degraded mode" phrasing and point the degraded-mode record at the power budget (§3.2).

Stated §7.2's bullets as deployment-level requirements with phase pointers.

Replaced "where feasible" in §8.2 with a deviation-recording rule, consistent with §1.1.

Added partner-side revocation behavior to §9 (stop, acknowledge, mark pre-revocation data, exclude from onward sharing).

Moved "Which existing open-source projects can be reused?" from blocking phase 1 to blocking phase 2, and added a phase 1 exit check that no blocking-phase-1 question remains unresolved.

Corrected the v0.4.0 changelog line about §5.1/§7.2 to describe what was actually changed.

Tightened prose throughout for readability: replaced "normative" in headings with plain "(a requirement)" or "(requirements)," simplified tables, and removed circular phrasing. This includes §6.2, §7.1, §7.2, and §9, which still carried "(normative)" headings in the v0.5.0 draft.

Changes in v0.5.1
Kept the header as chosen (title line plus a version line reading "Nov 16, 2026 · @Roxy · v0.5.1 · exploratory ideation").

Expanded acronyms on first use throughout the document, or replaced them with the plain term where the acronym wasn't earning its keep:

WAN → wide-area network (§2).

ITU → International Telecommunication Union (§1).

LoRa / LoRaWAN → long-range radio and the LoRaWAN protocol, introduced together (§3, §5).

BLE / BLE Mesh → Bluetooth Low Energy and BLE Mesh (§3, §5).

IP → internet protocol (§5).

e-stop → emergency stop (§5.1).

CRDT → conflict-free replicated data type (§4.4; §6.1 uses "CRDTs or similar" after the expansion).

MQTT → Message Queuing Telemetry Transport (§6.1).

GNSS → Global Navigation Satellite System (§6.2; §10 keeps "GNSS" after the expansion).

RTK → Real-Time Kinematic, with a short gloss on what it does (§10).

ISO 18497 → given a short descriptive gloss ("a standard for highly automated agricultural machinery") the first time it appears (§4.1).

ROS 2 → Robot Operating System 2 (§11.2).

IoT → Internet of Things, glossed once in the paper title it appears in (§11.1).

ID → device identifier on first use, then "ID" thereafter (§4.2, §4.4).

Lowercased "certificate authority" in running prose (§7.1, §8, §13) while keeping it capitalized in the defined-term sense where it appears as part of a named role. No meaning changed.

No requirements, decisions, dispositions, or phase criteria were altered.

A couple of notes on judgment calls:

LoRa is a technology name, not an acronym in common use (it's derived from "long range"), so I introduced it as "long-range radio (LoRa)" once and then used "LoRa" plainly. Same for LoRaWAN, which is the protocol on top of it.

CRDT is expanded at first use in §4.4 and then used freely, because it appears repeatedly and the expansion is long. Same for MQTT and GNSS.

ISO 18497 is a standard number, not an acronym, so I gave it a descriptive gloss rather than spelling anything out.

Community Node was capitalized inconsistently in the draft; I kept it capitalized where it's used as the defined term and left it alone otherwise. No acronym involved, just consistency.
