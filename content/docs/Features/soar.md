---
weight: 304
title: "SOAR"
description: ""
icon: "article"
date: "2026-02-04T09:38:26+03:00"
lastmod: "2026-02-04T09:38:26+03:00"
draft: false
toc: true
---

### Security Orchestration, Automation, and Response (SOAR – Shuffle)

The SOAR component of the iSIEM platform is implemented using Shuffle, an open-source security orchestration and automation tool. SOAR plays a critical role in transforming security monitoring from a reactive, manual process into a coordinated, automated, and repeatable response capability.

For SMEs, where security teams are small and resources are limited, SOAR enables faster and more consistent incident response without requiring a full-scale Security Operations Center (SOC).

#### Security Orchestration

Security orchestration refers to the coordination and integration of multiple security tools into a single, cohesive workflow.

Within the iSIEM platform, Shuffle orchestrates interactions between:

- Wazuh (alert generation and detection)

- Threat Intelligence platforms (MISP, OpenCTI)

- Case Management (TheHive)

- Infrastructure controls (firewalls, endpoint controls, notification systems)

Orchestration ensures that once an alert is generated, relevant systems automatically share information and context, eliminating tool silos and manual data transfer.

Value: Analysts gain a unified and structured response process instead of switching between multiple tools.

#### Security Automation

Automation focuses on reducing repetitive and time-consuming manual tasks that slow down incident response.

Shuffle enables automation through:

- Workflow-based playbooks

- Conditional logic based on alert severity or type

- Automated enrichment of alerts with threat intelligence

- Automatic creation and updating of incident cases

Example Automations:

- Enrich a Wazuh alert with IOCs from MISP

- Create a case in TheHive for high-severity alerts

- Notify relevant teams via email or messaging platforms

Value: Faster response times, reduced human error, and consistent handling of similar incidents.

#### Security Response

Response capabilities focus on containment and remediation actions taken after an incident is confirmed.

- Using Shuffle, response actions can include:

- Blocking malicious IP addresses or domains

- Disabling compromised user accounts

- Isolating affected endpoints

- Triggering remediation workflows or escalation procedures

Response actions can be fully automated or require analyst approval, depending on the organization’s risk tolerance and maturity level.

Value: Enables rapid containment while maintaining control and oversight.

#### SOAR Workflow Example (End-to-End)

- Wazuh detects a suspicious authentication pattern

- Shuffle receives the alert and initiates a playbook

- Threat intelligence is queried from MISP and OpenCTI

- Alert is enriched with contextual threat data

- A case is created in TheHive

- Analyst is notified with enriched information

- Optional containment actions are triggered

This workflow demonstrates how SOAR reduces manual effort while improving detection accuracy and response speed.

#### Governance, Control, and Compliance

SOAR workflows are:

- Documented and repeatable

- Logged for auditing and review

- Configurable based on security policies

This supports compliance requirements by ensuring incidents are handled in a consistent, auditable, and policy-driven manner.

#### Value to SMEs

The SOAR implementation using Shuffle provides SMEs with:

- Enterprise-grade automation without high licensing costs

- Faster incident response with limited staff

- Reduced operational complexity

- Improved consistency and security maturity

SOAR allows SMEs to scale their security operations efficiently, handling more incidents without proportional increases in cost or manpower.