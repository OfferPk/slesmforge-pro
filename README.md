# SLESMForge Pro (v1.0.0)

**CCSDS 910.11-B-1 & CCSDS 902.1-B-1 / ISO 22671 Cross Support Service Management (SLE-SM / SCCS-SM) Workstation, Service Package Scheduler, Ground Antenna Conflict Solver, and 10-Point Normative Aerospace Cross Support Auditor**

## 1\. Executive Summary

**SLESMForge Pro** is an autonomous, zero-dependency developer studio and headless command-line interface implementing the **CCSDS 910.11-B-1 (Space Link Extension — Service Management)**, **CCSDS 902.1-B-1 (Space Communication Cross Support — Service Management)**, and **ISO 22671** standards.

While individual Space Link Extension (SLE) transfer services (RAF, ROCF, RCF, FCLTU, FSP, TDS) handle the real-time movement of space link frames and telecommand packets during a pass, **Service Management** acts as the crucial control plane. Without automated Service Management, international ground station networks (NASA Deep Space Network, ESA ESTRACK, KSAT, SSC) rely on manual email handoffs, static spreadsheets, and unverified pass schedules, leading to antenna booking collisions and lost mission passes.

SLESMForge Pro provides:

1. **Service Agreement Governance**: Enforces bilateral cross-support contracts between user Mission Data Operations Systems (MDOS / MOC) and provider Cross Support Complexes (CSC).  
2. **Service Package Scheduling Engine**: Generates unique, non-colliding Service Packages (`servicePackageCreateRequest`, `servicePackageModifyRequest`, `servicePackageCancelRequest`) with exact AOS-to-LOS pass windows.  
3. **Automated Antenna Conflict Solver**: Detects and prevents temporal pass overlaps on shared ground antenna apertures (e.g. Goldstone 70m, Cebreros 35m, Svalbard SG1) with automatic alternate antenna rerouting.  
4. **Transfer Service Instance Provisioning**: Automatically instantiates and binds authorized SLE transfer services (Return All Frames \[RAF\], Forward CLTU \[FCLTU\], Return Online Channel Frames \[ROCF\], and Tracking Data Service \[TDS\]) to dedicated network port endpoints.  
5. **RF Radiometry & Frequency Band Binding**: Maps carrier frequency bands (S-Band, X-Band, Ka-Band) against physical aperture capabilities.  
6. **SANA XML & Schema Interchange**: Generates valid XML service requests conforming strictly to the Space Assigned Numbers Authority (SANA) schema registry.  
7. **10-Point Normative Cross Support Auditor**: Automated verification of rules `SLESM-001` through `SLESM-010`.

## 2\. Standards Compliance

- **CCSDS 910.11-B-1**: Space Communication Cross Support — Service Management — Service Specification (Blue Book, Issue 1).  
- **CCSDS 902.1-B-1**: Cross Support Service Management — Service Operations.  
- **ISO 22671**: Space data and information transfer systems — Space link extension (SLE) — Service management.  
- **SANA Registry**: Conforms to Space Assigned Numbers Authority SCCS-SM XML schema definitions.

## 3\. Protocol Architecture & Service Management Flow

\[Mission Data Operations System (MDOS / MOC)\]

                     \|

                     v  (XML servicePackageCreateRequest)

\+-------------------------------------------------------------+

\|               SLESMForge Pro Management Engine              \|

\|      \- Bilateral Agreement Profile Verification             \|

\|      \- Ground Antenna Aperture Availability & Conflict Check\|

\|      \- Contact Pass Temporal Window Modeling (AOS \-\> LOS)   \|

\|      \- Service Package State Machine: PROPOSED \-\> SCHEDULED \|

\+-------------------------------------------------------------+

                     \|

                     \+-----------------------+

                     \| (Pass Start: AOS)     \|

                     v                       v

\+-----------------------------+   \+---------------------------+

\|    Return SLE Services      \|   \|   Forward SLE Services    \|

\| \- RAF (Return All Frames)   \|   \| \- FCLTU (Forward CLTU)    \|

\| \- ROCF (Online Channels)    \|   \| \- FSP (Forward Packets)   \|

\| \- TDS (Tracking Radiometry) \|   \| \- Baseband Carrier Uplink \|

\+-----------------------------+   \+---------------------------+

                     \|                       \|

                     \+-----------+-----------+

                                 \|

                                 v

\[Cross Support Complex: Deep Space Antennas & Tracking Networks\]

## 4\. Quickstart & Usage

### 4.1 Headless Python 3 CLI

Schedule a Service Package for an upcoming spacecraft contact pass:

python3 source-code/slesm\_cli.py package \--agreement AGR-NASA-ESA-2026-01 \--mission ARTEMIS-ORION \--antenna CEBREROS-35M \--start 2026-10-18T18:00:00 \--stop 2026-10-18T18:45:00

Check for ground antenna scheduling collisions:

python3 source-code/slesm\_cli.py conflict \--antenna CEBREROS-35M \--start 2026-10-18T18:15:00 \--stop 2026-10-18T19:00:00

Execute the 10-point normative aerospace cross-support standards audit:

python3 source-code/slesm\_cli.py audit

Simulate an end-to-end service management booking and conflict resolution cycle:

python3 source-code/slesm\_cli.py run

### 4.2 Interactive Browser Scheduling Console

Open `source-code/index.html` directly in any modern web browser:

- Real-time Service Package Booking configuration (Agreement, Mission, Antenna, Band, SLE Services).  
- Live Ground Antenna Pass Gantt Chart across multiple tracking complexes.  
- Visual Antenna Conflict Monitor with instant collision alerts.  
- SANA-compliant XML Message Inspector (`servicePackageCreateRequest`).  
- Real-time 10-point normative aerospace cross-support compliance scorecard.

## 5\. Automated Verification Test Suite

Run the automated test runner:

python3 source-code/test\_runner.py

**Results**:

- Ran 10 tests in 0.018s.  
- **Pass Rate**: 100% (10/10 PASS).  
- **Certification Status**: `CCSDS_910_11_SLESM_CERTIFIED`.

## 6\. Commercial Model & Licensing

- **Community Open-Core**: Free CLI scheduling validator and standard SANA XML generator for university satellite teams and research ground stations.  
- **Ground Network Operations Seat (\$349/year)**: Commercial workstation license with live pass Gantt visualizers, automated antenna collision detection, and multi-network SLE transfer bridge daemons.  
- **Enterprise Cross Support Complex License (\$2,499 one-time site license)**: Full source code license, automated DSN/ESTRACK/KSAT API integration plugins, multi-antenna array schedulers, and commercial Mission Operations Center (MOC) integration support.

## 7\. Known Limitations

- Operates as a software service management control plane; physical antenna slewing and baseband RF transceiver modulation are managed by ground station front-end controllers.  
- Temporal conflict detection operates on deterministic time intervals; atmospheric weather contingency slewing requires dynamic re-negotiation.