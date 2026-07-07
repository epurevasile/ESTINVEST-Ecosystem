\# ECO-001 – ESTINVEST Ecosystem Context



| Proprietate | Valoare |

|-------------|----------|

| Cod document | ECO-001 |

| Titlu | ESTINVEST Ecosystem Context |

| Categorie | 00\_Ecosystem\_Foundation |

| Versiune | 1.0 |

| Stare | Draft |

| Autor | Estinvest \& ChatGPT |



\---



\# 1. Scopul documentului



Acest document definește arhitectura funcțională de nivel înalt a ecosistemului software ESTINVEST.



El stabilește:



\- aplicațiile care compun ecosistemul;

\- responsabilitatea fiecărei aplicații;

\- proprietatea asupra datelor (Data Ownership);

\- fluxurile principale de informații;

\- principiile comune de integrare.



Acest document reprezintă fundamentul tuturor proiectelor software dezvoltate în cadrul ESTINVEST.



\---



\# 2. Viziunea ecosistemului



Ecosistemul ESTINVEST este alcătuit din aplicații independente, fiecare având o responsabilitate clar definită.



Nicio aplicație nu trebuie să dubleze responsabilitatea altei aplicații.



Fiecare informație trebuie să aibă un singur proprietar (Single Source of Truth).



\---



\# 3. Componentele ecosistemului



\## 3.1 ESTINVEST Test (Onboarding)



Tip:



Aplicație publică.



Utilizatori:



\- clienți noi;

\- operatori KYC.



Responsabilități:



\- înregistrarea clientului;

\- colectarea documentelor;

\- identificarea clientului;

\- verificarea KYC;

\- aprobarea inițială;

\- generarea dosarului electronic al clientului.



Nu gestionează:



\- ordine;

\- tranzacții;

\- solduri;

\- portofolii;

\- contabilitate operațională.



\---



\## 3.2 ESTINVEST BackOffice Core



Tip:



Aplicație internă.



Utilizatori:



\- BackOffice;

\- Brokeri;

\- Traderi;

\- AML Officer;

\- Control Intern;

\- Auditori;

\- Administratori.



Responsabilități:



\- administrarea clienților activi;

\- administrarea conturilor;

\- administrarea portofoliilor;

\- administrarea ordinelor;

\- administrarea tranzacțiilor;

\- Unified Ledger;

\- settlement;

\- reconciliere;

\- raportare;

\- audit;

\- administrare operațională.



BackOffice reprezintă sistemul oficial de evidență operațională (System of Record).



\---



\## 3.3 ESTtrade



Tip:



Platformă de tranzacționare.



Utilizatori:



\- clienți;

\- brokeri;

\- agenți.



Responsabilități:



\- vizualizarea portofoliului;

\- vizualizarea soldurilor;

\- introducerea ordinelor;

\- urmărirea execuțiilor;

\- rapoarte pentru client;

\- notificări.



ESTtrade nu administrează datele oficiale ale clientului.



Aceste informații sunt furnizate de BackOffice.



\---



\# 4. Sisteme externe



Ecosistemul comunică cu:



\- Bursa de Valori București (Gateway BVB);

\- Depozitarul Central;

\- instituții bancare;

\- servicii de autentificare;

\- servicii de notificare;

\- servicii e-mail;

\- servicii SMS;

\- alte sisteme autorizate.



\---



\# 5. Proprietatea asupra datelor (Data Ownership)



| Tip de date | Proprietar |

|--------------|------------|

| Cerere de deschidere cont | Onboarding |

| Documente KYC | Onboarding |

| Status KYC | Onboarding |

| Client activ | BackOffice |

| Cont investiții | BackOffice |

| Portofolii | BackOffice |

| Solduri cash | BackOffice |

| Poziții instrumente financiare | BackOffice |

| Unified Ledger | BackOffice |

| Ordine | BackOffice |

| Tranzacții | BackOffice |

| Settlement | BackOffice |

| Audit | BackOffice |

| Date afișate clientului | ESTtrade |



\---



\# 6. Fluxul principal al clientului



```text

Client

&#x20;   │

&#x20;   ▼

ESTINVEST Test

(Onboarding)

&#x20;   │

&#x20;   ▼

Validare KYC

&#x20;   │

&#x20;   ▼

Transfer controlat

&#x20;   │

&#x20;   ▼

BackOffice Core

&#x20;   │

&#x20;   ▼

Creare Client

&#x20;   │

&#x20;   ▼

Creare Cont Principal

&#x20;   │

&#x20;   ▼

Activare Client

&#x20;   │

&#x20;   ▼

ESTtrade

```



\---



\# 7. Principiul "Single Source of Truth"



Fiecare informație trebuie administrată într-o singură aplicație.



Exemple:



Date KYC

→ Onboarding



Solduri

→ BackOffice



Portofolii

→ BackOffice



Ordine

→ BackOffice



Vizualizare portofoliu

→ ESTtrade



\---



\# 8. Principii de integrare



Toate comunicațiile dintre aplicații se realizează exclusiv prin API-uri standard.



Nu este permis accesul direct la baza de date a unei alte aplicații.



Integrarea trebuie să respecte:



\- REST API;

\- JSON;

\- autentificare securizată;

\- versionare API;

\- audit.



\---



\# 9. Principii comune



Întregul ecosistem respectă:



\- Single Source of Truth;

\- Unified Ledger;

\- Immutable Financial History;

\- Auditabilitate completă;

\- RBAC;

\- Least Privilege;

\- Segregation of Duties;

\- Four-Eyes Principle;

\- Defense in Depth;

\- DORA Compliance.



\---



\# 10. Model conceptual



```text

&#x20;                CLIENT

&#x20;                   │

&#x20;       ┌───────────┴───────────┐

&#x20;       │                       │

&#x20;       ▼                       ▼

ESTINVEST Test           ESTtrade

(Onboarding)          (Trading Platform)

&#x20;       │                       ▲

&#x20;       │                       │

&#x20;       ▼                       │

&#x20;  BackOffice Core──────────────┘

&#x20;       │

&#x20;       ├────────► Gateway BVB

&#x20;       │

&#x20;       ├────────► Depozitarul Central

&#x20;       │

&#x20;       ├────────► Bănci

&#x20;       │

&#x20;       └────────► Servicii externe

```



\---



\# 11. Obiective de arhitectură



Ecosistemul trebuie să fie:



\- modular;

\- scalabil;

\- sigur;

\- auditabil;

\- extensibil;

\- independent tehnologic;

\- ușor de integrat cu noi aplicații.



\---



\# 12. Evoluția ecosistemului



Arhitectura permite adăugarea ulterioară a:



\- aplicației mobile;

\- CRM;

\- AI Assistant;

\- portal parteneri;

\- raportare avansată;

\- servicii Open Banking;

\- integrare cu piețe internaționale.



\---



\# 13. Documente derivate



Acest document constituie baza pentru:



\- ECO-002 – System Landscape

\- ECO-003 – Data Ownership Matrix

\- ECO-004 – Integration Architecture

\- ECO-005 – API Standards

\- ECO-006 – Security Architecture



și pentru toate proiectele:



\- ESTINVEST Test

\- ESTINVEST BackOffice Core

\- ESTtrade



\---



\# Istoric versiuni



| Versiune | Data | Modificări |

|----------|------|------------|

| 1.0 | Iulie 2026 | Prima versiune |

