# INT-001_Integration_Hub

| Proprietate | Valoare |
|-------------|----------|
| Document | INT-001_Integration_Hub |
| Proiect | ESTINVEST Ecosystem |
| Versiune | 1.1 |
| Status | Approved |
| Domeniu | Enterprise Integration |
| Data | Iulie 2026 |
| Ultima actualizare | 2026-10-06 |

---

# Change Log

| Versiune | Data | Modificări |
|----------|------|------------|
| 1.0 | Iulie 2026 | Prima versiune |
| 1.1 | 2026-10-06 | Aliniere cu ADR-005 și ADR-006 privind boundary-ul Integration Hub și traseul prin ESTINVEST Gateway |

---

# 1. Scop

Acest document definește arhitectura și responsabilitățile Integration Hub din ecosistemul ESTINVEST.

Integration Hub reprezintă boundary-ul BackOffice față de sistemele externe.

---

# 2. Viziune

Integration Hub acționează ca un strat de integrare, transformare, securizare și monitorizare a comunicațiilor externe ale BackOffice.

Comunicațiile externe de business ale BackOffice sunt realizate prin Integration Hub.

ESTtrade poate consuma direct din ESTINVEST Gateway numai market data read-only, conform ADR-005. Această excepție nu autorizează trading direct și nu schimbă Integration Hub ca business boundary pentru BackOffice.

---

# 3. Obiective

Integration Hub urmărește:

- decuplarea aplicațiilor interne de sistemele externe;
- reutilizarea conectorilor;
- monitorizarea comunicațiilor;
- audit complet;
- suport pentru multiple protocoale;
- gestionarea erorilor și reluarea mesajelor.

---

# 4. Principii arhitecturale

## Single Integration Boundary

Integration Hub este punctul oficial de integrare externă pentru BackOffice.

Nu este ESTINVEST Gateway și nu este BVB / Arena Gateway.

---

## Loose Coupling

Aplicațiile nu cunosc detaliile tehnice ale sistemelor externe.

---

## API First

Aplicațiile interne utilizează API-uri standardizate.

---

## Event Driven

Evenimentele sunt distribuite prin Business Events.

---

## Secure by Design

Toate comunicațiile sunt autentificate și criptate.

---

# 5. Sisteme externe

Integration Hub gestionează comunicația cu:

- BVB / Arena Gateway, prin ESTINVEST Gateway;
- Depozitarul Central;
- Banca de decontare;
- Custozi externi;
- Piețe externe;
- Furnizori de cursuri valutare;
- Furnizori de date de piață;
- Servicii de notificare (e-mail, SMS, push).

Lista este extensibilă.

Pentru conectivitatea business către BVB:

BackOffice → Integration Hub → ESTINVEST Gateway → BVB / Arena Gateway

Retur:

BVB / Arena Gateway → ESTINVEST Gateway → Integration Hub → BackOffice

BackOffice nu comunică direct cu ESTINVEST Gateway.

ESTINVEST Gateway este serviciul specializat de market connectivity aflat în aval de Integration Hub. Integration Hub nu implementează protocolul Arena ca model BackOffice.

ESTINVEST Gateway izolează Arena protocol, Arena XML și Arena DTOs față de BackOffice.

Integration Hub comunică cu ESTINVEST Gateway printr-un contract intern ESTINVEST: request/response pentru request-urile inițiate prin Hub; evenimente/notificări asincrone Gateway → Hub pentru confirmations, trades și rejects relevante. Arena DTO/XML nu sunt modele BackOffice.

Distincție de denumire:

- Integration Hub = integrare generică business / external boundary;
- ESTINVEST Gateway = serviciu propriu de market connectivity;
- BVB / Arena Gateway = infrastructura externă BVB.

---

# 6. Aplicații interne

Integration Hub deservește:

- BackOffice Core;
- ESTtrade;
- Onboarding;
- AI Assistant;
- Reporting Services;
- aplicațiile viitoare.

---

# 7. Model logic

```text
BackOffice
    │
    ▼
Integration Hub
    │
    ▼
ESTINVEST Gateway
    │
    ▼
BVB / Arena Gateway

ESTtrade ── market data read-only ──► ESTINVEST Gateway
```

---

# 8. Componente

## API Gateway

Expune serviciile Integration Hub către aplicațiile interne. Nu este ESTINVEST Gateway și nu este BVB / Arena Gateway.

---

## Connector Manager

Gestionează conectorii pentru sistemele externe.

Pentru BVB, Integration Hub nu este client Arena; conectivitatea de piață este ESTINVEST Gateway, în aval de Hub.

---

## Message Transformer

Transformă mesajele între formatele interne și cele externe, în limitele contractelor Hub.

Pentru BVB, Arena protocol / XML / DTOs nu sunt modele BackOffice; traducerea Arena este responsabilitatea ESTINVEST Gateway.

---

## Event Dispatcher

Distribuie Business Events.

---

## Monitoring

Monitorizează starea comunicațiilor.

---

## Retry Manager

Reia automat comunicațiile eșuate conform politicilor definite.

---

# 9. Protocoale suportate

- REST
- FIX
- SFTP
- HTTPS
- WebSocket (unde este necesar)

Modelul este extensibil.

---

# 10. Gestionarea erorilor

Erorile sunt clasificate în:

- tehnice;
- funcționale;
- de comunicație;
- de autentificare;
- de validare.

Toate incidentele sunt înregistrate și pot fi reluate conform politicilor operaționale.

---

# 11. Audit

Se înregistrează:

- cererea primită;
- răspunsul primit;
- timpul de execuție;
- sistemul sursă;
- sistemul destinație;
- Correlation ID;
- rezultatul.

---

# 12. Securitate

Toate comunicațiile externe trebuie să utilizeze:

- TLS;
- autentificare;
- autorizare;
- certificate digitale, acolo unde sunt cerute.

Credențialele sunt administrate centralizat și nu sunt stocate în aplicațiile consumatoare.

---

# 13. Relația cu celelalte documente

Acest document completează:

- SYS-001_Ecosystem_Architecture;
- DATA-001_Data_Architecture_Principles;
- APP-002_ESTtrade_Family.

Documentele dedicate fiecărui conector (INT-002, INT-003 etc.) descriu implementarea specifică pentru fiecare sistem extern.

---

# 14. Concluzii

Integration Hub este componenta centrală de integrare a ecosistemului ESTINVEST.

Prin centralizarea comunicațiilor externe, acesta oferă:

- reutilizare;
- securitate;
- monitorizare;
- audit;
- scalabilitate;
- independență față de sistemele externe.

Toate aplicațiile ecosistemului trebuie să utilizeze Integration Hub pentru integrările externe de business, cu excepția market data read-only ESTtrade → ESTINVEST Gateway, conform ADR-005.

---

# Version History

| Versiune | Data | Modificări |
|----------|------|------------|
| 1.0 | Iulie 2026 | Prima versiune |
| 1.1 | 2026-10-06 | Aliniere cu ADR-005 și ADR-006 privind boundary-ul Integration Hub și traseul prin ESTINVEST Gateway |
