# INT-003_BVB_Gateway

| Proprietate | Valoare |
|-------------|----------|
| Document | INT-003_BVB_Gateway |
| Document ID | INT-003 |
| Titlu | BVB Gateway |
| Proiect | ESTINVEST Ecosystem |
| Versiune | 1.0 |
| Status | Approved |
| Data | Iulie 2026 |

---

# 1. Scop

Acest document descrie integrarea specifică BVB / Arena în ESTINVEST Ecosystem.

INT-003 completează INT-001 (Integration Hub) și reflectă deciziile aprobate din ADR-005…ADR-010. Nu înlocuiește aceste ADR-uri, nu este documentația internă de implementare a repository-ului ESTINVEST-Gateway și nu introduce decizii noi.

---

# 2. Non-scope

Următoarele rămân OPEN sau OUT OF SCOPE. Nu sunt autorizări de implementare și nu primesc valori în acest document:

- exact report command set;
- retry intervals / counts / timeouts / backoff / DLQ;
- Technical Store schema / enums;
- retention / purge / quotas;
- physical PostgreSQL placement;
- production deployment;
- HA/DR;
- endpoint-uri, status codes și payload schema interne;
- API / WebSocket / SSE pentru market data;
- proveniența oficială BVB a XSD-urilor locale;
- simulator path / toolchain în ESTINVEST-Gateway;
- FIX / alte piețe;
- redenumirea INT-003.

---

# 3. Relația cu documentele

- INT-001 Integration Hub — boundary generic al BackOffice față de sistemele externe.
- ADR-005 — boundary ESTINVEST Gateway, topologie canonică și excepția market data read-only.
- ADR-006 — contractul intern Hub ↔ ESTINVEST Gateway, transport v1 și semantica de livrare.
- ADR-007 — Technical Store Gateway și pending delivery durabil.
- ADR-008 — baseline Arena Gateway Protocol 3.1.4 și politica de validare XSD.
- ADR-009 — reuse din ESTtrade și rolul Arena Simulator.
- ADR-010 — scope funcțional v1 al ESTINVEST Gateway.

---

# 4. Distincție de denumire

Integration Hub = boundary generic al BackOffice pentru integrări externe business.

ESTINVEST Gateway = serviciu / aplicație ESTINVEST specializată în market connectivity.

BVB / Arena Gateway = infrastructura externă BVB.

Numele istoric al fișierului INT-003_BVB_Gateway nu schimbă aceste trei niveluri. Fișierul și Document ID nu se redenumesc prin această aprobare.

---

# 5. Topologie și trasee

Conform ADR-005, traseul business outbound este:

BackOffice
→ Integration Hub
→ ESTINVEST Gateway
→ BVB / Arena Gateway

Traseul business inbound este:

BVB / Arena Gateway
→ ESTINVEST Gateway
→ Integration Hub
→ BackOffice

BackOffice nu comunică direct cu ESTINVEST Gateway.

ESTtrade poate consuma direct din ESTINVEST Gateway numai market data read-only.

Această excepție nu permite:

- order submission;
- cancel / change;
- Trade creation;
- mutații Portfolio / Buying Power;
- scrieri Settlement / Ledger.

ESTINVEST Gateway nu este System of Record pentru entitățile de business. Ownership-ul pentru Customer, Account, Portfolio, Order, Trade, Position, Settlement și Ledgers rămâne în domeniile competente, în special BackOffice pentru Order / Trade.

---

# 6. Contract Hub ↔ ESTINVEST Gateway

Conform ADR-006:

- Integration Hub comunică cu ESTINVEST Gateway printr-un contract intern ESTINVEST, independent de Arena wire protocol;
- request / response se folosește pentru request-urile inițiate prin Hub;
- evenimentele / notificările relevante pe direcția Gateway → Hub sunt asincrone;
- transport v1 = HTTP/REST;
- nu se introduce un broker dedicat exclusiv acestei integrări în v1;
- evenimentele relevante Gateway → Hub au semantică at-least-once;
- duplicatele tehnice de delivery sunt permise;
- exactly-once nu este afirmat;
- idempotency business rămâne domain-owned;
- Arena DTO / XML nu sunt modele BackOffice.

Endpoint-uri, status codes, payload schemas și valori numerice de retry nu sunt definite aici.

---

# 7. Baseline protocol Arena

Conform ADR-008:

- baseline v1 = Arena Gateway Protocol 3.1.4;
- PDF-ul 3.1.4 este referința semantică;
- perechea XSD locală este baseline-ul structural;
- validarea XSD este obligatorie atât outgoing cât și incoming;
- XML invalid structural nu se transmite ca Arena command și nu produce business effect;
- discrepanțele PDF ↔ XSD se urmăresc explicit, fără rezolvare tacit;
- upgrade-ul de protocol / schemă necesită identificare, impact analysis, compatibility review, regression testing și aprobare explicită înainte de schimbarea baseline-ului.

Acest document nu afirmă proveniența oficială BVB a XSD-urilor locale.

---

# 8. Persistență tehnică

Conform ADR-007:

- ESTINVEST Gateway deține un Technical Store propriu;
- tehnologia v1 este PostgreSQL;
- store-ul este separat logic de baza operațională BackOffice;
- pending delivery durabil trebuie să supraviețuiască restartului Gateway;
- poate păstra stare tehnică de delivery, correlation, dedup, session / protocol și recovery;
- nu devine System of Record de business.

Schema exactă, enum-urile, retention și placement-ul fizic PostgreSQL nu sunt definite aici.

---

# 9. Reuse și simulator

Conform ADR-009:

- codul Gateway existent în ESTtrade este reference / reuse source;
- nu este implementation baseline și nu este production baseline;
- reuse-ul se face component-by-component, numai după verificare, și numai dacă respectă ADR-005…ADR-008;
- assumption-urile protocol / XML din implementarea existentă nu se preiau implicit;
- Arena Simulator aparține ecosistemului de development / test al ESTINVEST Gateway;
- simulatorul este un test double;
- succesul local cu simulatorul nu demonstrează conformitate BVB;
- baseline-ul de protocol rămâne ADR-008.

Acest document nu autorizează mutarea sau copierea de fișiere.

---

# 10. Scope funcțional v1

Conform ADR-010, v1 acoperă core production path:

- connectivity / session;
- market data Level 1;
- market data Level 2;
- standard order lifecycle;
- confirmations;
- rejects;
- trades;
- reports / recovery ca capability.

Comenzile de ordin aprobate în v1 sunt exact:

- AddOrderBuyCmd
- AddOrderSellCmd
- CancelOrderCmd
- ChgOrderCmd
- SuspendOrderCmd
- ReleaseOrderBuyCmd
- ReleaseOrderSellCmd

Exact report command set rămâne OPEN. Recovery / reconciliation este cerință de capability în v1, fără set de comenzi de report aprobat aici.

Nu fac parte din v1:

- Cross Orders;
- Market Maker;
- Deals.

Excluderea din v1 nu blochează o extindere ulterioară prin decizie separată.

---

# 11. Observații

INT-003 este un living integration document în Status Approved.

Deciziile de arhitectură rămân în ADR-005…ADR-010. Detaliile de implementare aparțin repository-ului ESTINVEST-Gateway.

Elementele marcate OPEN sau OUT OF SCOPE nu sunt autorizări de implementare.

Schimbarea baseline-ului (boundary, contract, protocol, persistence, reuse, scope v1) necesită governance-ul aplicabil din ADR-urile respective, nu o modificare unilaterală a INT-003.
