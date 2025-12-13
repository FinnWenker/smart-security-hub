# Smart Security Hub - Project Charter

**Project Manager**: [YOUR NAME HERE]  
**Start Date**: December 13, 2024  
**Target Completion**: February 7, 2025 (8 weeks)  
**Budget**: $250 maximum

## 1. Executive Summary

This project will design, prototype, and document a distributed IoT security system targeting budget-conscious students and renters. The goal is to demonstrate end-to-end product development skills applicable to hardware engineering, project management, and technical sales roles.

## 2. Problem Statement

**Business Problem**: 
College students and young renters face security concerns but cannot afford $300-500 upfront costs plus $10-30/month subscriptions for commercial systems. Apartment leases prohibit permanent installations.

**Technical Problem**:
No open-source, modular security solution exists that balances cost, ease-of-installation, and reliability for temporary living situations.

## 3. Project Objectives

### Primary Objectives:
1. **Functional Prototype**: Working 3-node sensor system with cloud notifications
2. **Cost Target**: Bill of materials under $200
3. **Documentation**: Professional-grade technical and business documentation
4. **Performance**: <30 second alert latency, 99% uptime over 72-hour test

### Secondary Objectives:
1. Demonstrate project management methodology (Agile)
2. Create investor-ready pitch materials
3. Publish open-source for portfolio visibility
4. Generate content for job interviews across 3 career paths

## 4. Success Criteria

| Metric | Target | Measurement Method |
|--------|--------|-------------------|
| Alert Latency | <30 seconds | Sensor trigger to phone notification |
| System Uptime | >99% | 72-hour continuous operation test |
| Total Cost | <$200 | Actual component receipts |
| Schedule Variance | ±1 week | Planned vs actual completion |
| Code Quality | >80% test coverage | Unit + integration tests |
| Documentation | 100% complete | Checklist of required docs |

## 5. Scope

### In Scope:
- 3 sensor nodes (motion + door sensors)
- 1 central hub (Raspberry Pi)
- Cloud backend with basic authentication
- Mobile app for iOS/Android
- Complete technical documentation
- Business case and market analysis
- Project management artifacts

### Out of Scope (Future Roadmap):
- Custom PCB fabrication (prototype will use dev boards)
- Professional mobile app (will use rapid development framework)
- Video surveillance integration
- Multi-user/multi-location support
- Commercial-grade enclosures

## 6. Stakeholders

| Stakeholder | Interest | Influence | Engagement Strategy |
|-------------|----------|-----------|---------------------|
| Potential Employers | Assess technical + PM skills | High | Regular GitHub updates, LinkedIn posts |
| End Users (testers) | Functionality, ease of use | Medium | User testing sessions, feedback surveys |
| Academic Advisor | Portfolio development | Medium | Mid-project review meeting |
| Roommates/Family | Test environment | Low | Informal feedback, safety validation |

## 7. Key Deliverables

### Hardware:
- [ ] System architecture diagram
- [ ] Component BOM with sourcing links
- [ ] Schematic diagrams for sensor nodes
- [ ] 3 working sensor node prototypes
- [ ] Assembled and configured hub

### Software:
- [ ] ESP32 firmware with OTA updates
- [ ] Raspberry Pi hub application
- [ ] Firebase backend with security rules
- [ ] Mobile app (MVP features)
- [ ] GitHub repository with CI/CD

### Documentation:
- [ ] Requirements specification
- [ ] API documentation
- [ ] User manual
- [ ] Test plan and results
- [ ] Architecture decision records

### Business Materials:
- [ ] Market analysis report
- [ ] Competitive comparison matrix
- [ ] ROI calculator (Excel)
- [ ] Product roadmap
- [ ] Investor pitch deck (10-12 slides)

### Project Management:
- [ ] Project charter (this document)
- [ ] Gantt chart / timeline
- [ ] Risk register
- [ ] Sprint retrospectives
- [ ] Final project report

## 8. Constraints

**Time**: 50 hours total (~6-7 hours/week over 8 weeks)  
**Budget**: $250 hard cap  
**Technical**: Must work on standard residential WiFi, no specialized networking  
**Physical**: Must be portable/removable (no permanent installation)  
**Regulatory**: Must comply with FCC Part 15 for unintentional radiators

## 9. Assumptions

1. Residential WiFi is available and reliable
2. Users have smartphone (iOS or Android)
3. Firebase free tier sufficient for prototype
4. Components available via Amazon/AliExpress within budget

## 10. Risks

| Risk | Probability | Impact | Mitigation Strategy |
|------|-------------|--------|---------------------|
| Component shipping delays | Medium | Medium | Order immediately; use Amazon Prime |
| WiFi reliability issues | Medium | High | Implement local buffering; test thoroughly |
| Budget overrun | Low | Medium | Track spending weekly; have contingency list |
| Scope creep | High | Medium | Strict scope document; defer features to v2.0 |
| Technical blockers (firmware bugs) | Medium | High | Allocate 20% buffer time; active community forums |
| Time management (exams) | High | High | Front-load work; build 1-week schedule buffer |

## 11. Project Approach

**Methodology**: Agile/Scrum adapted for solo development
- 2-week sprints
- Weekly sprint planning (Sundays)
- Daily 15-min self check-ins (morning)
- Sprint retrospectives documented

**Tools**:
- **Version Control**: GitHub
- **Project Tracking**: Trello
- **Documentation**: Markdown + Google Docs
- **Communication**: Personal log + LinkedIn updates

## 12. Budget Breakdown

| Category | Estimated Cost | Where to Buy | Notes |
|----------|----------------|--------------|-------|
| ESP32 dev boards (x3) | $22 | Amazon | Search: "ESP32 DevKit 3 pack" |
| Raspberry Pi 4 (2GB) Starter Kit | $65 | Amazon | Includes case, power supply, SD card |
| PIR Motion Sensors (x3) | $9 | Amazon | Search: "HC-SR401 3 pack" |
| Magnetic Door Sensors (x3) | $11 | Amazon | Search: "MC-38 5 pack" |
| DHT22 Temp/Humidity (x2) | $11 | Amazon | Temperature and humidity sensors |
| MQ-2 Smoke Sensor | $7 | Amazon | Gas/smoke detection |
| USB Power Adapters (x3) | $13 | Amazon | Search: "5V 2A USB charger 3 pack" |
| Plastic Project Boxes (x3) | $12 | Amazon | Search: "Small electronics enclosure 5 pack" |
| Command Strips | $8 | Amazon/Walmart | For mounting without drilling |
| Breadboard + Jumper Wires Kit | $10 | Amazon | Search: "breadboard jumper wire kit" |
| Micro USB cables (x3) | $8 | Amazon | To power ESP32 boards |
| Miscellaneous (resistors, LEDs) | $10 | Amazon | Various small components |
| **TOTAL** | **$186** | | |
| **BUFFER REMAINING** | **$64** | | For unexpected needs |

## 13. Timeline Summary

- **Weeks 1-2**: Planning, requirements, ordering components
- **Weeks 3-4**: Hardware assembly, firmware development
- **Weeks 5-6**: Backend and mobile app development
- **Weeks 7-8**: Integration testing, documentation, presentation prep

## Approval

**Project Manager**: Finn Wenker
**Date**: December 13, 2024  
**Signature**: Finn Wenker

---

## Change Log
| Date | Change Description | Approved By |
|------|-------------------|-------------|
| Dec 13, 2024 | Initial charter creation | [YOUR NAME] |