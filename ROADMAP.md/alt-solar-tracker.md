# Dual-FlatSat Cyber-Physical Space Systems Research Project

## Students
- **Fernanda Orduno** (Student 1 - Hardware Lead)
- **Julio Sandoval** (Student 2 - Communications Lead)
- **Jesus Garcia** (Student 3 - Cyber-Physical Integration Lead)
- **Jack Pritchard** (Student 4 - Cybersecurity Lead)

## Overview

> **Guiding Principle:** Prioritize working, reproducible experiments over adding features. A simple experiment that works repeatedly and produces measurable data is more valuable for research than a complicated demonstration that works only once.

**Research Question:** How can two low-cost FlatSat platforms be used to create reproducible cyber-physical spacecraft security experiments?

| Week | Dates | Focus | Checkpoint / Deliverable |
| :--- | :--- | :--- | :--- |
| **[Week 1](#week-1)** | 10/05 - 10/11 | Bring-Up and Division of Responsibilities | Both FlatSats boot and team members can interact with them. |
| **[Week 2](#week-2)** | 10/12 - 10/18 | Hardware and Communication Characterization | Architecture diagram showing hardware, software, comms, and control paths. |
| **[Week 3](#week-3)** | **10/19 - 10/25** | First FlatSat-to-FlatSat Communication | Live cross-FlatSat control demonstration (First major go/no-go). |
| **[Week 4](#week-4)** | 10/26 - 11/01 | Build the Control Framework | Multiple physical controls reliably generate remote commands. |
| **[Week 5](#week-5)** | 11/02 - 11/08 | Solar Tracker Telemetry Integration | Live telemetry dashboard displaying real-time physical changes. |
| **[Week 6](#week-6)** | 11/09 - 11/15 | Baseline Experiments | Baseline dataset and reproducible experiment procedure. |
| **[Week 7](#week-7)** | 11/16 - 11/22 | Cybersecurity Experiments | At least two reproducible attacks with observable physical effects. |
| **[Week 8](#week-8)** | 11/23 - 11/29 | Evaluation | Experimental results showing what distinguishes legitimate vs manipulated interaction. |
| **[Week 9](#week-9)** | 11/30 - 12/06 | Final Demonstration and Freeze | Final live demonstration, documentation, and report. |

---

## Team Roles & Subsystem Ownership

### Fernanda Orduno — Student 1: FlatSat Hardware Lead
**Primary Goal:** Understand, configure, and document the physical FlatSat platform.
*   **Tasks:**
    *   Inventory both FlatSats and their major components.
    *   Identify usable processors, interfaces, sensors (LIS2DH12 accelerometer, BME280 temp sensor), buttons, switches, LEDs, and communication interfaces.
    *   Establish reliable startup and recovery procedures.
    *   Determine which hardware components can be controlled programmatically.
    *   Develop basic hardware test programs.
    *   Support integration of physical controls and measure physical changes during attacks.
*   **Deliverables:** Hardware inventory, FlatSat architecture diagram, setup/recovery guide, hardware interface documentation, basic hardware test scripts.

### Julio Sandoval — Student 2: Communications Lead
**Primary Goal:** Establish reliable communication between FlatSat A and FlatSat B.
*   **Tasks:**
    *   Identify available communication mechanisms.
    *   Establish bidirectional communication.
    *   Define a simple command/message format.
    *   Develop software for sending commands between FlatSats.
    *   Capture communication traffic.
    *   Measure basic latency and reliability.
    *   Support the Solar Tracker telemetry transmission.
*   **Deliverables:** Working FlatSat-to-FlatSat communication, communication architecture diagram, command/message specification, communication scripts, example communication captures, basic latency/reliability measurements.

### Jesus Garcia — Student 3: Cyber-Physical Integration Lead
**Primary Goal:** Connect physical inputs to remotely controlled software behavior and build the visual interface.
*   **Tasks:**
    *   Identify appropriate physical controls.
    *   Develop software to read those controls.
    *   Convert physical inputs into commands.
    *   Work with Student 2 to transmit commands between FlatSats.
    *   Build a real-time "Mission Control" telemetry dashboard (UI) to visualize solar panel angle and temperature.
    *   Instrument the end-to-end interaction.
*   **Deliverables:** Physical input interface, input-to-command software, Telemetry Dashboard integration, documentation of the complete control pipeline, live demonstration.

### Jack Pritchard — Student 4: Cybersecurity Lead
**Primary Goal:** Characterize and manipulate communication between the two FlatSats in a controlled laboratory environment.
*   **Tasks:**
    *   Identify exposed services and protocols and capture normal communication.
    *   Identify trust assumptions between the two FlatSats.
    *   Determine whether commands are authenticated or integrity protected.
    *   Implement controlled experiments such as command replay, injection, modification, or dropping (e.g., spoofing tilt commands or dropping temperature alerts).
    *   Record the communication effect and physical system effect for each experiment.
    *   Collect benign and attack traces for comparison.
*   **Deliverables:** Basic attack-surface assessment, baseline traffic captures, at least two working security experiments, attack scripts/tools, benign and attack traces, short analysis of observable physical effects.

---

## Target Integration Demonstration

**Pipeline:** Physical Input ➔ FlatSat B ➔ Communication Link ➔ FlatSat A ➔ Dashboard / Physical Sensor Change

*The FlatSat A will act as a solar tracker (using the LIS2DH12 and BME280). FlatSat B acts as Mission Control. The implementation is kept modular so the dashboard can later be swapped for a spacecraft-relevant ground station application.*

---

## Detailed Nine-Week Schedule

### Week 1
**Dates:** 10/05 - 10/11
**Focus:** Bring-Up and Division of Responsibilities
*   [x] Both FlatSats must be operational.
*   [x] Reproduce the existing environment.
*   [x] Inspect the hardware/software.
*   [x] Establish the shared repository.
*   [x] Assign subsystem ownership.
*   **Checkpoint:** Both FlatSats boot and team members can interact with them.

### Week 2
**Dates:** 10/12 - 10/18
**Focus:** Hardware and Communication Characterization
*   [x] **Fernanda:** Document hardware and interfaces.
*   [x] **Julio:** Investigate communications.
*   [x] **Jesus:** Test physical I/O.
*   [x] **Jack:** Map software, services, protocols, and potential attack surfaces.
*   **Checkpoint:** Architecture diagram showing hardware, software, communication interfaces, and potential control paths.

### Week 3
**Dates:** **10/19 - 10/25 (CURRENT WEEK)**
**Focus:** First FlatSat-to-FlatSat Communication
*   [ ] The team focuses on one goal: make FlatSat B cause something observable to happen on FlatSat A.
*   [ ] A simple button-to-message-to-LED action is sufficient (e.g., button press on B turns on LED on A).
*   **Checkpoint:** Live cross-FlatSat control demonstration. *(This is the first major go/no-go milestone).*

### Week 4
**Dates:** 10/26 - 11/01
**Focus:** Build the Control Framework
*   [ ] Generalize the Week 3 demonstration into a simple command framework (e.g., TILT_LEFT, TILT_RIGHT, DEPLOY_PANEL, RESET).
*   [ ] Jack captures and studies this communication.
*   **Checkpoint:** Multiple physical controls reliably generate remote commands.

### Week 5
**Dates:** 11/02 - 11/08
**Focus:** Solar Tracker & Telemetry Integration
*   [ ] Connect the command framework to the physical sensors (accelerometer/temp).
*   [ ] Jesus builds the live telemetry dashboard to display the data.
*   **Checkpoint:** Live telemetry dashboard displaying real-time physical changes on FlatSat A.

### Week 6
**Dates:** 11/09 - 11/15
**Focus:** Baseline Experiments
*   [ ] Stabilize the system and run repeatable trials.
*   [ ] Collect commands, network traffic, timestamps, latency, input/output behavior, and errors.
*   **Checkpoint:** Baseline dataset and reproducible experiment procedure.

### Week 7
**Dates:** 11/16 - 11/22
**Focus:** Cybersecurity Experiments
*   [ ] Jack leads controlled manipulation experiments.
*   [ ] Prioritize replay and injection; attempt modification or delay/drop only if feasible.
*   **Checkpoint:** At least two reproducible attacks with observable physical effects.

### Week 8
**Dates:** 11/23 - 11/29
**Focus:** Evaluation
*   [ ] Run structured normal-versus-manipulated trials.
*   [ ] Record expected commands, transmitted commands, attack status, network evidence, target behavior, timing, and success/failure.
*   **Checkpoint:** Experimental results that show what observable evidence distinguishes legitimate from manipulated interaction.

### Week 9
**Dates:** 11/30 - 12/06
**Focus:** Final Demonstration and Freeze
*   [ ] Final live demonstration early in the week.
*   [ ] Freeze development.
*   [ ] Focus on reproducibility, documentation, report, and presentation.
*   **Checkpoint:** Final live demonstration, documentation, report, and presentation.

---

## Final Team Deliverables
1.  [ ] Working dual-FlatSat testbed.
2.  [ ] FlatSat hardware/software architecture diagram.
3.  [ ] Documented FlatSat-to-FlatSat communication mechanism.
4.  [ ] Physical remote-control interface.
5.  [ ] Solar Tracker Telemetry Dashboard demonstration.
6.  [ ] At least two reproducible cybersecurity experiments.
7.  [ ] Baseline and attack communication captures.
8.  [ ] Shared source-code repository with setup instructions.
9.  [ ] Final technical report.
10. [ ] Final team presentation and live demonstration.

---

## Minimum, Target, and Stretch Outcomes

*   **Minimum:** Two FlatSats communicate reliably and physical input on one produces observable behavior on the other.
*   **Target:** The team completes the Solar Tracker Dashboard demonstration and at least two controlled cybersecurity experiments.
*   **Stretch:** The team transfers one attack from the demonstration to a spacecraft-relevant command (e.g., spoofing an attitude control command) and collects sufficient experimental data to support a future poster or research paper.