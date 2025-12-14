# Smart Security Hub - Requirements Specification

**Version**: 1.0  
**Last Updated**: December 13, 2024  
**Status**: Draft

## 1. Introduction

### 1.1 Purpose
This document specifies functional and non-functional requirements for the Smart Security Hub, an IoT-based security monitoring system for residential environments.

### 1.2 Scope
The system will monitor entry points and environmental conditions, providing real-time alerts to users via mobile devices. Initial deployment targets single-occupant apartments or dorm rooms.

### 1.3 Definitions
- **Sensor Node**: ESP32-based device with attached sensors in plastic enclosure
- **Hub**: Raspberry Pi central controller
- **Event**: Any sensor state change (motion detected, door opened, etc.)
- **Alert**: User notification triggered by event meeting threshold criteria
- **MQTT**: Message Queuing Telemetry Transport - lightweight messaging protocol
- **OTA**: Over-The-Air firmware updates

## 2. User Stories

### 2.1 Security Monitoring

**US-001**: As a renter, I want to receive immediate notifications when my door opens while I'm away, so I can verify it's authorized entry.  
**Acceptance Criteria**:
- Notification received within 30 seconds of door opening
- Notification includes timestamp and sensor location
- User can mark event as "expected" or "suspicious"

**US-002**: As a student, I want motion alerts disabled during hours I'm typically home, so I don't receive false alarms.  
**Acceptance Criteria**:
- Scheduling interface in mobile app
- Motion detection auto-disables per schedule
- Manual override available

**US-003**: As a user on a tight budget, I want a system that costs less than $200 with no monthly fees, so I can afford security on a student budget.  
**Acceptance Criteria**:
- Total hardware cost under $200
- No required cloud subscriptions
- All core features work without recurring payments

### 2.2 Environmental Monitoring

**US-004**: As a user, I want smoke/CO detection alerts even when sensors are in "disarmed" mode, so I'm protected from hazards 24/7.  
**Acceptance Criteria**:
- Environmental sensors always active
- Critical alerts bypass "away/home" modes
- Distinct notification sound/priority

**US-005**: As a plant owner, I want temperature and humidity tracking, so I can maintain optimal growing conditions.  
**Acceptance Criteria**:
- Hourly readings logged
- Graphical trends in app (7-day view)
- Optional alerts for out-of-range conditions

### 2.3 System Management

**US-006**: As a user, I want to view event history for the past week, so I can identify patterns or review incidents.  
**Acceptance Criteria**:
- Searchable event log
- Filter by sensor type, date range
- Export to CSV functionality

**US-007**: As a developer, I want over-the-air firmware updates, so I can patch bugs without physical access to devices.  
**Acceptance Criteria**:
- OTA update mechanism in ESP32 firmware
- Rollback capability if update fails
- Update status visible in app

**US-008**: As a renter, I want easy installation without drilling holes, so I don't lose my security deposit.  
**Acceptance Criteria**:
- All sensors mount with Command strips or adhesive
- No permanent modifications required
- Easy removal leaving no damage

## 3. Functional Requirements

### 3.1 Sensor Nodes (ESP32)

**FR-001**: Sensor nodes SHALL read attached sensors at configurable intervals (default: 5 seconds for active monitoring, 60 seconds for sleep mode).

**FR-002**: Sensor nodes SHALL publish events to MQTT broker when state changes occur (motion detected, door opened, threshold exceeded).

**FR-003**: Sensor nodes SHALL buffer up to 50 events locally if WiFi connectivity is lost.

**FR-004**: Sensor nodes SHALL retry failed MQTT publishes with exponential backoff (1s, 2s, 4s, 8s, max 16s).

**FR-005**: Sensor nodes SHALL enter deep sleep mode after 60 seconds of inactivity to conserve power.

**FR-006**: Sensor nodes SHALL send heartbeat messages every 5 minutes to indicate operational status.

**FR-007**: Sensor nodes SHALL support OTA firmware updates via HTTPS.

**FR-008**: Sensor nodes SHALL have unique identifiers for tracking (e.g., "sensor-living-room", "sensor-door-main").

**FR-009**: Sensor nodes SHALL include onboard LED indicators for status (WiFi connected, event triggered, error state).

### 3.2 Central Hub (Raspberry Pi)

**FR-010**: Hub SHALL run local MQTT broker (Mosquitto) to receive sensor node messages.

**FR-011**: Hub SHALL apply business logic rules to determine if events warrant user alerts (e.g., door open + away mode = alert).

**FR-012**: Hub SHALL forward qualifying events to cloud backend via HTTPS POST.

**FR-013**: Hub SHALL maintain local SQLite database with 7-day event history.

**FR-014**: Hub SHALL monitor sensor node heartbeats and alert if node offline >10 minutes.

**FR-015**: Hub SHALL provide REST API for local network queries (e.g., "what's current temp?").

**FR-016**: Hub SHALL continue operating with local-only functionality if internet connection is lost.

**FR-017**: Hub SHALL synchronize buffered events to cloud when internet connectivity is restored.

### 3.3 Cloud Backend (Firebase)

**FR-018**: Backend SHALL authenticate users via email/password or OAuth (Google Sign-In).

**FR-019**: Backend SHALL store user profiles including alert preferences and sensor configurations.

**FR-020**: Backend SHALL trigger push notifications to registered mobile devices when receiving alert-worthy events.

**FR-021**: Backend SHALL provide RESTful API for:
- GET /events (with date range, sensor type filters)
- GET /sensors (current status of all nodes)
- POST /config (update alert thresholds, schedules)
- GET /history (historical data for charts)

**FR-022**: Backend SHALL rate-limit notifications to max 1 per sensor per minute to prevent alert fatigue.

**FR-023**: Backend SHALL store event data with timestamps in UTC format.

**FR-024**: Backend SHALL enforce data retention policy (delete events older than 30 days to stay within free tier).

### 3.4 Mobile Application

**FR-025**: App SHALL display real-time status of all sensors (online/offline, last reading, battery level if applicable).

**FR-026**: App SHALL show event timeline with filtering and search capabilities.

**FR-027**: App SHALL allow users to configure alert rules:
- Enable/disable by sensor
- Set home/away modes
- Schedule automation (e.g., "disarm motion sensors 6pm-8am")

**FR-028**: App SHALL display sensor data trends (temperature, humidity) as line charts.

**FR-029**: App SHALL allow users to export event data as CSV.

**FR-030**: App SHALL provide manual "arm/disarm" toggle for all sensors or individual sensors.

**FR-031**: App SHALL show notification history with timestamps.

## 4. Non-Functional Requirements

### 4.1 Performance

**NFR-001**: Alert latency SHALL be <30 seconds from sensor trigger to mobile notification (95th percentile).

**NFR-002**: Mobile app SHALL load current sensor status in <3 seconds on 4G LTE.

**NFR-003**: System SHALL support up to 10 concurrent sensor nodes without degradation.

**NFR-004**: Hub SHALL process 100 events/minute without queuing delays.

**NFR-005**: Sensor nodes SHALL consume <5W during active monitoring.

**NFR-006**: System SHALL achieve 99% uptime over any 72-hour test period.

### 4.2 Reliability

**NFR-007**: Sensor nodes SHALL automatically reconnect to WiFi within 30 seconds of network restoration.

**NFR-008**: No data loss SHALL occur during WiFi outages up to 1 hour (local buffering capacity).

**NFR-009**: Hub SHALL continue local operations if cloud backend unavailable.

**NFR-010**: Sensor node firmware SHALL include watchdog timer to recover from crashes.

**NFR-011**: Hub SHALL automatically restart MQTT broker if it crashes.

### 4.3 Security

**NFR-012**: All communications SHALL use TLS 1.2 or higher (MQTT over TLS, HTTPS).

**NFR-013**: Firebase SHALL enforce authentication on all API endpoints (no anonymous access).

**NFR-014**: Sensor node OTA updates SHALL verify cryptographic signatures before installation.

**NFR-015**: User passwords SHALL be hashed with bcrypt (cost factor ≥12).

**NFR-016**: MQTT broker SHALL require username/password authentication.

**NFR-017**: API keys and credentials SHALL NOT be hardcoded in source code (use environment variables).

### 4.4 Usability

**NFR-018**: Mobile app SHALL be usable without technical documentation (intuitive UI).

**NFR-019**: System setup (add new sensor) SHALL require <5 minutes for non-technical users.

**NFR-020**: Error messages SHALL provide actionable guidance (e.g., "Check WiFi password" not "Connection failed").

**NFR-021**: Sensor nodes SHALL have clear LED indicators for status (WiFi connected = solid green, searching = blinking blue, error = red).

### 4.5 Maintainability

**NFR-022**: Codebase SHALL have >80% unit test coverage for critical functions.

**NFR-023**: All configuration SHALL be externalized (no hardcoded IPs, credentials).

**NFR-024**: Firmware SHALL log errors to local flash for remote debugging.

**NFR-025**: Code SHALL follow consistent style guidelines (use linters).

**NFR-026**: All functions SHALL have descriptive comments explaining purpose.

### 4.6 Portability

**NFR-027**: Sensor nodes SHALL be mountable with Command strips (no drilling required).

**NFR-028**: Entire system SHALL be removable within 15 minutes leaving no wall damage.

**NFR-029**: Sensors SHALL fit in plastic project boxes (max dimensions: 4"x3"x2").

**NFR-030**: Hub SHALL fit in standard Raspberry Pi case included in starter kit.

### 4.7 Cost

**NFR-031**: Total bill of materials SHALL not exceed $200 for 1 hub + 3 sensor nodes.

**NFR-032**: Cloud hosting costs SHALL remain within Firebase free tier limits (<10K writes/day, <1GB storage).

**NFR-033**: No monthly subscription fees SHALL be required for core functionality.

## 5. Hardware Requirements

### 5.1 Sensor Node Hardware

**HR-001**: Each sensor node SHALL use ESP32 development board as microcontroller.

**HR-002**: Sensor nodes SHALL support the following sensor types:
- PIR motion sensors (HC-SR501 or equivalent)
- Magnetic door/window sensors (MC-38 or equivalent)
- Temperature/humidity sensors (DHT22 or equivalent)
- Gas/smoke sensors (MQ-2 or equivalent)

**HR-003**: Sensor nodes SHALL be powered by 5V USB power adapters.

**HR-004**: Sensor nodes SHALL fit within plastic project box enclosures (dimensions ≤4"x3"x2").

**HR-005**: Enclosures SHALL have mounting points compatible with Command strips.

### 5.2 Hub Hardware

**HR-006**: Hub SHALL use Raspberry Pi 4 (2GB RAM minimum).

**HR-007**: Hub SHALL include SD card (16GB minimum) for OS and data storage.

**HR-008**: Hub SHALL have WiFi connectivity (built-in or USB adapter).

**HR-009**: Hub SHALL be housed in protective case (included in starter kit).

## 6. System Constraints

**CON-001**: Must operate on standard 2.4GHz WiFi (802.11 b/g/n) - 5GHz optional.

**CON-002**: Must not require permanent installation (Command strips, no drilling).

**CON-003**: Must comply with FCC Part 15 (unlicensed radio devices).

**CON-004**: Development timeline cannot exceed 8 weeks (50 total hours).

**CON-005**: Must use commercially available components (no custom PCB fabrication).

**CON-006**: Must not require 3D printer access for enclosures.

## 7. Dependencies

**DEP-001**: Requires active WiFi network with internet connectivity.

**DEP-002**: Requires smartphone running iOS 12+ or Android 8+.

**DEP-003**: Requires Firebase account (free tier sufficient).

**DEP-004**: Requires Amazon account for component ordering.

**DEP-005**: Requires basic hand tools (screwdriver, drill for enclosure modifications).

**DEP-006**: Requires Arduino IDE or PlatformIO for ESP32 development.

**DEP-007**: Requires Python 3.8+ for Raspberry Pi hub application.

## 8. Interface Requirements

### 8.1 Hardware Interfaces

**IF-001**: ESP32 GPIO pins SHALL connect to sensors via standard jumper wires.

**IF-002**: Sensors SHALL use 3.3V or 5V logic levels compatible with ESP32.

**IF-003**: USB power adapters SHALL provide 5V at minimum 1A current.

### 8.2 Software Interfaces

**IF-004**: ESP32 SHALL communicate with hub via MQTT protocol (port 1883 or 8883 for TLS).

**IF-005**: Hub SHALL communicate with Firebase via HTTPS REST API (port 443).

**IF-006**: Mobile app SHALL communicate with Firebase via Firebase SDK.

**IF-007**: Local hub API SHALL use HTTP REST (port 8080).

### 8.3 Communication Protocols

**IF-008**: MQTT messages SHALL use JSON format for payload.

**IF-009**: REST API SHALL accept/return JSON formatted data.

**IF-010**: All timestamps SHALL use ISO 8601 format (UTC).

## 9. Data Requirements

### 9.1 Data Storage

**DR-001**: Hub SHALL store events in SQLite database with schema:
```
events (
  id INTEGER PRIMARY KEY,
  sensor_id TEXT,
  event_type TEXT,
  value TEXT,
  timestamp DATETIME,
  uploaded BOOLEAN
)
```

**DR-002**: Firebase SHALL store events in Realtime Database with structure:
```
users/
  {userId}/
    sensors/
      {sensorId}/
        status: "online"
        lastReading: {...}
    events/
      {eventId}/
        sensorId: "..."
        type: "..."
        timestamp: "..."
```

### 9.2 Data Retention

**DR-003**: Hub SHALL retain 7 days of event history locally.

**DR-004**: Firebase SHALL retain 30 days of event history (automatic deletion after 30 days).

**DR-005**: User preferences and configuration SHALL be retained indefinitely.

## 10. Priority Matrix

| Requirement ID | Priority | Complexity | Sprint |
|----------------|----------|------------|--------|
| FR-001 to FR-009 | Must Have | Medium | 2-3 |
| FR-010 to FR-017 | Must Have | High | 3-4 |
| FR-018 to FR-024 | Must Have | Medium | 4-5 |
| FR-025 to FR-031 | Must Have | Medium | 5-6 |
| NFR-001 to NFR-033 | Must Have | Varies | Throughout |
| HR-001 to HR-009 | Must Have | Low | 1-3 |

## 11. Future Enhancements (v2.0)

**Out of scope for this project but documented for roadmap:**

- Camera integration with motion-triggered recording
- Multi-user support with role-based permissions
- Geofencing for automatic home/away mode
- Integration with smart locks
- Voice assistant integration (Alexa, Google Home)
- Custom PCB design for cost reduction at scale
- Solar panel compatibility for off-grid deployment
- Machine learning for activity pattern recognition
- Integration with existing smart home platforms (Home Assistant, SmartThings)

## 12. Acceptance Testing

Each requirement will be validated through:

### 12.1 Unit Tests
- Individual function/module testing
- Mock sensor inputs
- API endpoint testing

### 12.2 Integration Tests
- Component interaction testing
- MQTT message flow verification
- End-to-end data pipeline validation

### 12.3 System Tests
- End-to-end user scenarios
- Performance benchmarking
- Reliability testing (72-hour continuous operation)

### 12.4 User Acceptance Tests
- Real-world usage by test users
- Usability feedback
- Installation ease verification

**Test plan details in `docs/test-plan.md`.**

## 13. Traceability Matrix

| User Story | Functional Requirements | Test Cases |
|------------|------------------------|------------|
| US-001 | FR-002, FR-011, FR-020 | TC-001, TC-002 |
| US-002 | FR-027 | TC-003 |
| US-003 | NFR-031, NFR-032, NFR-033 | TC-004 |
| US-004 | FR-011, FR-020 | TC-005 |
| US-005 | FR-001, FR-028 | TC-006 |
| US-006 | FR-013, FR-026, FR-029 | TC-007 |
| US-007 | FR-007 | TC-008 |
| US-008 | NFR-027, NFR-028, HR-005 | TC-009 |

*Full test cases to be developed in test plan document.*

## 14. Assumptions and Risks

### 14.1 Assumptions
1. Users have reliable residential WiFi
2. Users willing to perform basic setup (connecting to WiFi, app installation)
3. Firebase free tier limits sufficient for single-user prototype
4. Component availability and pricing stable

### 14.2 Technical Risks
1. **WiFi Reliability**: Mitigation = local buffering, offline mode
2. **Power Outages**: Mitigation = document in user manual (future: battery backup)
3. **Sensor False Positives**: Mitigation = adjustable sensitivity, scheduling
4. **Cloud Service Changes**: Mitigation = design for portability (can switch from Firebase)

## 15. Compliance and Standards

**COMP-001**: System SHALL comply with FCC Part 15 for unlicensed devices.

**COMP-002**: System SHALL follow Firebase terms of service and usage limits.

**COMP-003**: Mobile app SHALL comply with iOS App Store and Google Play Store guidelines (if published).

**COMP-004**: Code SHALL be released under MIT License (open source).

---

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Dec 13, 2024 | Finn | Initial requirements specification |

---

## Approval Signatures

**Requirements Author**: Finn  
**Date**: December 13, 2024

**Project Manager**: Finn  
**Date**: December 13, 2024

---

## Appendix A: Glossary

- **ESP32**: Low-cost, low-power microcontroller with WiFi and Bluetooth
- **Raspberry Pi**: Single-board computer running Linux
- **MQTT**: Lightweight publish-subscribe messaging protocol
- **Firebase**: Google's mobile/web application development platform
- **PIR**: Passive Infrared (motion detection sensor)
- **DHT22**: Digital humidity and temperature sensor
- **SQLite**: Embedded relational database
- **REST**: Representational State Transfer (API architecture)
- **OTA**: Over-The-Air (wireless firmware updates)
- **TLS**: Transport Layer Security (encryption protocol)