# Smart Security Hub 🔒

> Affordable, open-source security system for students and renters - no subscriptions required.

## Project Overview

A distributed IoT security system that monitors entry points, detects motion, and tracks environmental hazards. Built with ESP32 sensor nodes, Raspberry Pi hub, and cloud-based alerting.

Traditional smart home security (Ring, SimpliSafe) costs $300-500 upfront plus $10-30/month subscriptions. Students and renters need affordable solutions without long-term contracts.

### The Solution
Open-source, self-hosted security system for <$200 with zero recurring fees. Modular design allows users to start small and expand.

## Key Features
- Real-time push notifications for security events
- Multi-sensor support (motion, door/window, smoke, environmental)
- 7-day local data retention + cloud backup
- Battery backup for power outages
- Mobile dashboard for monitoring and configuration

## System Architecture
```
[Sensor Nodes (ESP32)] --MQTT--> [Hub (RPi4)] --HTTPS--> [Cloud (Firebase)] --Push--> [Mobile App]
```

## Tech Stack
- **Hardware**: ESP32, Raspberry Pi 4, PIR sensors, magnetic switches
- **Firmware**: C++ (Arduino), Python (Hub)
- **Backend**: Firebase Realtime Database, Cloud Functions
- **Mobile**: React Native
- **Protocols**: MQTT, HTTPS, WebSockets

## Project Status
🚧 **In Development** - Week 1: Planning & Requirements

## Timeline
- **Week 1-2**: Requirements, architecture, component ordering
- **Week 3-4**: Hardware prototyping and firmware development
- **Week 5-6**: Cloud integration and mobile app
- **Week 7-8**: Testing, documentation, and polish

## Business Case
| Feature | This Project | Ring Alarm | SimpliSafe |
|---------|--------------|------------|------------|
| Upfront Cost | ~$180 | $300 | $250 |
| Monthly Fee | $0 | $10-20 | $15-28 |
| 3-Year Total | $180 | $660-1,020 | $790-1,258 |
| **Savings** | **Baseline** | **-73%** | **-77%** |

## Documentation
- [Requirements Document](./project-management/requirements.md)
- [System Architecture](./docs/architecture.md)
- [Project Timeline](./project-management/timeline.md)
- [Test Plan](./docs/test-plan.md)

## Author
Finn Wenker - Computer Engineering Student  
Building this to demonstrate full-stack IoT development and project management skills.

---
📫https://www.linkedin.com/in/finn-wenker-557b86304/
💼 Portfolio
📧 finn.wenker@gmail.com