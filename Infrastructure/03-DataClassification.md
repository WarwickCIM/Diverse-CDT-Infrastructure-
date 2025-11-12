## 3 Data governance & security model

A robust governance framework is essential to manage the movement of data between the architectural zones. This framework is built on a formal Data Classification Policy and a secure "airlock" protocol for all sensitive data movement.

### 3.1 Data Classification and Handling Policy

A formal policy is the central artifact that determines the required security controls and legal agreements for any given project.

#### L1-L3: Data Managed by the CDT

The following three levels describe all data that will be stored, processed, or managed on the CDT's infrastructure.

| Level  | Classification                            | Tag | Description & Examples                                                                                                                                               | Permitted Storage Location                               | Required Access Controls                                                                                      |
| :----- | :---------------------------------------- | :-- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------ |
| **L1** | **Public**                                | O   | Open-source code, published papers, anonymized public datasets.                                                                                                      | Public GitHub repository, program website.               | Public access.                                                                                                |
| **L2** | **Internal**                              | I   | Course materials, draft manuscripts, non-sensitive research data for internal collaboration.                                                                         | Program's private GitHub organization (via token access) | University SSO authentication required. Access controlled at the group/project level.                         |
| **L3** | **Highly Restricted**<br>**(in CDT TRE)** | R   | **Personally Identifiable Information (PII), raw proprietary data, commercially sensitive IP** that is permitted to be stored _within_ the CDT's secure environment. | **Trusted Research Environment (TRE) only.**             | **MFA mandatory.** Access via secure gateway (VPN/Bastion). No internet access from VRE. All activity logged. |

---

#### L4: Partner-Managed (External)

> **Note:** This category is handled separately to make it clear that the CDT **does not take responsibility for this data** and it **never** touches CDT infrastructure.

This level is for extreme-case, highly sensitive data that an industry partner cannot or will not allow to leave their own environment.

| Level  | Classification                        | Tag | Description & Examples                                                                                                                | Permitted Storage Location                                                                                  | Required Access Controls                                                                                                                            |
| :----- | :------------------------------------ | :-- | :------------------------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| **L4** | **Partner-Managed**<br>**(External)** | E   | **Extreme-case sensitive data that MUST NOT leave the partner's environment.** All work is performed on partner-owned infrastructure. | **Partner-managed environment ONLY.**<br>(This data is _never_ stored on university or CDT infrastructure). | **Managed entirely by the partner.** Access via partner credentials and mechanisms (e.g., physical access to a secure room, partner-issued laptop). |

### 3.2 Data Flow and Governance

- **Authentication and Authorization:**

  - For on-prem systems, a unified identity management solution, federated with the university's central SSO service, will assign roles (e.g., 'Student', 'Faculty', 'Industry-Mentor-ProjectX'). These roles enforce Role-Based Access Control (RBAC) across all components.

  - For online systems like GitHub and observable current group invites will be maintained.

- **Data Ingress/Egress Protocols for TRE:** The boundary of the Secure Partner Zone (TRE) is the most critical control point. All data movement is managed by a formal "airlock" protocol.

  - **Ingress:** Data from an industry partner (classified L3) is first uploaded to a secure, isolated staging area. Here, it undergoes automated checks (e.g., malware scanning) before being formally ingested into the project's dedicated storage within the TRE. This process is fully logged.
  - **Egress:** No data can leave the TRE without undergoing a formal review process that embodies the "Safe Outputs" principle. A researcher must submit an egress request (e.g., for a chart, an aggregated table, or a trained synthetic data model). This request is placed in a secure review queue for manual approval from multiple stakeholders (e.g., PI, Data Security Officer, Industry Partner). This human-in-the-loop process is the ultimate safeguard against accidental data leakage.

### 3.3 Data Access Solutions for Sensitive Data

When partners cannot share raw (L3) data, the TRE will facilitate two crucial alternative workflows:

1.  **Schema Generation:** For projects where the _structure_ of the data is sufficient, a formal process will be used:

    - The partner provides a detailed description of the data's properties, characteristics, and constraints.
    - The RSE and student use this to create a formal, shareable **data schema** (e.g., using JSON Schema, XML Schema, or other database schema tools).
    - This schema is version-controlled in GitHub and used to build analysis pipelines that can be run on the real data within the partner's environment later.

2.  **Synthetic Data Generation:** To create a high-fidelity, statistically similar but anonymous dataset, a secure workflow will be used:

    - **Step 1 (Ingress):** The partner's **L3 confidential dataset** is securely transferred _into_ the TRE.
    - **Step 2 (Training):** Inside the TRE (on the on-prem server), the researcher uses an approved Python library (e.g., **Synthetic Data Vault (SDV)**, **YData-Synthetic**, or GAN-based models like **CTGAN**) to train a generative model. This model learns the statistical patterns of the real data.
    - **Step 3 (Generation):** The _model_ is used to generate a new, synthetic dataset. Differential privacy techniques can be applied during this step to provide mathematical privacy guarantees.
    - **Step 4 (Egress):** The _synthetic_ dataset (now classified as L2) and a quality report (e.g., from SDMetrics) undergo the formal "Airlock" egress review to validate its utility and privacy. Once approved, this privacy-safe dataset can be moved to the main GitHub organization for wider student use.
