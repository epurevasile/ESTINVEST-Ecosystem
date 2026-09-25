 
# DATA-001_Data_Architecture_Principles

| Proprietate | Valoare |
|-------------|----------|
| Document | DATA-001_Data_Architecture_Principles |
| Proiect | ESTINVEST Ecosystem |
| Versiune | 1.0 |
| Status | Approved |
| Domeniu | Enterprise Data Architecture |
| Data | Iulie 2026 |

---

# 1. Scop

Acest document definește arhitectura oficială a datelor utilizată în toate aplicațiile ecosistemului ESTINVEST.

Obiectivul este asigurarea unei guvernanțe unitare a datelor, eliminarea duplicării și stabilirea proprietății fiecărei categorii de date.

Toate aplicațiile trebuie să respecte principiile descrise în acest document.

---

# 2. Obiective

- Single Source of Truth;
- eliminarea duplicării datelor;
- separarea responsabilităților;
- integrare simplificată;
- auditabilitate;
- scalabilitate;
- reutilizarea modelelor de date.

---

# 3. Clasificarea datelor

Arhitectura datelor este împărțită în trei categorii.

```text
Enterprise Data

├── Master Data
├── Reference Data
└── Transactional Data
```

---

# 4. Master Data

Master Data descrie entitățile fundamentale ale domeniului de business.

Exemple:

- Customer
- CustomerBankAccount
- Account
- Portfolio
- Financial Instrument
- Organization
- User
- Role

Master Data este administrat centralizat și reutilizat de toate aplicațiile.

---

# 5. Reference Data

Reference Data conține informații reutilizabile și clasificări comune.

Exemple:

- Currency
- Country
- Language
- Market
- Exchange
- Bank
- Settlement Calendar
- Order Type
- Transaction Type
- Portfolio Type
- Corporate Action Type
- Status
- Error Codes
- Permission

Reference Data este administrat centralizat și versionat.

---

# 6. Transactional Data

Transactional Data reprezintă activitatea operațională a sistemului.

Exemple:

- Order
- Trade
- Settlement
- Cash Ledger
- Securities Ledger
- Corporate Action
- Audit Event
- Business Event

Datele tranzacționale sunt imuabile acolo unde natura procesului o impune (Ledger, Audit).

---

# 7. Proprietatea datelor

Fiecare categorie de date are un proprietar unic.

| Domeniu | Proprietar |
|----------|------------|
| Customer | BackOffice |
| Account | BackOffice |
| Portfolio | BackOffice |
| Orders | BackOffice |
| Trades | BackOffice |
| Settlement | BackOffice |
| Reference Data | Enterprise |
| Identity | IAM |
| Onboarding Data | Onboarding |
| Knowledge Base | AI Assistant |

---

# 8. Principii

## Single Source of Truth

Fiecare informație este administrată într-un singur loc.

---

## Data Ownership

Fiecare set de date are un proprietar clar.

---

## No Data Duplication

Aplicațiile nu își creează copii permanente ale datelor altor aplicații.

---

## API First

Accesul la date se face prin API-uri oficiale.

---

## Event Driven

Actualizările importante generează Business Events.

---

## Auditability

Toate modificările importante sunt auditate.

---

## Versioning

Reference Data și Master Data suportă versionare acolo unde este necesar.

---

# 9. Fluxul datelor

```text
Master Data
        │
        ▼
Business Processes
        │
        ▼
Transactional Data
        │
        ▼
Reporting
        │
        ▼
AI Assistant
```

---

# 10. Relația dintre aplicații

```text
Onboarding
      │
      ▼
BackOffice
      │
      ▼
ESTtrade
      │
      ▼
Reporting

             ▲
             │
      AI Assistant
```

Toate aplicațiile utilizează aceleași definiții pentru Master Data și Reference Data.

---

# 11. Guvernanța datelor

Guvernanța datelor include:

- administrarea modelelor;
- aprobarea modificărilor;
- versionare;
- audit;
- controlul calității datelor.

Modificările structurale trebuie aprobate prin Architecture Decision Records (ADR).

---

# 12. Relația cu celelalte documente

Acest document este implementat prin:

- BackOffice → DM-016_Reference_Data_Model;
- ESTtrade → View Models;
- Onboarding → Customer Master Data;
- IAM → Identity Data;
- AI Assistant → Knowledge Model.

---

# 13. Beneficii

Aplicarea acestor principii oferă:

- consistență între aplicații;
- integrare simplificată;
- reducerea redundanței;
- auditabilitate;
- întreținere mai ușoară;
- scalabilitate.

---

# 14. Concluzii

DATA-001 reprezintă documentul de referință pentru arhitectura datelor în ecosistemul ESTINVEST.

Toate aplicațiile existente și viitoare trebuie să respecte clasificarea datelor, regulile de proprietate și principiile de guvernanță definite în acest document.