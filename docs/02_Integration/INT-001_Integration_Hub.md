# INT-001_Integration_Hub

| Proprietate | Valoare |
|-------------|----------|
| Document | INT-001_Integration_Hub |
| Proiect | ESTINVEST Ecosystem |
| Versiune | 1.0 |
| Status | Approved |
| Domeniu | Enterprise Integration |
| Data | Iulie 2026 |

---

# 1. Scop

Acest document definește arhitectura și responsabilitățile Integration Hub din ecosistemul ESTINVEST.

Integration Hub reprezintă componenta unică prin care aplicațiile interne comunică cu sistemele externe.

---

# 2. Viziune

Nicio aplicație din ecosistem nu comunică direct cu sisteme externe.

Toate comunicațiile externe sunt realizate exclusiv prin Integration Hub.

Acesta acționează ca un strat de integrare, transformare, securizare și monitorizare a comunicațiilor.

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

## Single Gateway

Există un singur punct oficial de integrare.

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

- BVB Gateway;
- Depozitarul Central;
- Banca de decontare;
- Custozi externi;
- Piețe externe;
- Furnizori de cursuri valutare;
- Furnizori de date de piață;
- Servicii de notificare (e-mail, SMS, push).

Lista este extensibilă.

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
                  Integration Hub

      ┌───────────────┼────────────────┐
      │               │                │
      ▼               ▼                ▼
 BackOffice      ESTtrade        Onboarding
      │
      ▼
Business Events
      │
      ▼
Integration Services
      │
      ▼
Message Transformation
      │
      ▼
External Connectors
      │
      ▼
External Systems
```

---

# 8. Componente

## API Gateway

Expune serviciile Integration Hub către aplicațiile interne.

---

## Connector Manager

Gestionează conectorii pentru fiecare sistem extern.

---

## Message Transformer

Transformă mesajele între formatele interne și cele externe.

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

Toate aplicațiile ecosistemului trebuie să utilizeze Integration Hub pentru orice integrare externă.
