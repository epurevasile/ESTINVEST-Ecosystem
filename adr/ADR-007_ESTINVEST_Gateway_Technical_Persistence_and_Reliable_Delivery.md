# ADR-007 – ESTINVEST Gateway Technical Persistence and Reliable Delivery



| Proprietate | Valoare                 |

| ----------- | ----------------------- |

| ADR ID      | ADR-007                 |

| Titlu       | ESTINVEST Gateway Technical Persistence and Reliable Delivery |

| Status      | Approved                |

| Data        | 2026-10-06              |

| Owner       | Enterprise Architecture |

| Domeniu     | ESTINVEST Ecosystem     |

| Autor       | ESTINVEST \& ChatGPT     |



---



# Change Log



| Versiune | Data       | Modificări     |

| -------- | ---------- | -------------- |

| 1.0      | 2026-10-06 | Prima versiune |



---



# 1. Context



ADR-005 (Approved) stabilește că ESTINVEST Gateway este o componentă distinctă de Integration Hub, nu este System of Record pentru entități business și că ownership-ul business rămâne în BackOffice.



ADR-006 (Approved) stabilește că evenimentele/notificările relevante Gateway → Hub se livrează cu semantică **at-least-once**, că duplicate tehnice sunt permise, că exactly-once nu este afirmat și că idempotency business rămâne domain-owned. Detaliile numerice de retry sunt out of scope în ADR-006.



Pentru a susține at-least-once și supraviețuirea restartului, ESTINVEST Gateway necesită o decizie privind **persistența tehnică** și **starea durabilă de delivery**, fără a transfera ownership business către Gateway.



ADR-007 definește Technical Persistence and Reliable Delivery. **Nu modifică ADR-005 sau ADR-006.**



---



# 2. Problem Statement



Trebuie stabilit formal:



1. dacă delivery state Gateway → Hub trebuie să fie durabil la restart;

2. unde se stochează această stare tehnică (Gateway-owned vs BackOffice operational DB vs broker);

3. ce tehnologie de bază de date este adoptată pentru Technical Store v1;

4. ce tipuri de date tehnice sunt permise și ce ownership business este interzis;

5. cum se delimitează technical dedup de business idempotency;

6. ce principii de acces/securitate se aplică, fără a inventa implementarea concretă.



Fără aceste clarificări, implementarea riscă pierderea pending deliveries la restart, amestecul stării tehnice cu BackOffice operational DB sau confuzia Technical Store cu System of Record.



---



# 3. Decision Drivers



* respectarea ADR-005 (Gateway ≠ business SoR; ownership BackOffice);

* respectarea ADR-006 (at-least-once; duplicate tehnice permise; exactly-once neafirmat; idempotency business domain-owned; fără broker dedicat Hub ↔ Gateway în v1);

* restart resilience pentru pending deliveries;

* decuplare față de BackOffice operational database;

* aliniere la stack-ul aprobat (PostgreSQL pentru aplicații noi — ECO-006, STD-DB-001);

* least privilege și secrets management (STD-SEC-001);

* evitarea inventării premature de schema, retry numeric sau retention days.



---



# 4. Considered Options



## Opțiunea A



### Descriere



Fără persistență durabilă — delivery state doar in-memory.



### Avantaje



* implementare simplă;

* fără bază de date dedicată pentru delivery state.



### Dezavantaje



* pierde pending delivery state la restart;

* incompatibil cu cerința aprobată de restart survival și insuficient pentru susținerea delivery-ului at-least-once peste restart.



---



## Opțiunea B



### Descriere



Persistență tehnică proprie Gateway în PostgreSQL, separată logic de BackOffice operational DB.



### Avantaje



* restart resilience pentru durable pending delivery;

* susține at-least-once fără a afirma exactly-once;

* decuplează starea tehnică de integrare de business operational DB;

* aliniată la PostgreSQL ca standard pentru aplicații noi.



### Dezavantaje



* complexitate suplimentară de persistență;

* cleanup/retention trebuie definite ulterior;

* risc de creștere a stocării dacă lifecycle-ul tehnic nu este controlat.



---



## Opțiunea C



### Descriere



Folosirea BackOffice operational DB pentru pending delivery / retry state al Gateway.



### Avantaje



* reutilizare a unei baze existente;

* mai puține instanțe de administrat (aparent).



### Dezavantaje



* amestecă technical integration state cu business operational DB;

* crește coupling-ul Gateway–BackOffice;

* contrazice ownership boundary (ADR-005).



---



## Opțiunea D



### Descriere



Introducerea unui message broker ca persistence/delivery mechanism principal doar pentru a evita Technical Store.



### Avantaje



* retry/persistence nativă la nivel de broker;

* async nativ.



### Dezavantaje



* contrazice ADR-006 v1 (fără broker dedicat pentru Hub ↔ Gateway);

* introduce infrastructură nouă nejustificată.



---



# 5. Decision



Opțiunea aprobată:



**Opțiunea B — Persistență tehnică proprie Gateway în PostgreSQL, separată logic de BackOffice operational DB.**



Motivația alegerii și deciziile consemnate:



### Relația cu ADR-005 / ADR-006



* ADR-007 respectă ADR-005 și ADR-006;

* ADR-007 nu modifică ADR-005 sau ADR-006;

* Gateway rămâne distinct de Integration Hub și nu este System of Record business;

* delivery Gateway → Hub rămâne at-least-once; exactly-once nu este afirmat; idempotency business rămâne domain-owned.



### Durable delivery state



* evenimentele/notificările relevante Gateway → Hub care nu au primit încă acknowledgement tehnic trebuie să **supraviețuiască restartului** ESTINVEST Gateway;

* Gateway trebuie să aibă **durable delivery state**;

* aceasta este **stare tehnică de integrare**, nu business state.



### Gateway-owned technical persistence



* ESTINVEST Gateway deține propria persistență tehnică necesară pentru: reliable delivery; lifecycle tehnic de integrare; correlation; recovery tehnic; protocol/session state atunci când este necesar;

* această persistență este **separată logic** de BackOffice operational database;

* Gateway **nu** scrie direct în BackOffice operational DB pentru a-și gestiona technical delivery state.



### Database technology



* Technical Store v1 utilizează **PostgreSQL**;

* Gateway are propriul acces controlat la această bază;

* physical PostgreSQL placement (aceeași instanță / server separat / cluster separat) = **OUT OF SCOPE**.



### Allowed technical data



Technical Store poate conține numai stare tehnică necesară integrării, inclusiv:



* pending deliveries;

* delivery status;

* retry state;

* technical acknowledgement state;

* external identifiers;

* internal/external correlation identifiers;

* technical dedup metadata;

* Arena/session/protocol state, când este necesar;

* recovery checkpoints;

* technical error/recovery metadata;

* payload tehnic necesar retransmisiei sau recovery-ului.



### Business payload limitation



* business-related payload poate fi păstrat numai dacă este necesar tehnic pentru retransmission, correlation, recovery sau diagnostics controlate;

* persistența unui astfel de payload **nu** transferă business ownership către Gateway.



### Forbidden business ownership



Gateway Technical Store **nu** devine System of Record și **nu** deține lifecycle business pentru:



* Customer; Account; Portfolio; Order; Trade; Position; Settlement; Cash Ledger; Securities Ledger.



Gateway **nu** decide starea business finală a unui Order/Trade pe baza propriului Technical Store.



### Reliable delivery model (conceptual)



1. Gateway primește / produce un event relevant.

2. Dacă trebuie livrat către Hub, creează/înregistrează durable pending delivery înainte ca mesajul să poată fi considerat sigur pentru retransmisie.

3. Livrează prin transportul definit de ADR-006 (HTTP/REST intern; async semantic Gateway → Hub).

4. Până la technical acknowledgement, poate retransmite.

5. După acknowledgement tehnic, delivery state poate fi marcat corespunzător.

6. Business result rămâne separat de technical acknowledgement.

7. Restart-ul Gateway nu trebuie să piardă pending delivery state.



Schema exactă a stărilor / enum-urilor = OUT OF SCOPE.



### Idempotency / dedup boundary



* Technical Store poate păstra metadata pentru technical deduplication;

* aceasta **nu** înlocuiește business idempotency;

* domain-owned business dedup rămâne în BackOffice / domain owner conform ADR-006;

* Gateway poate evita retransmisii inutile sau duplicate tehnice evidente, dar **nu** decide business equivalence pentru Order/Trade.



### Retention / cleanup (principiu)



* technical state trebuie să aibă lifecycle și cleanup controlat;

* retention days, purge intervals, archive periods, storage quotas = **OUT OF SCOPE**.



---



# 6. Consequences



## Beneficii



* restart resilience pentru pending deliveries;

* reliable at-least-once delivery susținut de stare durabilă;

* decuplare față de BackOffice operational DB;

* trasabilitate tehnică (correlation, delivery status, recovery metadata).



## Limitări



* complexitate suplimentară de database/persistence;

* cleanup/retention de definit ulterior;

* eventual storage growth dacă lifecycle-ul tehnic nu este controlat;

* physical placement PostgreSQL nerezolvat aici.



## Riscuri



* Technical Store confundat cu business System of Record;

* payload retention excesiv;

* divergence între technical delivery state și business state dacă boundary-ul este implementat greșit.



---



# 7. Impact Analysis



| Domeniu      | Impact |

| ------------ | ------ |

| Business     | Fără transfer de ownership Order/Trade/etc. către Gateway; business result separat de technical ack. |

| Architecture | Gateway-owned PostgreSQL Technical Store, separat logic de BackOffice DB; ADR-005/006 neschimbate. |

| Security     | Credentials dedicate, least privilege, secrets în afara repository, logging fără payload sensibil/credentials. |

| Performance  | Persistență înainte de retransmisie sigură; impactul cantitativ nu este evaluat aici. |

| Operations   | Administrare Technical Store; cleanup controlat de definit; placement fizic out of scope. |

| Development  | Implementare durable pending delivery în ESTINVEST-Gateway; fără scriere delivery state în BackOffice DB. |



---



# 8. Alternatives Rejected



| Alternativă | Motivul respingerii |

| ----------- | ------------------- |

| Opțiunea A — delivery state in-memory | Pierde pending delivery state la restart; incompatibil cu cerința aprobată de restart survival; insuficient pentru susținerea delivery-ului at-least-once peste restart (fără a afirma că at-least-once, luat izolat, cere obligatoriu persistence). |

| Opțiunea C — pending delivery în BackOffice operational DB | Amestecă technical integration state cu business DB; crește coupling; contrazice ownership boundary. |

| Opțiunea D — message broker ca persistence principal | Contrazice ADR-006 v1 fără broker dedicat; introduce infrastructură nouă nejustificată. |



---



# 9. Dependencies



Documente și componente afectate:



* ADR-005 – ESTINVEST Gateway System Boundary and Role (Approved; neschimbat)

* ADR-006 – Integration Hub ↔ ESTINVEST Gateway Communication Architecture (Approved; neschimbat)

* ECO-003 – Data Ownership Matrix

* ECO-006 – Technology Stack (PostgreSQL)

* STD-DB-001 – Database Design Standard

* STD-SEC-001 – Security Standard

* ADR-008…ADR-010 (viitoare) – Arena/XSD, simulator, v1 functional scope / detalii ulterioare



---



# 10. Implementation Notes



Acțiunile necesare pentru implementarea deciziei:



* Pasul 1 — Aprobarea ADR-007 (trecerea din Proposed în statusul de aprobare prevăzut de procesul ADR).

* Pasul 2 — Definirea ulterioară a schemei Technical Store, status enum, retry numeric și retention (out of scope aici).

* Pasul 3 — Provisioning acces PostgreSQL dedicat Gateway (credentials dedicate, least privilege), fără a decide physical placement în acest ADR.

* Pasul 4 — Implementarea durable pending delivery conform modelului conceptual și a transportului ADR-006, fără scriere în BackOffice operational DB pentru delivery state.



### Security / access principles



Fără a inventa implementarea concretă:



* Gateway trebuie să folosească DB credentials dedicate;

* least privilege;

* secrets nu se păstrează în repository;

* conexiunea DB trebuie protejată conform standardelor aplicabile (STD-SEC-001);

* logging-ul nu trebuie să expună payload sensibil sau credentials;

* se utilizează mecanismele prevăzute de standard (inclusiv `.env` în dezvoltare); nu se inventează un secret manager specific dacă standardul nu îl impune explicit.



---



# 11. Validation



Cum se verifică implementarea deciziei:



* validare arhitecturală: Technical Store = PostgreSQL; separat logic de BackOffice operational DB;

* validare arhitecturală: restart Gateway nu pierde pending delivery state;

* review: Gateway ≠ business SoR; fără lifecycle business Order/Trade în Technical Store;

* review: technical dedup ≠ business idempotency; domain-owned dedup rămâne la BackOffice/domain owner;

* review: exactly-once neafirmat; broker neintrodus; fără schema/tables inventate în acest ADR;

* verificarea securității: credentials dedicate, secrets absente din repository, logging fără credentials/payload sensibil;

* code review / teste în faza de implementare.



---



# 12. Related Documents



| Document | Rol                      |

| -------- | ------------------------ |

| ADR-005  | Boundary Gateway — neschimbat |

| ADR-006  | Hub ↔ Gateway communication / at-least-once — neschimbat |

| ECO-003  | Data Ownership Matrix |

| ECO-006  | Technology Stack — PostgreSQL |

| STD-DB-001 | Database Design Standard |

| STD-SEC-001 | Security — secrets, least privilege, DB security, logging |

| ADR-008…ADR-010 | Arena/XSD, simulator, scope v1 — out of scope |



---



# Version History



| Versiune | Data       | Autor               | Modificări     |

| -------- | ---------- | ------------------- | -------------- |

| 1.0      | 2026-10-06 | ESTINVEST \& ChatGPT | Prima versiune |


