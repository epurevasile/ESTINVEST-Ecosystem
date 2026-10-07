\# ECO-007 – Development Roadmap



| Proprietate | Valoare |

|-------------|----------|

| Document ID | ECO-007 |

| Titlu | Development Roadmap |

| Categorie | Ecosystem Foundation |

| Versiune | 1.1 |

| Status | Approved |

| Clasificare | Internal |

| Repository | ESTINVEST-Ecosystem |

| Owner | Enterprise Architecture |

| Ultima actualizare | 2026-10-06 |



\---



\# Change Log



| Versiune | Data | Modificări |

|----------|------|------------|

| 1.0 | 2026-07-07 | Prima versiune |

| 1.1 | 2026-10-06 | Aliniere roadmap Gateway cu baseline-ul arhitectural ADR-005…ADR-010 |



\---



\# 1. Purpose



Acest document definește direcția de dezvoltare a \*\*ESTINVEST Ecosystem\*\* și stabilește ordinea de implementare a aplicațiilor și serviciilor care compun ecosistemul.



Roadmap-ul oferă o imagine unitară asupra etapelor de dezvoltare și reprezintă documentul de referință pentru planificarea proiectelor.



\---



\# 2. Vision



Obiectivul ecosistemului este dezvoltarea unei platforme integrate pentru activitatea unui SSIF, care să acopere întregul ciclu de viață al relației cu clientul:



\- onboarding;

\- administrarea clienților;

\- tranzacționare;

\- operațiuni back-office;

\- raportare;

\- integrare cu infrastructura pieței de capital;

\- servicii AI.



\---



\# 3. Ecosystem Components



| Nr. | Componentă | Status |

|----:|------------|--------|

| 01 | ESTINVEST Onboarding | În teste |

| 02 | ESTINVEST BackOffice Core | În dezvoltare |

| 03 | ESTtrade | În proiectare |

| 04 | ESTINVEST Gateway | Planificat |

| 05 | Arena Gateway Simulator | Planificat |

| 06 | Reporting Service | Planificat |

| 07 | Notification Service | Planificat |

| 08 | AI Services | În dezvoltare |

| 09 | Identity \& Access Management | Planificat |

| 10 | Integration Services | Planificat |

| 11 | Shared Libraries | În dezvoltare continuă |

| 12 | Enterprise Documentation | Activ |

| 13 | Monitoring \& Operations | Planificat |



\---



\# 4. Development Principles



Toate componentele respectă următoarele principii:



\- Architecture First;

\- Documentation First;

\- Security by Design;

\- API First;

\- Modular Design;

\- Reuse Before Build;

\- AI Assisted Development.



\---



\# 5. Development Phases



\## Phase 1 – Foundation



Obiective:



\- definirea arhitecturii enterprise;

\- standardizarea documentației;

\- definirea tehnologiilor;

\- stabilirea metodologiei.



\*\*Status:\*\* Finalizat.



\---



\## Phase 2 – Core Business Applications



Obiective:



\- finalizarea Onboarding;

\- dezvoltarea BackOffice Core;

\- dezvoltarea ESTtrade.



Prioritate: \*\*Critică\*\*



\---



\## Phase 3 – Market Connectivity



Obiective:



\- ESTINVEST Gateway;

\- Arena Gateway Simulator;

\- integrarea cu BVB;

\- integrarea cu Depozitarul Central;

\- integrarea cu partenerii externi (ex. KBC).



Pentru conectivitatea BVB, baseline-ul arhitectural este definit de ADR-005…ADR-010. ESTINVEST Gateway este componenta specializată de market connectivity, iar Arena Gateway Simulator este test double în ecosistemul de development/test al ESTINVEST Gateway. Statusurile din acest roadmap descriu stadiul de implementare și nu modifică statusul deciziilor arhitecturale aprobate.



\---



\## Phase 4 – Shared Services



Obiective:



\- Notification Service;

\- Reporting Service;

\- Identity \& Access Management;

\- Shared Libraries.



\---



\## Phase 5 – AI \& Automation



Obiective:



\- integrarea serviciilor AI;

\- automatizarea documentației;

\- asistenți AI pentru utilizatori și dezvoltatori;

\- RAG pentru documentația internă.



\---



\## Phase 6 – Operations \& Monitoring



Obiective:



\- monitorizare centralizată;

\- backup și recovery;

\- observabilitate;

\- dashboard-uri operaționale;

\- managementul incidentelor.



\---



\# 6. Current Priorities



\## Prioritatea 1



Finalizarea aplicației \*\*ESTINVEST Onboarding\*\*.



\---



\## Prioritatea 2



Dezvoltarea \*\*ESTINVEST BackOffice Core\*\*.



\---



\## Prioritatea 3



Dezvoltarea \*\*ESTtrade\*\*.



\---



\## Prioritatea 4



Integrarea cu infrastructura pieței de capital.



\---



\# 7. Long-Term Objectives



Pe termen lung, ecosistemul urmărește:



\- automatizarea proceselor operaționale;

\- reducerea intervențiilor manuale;

\- integrarea serviciilor AI;

\- reutilizarea componentelor comune;

\- scalabilitate și disponibilitate ridicată.



\---



\# 8. Success Criteria



Roadmap-ul este considerat implementat atunci când:



\- toate aplicațiile folosesc standardele ecosistemului;

\- documentația este completă și actualizată;

\- integrarea dintre aplicații este funcțională;

\- procesele operaționale sunt automatizate;

\- serviciile AI sunt integrate în fluxurile de lucru.



\---



\# 9. Governance



Roadmap-ul este revizuit periodic de echipa de arhitectură și actualizat în funcție de evoluția proiectelor și de cerințele de business.



\---



\# 10. Related Documents



\- ECO-001 – Ecosystem Context

\- ECO-002 – System Landscape

\- ECO-004 – Integration Architecture

\- ECO-006 – Technology Stack

\- STD-APP-001 – Application Documentation Standard

\- ADR-005 – ESTINVEST Gateway System Boundary and Role

\- ADR-006 – Integration Hub ↔ ESTINVEST Gateway Communication Architecture

\- ADR-007 – ESTINVEST Gateway Technical Persistence and Reliable Delivery

\- ADR-008 – BVB Arena Gateway Protocol Compliance

\- ADR-009 – Gateway Connector Reuse and Arena Simulator Strategy

\- ADR-010 – ESTINVEST Gateway v1 Functional Scope



\---



\# Version History



| Versiune | Data | Autor | Modificări |

|----------|------|-------|------------|

| 1.0 | 2026-07-07 | ESTINVEST \& ChatGPT | Prima versiune |

| 1.1 | 2026-10-06 | ESTINVEST \& ChatGPT | Aliniere roadmap Gateway cu baseline-ul arhitectural ADR-005…ADR-010 |

