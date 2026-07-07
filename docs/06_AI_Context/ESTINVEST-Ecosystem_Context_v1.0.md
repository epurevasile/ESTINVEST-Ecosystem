\# ESTINVEST Ecosystem Context v1.0



| Proprietate | Valoare |

|-------------|----------|

| Document | ESTINVEST-Ecosystem\_Context\_v1.0 |

| Versiune | 1.0 |

| Data | Iulie 2026 |

| Status | Active |

| Autor | ESTINVEST \& ChatGPT |



\---



\# 1. Scopul documentului



Acest document reprezintă contextul oficial al proiectului \*\*ESTINVEST Ecosystem\*\*.



Scopul lui este:



\- reluarea rapidă a proiectului într-o conversație nouă;

\- păstrarea deciziilor importante;

\- furnizarea contextului necesar pentru ChatGPT, Cursor și Claude;

\- evitarea pierderii informațiilor între sesiuni.



Acest document trebuie actualizat la finalul fiecărei etape importante.



\---



\# 2. Obiectivul proiectului



Se dezvoltă o platformă software enterprise destinată activității unei societăți de servicii de investiții financiare (SSIF).



Platforma este construită ca un ecosistem format din aplicații independente care comunică prin API-uri standard și respectă reguli comune de arhitectură.



\---



\# 3. Componentele ecosistemului



\## 3.1 ESTINVEST Onboarding



Aplicația publică utilizată pentru:



\- înregistrarea clienților;

\- colectarea documentelor;

\- verificarea identității;

\- procesul KYC;

\- aprobarea inițială.



Implementare actuală:



\- Backend JavaScript

\- Frontend JavaScript

\- MariaDB



Repository separat.



\---



\## 3.2 ESTINVEST BackOffice Core



Aplicația internă.



Responsabilă pentru:



\- administrarea clienților activi;

\- conturi;

\- portofolii;

\- ordine;

\- tranzacții;

\- Unified Ledger;

\- settlement;

\- reconciliere;

\- audit.



Implementare:



\- NestJS

\- PostgreSQL

\- Prisma



Repository separat.



\---



\## 3.3 ESTtrade



Platforma de tranzacționare destinată clienților și brokerilor.



Consumă informații publicate de BackOffice.



Nu reprezintă sursa oficială a datelor operaționale.



Repository separat.



\---



\# 4. Repository-ul ESTINVEST-Ecosystem



Acest proiect NU conține codul aplicațiilor.



Conține exclusiv:



\- documentație;

\- standarde;

\- prompturi;

\- șabloane;

\- Architecture Decision Records (ADR).



Structura aprobată:



```text

ESTINVEST-Ecosystem

│

├── adr

├── docs

├── prompts

├── standards

├── templates

└── README.md

```



\---



\# 5. Structura documentației



\## docs



Conține documentația de arhitectură.



Structura actuală:



```text

00\_Ecosystem\_Foundation

01\_Applications

02\_Integration

03\_Data

04\_Security

05\_Architecture

06\_AI\_Context

```



\---



\## standards



Conține standardele proiectului.



Structura actuală:



```text

00\_Project\_Methodology

AI

Coding

Documentation

Git

Naming

Security

```



\---



\## prompts



Conține prompturi reutilizabile pentru AI.



Structura actuală:



```text

00\_Guides

01\_Audit

02\_Architecture

03\_Implementation

04\_Refactoring

05\_Testing

06\_Documentation

```



\---



\## templates



Conține șabloane reutilizabile.



Exemple:



\- ADR

\- APP

\- ECO

\- PROMPT

\- STD



\---



\## adr



Conține toate Architecture Decision Records.



\---



\# 6. Documente elaborate



\## Ecosystem



\- ECO-001 – Ecosystem Context

\- ECO-002 – System Landscape

\- ECO-003 – Data Ownership Matrix

\- ECO-004 – Integration Architecture



\---



\## Applications



\- APP-001 – ESTINVEST Onboarding



\---



\## Standards



\- STD-000 – Project Working Method

\- STD-AI-000 – AI Working Method

\- STD-AI-001 – AI Collaboration Guide



\---



\## ADR



Au fost create primele documente ADR ca structură.



Conținutul va fi completat ulterior.



\---



\# 7. Principii aprobate



Au fost aprobate următoarele principii:



\- Business First

\- Architecture First

\- Documentation First

\- API First

\- Security by Design

\- Single Source of Truth

\- Data Ownership

\- Unified Ledger

\- Auditabilitate completă

\- RBAC

\- Least Privilege

\- Defense in Depth



\---



\# 8. Modelul ecosistemului



```text

&#x20;                CLIENT

&#x20;                   │

&#x20;                   ▼

&#x20;       ESTINVEST Onboarding

&#x20;                   │

&#x20;               REST API

&#x20;                   │

&#x20;                   ▼

&#x20;      ESTINVEST BackOffice Core

&#x20;                   │

&#x20;       ┌───────────┼─────────────┐

&#x20;       │           │             │

&#x20;       ▼           ▼             ▼

&#x20;   ESTtrade   Reporting      Notification

&#x20;       │

&#x20;       ▼

&#x20;  Gateway BVB

&#x20;       │

&#x20;       ▼

Depozitarul Central

```



\---



\# 9. Rolurile AI



\## ChatGPT



Rol:



Enterprise Architect



Responsabilități:



\- arhitectură;

\- standarde;

\- documentație;

\- strategie;

\- integrare.



\---



\## Cursor



Rol:



Senior Software Engineer



Responsabilități:



\- analiza codului;

\- implementare;

\- refactorizare;

\- documentație tehnică.



\---



\## Claude



Rol:



Senior Code Reviewer



Responsabilități:



\- audit tehnic;

\- code review;

\- optimizare.



\---



\# 10. Metodologia de lucru



Ordinea aprobată:



```text

Business



↓



Architecture



↓



Standards



↓



Data Model



↓



API Design



↓



Implementation



↓



Testing



↓



Deployment

```



Nicio implementare importantă nu începe înainte de existența documentației de arhitectură.



\---



\# 11. Stadiul proiectului



Finalizat:



\- structura Enterprise Architecture;

\- structura repository-ului;

\- metodologia de lucru;

\- primele documente Ecosystem;

\- primele standarde.



În curs:



\- definirea standardelor;

\- biblioteca de prompturi.



Neînceput:



\- auditul aplicației ESTINVEST Onboarding;

\- auditul BackOffice;

\- integrarea completă dintre aplicații.



\---



\# 12. Următoarea etapă



Obiectivul imediat este auditarea arhitecturală a aplicației ESTINVEST Onboarding.



Auditul NU va modifica codul.



Prima etapă este inventarierea completă a aplicației:



\- module;

\- API-uri;

\- baza de date;

\- ecrane;

\- roluri;

\- fluxuri;

\- integrări externe.



Documentele vor fi generate cu ajutorul Cursor și analizate ulterior în raport cu arhitectura ecosistemului.



\---



\# 13. Decizii importante



Au fost aprobate următoarele decizii:



\- aplicațiile rămân în repository-uri separate;

\- documentația comună este centralizată în ESTINVEST-Ecosystem;

\- prompturile sunt tratate ca artefacte oficiale;

\- standardele sunt separate de documentație;

\- ADR-urile sunt păstrate într-un folder dedicat.



\---



\# 14. Recomandări pentru reluarea proiectului



La începutul unei noi sesiuni:



1\. Se încarcă acest document.

2\. Se indică etapa la care s-a ajuns.

3\. Se precizează obiectivul sesiunii.

4\. Se continuă exclusiv pe baza documentelor existente.



\---



\# 15. Obiectiv pe termen lung



Construirea unei platforme software enterprise moderne pentru ESTINVEST, bazată pe:



\- aplicații independente;

\- standarde comune;

\- integrare prin API;

\- documentație completă;

\- metodologie unitară;

\- colaborare eficientă între oameni și instrumente AI.



Acest document reprezintă punctul oficial de continuitate al proiectului și trebuie actualizat la finalul fiecărei etape majore.



\---



\# Istoric versiuni



| Versiune | Data | Modificări |

|----------|------|------------|

| 1.0 | Iulie 2026 | Prima versiune |

