 # SYS-001_Ecosystem_Architecture

| Proprietate | Valoare |
|-------------|----------|
| Document | SYS-001_Ecosystem_Architecture |
| Document ID | SYS-001 |
| Titlu | Ecosystem Architecture |
| Proiect | ESTINVEST Ecosystem |
| Versiune | 1.1 |
| Status | Approved |
| Domeniu | Enterprise Architecture |
| Data | Iulie 2026 |
| Ultima actualizare | 2026-10-06 |

---

# Change Log

| Versiune | Data | Modificări |
|----------|------|------------|
| 1.0 | Iulie 2026 | Prima versiune |
| 1.1 | 2026-10-06 | Aliniere cu ADR-005 privind boundary-ul Integration Hub, ESTINVEST Gateway și excepția market data read-only |

---

# 1. Scop

Acest document definește arhitectura oficială a ecosistemului software ESTINVEST.

El stabilește aplicațiile care compun ecosistemul, responsabilitățile fiecăreia, principiile de colaborare și regulile de integrare.

Toate aplicațiile existente și viitoare trebuie să respecte principiile definite în acest document.

---

# 2. Viziune

Ecosistemul ESTINVEST este construit ca o colecție de aplicații independente, specializate, integrate prin API-uri și Business Events.

Fiecare aplicație este responsabilă pentru propriul domeniu de business și pentru propriile date.

Nicio aplicație nu accesează direct baza de date a altei aplicații.

---

# 3. Obiective

Principalele obiective sunt:

- separarea responsabilităților;
- scalabilitate;
- disponibilitate ridicată;
- securitate;
- auditabilitate;
- integrare facilă;
- reutilizarea serviciilor;
- independența aplicațiilor.

---

# 4. Aplicațiile ecosistemului

Ecosistemul este alcătuit din următoarele aplicații principale:

## BackOffice Core

Responsabilități:

- administrare clienți;
- administrare conturi;
- administrare portofolii;
- Order Management;
- Trade Management;
- Settlement;
- Ledger;
- Reporting;
- Audit;
- AML și Compliance.

---

## ESTtrade

Familia aplicațiilor de tranzacționare.

Canale:

- Web
- Mobile
- Broker Console
- Trader Console

Responsabilități:

- introducerea ordinelor;
- operațiuni financiare;
- vizualizarea portofoliului;
- notificări.

---

## Onboarding

Responsabilități:

- deschiderea relației cu clientul;
- KYC;
- colectarea documentelor;
- validări AML inițiale;
- transmiterea clientului către BackOffice.

---

## Integration Hub

Responsabilități:

- integrarea business cu BVB prin ESTINVEST Gateway;
- integrarea cu Depozitarul Central;
- integrarea cu banca de decontare;
- integrarea cu custozi externi;
- integrarea cu piețe externe;
- transformarea mesajelor;
- monitorizarea comunicațiilor.

---

## AI Assistant

Responsabilități:

- asistență pentru utilizatori;
- suport operațional;
- căutare în documentație;
- analiză și explicații.

AI Assistant utilizează exclusiv API-uri și surse de date aprobate.

---

## Mobile App

Canal dedicat accesului de pe dispozitive mobile.

Utilizează aceleași servicii ca aplicația Web.

---

## Reporting Services

Responsabilități:

- rapoarte operaționale;
- rapoarte ASF;
- rapoarte MiFID II;
- rapoarte DORA;
- rapoarte interne.

---

## Identity & Access Management (IAM)

Responsabilități:

- autentificare;
- autorizare;
- Single Sign-On (SSO);
- administrarea rolurilor și permisiunilor;
- politici de securitate.

---

# 5. Arhitectura logică

```text
                    ESTINVEST Ecosystem

                          IAM
                           │
      ┌────────────────────┼────────────────────┐
      │                    │                    │
      ▼                    ▼                    ▼
 Onboarding          ESTtrade Family      AI Assistant
      │                    │
      │                    ▼
      │             BackOffice Core
      │                    │
      └──────────────┬─────┘
                     ▼
              Integration Hub
                     │
     ┌───────────────┼─────────────────────┐
     ▼               ▼                     ▼
 Banking API   ESTINVEST Gateway   Depozitarul Central
                     │
                     ▼
              BVB / Arena Gateway

ESTtrade ── market data read-only ──► ESTINVEST Gateway
```

---

# 6. Principii arhitecturale

## API First

Aplicațiile comunică prin API-uri bine definite.

---

## Event Driven

Evenimentele de business sunt mecanismul standard de comunicare asincronă.

---

## Domain Driven Design

Fiecare aplicație deține propriul domeniu funcțional.

---

## Single Source of Truth

Fiecare categorie de date are un proprietar unic.

---

## Loose Coupling

Aplicațiile sunt independente și pot evolua separat.

---

## Security by Design

Securitatea este integrată în arhitectură.

---

## Audit by Design

Toate procesele importante sunt auditate.

---

# 7. Proprietatea datelor

BackOffice este proprietarul datelor operaționale.

Onboarding este proprietarul procesului de deschidere a relației cu clientul.

Integration Hub este proprietarul comunicațiilor externe.

IAM este proprietarul identităților și al drepturilor de acces.

---

# 8. Integrarea

Comunicațiile externe de business ale BackOffice utilizează Integration Hub.

Nu este permisă conectarea directă a unei aplicații la sistemele externe pentru business:

- BVB;
- Depozitarul Central;
- banca de decontare;
- custozi externi.

Pentru BVB, traseul business este:

BackOffice
→ Integration Hub
→ ESTINVEST Gateway
→ BVB / Arena Gateway

Excepție aprobată (ADR-005): ESTtrade poate consuma direct din ESTINVEST Gateway numai market data read-only. Această excepție nu autorizează trading direct și nu reprezintă un bypass general al BackOffice sau Integration Hub.

---

# 9. Arhitectura datelor

Datele sunt împărțite în:

- Master Data;
- Reference Data;
- Transactional Data.

Definițiile oficiale sunt documentate în:

**DATA-001_Data_Architecture_Principles.md**

---

# 10. Securitate

Toate aplicațiile utilizează serviciile IAM pentru:

- autentificare;
- autorizare;
- administrarea sesiunilor.

Politicile de securitate sunt definite în documentele din categoria **SEC**.

---

# 11. Observabilitate

Toate aplicațiile trebuie să furnizeze:

- loguri;
- metrici;
- Business Events;
- Audit Events;
- Health Checks.

Aceste informații sunt utilizate pentru monitorizare și suport operațional.

---

# 12. Scalabilitate

Arhitectura permite adăugarea de noi aplicații fără modificarea principiilor fundamentale.

Exemple:

- CRM;
- Portal Parteneri;
- Open API Gateway;
- AI Trading Assistant;
- noi canale digitale.

---

# 13. Relația cu celelalte documente

Acest document este documentul părinte pentru:

- APP – Applications;
- INT – Integration;
- DATA – Data Architecture;
- SEC – Security;
- AI – AI Context.

Toate aceste documente trebuie să respecte principiile definite aici.

---

# 14. Concluzii

SYS-001 definește arhitectura oficială a ecosistemului ESTINVEST.

El reprezintă fundamentul pe baza căruia sunt proiectate și dezvoltate toate aplicațiile din ecosistem, asigurând o separare clară a responsabilităților, o integrare coerentă și o evoluție controlată a platformei.

---

# Version History

| Versiune | Data | Modificări |
|----------|------|------------|
| 1.0 | Iulie 2026 | Prima versiune |
| 1.1 | 2026-10-06 | Aliniere cu ADR-005 privind boundary-ul Integration Hub, ESTINVEST Gateway și excepția market data read-only |
