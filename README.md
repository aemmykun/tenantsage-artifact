# tenantsage-artifact

## System Architecture

```mermaid
flowchart LR
    users(["Enterprise Users<br/>Staff · Managers · Admins"])
    operators(["Governance / Security Operators<br/>Policy · Audit · Incident Review"])

    subgraph tenantsage_boundary["TenantSage™"]
        direction TB
        tenantsage["TenantSage Platform<br/>Governance Execution & Authority Control Plane<br/>S0–S8 governed execution"]
    end

    idp["Enterprise IdP / IAM<br/>OIDC / JWKS · identity · assignments"]
    knowledge["Enterprise Knowledge Systems<br/>DMS · Drive · Notion · databases"]
    llm["Model Provider / LLM Runtime<br/>Generation only from governed context"]
    actions["Enterprise Action Systems<br/>Workflow · ticketing · operational APIs"]
    audit["Audit / Compliance Consumers<br/>Evidence review · export · investigation"]

    users -->|"requests + explicit activeScopeId"| tenantsage
    operators -->|"policy administration / review"| tenantsage
    tenantsage <-->|"verify identity; consume authority inputs"| idp
    tenantsage <-->|"retrieve only inside EEB"| knowledge
    tenantsage <-->|"governed context / candidate output"| llm
    tenantsage -->|"validated, authorised actions"| actions
    tenantsage -->|"receipts + evidence ledger"| audit

    classDef person fill:#143243,stroke:#42E7E9,color:#F3F8F9,stroke-width:2px;
    classDef system fill:#0B2938,stroke:#42E7E9,color:#F3F8F9,stroke-width:2px;
    classDef tenantsage fill:#103446,stroke:#86E89A,color:#F3F8F9,stroke-width:3px;
    class users,operators person;
    class idp,knowledge,llm,actions,audit system;
    class tenantsage tenantsage;
    style tenantsage_boundary fill:#06141E,stroke:#86E89A,stroke-width:2px,stroke-dasharray:8 5,color:#86E89A
    linkStyle default stroke:#FFB34D,stroke-width:2px,color:#B6C9CF
```

### Key Components

- **TenantSage Platform**: Central governance execution and authority control plane supporting S0–S8 security levels
- **Enterprise Users**: Staff, managers, and admins making requests with explicit scope identifiers
- **Governance/Security Operators**: Policy administrators and audit reviewers
- **Enterprise IdP/IAM**: Identity verification and authority assignments via OIDC/JWKS
- **Knowledge Systems**: Document management, Drive, Notion, and databases (access controlled by Enterprise Execution Boundary)
- **LLM Runtime**: Generates output only from governed context
- **Action Systems**: Executes validated and authorized workflows, ticketing, and operational APIs
- **Audit/Compliance**: Maintains receipts and evidence ledger for investigation and export
