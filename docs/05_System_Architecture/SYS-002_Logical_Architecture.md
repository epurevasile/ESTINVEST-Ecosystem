 # SYS-002_Logical_Architecture

| Proprietate | Valoare |
|-------------|----------|
| Document | SYS-002_Logical_Architecture |
| Document ID | SYS-002 |
| Titlu | Logical Architecture |
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
| 1.1 | 2026-10-06 | Aliniere cu ADR-005 privind traseul BVB și excepția market data read-only; clarificare Market Data Service |

---

# 1. Scop

Acest document definește arhitectura logică a ecosistemului ESTINVEST.

Arhitectura logică descrie componentele principale ale ecosistemului, responsabilitățile acestora și relațiile dintre ele, independent de tehnologiile utilizate și de infrastructura fizică.

---

# 2. Principii

Arhitectura logică respectă următoarele principii:

- Separation of Concerns;
- Domain Driven Design;
- API First;
- Event Driven Architecture;
- Single Source of Truth;
- Security by Design;
- Audit by Design;
- Loose Coupling.

---

# 3. Structura logică

Ecosistemul este organizat în două categorii principale de componente:

```text
ESTINVEST Ecosystem

├── Business Applications
└── Enterprise Services
```

---

# 4. Business Applications

Business Applications implementează procesele de business și oferă funcționalități utilizatorilor.

## Aplicații actuale

- ESTINVEST Onboarding
- ESTtrade
- BackOffice Core
- Mobile App

## Aplicații viitoare

- CRM
- Partner Portal
- Open API Portal
- Advisor Portal

---

# 5. Enterprise Services

Enterprise Services oferă servicii comune reutilizabile de toate aplicațiile.

Aceste servicii nu implementează procese de business specifice unei aplicații.

## Servicii actuale

- IAM
- Integration Hub
- AI Assistant

## Servicii planificate

- Notification Service
- Document Service
- Reporting Service
- Market Data Service
- Scheduler Service
- Configuration Service

Market Data Service este un serviciu planificat. Acest document nu îi atribuie rolul de sursă pentru market data BVB și nu înlocuiește calea aprobată ESTINVEST Gateway → ESTtrade pentru market data read-only conform ADR-005.

---

# 6. Relația dintre Business Applications și Enterprise Services

```text
                 Business Applications

      Onboarding   ESTtrade   BackOffice   Mobile
             │         │          │          │
             └─────────┼──────────┘
                       │
                       ▼
                Enterprise Services

     IAM
     Integration Hub
     AI Assistant
     Notification Service
     Reporting Service
     Document Service
```

Business Applications utilizează Enterprise Services prin API-uri și Business Events.

Enterprise Services nu conțin logică specifică unei aplicații.

---

# 7. Domenii funcționale

Arhitectura este împărțită în următoarele domenii:

- Applications
- Integration
- Data
- Security
- AI
- Reporting
- Infrastructure

Fiecare domeniu este documentat separat.

---

# 8. Fluxul principal

```text
Client

↓

ESTtrade

↓

BackOffice

↓

Enterprise Services

↓

Integration Hub

↓

Sisteme Externe
```

Evenimentele și răspunsurile urmează traseul invers.

Pentru fluxurile business către BVB, traseul canonic este:

BackOffice
→ Integration Hub
→ ESTINVEST Gateway
→ BVB / Arena Gateway

Returul urmează traseul invers.

ESTtrade poate consuma direct din ESTINVEST Gateway numai market data read-only conform ADR-005. Această excepție nu autorizează trading direct și nu reprezintă un bypass general al BackOffice sau Integration Hub.

---

# 9. Guvernanță

Toate componentele trebuie să respecte:

- SYS-001_Ecosystem_Architecture;
- DATA-001_Data_Architecture_Principles;
- documentele SEC;
- documentele INT.

---

# 10. Principii de evoluție

Adăugarea unei aplicații noi nu trebuie să necesite modificarea serviciilor existente, ci doar integrarea cu acestea.

Adăugarea unui serviciu nou nu trebuie să afecteze aplicațiile existente, dacă interfețele publice rămân compatibile.

---

# 11. Beneficii

Această arhitectură oferă:

- reutilizarea serviciilor comune;
- separarea responsabilităților;
- reducerea duplicării;
- dezvoltare independentă;
- testare izolată;
- scalabilitate;
- mentenanță simplificată.

---

# 12. Relația cu celelalte documente

Acest document completează:

- SYS-001_Ecosystem_Architecture
- APP-xxx
- INT-xxx
- DATA-xxx
- SEC-xxx
- AI-xxx

și reprezintă modelul logic de referință pentru întregul ecosistem.

---

# 13. Concluzii

Arhitectura logică separă clar aplicațiile de business de serviciile enterprise, oferind o bază solidă pentru dezvoltarea și extinderea ecosistemului ESTINVEST.

Această separare permite reutilizarea serviciilor comune, evoluția independentă a aplicațiilor și integrarea facilă a unor noi componente fără modificări fundamentale ale arhitecturii.

---

# Version History

| Versiune | Data | Modificări |
|----------|------|------------|
| 1.0 | Iulie 2026 | Prima versiune |
| 1.1 | 2026-10-06 | Aliniere cu ADR-005 privind traseul BVB și excepția market data read-only; clarificare Market Data Service |
