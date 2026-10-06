\# ECO-004 – Integration Architecture



| Proprietate | Valoare |

|-------------|----------|

| Cod document | ECO-004 |

| Titlu | Integration Architecture |

| Categorie | 00\_Ecosystem\_Foundation |

| Versiune | 1.0 |

| Stare | Draft |

| Autor | Estinvest \& ChatGPT |



\---



\# 1. Scopul documentului



Acest document definește arhitectura de integrare a ecosistemului software ESTINVEST.



El stabilește:



\- modul în care aplicațiile comunică;

\- tipurile de integrare utilizate;

\- responsabilitățile fiecărei aplicații;

\- regulile generale pentru schimbul de informații.



\---



\# 2. Principii de integrare



Toate aplicațiile comunică exclusiv prin interfețe bine definite.



Nu este permis:



\- accesul direct la baza de date a altei aplicații;

\- modificarea datelor administrate de o altă aplicație;

\- ocolirea API-urilor oficiale.



\---



\# 3. Modelul de integrare



```text

\&#x20;                   CLIENT

\&#x20;                      │

\&#x20;                      ▼

\&#x20;             ESTINVEST TEST

\&#x20;                (Onboarding)

\&#x20;                      │

\&#x20;                REST API

\&#x20;                      │

\&#x20;                      ▼

\&#x20;         ESTINVEST BackOffice Core

\&#x20;                      │

\&#x20;                      ├──────────► ESTtrade

\&#x20;                      ├──────────► Reporting

\&#x20;                      ├──────────► Notification

\&#x20;                      │

\&#x20;                      ▼

\&#x20;              Integration Hub

\&#x20;                      │

\&#x20;                      ▼

\&#x20;             ESTINVEST Gateway

\&#x20;                      │

\&#x20;                      ▼

\&#x20;          BVB / Arena Gateway

```

Notă: traseul business este BackOffice → Integration Hub → ESTINVEST Gateway → BVB / Arena Gateway. ESTtrade, Reporting și Notification sunt componente distincte consumatoare ale BackOffice; Integration Hub nu este copil al Reporting.



\---



\# 4. Integrarea Onboarding → BackOffice



Scop:



Transferul unui client aprobat.



Flux:



1\. Clientul finalizează înregistrarea.

2\. Operatorul validează dosarul.

3\. Onboarding transmite clientul către BackOffice.

4\. BackOffice validează datele.

5\. BackOffice creează:

&#x20;  - Customer;

&#x20;  - Cont principal;

&#x20;  - Portofolii;

&#x20;  - profil operațional.



\---



\# 5. Integrarea BackOffice → ESTtrade



BackOffice publică informațiile oficiale.



ESTtrade consumă:



\- client;

\- portofolii;

\- solduri;

\- ordine;

\- execuții;

\- notificări.



ESTtrade nu modifică direct datele operaționale.



Pentru trading / operații business, traseul este:



ESTtrade → BackOffice → Integration Hub → ESTINVEST Gateway → BVB / Arena Gateway



ESTtrade poate consuma direct din ESTINVEST Gateway numai market data read-only, conform ADR-005. Această excepție nu autorizează trading direct.



\---



\# 6. Integrarea cu ESTINVEST Gateway / BVB Arena Gateway



Pentru fluxurile business, traseul canonic este:



BackOffice → Integration Hub → ESTINVEST Gateway → BVB / Arena Gateway



Retur:



BVB / Arena Gateway → ESTINVEST Gateway → Integration Hub → BackOffice



BackOffice nu comunică direct cu ESTINVEST Gateway pentru fluxurile business. Integration Hub rămâne boundary-ul BackOffice față de sistemele externe.



Integration Hub comunică cu ESTINVEST Gateway printr-un contract intern ESTINVEST. ESTINVEST Gateway izolează Arena protocol / XML / DTOs de BackOffice; Arena nu este expusă ca model aplicațional BackOffice.



ESTINVEST Gateway transmite către Integration Hub ca evenimente/notificări asincrone, conform ADR-006:



\- confirmări;

\- execuții;

\- anulări;

\- respingeri.



BackOffice actualizează Unified Ledger. ESTINVEST Gateway nu este System of Record pentru Order / Trade.



\---



\# 7. Integrarea cu Depozitarul Central



BackOffice schimbă informații privind:



\- settlement;

\- poziții;

\- corporate actions;

\- reconciliere.



Implementarea exactă va respecta specificațiile oficiale ale Depozitarului Central.



\---



\# 8. Integrarea cu instituțiile bancare



BackOffice gestionează:



\- alimentări;

\- retrageri;

\- reconciliere bancară.



Toate operațiunile generează înregistrări în Unified Ledger.



\---



\# 9. Contractele API



Toate API-urile trebuie să respecte:



\- REST;

\- HTTPS;

\- JSON;

\- versionare (`/api/v1`);

\- autentificare;

\- autorizare;

\- validare.



Documentarea API-urilor se realizează prin OpenAPI (Swagger).



\---



\# 10. Tipuri de integrare



| Tip | Utilizare |

|------|-----------|

| REST API | Comunicare între aplicații |

| FIX | Gateway BVB |

| HTTPS | Comunicare securizată |

| Batch Import/Export | Procese programate, unde este necesar |

| WebSocket | Actualizări în timp real (ESTtrade) |



\---



\# 11. Gestionarea erorilor



Toate integrările trebuie să:



\- returneze coduri standard HTTP;

\- furnizeze mesaje de eroare clare;

\- permită retransmiterea controlată;

\- înregistreze toate erorile în jurnalul aplicației.



\---



\# 12. Securitatea integrărilor



Toate comunicațiile utilizează:



\- TLS;

\- JWT sau OAuth2, în funcție de scenariu;

\- RBAC;

\- Audit Trail.



Accesul este acordat pe principiul Least Privilege.



\---



\# 13. Principii operaționale



\- API First

\- Loose Coupling

\- High Cohesion

\- Single Source of Truth

\- Data Ownership

\- Idempotență pentru operațiunile critice

\- Trasabilitate completă



\---



\# 14. Evoluția arhitecturii



Modelul permite integrarea ulterioară cu:



\- aplicația mobilă;

\- CRM;

\- AI Assistant;

\- Open Banking;

\- piețe internaționale;

\- servicii suplimentare de raportare.



\---



\# 15. Relația cu celelalte documente



Acest document completează:



\- ECO-001 – Ecosystem Context

\- ECO-002 – System Landscape

\- ECO-003 – Data Ownership Matrix



și constituie baza pentru:



\- ECO-005 – Application Responsibilities

\- APP-001 – ESTINVEST Test

\- APP-002 – BackOffice

\- APP-003 – ESTtrade



\---



\# Istoric versiuni



| Versiune | Data | Modificări |

|----------|------|------------|

| 1.0 | Iulie 2026 | Prima versiune |

