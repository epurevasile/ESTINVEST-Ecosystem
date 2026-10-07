\# ESTINVEST Ecosystem Documentation Index



| Proprietate        | Valoare                 |

| ------------------ | ----------------------- |

| Document           | Documentation Index     |

| Versiune           | 1.0                     |

| Status             | Active                  |

| Repository         | ESTINVEST-Ecosystem     |

| Owner              | Enterprise Architecture |

| Ultima actualizare | 2026-10-06              |



\---



\# Scop



Acest document reprezintă catalogul oficial al documentației \*\*ESTINVEST Ecosystem\*\*.



El oferă o imagine de ansamblu asupra tuturor documentelor existente, facilitând navigarea, identificarea documentelor lipsă și planificarea dezvoltării documentației.



\---



\# Structura documentației



```text

ESTINVEST-Ecosystem

│

├── adr

├── docs

├── prompts

├── standards

└── templates

```



\---



\# 1. Ecosystem Foundation



| ID      | Document                    | Status   |

| ------- | --------------------------- | -------- |

| ECO-001 | ESTINVEST Ecosystem Context | Draft    |

| ECO-002 | System Landscape            | Draft    |

| ECO-003 | Data Ownership Matrix       | Draft    |

| ECO-004 | Integration Architecture    | Draft    |

| ECO-005 | Repository Roles            | Draft    |

| ECO-006 | Technology Stack            | Approved |

| ECO-007 | Development Roadmap         | Approved |

| ECO-008 | Ecosystem Glossary          | Approved |

| ECO-009 | Ecosystem Principles        | Approved |

| ECO-010 | Project Status              | Active   |



\---



\# 2. Applications



| ID      | Document             | Status   |

| ------- | -------------------- | -------- |

| APP-001 | ESTINVEST Onboarding | Draft    |

| APP-002 | ESTtrade Family      | Approved |

| APP-003 | BackOffice Core      | Draft    |

| APP-004 | AI Assistant         | Draft    |

| APP-005 | Mobile App           | Draft    |

| APP-006 | Reporting Services   | Draft    |



\---



\# 3. Integration



| ID      | Document             | Status   |

| ------- | -------------------- | -------- |

| INT-001 | Integration Hub      | Approved |

| INT-002 | Banking API          | Draft    |

| INT-003 | BVB Gateway          | Approved |

| INT-004 | Depozitarul Central  | Draft    |

| INT-005 | External Markets     | Draft    |

| INT-006 | External Custodians  | Draft    |



\---



\# 4. Data



| ID       | Document                     | Status   |

| -------- | ---------------------------- | -------- |

| DATA-001 | Data Architecture Principles | Approved |

| DATA-002 | Master Data Model            | Draft    |

| DATA-003 | Reference Data Model         | Draft    |

| DATA-004 | Transactional Data Model     | Draft    |

| DATA-005 | Data Governance              | Draft    |



\---



\# 5. Security



| ID      | Document             | Status   |

| ------- | -------------------- | -------- |

| SEC-001 | IAM                  | Approved |

| SEC-002 | Authentication       | Draft    |

| SEC-003 | Authorization        | Draft    |

| SEC-004 | DORA Compliance      | Draft    |

| SEC-005 | Audit and Compliance | Draft    |



\---



\# 6. System Architecture



| ID      | Document                  | Status   |

| ------- | ------------------------- | -------- |

| SYS-001 | Ecosystem Architecture    | Approved |

| SYS-002 | Logical Architecture      | Approved |

| SYS-003 | Physical Architecture     | Draft    |

| SYS-004 | Application Communication | Draft    |

| SYS-005 | Deployment Model          | Draft    |

| SYS-006 | Technology Stack          | Draft    |



\---



\# 7. AI Context



| ID     | Document                  | Status |

| ------ | ------------------------- | ------ |

| AI-001 | AI Assistant Architecture | Draft  |

| AI-002 | Knowledge Management      | Draft  |

| AI-003 | RAG Architecture          | Draft  |

| AI-004 | AI Governance             | Draft  |



\---



\# 8. Standards



| ID          | Document                           | Status   |

| ----------- | ---------------------------------- | -------- |

| STD-AI-001  | AI Development Standard            | Approved |

| STD-API-001 | API Design Standard                | Approved |

| STD-COD-001 | Coding Standard                    | Approved |

| STD-DB-001  | Database Design Standard           | Approved |

| STD-DEV-001 | Development Workflow Standard      | Approved |

| STD-APP-001 | Application Documentation Standard | Approved |

| STD-GIT-001 | Git Workflow Standard              | Approved |

| STD-NAM-001 | Naming Convention Standard         | Approved |

| STD-SEC-001 | Security Standard                  | Approved |



\---



\# 9. ADR



| ID      | Titlu                      | Status |

| ------- | -------------------------- | ------ |

| ADR-001 | Onboarding Uses MariaDB    | ⏳      |

| ADR-002 | BackOffice Uses PostgreSQL | ⏳      |

| ADR-003 | Client Model               | ⏳      |

| ADR-004 | Unified Ledger             | ⏳      |

| ADR-005 | ESTINVEST Gateway System Boundary and Role | ✅ |

| ADR-006 | Integration Hub ↔ ESTINVEST Gateway Communication Architecture | ✅ |

| ADR-007 | ESTINVEST Gateway Technical Persistence and Reliable Delivery | ✅ |

| ADR-008 | BVB Arena Gateway Protocol Compliance | ✅ |

| ADR-009 | Gateway Connector Reuse and Arena Simulator Strategy | ✅ |

| ADR-010 | ESTINVEST Gateway v1 Functional Scope | ✅ |



\---



\# 10. Templates



| Template        | Status |

| --------------- | ------ |

| APP\_Template    | ✅      |

| ECO\_Template    | ✅      |

| ADR\_Template    | ✅      |

| STD\_Template    | ✅      |

| PROMPT\_Template | ✅      |

| README\_Template | ✅      |



\---



\# Legendă



| Simbol | Semnificație |

| ------ | ------------ |

| ✅      | Finalizat    |

| 🟡     | În revizuire |

| ⏳      | Planificat   |

| 🔴     | Suspendat    |



\---



\# Reguli



\* Fiecare document are un identificator unic.

\* Fiecare document utilizează șablonul corespunzător.

\* Toate modificările sunt urmărite prin Git.

\* Acest index trebuie actualizat ori de câte ori se creează sau se aprobă un document nou.

\* Pentru documentele care definesc explicit `Status` sau `Stare` în propriul header, indexul reproduce literal acel status; simbolurile din legendă se utilizează pentru elementele fără lifecycle status explicit în documentul propriu.



