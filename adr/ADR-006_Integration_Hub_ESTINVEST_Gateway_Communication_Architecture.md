# ADR-006 – Integration Hub ↔ ESTINVEST Gateway Communication Architecture



| Proprietate | Valoare                 |

| ----------- | ----------------------- |

| ADR ID      | ADR-006                 |

| Titlu       | Integration Hub ↔ ESTINVEST Gateway Communication Architecture |

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



ADR-005 (Approved) stabilește boundary-ul ESTINVEST Gateway:



* traseu canonic business: BackOffice → Integration Hub → ESTINVEST Gateway → BVB / Arena Gateway;

* retur canonic: BVB / Arena Gateway → ESTINVEST Gateway → Integration Hub → BackOffice;

* Integration Hub rămâne boundary-ul BackOffice față de sistemele externe;

* ESTINVEST Gateway nu este System of Record pentru entitățile de business operaționale;

* ESTtrade → ESTINVEST Gateway direct este interzis pentru business writes; Gateway → ESTtrade direct este permis numai pentru market data read-only.



Rămâne de formalizat **cum** comunică Integration Hub cu ESTINVEST Gateway: contractul intern, modelele request/response și async, transportul v1 și semantica de livrare.



ADR-006 definește comunicația Hub ↔ Gateway. **Nu modifică ADR-005.**



---



# 2. Problem Statement



Trebuie stabilit formal:



1. ce contract utilizează Integration Hub ↔ ESTINVEST Gateway și cum se izolează de Arena wire protocol;

2. cum se separă request/response de evenimentele/notificările asincrone;

3. ce transport v1 este adoptat în ambele direcții;

4. ce semantică de delivery se aplică evenimentelor Gateway → Hub;

5. unde se află responsabilitatea pentru prevenirea efectelor business duplicate;

6. diferența conceptuală între succesul de transport HTTP, acknowledgement-ul tehnic și rezultatul de business.



Fără aceste clarificări, implementarea riscă expunerea Arena DTO/XML către BackOffice, introducerea prematură a unui broker sau confuzia între HTTP 2xx și business success.



---



# 3. Decision Drivers



* respectarea ADR-005 (boundary, Hub ca boundary BackOffice, Gateway ≠ SoR);

* decuplarea BackOffice de Arena protocol / XML / DTOs;

* suport pentru comenzi inițiate și pentru evenimente unsolicited;

* simplitate operațională în v1 (fără broker dedicat Hub ↔ Gateway);

* livrare fiabilă a notificărilor relevante (at-least-once);

* prevenirea efectelor business duplicate fără a muta ownership-ul Order/Trade în Gateway;

* aliniere la REST intern și la principiile de idempotency din STD-API-001 (la nivel de domeniu, nu ca exactly-once pe transport);

* mentenanță și evolutivitate a contractului intern independent de versiunea Arena.



---



# 4. Considered Options



## Opțiunea A



### Descriere



HTTP/REST request/response în ambele direcții, fără semantică async explicită pentru notificări.



### Avantaje



* model simplu, familiar;

* un singur tip de interacțiune de modelat.



### Dezavantaje



* nu acoperă clar evenimentele unsolicited (confirmations, rejects, trades);

* riscă tratamentul greșit al notificărilor ca simple răspunsuri sincron la request-ul curent.



---



## Opțiunea B



### Descriere



HTTP/REST pentru request/response și pentru async notifications, cu semantică async explicită pe direcția Gateway → Hub și delivery at-least-once pentru evenimentele/notificările relevante.



### Avantaje



* păstrează transportul simplu (HTTP/REST intern) în v1;

* separă clar modelele request/response și async event/notification;

* permite retransmitere și duplicate tehnice fără a afirma exactly-once;

* nu introduce broker nou pentru această integrare în v1.



### Dezavantaje



* HTTP nu este un event bus; disciplina de receipt/ack și retry trebuie definită ulterior (out of scope aici);

* duplicate tehnice trebuie gestionate de domeniul care deține efectul business.



---



## Opțiunea C



### Descriere



Message broker dedicat pentru Gateway ↔ Hub din v1 (ex. RabbitMQ, Kafka, NATS).



### Avantaje



* async nativ la nivel de infrastructură;

* pattern-uri mature de pub/sub și retry.



### Dezavantaje



* introduce infrastructură nouă nejustificată în v1 doar pentru această integrare;

* crește complexitatea operațională prematur.



---



## Opțiunea D



### Descriere



Expunerea directă a Arena DTO/XML către Integration Hub / BackOffice.



### Avantaje



* mai puțină traducere în Gateway;

* mapare aparent directă la mesajele de piață.



### Dezavantaje



* cuplare puternică la protocolul Arena;

* Arena XML / DTOs / envelopes ar deveni modele BackOffice;

* evolutivitatea internă ar depinde de wire protocol-ul pieței.



---



# 5. Decision



Opțiunea aprobată:



**Opțiunea B — HTTP/REST request/response și async notifications, cu semantică async + at-least-once pe Gateway → Hub.**



Motivația alegerii și deciziile consemnate:



### Relația cu ADR-005



* ADR-005 definește boundary-ul sistemului;

* ADR-006 definește comunicația Hub ↔ Gateway;

* ADR-006 nu modifică ADR-005;

* ADR-006 nu introduce BackOffice → Gateway direct, ESTtrade → Gateway direct pentru business, sau Gateway ca System of Record.



### Contract boundary



* Integration Hub ↔ ESTINVEST Gateway utilizează un **contract intern ESTINVEST**;

* contractul este **independent** de Arena Gateway wire protocol;

* Arena XML / Arena DTOs / Arena message envelopes **nu** sunt expuse ca modele BackOffice;

* ESTINVEST Gateway este responsabil de traducerea:



```text

internal ESTINVEST contract

↔

Arena protocol / XML / DTOs

```



### Două modele de comunicare



1. **REQUEST / RESPONSE** — pentru comenzi sau request-uri inițiate de BackOffice prin Integration Hub (exemple conceptuale: order command; cancel/change/suspend/release; report request; alte request-uri explicite). Exact Arena command set = OUT OF SCOPE (ADR-010).

2. **ASYNC EVENT / NOTIFICATION** — pentru evenimente care apar independent de request-ul curent (order confirmations; rejects; trade notifications; unsolicited market/business events relevante; recovery/report results când semantic se comportă asincron). Nu se presupune că toate Arena messages devin business events.



### Transport v1



* Integration Hub → ESTINVEST Gateway: **HTTP/REST intern**;

* ESTINVEST Gateway → Integration Hub: **HTTP/REST intern**;

* Gateway → Hub este **semantic asincron** chiar dacă livrarea tehnică se face prin HTTP request;

* HTTP nu este descris ca event bus;

* nu se introduce RabbitMQ, Kafka, NATS sau alt broker în v1 doar pentru această integrare.



### Delivery semantics (Gateway → Hub, evenimente/notificări relevante)



* delivery semantic = **at-least-once**;

* Gateway poate retransmite până primește confirmarea tehnică de recepție;

* duplicate tehnice de delivery sunt permise;

* **exactly-once nu este afirmat**;

* duplicate business effects sunt **interzise**;

* responsabilitatea pentru prevenirea efectelor business duplicate aparține **domeniului care deține efectul**;

* Gateway poate folosi identificatori și metadata tehnică pentru dedup/correlation;

* Gateway **nu** devine owner al idempotency business pentru Order/Trade.



### Receipt / acknowledgement (conceptual)



Se diferențiază:



* HTTP transport success;

* technical receipt acknowledgement;

* business acceptance / business processing result.



HTTP 2xx **nu** înseamnă automat business success.



Detaliile exacte de endpoint, status codes, payload, retry intervals, timeout values, retry count, backoff, dead-letter = **OUT OF SCOPE** în ADR-006.



### Correlation / identifiers (conceptual)



* request correlation obligatorie când există request/response context;

* external Arena identifiers pot fi păstrați/transmiși ca metadata;

* internal contract identifiers trebuie să fie distincte de Arena wire semantics;

* consumerii BackOffice nu depind de `csq` ca business identifier.



Schema concretă de payload = OUT OF SCOPE.



---



# 6. Consequences



## Beneficii



* contract intern decuplat de Arena wire protocol;

* modele clare request/response vs async notification;

* transport v1 simplu (HTTP/REST) fără broker dedicat;

* at-least-once pentru notificările relevante Gateway → Hub;

* ownership-ul idempotency business rămâne la domeniu (Order/Trade etc.).



## Limitări



* detalii de endpoints, retry numeric, timeouts și dead-letter rămân de definit ulterior;

* Technical Store / persistence pentru pending delivery nu este decis aici (ADR-007+);

* HTTP ca vehicul async necesită disciplină operațională (ack tehnic vs business result).



## Riscuri



* confuzia între HTTP 2xx și business success;

* tratarea greșită a duplicate tehnice ca duplicate business;

* extinderea prematură către broker sau expunerea Arena DTO către BackOffice în implementare.



---



# 7. Impact Analysis



| Domeniu      | Impact |

| ------------ | ------ |

| Business     | BackOffice consumă contract intern via Hub; fără Arena DTO/XML ca model de business. |

| Architecture | Hub ↔ Gateway = HTTP/REST v1; async semantic pe Gateway → Hub; ADR-005 neschimbat. |

| Security     | Comunicație internă Hub ↔ Gateway; fără expunerea protocolului Arena către BackOffice. |

| Performance  | At-least-once poate genera retransmisii; impactul cantitativ nu este evaluat aici. |

| Operations   | Fără broker nou în v1 pentru această integrare; operațional pe HTTP/REST intern. |

| Development  | Gateway implementează traducerea internal ↔ Arena; Hub integrează contractul intern. |



---



# 8. Alternatives Rejected



| Alternativă | Motivul respingerii |

| ----------- | ------------------- |

| Opțiunea A — HTTP/REST fără semantică async explicită | Nu acoperă clar evenimentele/notificările unsolicited și riscă modelarea greșită a fluxurilor async. |

| Opțiunea C — Message broker dedicat în v1 | Introduce infrastructură nouă nejustificată în v1 doar pentru Hub ↔ Gateway. |

| Opțiunea D — Expunerea Arena DTO/XML către Hub / BackOffice | Creează coupling la protocolul Arena; contrazice izolarea contractului intern. |



---



# 9. Dependencies



Documente și componente afectate:



* ADR-005 – ESTINVEST Gateway System Boundary and Role (Approved; boundary neschimbat)

* INT-001 – Integration Hub

* ECO-004 – Integration Architecture

* SYS-001 – Ecosystem Architecture

* STD-API-001 – API Design Standard (REST / idempotency principles)

* ADR-007…ADR-010 (viitoare) – Technical Store / delivery persistence; Arena/XSD; simulator; v1 functional scope



---



# 10. Implementation Notes



Acțiunile necesare pentru implementarea deciziei:



* Pasul 1 — Aprobarea ADR-006 (trecerea din Proposed în statusul de aprobare prevăzut de procesul ADR).

* Pasul 2 — Definirea ulterioară a contractului intern (endpoints, payloads, status codes, ack tehnic) fără a expune Arena DTO/XML către BackOffice.

* Pasul 3 — Consemnarea în ADR-uri separate a Technical Store / pending delivery, Arena/XSD compliance, simulator și scope funcțional v1.

* Pasul 4 — Implementarea în Integration Hub și ESTINVEST-Gateway conform ADR-005 + ADR-006, fără broker dedicat Hub ↔ Gateway în v1.



---



# 11. Validation



Cum se verifică implementarea deciziei:



* validare arhitecturală: nu există expunere Arena DTO/XML ca modele BackOffice;

* validare arhitecturală: transport v1 Hub ↔ Gateway = HTTP/REST în ambele direcții;

* validare arhitecturală: Gateway → Hub tratat ca async semantic, nu ca event bus generic;

* review: delivery at-least-once; exactly-once neafirmat; duplicate business effects interzise; idempotency business domain-owned;

* review: ADR-005 neschimbat; fără BackOffice → Gateway direct și fără business ESTtrade → Gateway;

* verificarea documentației și code review în faza de implementare.



---



# 12. Related Documents



| Document | Rol                      |

| -------- | ------------------------ |

| ADR-005  | Boundary sistem Gateway (Approved) — neschimbat de ADR-006 |

| INT-001  | Integration Hub — boundary extern BackOffice |

| ECO-004  | Integration Architecture |

| SYS-001  | Ecosystem Architecture — comunicații externe via Hub |

| STD-API-001 | Principii REST / idempotency |

| ADR-007…ADR-010 | Persistence, Arena/XSD, simulator, scope v1 — out of scope |



---



# Version History



| Versiune | Data       | Autor               | Modificări     |

| -------- | ---------- | ------------------- | -------------- |

| 1.0      | 2026-10-06 | ESTINVEST \& ChatGPT | Prima versiune |


