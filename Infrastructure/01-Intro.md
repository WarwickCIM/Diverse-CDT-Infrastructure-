# CDT Infrastructure Hub Proposal

> **Purpose:** A complete design and communication bundle that enables PhD students to perform data-visualisation research using Observable, d3, R, Python and Jupyter notebooks while also supporting secure, NDA-protected collaborations with industry partners.

**Audience:** Industry Partners, Faculty, and Program Administrators.

## overview (what you will find in this document)

1. Executive Summary
2. [System Design - full technical & governance proposal](./cdt-infra-system-design)
3. [Data governance & security model](https://observablehq.com/d/376098b2860ed78b)
4. [Prototype / proof-of-concept checklist & minimal configuration](Prototype / proof-of-concept checklist & minimal configuration)
5. [Governance & data classification template (for industry data)](Governance & data classification template (for industry data))
6. [Implementation roadmap, milestones, acceptance criteria](Implementation roadmap, milestones, acceptance criteria)

## 1.1 Executive summary

Our primary goal is to create a vibrant, collaborative research environment and a secure, hybrid research platform for the Diverse CDT that:

- a. encourages open knowledge sharing among PhD students (Observable notebooks , public repos, tutorial hubs)
- b. supports reproducible analysis by providing **secondary "escape hatches"** (Jupyter, RStudio, `conda`/`renv` environments) for specialised tasks where the default platform is not suitable.
- c. provides segregated secure environments for industry collaborators to work on proprietary data (secure on-prem servers , private containers, VPN or dedicated tenant).

To achieve this, our platform is built on an **"Observable-first"** approach.

### Our "Observable-First" Philosophy

- **The Default Path:** For the vast majority of work, students will use the **Observable** platform, our official CDT partner. This is a secure, browser-based tool that requires no setup and is the default, fully supported environment for data visualization and analysis.
- **"Escape Hatches" for Specialists:** We recognize some projects have unique requirements. For these _exceptional cases_, we provide "escape hatches" —access to specialist tools like Python (Jupyter), R, and containerized environments. **These are not the standard expectation for all students**; they are supplementary tools available when a project truly needs them.

The proposal balances openness and confidentiality through data classification, access controls, and modular infrastructure components.

### Objectives

- Fast onboarding for new students
- Reproducible computational environments (Python/R/Jupyter/Observable interoperability)
- Clear separation of open vs confidential data
- Easy sharing of techniques and non-sensitive artifacts
- Ability to run compute on cloud or locally depending on nature of data.
- Long-term maintainability by the RSE

### Non-goals

- Replace university-wide identity or central storage (will integrate with existing SSO and storage where possible)
- Perform heavy enterprise IT tasks beyond the RSE remit (e.g., full-time SOC)
