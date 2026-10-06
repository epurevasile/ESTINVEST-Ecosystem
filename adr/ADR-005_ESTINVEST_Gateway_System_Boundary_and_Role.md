# ADR-005 – ESTINVEST Gateway System Boundary and Role



| Proprietate | Valoare                 |

| ----------- | ----------------------- |

| ADR ID      | ADR-005                 |

| Titlu       | ESTINVEST Gateway System Boundary and Role |

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



ESTINVEST Ecosystem include componente pentru tranzacționare (ESTtrade), evidență operațională (BackOffice), integrare externă (Integration Hub) și conectivitate cu Bursa de Valori București.



Documentele existente (ECO-002, ECO-004, INT-001, SYS-001, APP-002) descriu Gateway Connector / BVB Gateway și Integration Hub, însă prezintă uneori trasee simplificate de forma BackOffice → Gateway, fără a formaliza relația canonică dintre Integration Hub și ESTINVEST Gateway.



Este necesară o decizie de arhitectură care stabilească:



* situația actuală: existența conceptuală a Gateway Connector (ECO-002 §8.4, repository ESTINVEST-Gateway, System of Record = Nu) și a Integration Hub ca punct de integrare externă (INT-001, SYS-001);

* problema identificată: ambiguitatea topologiei Hub ↔ Gateway și a eventualelor căi directe BackOffice/ESTtrade → Gateway;

* constrângerile: Separation of Responsibilities; BackOffice ca System of Record pentru datele operaționale; Integration Hub ca boundary față de sistemele externe;

* obiectivele: delimita rolul ESTINVEST Gateway, traseul canonic de business către BVB / Arena Gateway și excepția controlată pentru market data read-only către ESTtrade.



---



# 2. Problem Statement



Trebuie stabilit formal:



1. dacă ESTINVEST Gateway este o componentă distinctă de Integration Hub sau parte din acesta;

2. traseul canonic pentru fluxurile business către și de la BVB / Arena Gateway;

3. cine este consumatorul direct al ESTINVEST Gateway pentru operații business;

4. dacă ESTtrade poate comunica cu ESTINVEST Gateway și în ce condiții;

5. că ESTINVEST Gateway nu este System of Record pentru entitățile de business operaționale.



Fără această clarificare, diagramele simplificate existente pot fi interpretate greșit ca arhitecturi alternative.



---



# 3. Decision Drivers



* separarea responsabilităților între Integration Hub și market connectivity;

* păstrarea Integration Hub ca boundary BackOffice față de sistemele externe (INT-001, SYS-001);

* ownership operațional în BackOffice pentru Order / Trade și domeniile conexe (ECO-002, ECO-003);

* evitarea bypass-ului business ESTtrade → piață;

* mentenanță și lifecycle independent pentru conectivitatea de piață;

* posibilitatea unei excepții controlate pentru market data read-only;

* conformitate cu principiile Loose Coupling și Single Source of Truth;

* aliniere ulterioară a documentației Ecosystem la decizia formală.



---



# 4. Considered Options



## Opțiunea A



### Descriere



Gateway integrat în Integration Hub (fără serviciu/aplicație Gateway distinctă).



### Avantaje



* un singur deployabil de integrare;

* mai puține hop-uri în topologie.



### Dezavantaje



* amestecă boundary-ul generic de integrare cu particularitățile protocolului/infrastructurii de piață;

* lifecycle și scalare cuplate;

* reduce claritatea ownership-ului pentru market connectivity.



---



## Opțiunea B



### Descriere



ESTINVEST Gateway ca serviciu/aplicație distinctă, deployabilă independent, aflată în aval de Integration Hub pentru fluxurile business către BVB / Arena Gateway.



Excepție controlată: ESTtrade poate consuma direct din ESTINVEST Gateway numai market data read-only.



### Avantaje



* separă Integration Hub de market connectivity specializat;

* păstrează Hub ca boundary BackOffice față de externe;

* permite lifecycle propriu pentru Gateway (repository ESTINVEST-Gateway);

* ESTtrade → ESTINVEST Gateway direct este interzis pentru business writes;

* ESTINVEST Gateway → ESTtrade direct este permis numai pentru market data read-only.



### Dezavantaje



* topologie cu un hop suplimentar (Hub → Gateway);

* necesită alinierea documentelor care prezintă trasee simplificate BackOffice → Gateway.



---



## Opțiunea C



### Descriere



BackOffice comunică direct cu ESTINVEST Gateway pentru fluxurile business, ocolind Integration Hub.



### Avantaje



* traseu mai scurt între BackOffice și piață;

* apropiat de diagramele simplificate existente (ECO-002 / ECO-004).



### Dezavantaje



* contrazice rolul Integration Hub ca punct unic de integrare externă (INT-001, SYS-001);

* diluează boundary-ul BackOffice față de sistemele externe;

* crește riscul de cuplare directă la particularitățile de piață.



---



## Opțiunea D



### Descriere



ESTtrade comunică direct cu ESTINVEST Gateway pentru trading.



### Avantaje



* traseu mai direct către piață.



### Dezavantaje



* bypass BackOffice Order Management;

* bypass Integration Hub pentru business;

* compromite ownership-ul BackOffice;

* creează cale alternativă pentru Order/Trade;

* contrazice boundary-ul adoptat.



---



# 5. Decision



Opțiunea aprobată:



**Opțiunea B — ESTINVEST Gateway ca serviciu distinct în aval de Integration Hub, cu excepție read-only market data pentru ESTtrade.**



Motivația alegerii:



* ESTINVEST Gateway este o componentă tehnică distinctă conceptual de Integration Hub și nu îl înlocuiește;

* ESTINVEST Gateway este un serviciu/aplicație deployabilă independent, cu lifecycle propriu;

* repository-ul separat este **ESTINVEST-Gateway**; calea locală de dezvoltare adoptată este `C:\ESTINVEST-Gateway` (nu este cerință de production/deployment);

* pentru fluxurile business către BVB, traseul canonic este:



```text

BackOffice

→ Integration Hub

→ ESTINVEST Gateway

→ BVB / Arena Gateway

```



* retur canonic:



```text

BVB / Arena Gateway

→ ESTINVEST Gateway

→ Integration Hub

→ BackOffice

```



* Integration Hub rămâne boundary-ul BackOffice față de sistemele externe;

* rolul ESTINVEST Gateway este market connectivity specializat și implementarea particularităților protocolului/infrastructurii de piață;

* BackOffice nu comunică direct cu ESTINVEST Gateway pentru fluxurile business;

* pentru Order commands, confirmations, rejects, Trade notifications și reports/recovery business, consumerul direct al ESTINVEST Gateway este Integration Hub;

* ESTtrade poate consuma direct din ESTINVEST Gateway numai informații read-only de market data pe calea:



```text

ESTINVEST Gateway → ESTtrade

```



* această excepție **nu** autorizează: order submission; cancel/change order; Trade creation; Portfolio mutation; Buying Power mutation; Settlement; Cash Ledger; Securities Ledger; alte business writes;

* pentru operațiile business ale ESTtrade, traseul rămâne:



```text

ESTtrade

→ BackOffice

→ Integration Hub

→ ESTINVEST Gateway

→ BVB

```



* ESTINVEST Gateway **nu** este System of Record pentru: Customer; Account; Portfolio; Order; Trade; Position; Settlement; Cash Ledger; Securities Ledger. Ownership-ul business rămâne în sistemele deja stabilite, în special BackOffice pentru Order/Trade și domeniile operaționale relevante.



---



# 6. Consequences



## Beneficii



* topologie canonică clară Hub → Gateway pentru business;

* separarea rolurilor Integration Hub vs market connectivity;

* protecție împotriva bypass-ului business ESTtrade → Gateway;

* confirmarea explicită că Gateway nu este System of Record de business;

* bază pentru ADR-urile următoare (transport, persistence, protocol, simulator, scope v1).



## Limitări



* diagramele/documentele existente care prezintă simplificat BackOffice → Gateway necesită aliniere documentară ulterioară;

* detaliile de transport Hub ↔ Gateway, delivery, Technical Store, Arena/XSD, simulator și scope funcțional v1 nu sunt stabilite aici;

* starea tehnică proprie a Gateway (persistence / reliable delivery) este în afara acestui ADR.



## Riscuri



* interpretarea greșită a excepției market data ca bypass general al Integration Hub;

* întârzierea alinierii documentelor Ecosystem poate menține ambiguitatea operațională;

* implementarea prematură a unor detalii out-of-scope înaintea ADR-urilor dedicate.



---



# 7. Impact Analysis



| Domeniu      | Impact |

| ------------ | ------ |

| Business     | Fluxurile business către BVB trec obligatoriu prin BackOffice și Integration Hub; ESTtrade nu scrie business direct în Gateway. |

| Architecture | ESTINVEST Gateway este componentă distinctă în aval de Integration Hub; diagramele simplificate BackOffice → Gateway nu constituie arhitectură alternativă. |

| Security     | Boundary-ul extern al BackOffice rămâne Integration Hub; excepția market data este read-only. |

| Performance  | Hop suplimentar Hub → Gateway pentru business; impactul cantitativ nu este evaluat în acest ADR. |

| Operations   | Lifecycle și repository separate pentru Gateway (`ESTINVEST-Gateway`); calea locală `C:\ESTINVEST-Gateway` este doar pentru dezvoltare. |

| Development  | Documentele Ecosystem relevante vor necesita ulterior aliniere la ADR-005; implementarea urmează după aprobare și ADR-urile conexe. |



---



# 8. Alternatives Rejected



| Alternativă | Motivul respingerii |

| ----------- | ------------------- |

| Gateway integrat în Integration Hub (Opțiunea A) | Amestecă boundary-ul generic de integrare cu market connectivity specializat; reduce independența de lifecycle. |

| BackOffice comunică direct cu Gateway (Opțiunea C) | Încalcă rolul Integration Hub ca boundary față de sistemele externe; creează o arhitectură alternativă la modelul Hub. |

| ESTtrade comunică direct cu Gateway pentru trading | Permite bypass business (orders/trades/ledger); contrazice ownership-ul BackOffice și traseul canonic prin Hub. |



---



# 9. Dependencies



Documente și componente afectate:



* ECO-002 – System Landscape (Gateway Connector; diagrame simplificate)

* ECO-003 – Data Ownership Matrix

* ECO-004 – Integration Architecture (traseu BackOffice → Gateway)

* INT-001 – Integration Hub

* INT-003 – BVB Gateway (Draft; de aliniat ulterior)

* SYS-001 – Ecosystem Architecture

* SYS-002 – Logical Architecture

* APP-002 – ESTtrade Family

* ADR-006…ADR-010 (viitoare; out of scope aici) – transport Hub↔Gateway, delivery semantics, Technical Store, Arena/XSD, simulator, v1 functional scope



---



# 10. Implementation Notes



Acțiunile necesare pentru implementarea deciziei:



* Pasul 1 — Aprobarea ADR-005 (trecerea din Proposed în statusul de aprobare prevăzut de procesul ADR).

* Pasul 2 — Alinierea ulterioară a documentelor Ecosystem care prezintă simplificat BackOffice → Gateway, astfel încât traseul canonic business să fie BackOffice → Integration Hub → ESTINVEST Gateway.

* Pasul 3 — Consemnarea în ADR-uri separate a subiectelor out of scope (transport, delivery, Technical Store, Arena/XSD, simulator, scope v1).

* Pasul 4 — Bootstrap / dezvoltare în repository-ul `ESTINVEST-Gateway` (cale locală de dezvoltare `C:\ESTINVEST-Gateway`) conform deciziilor aprobate, fără a trata calea locală ca cerință de production.



---



# 11. Validation



Cum se verifică implementarea deciziei:



* validare arhitecturală: niciun flux business BackOffice → ESTINVEST Gateway care ocolește Integration Hub;

* validare arhitecturală: ESTtrade → ESTINVEST Gateway doar pentru market data read-only; fără order/trade/portfolio/ledger writes pe această cale;

* verificarea documentației: ECO/INT/SYS/APP relevante aliniază traseul canonic după aprobare;

* review: Gateway nu este documentat ca System of Record pentru entitățile de business listate;

* code review / teste (în faza de implementare): respectarea boundary-urilor de mai sus.



---



# 12. Related Documents



| Document | Rol                      |

| -------- | ------------------------ |

| ECO-002  | System Landscape – Gateway Connector, SoR = Nu, repository ESTINVEST-Gateway |

| ECO-003  | Data Ownership – Order/Trade ownership BackOffice |

| ECO-004  | Integration Architecture – trasee de aliniat |

| INT-001  | Integration Hub – boundary extern |

| INT-003  | BVB Gateway – document Draft de completat/aliniat |

| SYS-001  | Ecosystem Architecture – comunicații externe via Hub |

| SYS-002  | Logical Architecture – Enterprise Services / Hub |

| APP-002  | ESTtrade Family – fără comunicație directă BVB pentru business |

| ADR-006…ADR-010 | Decizii conexe out of scope pentru ADR-005 |



---



# Version History



| Versiune | Data       | Autor               | Modificări     |

| -------- | ---------- | ------------------- | -------------- |

| 1.0      | 2026-10-06 | ESTINVEST \& ChatGPT | Prima versiune |


