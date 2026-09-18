# ADR-001 — Documentation Language Standard for FixGo

- **ID:** ADR-001
- **Date:** 2026-09-18
- **Status:** Accepted
- **Authors:** Johan Andrés Liñan Esquivel, Juan David Romero Calderón, Gabriel Tijaro Jiménez, Mateo Esteban Ramírez Garzón

---

## Context

The software engineering industry standard—including framework documentations, open-source tools, API specs, and AI copilots—operates predominantly in English. Mixing languages across project artifacts (e.g., Spanish architectural docs alongside English Java/Python code) creates a cognitive translation boundary. This friction slows down developer onboarding, introduces naming inconsistencies, and degrades searchability across repository tools and CI/CD pipelines.

Establishing a single, explicit documentation language rule from day one prevents mismatched variable names, inconsistent commit messages, and conflicting domain terminology across the FixGo platform.

**Known constraints:**

- All team members must align on a canonical domain glossary for FixGo business terms (e.g., drivers, workshop owners, tow dispatch, roadside assistance).
- Technical documentation must adhere strictly to repository governance standards (`00-governance/`).

---

## Decision

**We decided:** Adopt **English** as the single canonical language for all codebase artifacts, documentation files, database schemas, API contracts, and version control messages across the FixGo ecosystem.

**Justification:**
Using English eliminates the translation boundary between source code, system architecture, and external dependencies. It guarantees consistency across technical artifacts, GitHub Pull Requests, and automated tools.

---

## Evaluated alternatives

| Alternative                              | Pros                                                                                                                                             | Cons                                                                                                                                | Reason for discarding                                                                   |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Everything in English (Chosen)**       | Industry standard; seamless integration with frameworks, libraries, CI/CD tools, and AI assistants; eliminates cognitive translation boundaries. | Initial effort required for team members accustomed to writing docs in Spanish.                                                     | — (chosen)                                                                              |
| **Everything in Spanish**                | Native language for team members; direct alignment with local business stakeholders.                                                             | Clunky mix with code syntax (`public class TallerMecanico`); inconsistent with third-party libraries; limits open-source potential. | Disrupts code readability and creates friction with development frameworks.             |
| **Hybrid (Spanish Docs / English Code)** | Lower barrier for initial documentation drafting.                                                                                                | High risk of domain drift; requires maintaining two sets of domain terms; complicates API contract definitions.                     | Creates permanent translation debt between architecture specifications and source code. |

---

## Decision Matrix by Artifact

| Artifact                                    | Language | Canonical Guideline                                                            |
| ------------------------------------------- | -------- | ------------------------------------------------------------------------------ |
| Codebase (Variables, Classes, Methods)      | English  | Strictly follow language style guides and clean code conventions.              |
| Database Schemas (Tables, Columns, Indexes) | English  | Mirror domain entities (e.g., `workshops`, `assistance_requests`).             |
| Git Commits & Branch Names                  | English  | Enforce Conventional Commits (`feat(dispatch): add driver location tracking`). |
| System Documentation (`.mdux`, `.md`)       | English  | Keep single source of truth across all architectural modules.                  |
| OpenAPI Contracts & API Specs               | English  | All endpoint descriptions, query params, and JSON schemas in English.          |
| Internal Logs & Observability Metrics       | English  | Ensure fast searching and indexing in centralized logging tools.               |

---

## Consequences

**Positive:**

- Eliminates cognitive switching between Spanish documentation and English source code.
- Establishes a clean, professional standard across GitHub pull requests and commit history.
- Accelerates integration with external SDKs (Mapbox, PostgreSQL/PostGIS, RabbitMQ).

**Negative / Trade-offs:**

- Requires careful translation of local business domain terms prior to usage.

**Impact on the system:**

- Affected services: All FixGo microservices (`fixgo-iam-service`, `fixgo-dispatch-service`, `fixgo-location-service`, `fixgo-workshop-service`).
- Documents that must be updated: `01-context/glossary.mdux`, `00-governance/documentation-rules.mdux`.

---

## Risks

| Risk                                                       | Probability | Impact | Mitigation                                                                        |
| ---------------------------------------------------------- | ----------- | ------ | --------------------------------------------------------------------------------- |
| Misinterpretation or ambiguous translation of domain terms | Medium      | Medium | Maintain and consult the canonical domain glossary in `01-context/glossary.mdux`. |

---

## References

- Domain Glossary → `01-context/glossary.mdux`
- Governance Documentation Rules → `00-governance/documentation-rules.mdux`
