# ECO-002 – System Landscape

| Proprietate | Valoare |
|-------------|----------|
| Document ID | ECO-002 |
| Titlu | System Landscape |
| Categorie | 00_Ecosystem_Foundation |
| Versiune | 1.1 |
| Status | Draft |
| Clasificare | Internal |
| Owner | Enterprise Architecture |
| Repository | ESTINVEST-Ecosystem |
| Ultima actualizare | 07 Iulie 2026 |
| Autor | ESTINVEST & ChatGPT |

---

# Change Log

| Versiune | Data | Modificări |
|-----------|------|------------|
| 1.0 | Iulie 2026 | Prima versiune |
| 1.1 | Iulie 2026 | Actualizare arhitecturală, completarea catalogului aplicațiilor, clarificarea responsabilităților și alinierea cu arhitectura Ecosystem |

---

# 1. Scopul documentului

Acest document descrie arhitectura de ansamblu a platformei **ESTINVEST Ecosystem**.

Documentul identifică toate aplicațiile, serviciile și sistemele externe care participă la procesele de business și definește responsabilitatea fiecărei componente.

System Landscape reprezintă documentul de referință pentru întreaga arhitectură software și constituie baza tuturor documentelor de proiectare și implementare.

---

# 2. Domeniul de aplicare

Documentul acoperă:

- aplicațiile de business;
- serviciile comune;
- componentele tehnice;
- sistemele externe;
- relațiile dintre aplicații;
- responsabilitatea fiecărei componente;
- principiile generale de integrare.

Nu descrie implementarea internă a fiecărei aplicații. Aceste informații sunt documentate separat în documentele dedicate fiecărei aplicații.

---

# 3. Audiență

Acest document este destinat:

- Enterprise Architects;
- Software Architects;
- Product Owners;
- Project Managers;
- dezvoltatorilor;
- echipei QA;
- echipei DevOps;
- auditorilor interni și externi.

---

# 4. Documente asociate

Acest document este completat de:

- ECO-001 – Ecosystem Context
- ECO-003 – Data Ownership Matrix
- ECO-004 – Integration Architecture
- APP-xxx – Documentația aplicațiilor
- ADR-xxx – Architecture Decision Records
- STD-xxx – Standards

---

# 5. Viziunea ecosistemului

Platforma software ESTINVEST este construită ca un ecosistem de aplicații independente, fiecare având responsabilități bine definite.

Aplicațiile comunică exclusiv prin interfețe standardizate și nu își dublează responsabilitățile.

Fiecare aplicație poate evolua independent fără a afecta celelalte componente ale ecosistemului.

Arhitectura urmărește:

- modularitate;
- scalabilitate;
- disponibilitate ridicată;
- securitate;
- auditabilitate completă;
- mentenanță simplificată;
- posibilitatea extinderii fără modificări majore.

---

# 6. Principii arhitecturale

Întregul ecosistem respectă următoarele principii:

## Business First

Arhitectura este construită în jurul proceselor de business.

---

## Documentation First

Nicio componentă importantă nu este implementată înainte de existența documentației de arhitectură.

---

## API First

Comunicarea dintre aplicații se realizează exclusiv prin API-uri bine definite.

---

## Single Responsibility

Fiecare aplicație are un singur scop principal.

---

## Single Source of Truth

Fiecare categorie de date are un singur proprietar.

---

## Data Ownership

Proprietatea datelor este clar definită și documentată.

---

## Security by Design

Securitatea este integrată în arhitectură încă din faza de proiectare.

---

## Audit by Design

Toate operațiile importante sunt auditate.

---

## Loose Coupling

Aplicațiile sunt cât mai independente una de alta.

---

## High Cohesion

Funcționalitățile înrudite sunt grupate în aceeași aplicație.

---

# 7. Componentele ecosistemului

Platforma este alcătuită din patru categorii principale de componente:

- aplicații de business;
- servicii comune;
- infrastructură tehnică;
- sisteme externe.

---

# 7.1 Aplicații de business

| Aplicație | Rol | Status |
|------------|-----|--------|
| ESTINVEST Onboarding | Onboarding și KYC | Existing |
| ESTINVEST BackOffice Core | Sistem operațional principal | In Development |
| ESTtrade | Platformă de tranzacționare | In Development |
| Reporting Service | Rapoarte și statistici | Planned |
| Notification Service | Email, SMS, Push | Planned |
| AI Services | Asistență operațională și Knowledge Base | Planned |

---

# 7.2 Servicii comune

| Serviciu | Rol |
|-----------|-----|
| Identity & Access Management | Autentificare și autorizare |
| Audit Service | Centralizarea jurnalelor de audit |
| Document Service | Administrarea documentelor |
| Scheduler | Procese automate și job-uri programate |
| Configuration Service | Administrarea configurațiilor |
| Logging Service | Centralizarea logurilor |
| Monitoring Service | Monitorizarea platformei |

---

# 7.3 Infrastructură

Platforma utilizează următoarele componente tehnologice:

- Docker;
- PostgreSQL;
- MariaDB;
- Git;
- GitHub;
- Cloudflare;
- Reverse Proxy;
- Backup Services;
- Monitoring & Observability.

---

# 7.4 Sisteme externe

| Sistem | Rol |
|----------|-----|
| Bursa de Valori București | Executarea ordinelor |
| Depozitarul Central | Settlement și custodie |
| Instituții bancare | Transferuri de numerar |
| Furnizori Email | Comunicarea cu clienții |
| Furnizori SMS | Notificări |
| Cloudflare | Protecție și acces public |


----

# 8. Catalogul aplicațiilor

Acest capitol descrie toate aplicațiile care fac parte din ESTINVEST Ecosystem și responsabilitatea fiecăreia.

---

# 8.1 ESTINVEST Onboarding

## Scop

Aplicația publică destinată înregistrării și activării clienților noi.

## Responsabilități

- înregistrarea clientului;
- colectarea documentelor;
- verificarea identității;
- procesul KYC;
- evaluarea inițială AML;
- aprobarea deschiderii relației contractuale;
- transferul clientului către BackOffice.

## Nu administrează

- ordine;
- tranzacții;
- portofolii;
- solduri;
- contabilitate.

## System of Record

Nu.

BackOffice devine proprietarul oficial al datelor după aprobarea clientului.

## Repository

ESTINVEST-Onboarding

## Tehnologie

- JavaScript
- MariaDB

## Status

Existing

---

# 8.2 ESTINVEST BackOffice Core

## Scop

Sistemul operațional principal al societății.

Reprezintă sursa oficială a tuturor datelor operaționale.

## Responsabilități

- administrarea clienților;
- administrarea conturilor;
- administrarea portofoliilor;
- administrarea ordinelor;
- administrarea tranzacțiilor;
- Unified Ledger;
- settlement;
- reconciliere;
- raportări;
- audit;
- administrarea utilizatorilor și rolurilor.

## System of Record

Da.

BackOffice este singura aplicație care deține datele operaționale oficiale.

## Repository

ESTINVEST-BackOffice

## Tehnologie

- NestJS
- PostgreSQL
- Prisma

## Status

In Development

---

# 8.3 ESTtrade

## Scop

Platforma utilizată de clienți și brokeri pentru tranzacționare.

## Responsabilități

- autentificarea utilizatorilor;
- introducerea ordinelor;
- afișarea portofoliului;
- afișarea soldurilor;
- afișarea istoricului tranzacțiilor;
- notificarea utilizatorilor.

## Nu administrează

- evidența contabilă;
- settlement;
- reconcilierea;
- date operaționale oficiale.

## System of Record

Nu.

Consumă exclusiv servicii furnizate de BackOffice Core.

## Repository

ESTtrade

## Tehnologie

Va fi stabilită în documentația aplicației.

## Status

In Development

---

# 8.4 Gateway Connector

## Scop

Interfața dintre aplicațiile ESTINVEST și infrastructura Bursei de Valori București.

## Responsabilități

- transmiterea ordinelor;
- recepționarea execuțiilor;
- sincronizarea statusurilor;
- gestionarea conexiunilor principale și de rezervă.

## System of Record

Nu.

## Repository

ESTINVEST-Gateway

## Status

Planned

---

# 8.5 Gateway Simulator

## Scop

Simularea completă a Gateway-ului BVB pentru dezvoltare și testare.

## Responsabilități

- simularea ordinelor;
- simularea execuțiilor;
- simularea erorilor;
- testarea aplicațiilor fără conectare la infrastructura BVB.

## Status

Planned

---

# 8.6 Reporting Service

## Scop

Generarea rapoartelor operaționale și manageriale.

## Responsabilități

- rapoarte pentru management;
- rapoarte către autorități;
- statistici;
- exporturi.

## Status

Planned

---

# 8.7 Notification Service

## Scop

Centralizarea tuturor notificărilor generate de ecosistem.

## Responsabilități

- Email;
- SMS;
- Push Notifications;
- notificări interne.

## Status

Planned

---

# 8.8 AI Services

## Scop

Servicii de inteligență artificială utilizate de aplicațiile ecosistemului.

## Responsabilități

- Knowledge Base;
- RAG;
- AI Assistant;
- analiză documente;
- căutare semantică;
- suport operațional.

## Status

Planned

---

# 9. Relațiile dintre aplicații

Responsabilitățile aplicațiilor sunt strict delimitate.

```text
                 CLIENT
                    │
                    ▼
        ESTINVEST Onboarding
                    │
                    ▼
      ESTINVEST BackOffice Core
                    │
      ┌─────────────┼──────────────┐
      │             │              │
      ▼             ▼              ▼
 ESTtrade     Reporting      Notification
      │
      ▼
 Gateway Connector
      │
      ▼
 Bursa de Valori București
      │
      ▼
 Depozitarul Central
```

---

# 10. Principiile de comunicare

Aplicațiile comunică exclusiv prin interfețe bine definite.

Nu este permis accesul direct la baza de date a unei alte aplicații.

Fiecare aplicație publică propriile servicii și consumă serviciile celorlalte aplicații prin API-uri standardizate.

Principiile utilizate sunt:

- API First;
- Loose Coupling;
- High Cohesion;
- Single Source of Truth;
- Contract Based Integration.

---

# 11. Responsabilitatea datelor

Proprietatea datelor este distribuită astfel:

| Tip date | Proprietar |
|----------|------------|
| Date KYC | ESTINVEST Onboarding |
| Clienți activi | BackOffice Core |
| Conturi | BackOffice Core |
| Portofolii | BackOffice Core |
| Ordine | BackOffice Core |
| Tranzacții | BackOffice Core |
| Ledger | BackOffice Core |
| Rapoarte | Reporting Service |
| Notificări | Notification Service |

Detalierea completă a responsabilităților este prezentată în documentul **ECO-003 – Data Ownership Matrix**.


-----

# 12. Fluxurile principale de business

Acest capitol prezintă fluxurile operaționale principale din cadrul ESTINVEST Ecosystem.

---

## 12.1 Înregistrarea unui client nou

```text
Client
   │
   ▼
ESTINVEST Onboarding
   │
   │  KYC / AML
   ▼
Aprobare
   │
   ▼
BackOffice Core
   │
   ▼
Creare client
Creare cont
Creare portofoliu
```

La finalul procesului, BackOffice Core devine proprietarul oficial al datelor operaționale ale clientului.

---

## 12.2 Introducerea unui ordin

```text
Client / Broker
       │
       ▼
   ESTtrade
       │
REST API
       │
       ▼
BackOffice Core
       │
Validări
       │
       ▼
Gateway Connector
       │
       ▼
Bursa de Valori București
```

Toate validările operaționale sunt efectuate în BackOffice Core înainte de transmiterea ordinului către piață.

---

## 12.3 Execuția ordinului

```text
Bursa de Valori București
           │
           ▼
Gateway Connector
           │
           ▼
BackOffice Core
           │
           ├────────► Unified Ledger
           │
           ├────────► Settlement
           │
           ├────────► Reporting
           │
           └────────► Notification
                         │
                         ▼
                     ESTtrade
```

Execuția ordinului actualizează simultan toate componentele operaționale relevante.

---

## 12.4 Settlement

```text
BackOffice Core
        │
        ▼
Depozitarul Central
        │
        ▼
Confirmare Settlement
        │
        ▼
Unified Ledger
        │
        ▼
Portofolii
```

Settlement-ul reprezintă confirmarea finală a transferului instrumentelor financiare și a numerarului.

---

## 12.5 Raportare

```text
BackOffice Core
        │
        ▼
Reporting Service
        │
        ├── Management
        ├── Autorități
        ├── Operațional
        └── Clienți
```

Reporting Service utilizează exclusiv date validate provenite din BackOffice Core.

---

# 13. Bazele de date

| Componentă | Bază de date | Rol |
|------------|--------------|-----|
| ESTINVEST Onboarding | MariaDB | Date KYC și onboarding |
| BackOffice Core | PostgreSQL | Date operaționale |
| ESTtrade | Fără bază operațională proprie* | Consumă servicii BackOffice |
| Reporting Service | PostgreSQL / Data Mart (viitor) | Raportare |
| AI Services | Vector Database (viitor) | Knowledge Base |

\* ESTtrade poate utiliza mecanisme locale de cache fără a deveni sursa oficială a datelor.

---

# 14. Protocoale de integrare

## Comunicare internă

- REST API
- HTTPS
- JSON
- JWT
- OAuth2 (unde este necesar)

---

## Comunicare cu Bursa de Valori București

- FIX Protocol
- Gateway BVB

---

## Comunicare cu sisteme externe

- REST API
- HTTPS
- SFTP (unde este solicitat)
- Formate oficiale impuse de instituțiile partenere

---

# 15. Principii privind integrarea

Întregul ecosistem respectă următoarele reguli:

- aplicațiile nu accesează direct baza de date a altor aplicații;
- integrarea se realizează exclusiv prin API-uri;
- fiecare serviciu este responsabil pentru propriile date;
- contractele API sunt versionate;
- modificările incompatibile se introduc numai prin versiuni noi ale API-urilor;
- toate comunicațiile sunt criptate.

---

# 16. Extensibilitatea ecosistemului

Arhitectura permite adăugarea de noi componente fără modificări majore ale aplicațiilor existente.

Exemple de extensii planificate:

- aplicație mobilă ESTtrade;
- CRM;
- integrare Open Banking;
- integrare cu brokeri internaționali;
- piețe externe;
- servicii AI suplimentare;
- Business Intelligence;
- Data Warehouse;
- Portal parteneri;
- Portal instituțional.

---

# 17. Relația cu celelalte documente

Acest document trebuie citit împreună cu:

| Document | Rol |
|----------|-----|
| ECO-001 – Ecosystem Context | Context general |
| ECO-003 – Data Ownership Matrix | Proprietatea datelor |
| ECO-004 – Integration Architecture | Arhitectura integrării |
| APP-xxx | Documentația aplicațiilor |
| ADR-xxx | Decizii de arhitectură |
| STD-xxx | Standarde de dezvoltare |

---

# 18. Concluzii

ESTINVEST Ecosystem este construit ca o platformă software modulară, în care fiecare aplicație are responsabilități bine delimitate și comunică prin interfețe standardizate.

Separarea clară a responsabilităților, utilizarea principiului **Single Source of Truth** și integrarea prin API-uri permit dezvoltarea independentă a componentelor, menținând în același timp consistența și integritatea datelor.

Acest document reprezintă referința principală pentru înțelegerea arhitecturii funcționale a ecosistemului și constituie baza pentru proiectarea tuturor aplicațiilor viitoare.

---

# Istoric versiuni

| Versiune | Data | Modificări |
|-----------|------|------------|
| 1.0 | Iulie 2026 | Prima versiune |
| 1.1 | Iulie 2026 | Revizuire completă, aliniere cu arhitectura Enterprise, introducerea catalogului aplicațiilor, clarificarea responsabilităților, completarea fluxurilor și standardizarea documentului |



