---
weight: 100
title: "Overview"
author: "Nzomo"
icon: "circle"
description: "This documentation provides an overview of the iSIEM (Integrated Security Information and Event Management) project implemented using Wazuh."
date: "2026-02-04T09:38:26+03:00"
lastmod: "2026-02-04T09:38:26+03:00"
draft: false
toc: true
---


### Summary
![isiem](https://raw.githubusercontent.com/innovations-team/images/refs/heads/main/12.png)  

This project implements an In-house SOC ecosystem that leverages Open-source solutions to achieve centralized security visibility, detection and response. It aims to improve security visibility, threat detection, compliance monitoring, and incident response across the organization’s infrastructure.

### Background
#### Project Origin

The iSIEM project originated from a critical challege faced by Small and Medium sized Enterprises (SMEs): the need to maintain adequate cybersecurity controls while operating under limited budgets.

SMEs in kenya are increasingly subject to regulatory and compliance pressures from the Office of Data Protection Commissioner, industry standards(SASRA & CBK) guidelines. Yet, most of the commercial SIEM solutions are prohibitively expensive due to high licencing, infrastructure and operating costs.

We came up with iSIEM by consolidating OpenSource solutions that together they:
- Provide SMEs with an afordable, enterprise grade security monitoring solution.
- Help SMEs proove their commmitment to security to regulators 

Wazuh was selected as the core SIEM platform due to its open-source model, scalability, compliance capabilities and sustainability for environments with constrained resources.


{{< tabs tabTotal="2">}}
{{% tab tabName="Challenges" %}}

**SME Challenges**

- Lack of centralized visibility into security logs.
- Delayed detection of security incidents.
- No dashboard for security monitoring
- Limited insight into vulnerabilities and system integrity changes.

{{% /tab %}}
{{% tab tabName="Solution" %}}

**How iSIEM Solves**

- Centralizing logs from multiple endpoints.
- Enabling real-time detection and alerting
- Providing actionable dadhboards for security analysis.
- Improving incident response and investigation workflows.
- Enhancing compliance and security posture.

{{% /tab %}}
{{< /tabs >}}

### Architecture Overview

The deployment model for wazuh is Multi-node.

![Architetcure](https://raw.githubusercontent.com/innovations-team/images/refs/heads/main/UNIFIED%20SIEM%20%26%20CTI%20INTEGRATION%20ARCHITECTURE.png)


### Project Timeline
```
timeline
    title: TImeline
    Jan 18th -24th : Research and Tool Evaluation
    Jan 25rd -Jan 31st : Architecture Design
                       : Planning
    Feb 1st -Feb 7th : Deployment & Core Installation

```
