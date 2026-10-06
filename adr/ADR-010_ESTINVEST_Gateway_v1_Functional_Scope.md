# ADR-010 – ESTINVEST Gateway v1 Functional Scope



| Proprietate | Valoare                 |

| ----------- | ----------------------- |

| ADR ID      | ADR-010                 |

| Titlu       | ESTINVEST Gateway v1 Functional Scope |

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



ADR-005…ADR-009 (Approved) stabilesc boundary-ul ESTINVEST Gateway, comunicația Hub ↔ Gateway, Technical Store, conformitatea Arena 3.1.4 / XSD și strategia de reuse / simulator.



Rămâne de formalizat **scope-ul funcțional v1** al ESTINVEST Gateway: ce trebuie acoperit end-to-end pentru fluxul operațional ESTINVEST și ce rămâne exclus din v1.



Surse Arena disponibile (verificate read-only): PDF Arena Gateway Protocol 3.1.4; `arena-gateway-messages.xsd`; `arena-gateway-constraints.xsd`.



ADR-010 definește ESTINVEST Gateway v1 Functional Scope. **Nu modifică ADR-005…ADR-009.**



---



# 2. Problem Statement



Trebuie stabilit formal:



1. ce capabilități face parte din Gateway v1 (connectivity, market data, orders, trades, recovery);

2. care este setul exact de Arena business commands pentru standard Order lifecycle în v1;

3. cum se tratează confirmations / rejects / trade notifications fără a muta ownership business;

4. că recovery/reconciliation este în v1, fără a fixa prematur exact report command set;

5. ce funcții Arena specializate sunt excluse din v1;

6. cum se respectă excepția market data read-only către ESTtrade (ADR-005).



Fără aceste clarificări, implementarea riscă scope creep spre întreg protocolul Arena, moștenirea L2 legacy incomplet sau confundarea market-data path cu trading path.



---



# 3. Decision Drivers



* respectarea ADR-005…ADR-009;

* v1 util end-to-end (nu doar market-data demo);

* suport pentru ESTtrade market visibility (L1/L2) și pentru BackOffice Order/Trade via Integration Hub;

* controlul scope-ului (fără Cross / Market Maker / Deals fără cerință aprobată);

* conformitate Arena 3.1.4 + XSD (ADR-008);

* evitarea inventării unui exact report set neaprobat;

* evitarea inventării API-ului concret market-data către ESTtrade (WebSocket/SSE etc.).



---



# 4. Considered Options



## Opțiunea A



### Descriere



v1 = market-data-only.



### Avantaje



* scope redus;

* livrare aparent mai rapidă.



### Dezavantaje



* nu acoperă integrarea reală BackOffice Order/Trade.



---



## Opțiunea B



### Descriere



v1 = core production path: connectivity/session + L1/L2 + standard Order lifecycle + confirmations/rejects + Trades + recovery/reconciliation support.



### Avantaje



* util end-to-end;

* acoperă vizibilitatea pieței și path-ul operațional de trading via Hub;

* scope controlat față de întregul protocol Arena.



### Dezavantaje



* efort mai mare decât market-data-only;

* exact report command set pentru recovery rămâne de specificat ulterior.



---



## Opțiunea C



### Descriere



v1 = implementarea întregului protocol Arena, inclusiv Cross / MM / Deals / etc.



### Avantaje



* acoperire maximală a protocolului.



### Dezavantaje



* mărește inutil scope-ul inițial cu funcții specializate fără cerință aprobată.



---



## Opțiunea D



### Descriere



v1 = trading/orders fără Level 2 market data complet.



### Avantaje



* efort redus pe L2.



### Dezavantaje



* ESTtrade are nevoie de market visualization; Gateway v1 trebuie să ofere Market Data L1/L2.



---



# 5. Decision



Opțiunea aprobată:



**Opțiunea B — v1 = core production path (connectivity/session + L1/L2 + standard Order lifecycle + confirmations/rejects + Trades + recovery/reconciliation support).**



Motivația alegerii și deciziile consemnate:



### Relația cu ADR-005…009



* ADR-010 respectă ADR-005…009 și **nu le modifică**;

* business prin Integration Hub; ESTtrade direct numai market data read-only; Gateway ≠ business SoR;

* Hub ↔ Gateway = contract intern / HTTP/REST / request-response + async / at-least-once (ADR-006);

* Technical Store PostgreSQL / durable pending delivery (ADR-007);

* Arena 3.1.4 + XSD validation mandatory (ADR-008);

* ESTtrade code = reference/reuse only; simulator owned de ESTINVEST Gateway (ADR-009).



### A. Connectivity / session



ESTINVEST Gateway v1 trebuie să acopere:



* TCP connectivity către Arena Gateway;

* Arena framing;

* MD5 wire handling;

* XML serialization / parsing;

* XSD validation conform ADR-008;

* login;

* logout;

* heartbeat;

* csq request/response correlation;

* reconnect;

* session lifecycle necesar funcționării.



Timeout-uri / retry counts = OUT OF SCOPE.



### B. Market structure / initial environment



* login / UserEnv handling;

* market picture / symbol-market information necesară funcționării;

* load / subscribe mechanics necesare market data.



Nu este un catalog exhaustiv de DTO-uri.



### C. Market Data Level 1



* v1 include Market Data Level 1 necesar ESTtrade;

* trebuie respectat modelul Arena 3.1.4;

* lista completă de ticker labels nu este fixată aici dacă nu a fost aprobată separat.



### D. Market Data Level 2



* v1 include Level 2 complet conform modelului Arena:

  - initial/full order book state;

  - snapshot;

  - incremental updates;

  - cache initialization;

  - cache invalidation / reload handling necesar;

  - GetMarketByOrder / MboDto / ActionTickersPack model conform protocolului;



* **nu** se permite substituirea permanentă a modelului incremental cu full MBO refresh ca în implementarea legacy.



### E. Standard Order lifecycle



v1 include **exact** următoarele Arena business commands:



* `AddOrderBuyCmd`

* `AddOrderSellCmd`

* `CancelOrderCmd`

* `ChgOrderCmd`

* `SuspendOrderCmd`

* `ReleaseOrderBuyCmd`

* `ReleaseOrderSellCmd`



și răspunsurile / reject-urile / `OrdDto` necesare acestora.



### Confirmations / rejects / order events



v1 trebuie să proceseze:



* order confirmations;

* order rejects;

* order state notifications relevante;

* error/reject handling necesar;

* correlation cu request-ul intern și external identifiers.



Ownership neschimbat: **BackOffice rămâne owner-ul Order.** Gateway transmite informația prin Integration Hub conform ADR-005/006.



### Trade notifications



v1 include:



* trade/execution notifications relevante;

* `HalfTrdDto` unde protocolul îl cere;

* partial executions;

* external execution/trade identifiers necesari;

* forward către Integration Hub prin contractul intern.



Gateway **nu** creează Trade business ca System of Record. BackOffice Trade Domain rămâne owner-ul business.



### Reports / recovery



v1 include funcționalitate de:



* reports necesare operațional;

* recovery;

* reconciliation support;

* gap filling / state reconstruction unde Arena oferă mecanismul.



**Exact report command set required for v1 recovery/reconciliation remains to be specified in a subordinate integration specification / implementation decision, without changing the requirement that recovery/reconciliation capability is part of v1.**



Exemple protocol-level (**PROTOCOL CANDIDATES / NOT YET APPROVED AS EXACT V1 SET**):



* `FindFreshOrderReportCmd`

* `GetGenericOrderAuditCmd`

* `GetOrdersDailyLogCmd`

* `GetOutstandingOrdersCmd`

* `GetDailyTradesCmd`



### V1 exclusions (approved)



Următoarele **nu** fac parte din scope-ul v1, dacă nu apare ulterior cerință business aprobată:



* `AddCrossOrdersCmd` / Cross Orders;

* `UpdateMMOrdersCmd`;

* `CancelMMOrdersCmd`;

* Market Maker workflow;

* `AddDealBuyCmd`;

* `AddDealSellCmd`;

* `CancelDealCmd`;

* `ConfirmDealBuyCmd`;

* `ConfirmDealSellCmd`;

* `RefuseDealCmd`;

* Deals workflow;

* Mail;

* Change Password;

* alte operații specializate Arena necerute de fluxul operațional curent.



Excluderea din v1 **nu** înseamnă că arhitectura le blochează ulterior.



### Market data direct to ESTtrade



* ESTtrade poate consuma direct din Gateway doar market data read-only conform ADR-005;

* v1 trebuie să permită distribuirea market data către ESTtrade fără a trece business flow prin BackOffice;

* această cale **nu** autorizează trading direct;

* exact API/WebSocket/SSE mechanism = OUT OF SCOPE dacă nu există deja decizie aprobată.



### Error / recovery boundary



* technical/protocol errors rămân technical integration errors;

* nu se transformă automat în business reject dacă protocolul nu a produs acel rezultat;

* reconnect/recovery trebuie să poată reconstitui starea tehnică necesară;

* business reconciliation rămâne responsabilitate BackOffice/domain owner, cu suportul datelor/rapoartelor Gateway.



### Simulator coverage expectation



* simulatorul trebuie să permită testarea funcționalităților v1 aprobate;

* **nu** se afirmă că simulatorul le implementează deja;

* conform ADR-009: simulator = test double; nu este protocol authority; nu dovedește BVB compatibility.



---



# 6. Consequences



## Beneficii



* v1 util end-to-end, nu doar market-data demo;

* suportă atât ESTtrade market visibility cât și BackOffice Order/Trade integration;

* scope controlat;

* funcții specializate pot fi adăugate ulterior.



## Limitări



* recovery exact report set încă necesită specificare;

* funcții specializate Arena sunt amânate;

* necesită implementare completă L2, nu aproximarea legacy.



## Riscuri



* scope creep spre întreg protocolul Arena;

* tratarea report candidates ca mandatory fără aprobare;

* moștenirea L2 legacy incomplet;

* confundarea market-data path cu trading path.



---



# 7. Impact Analysis



| Domeniu      | Impact |

| ------------ | ------ |

| Business     | Path operațional Order/Trade via Hub; ownership BackOffice neschimbat; MD read-only către ESTtrade. |

| Architecture | Core production path v1; L1+L2; 7 Order commands; exclusions Cross/MM/Deals; ADR-005…009 neschimbate. |

| Security     | Fără bypass trading ESTtrade→Gateway; technical errors ≠ business rejects automate. |

| Performance  | L2 incremental + validare XSD; impact cantitativ neevaluat aici. |

| Operations   | Recovery/reconciliation capability în v1; exact report set ulterior. |

| Development  | Implementare conform scope; simulator testează v1; fără inventarea API MD concret aici. |



---



# 8. Alternatives Rejected



| Alternativă | Motivul respingerii |

| ----------- | ------------------- |

| Opțiunea A — market-data-only | Nu acoperă integrarea reală BackOffice Order/Trade. |

| Opțiunea C — întregul protocol Arena | Mărește inutil scope-ul cu funcții specializate fără cerință aprobată. |

| Opțiunea D — orders fără L2 complet | ESTtrade are nevoie de market visualization; v1 trebuie L1/L2. |



---



# 9. Dependencies



Documente și componente afectate:



* ADR-005 – System Boundary (Approved; neschimbat)

* ADR-006 – Hub ↔ Gateway Communication (Approved; neschimbat)

* ADR-007 – Technical Persistence (Approved; neschimbat)

* ADR-008 – Arena Protocol Compliance (Approved; neschimbat)

* ADR-009 – Reuse and Simulator Strategy (Approved; neschimbat)

* Arena Gateway Protocol 3.1.4 / XSD structural baseline

* Subordinate integration specification — exact report command set (ulterior)



---



# 10. Implementation Notes



Acțiunile necesare pentru implementarea deciziei:



* Pasul 1 — Aprobarea ADR-010 (trecerea din Proposed în statusul de aprobare prevăzut de procesul ADR).

* Pasul 2 — Implementarea connectivity/session + XSD validation + L1/L2 conform protocolului (fără L2 legacy refresh-as-substitute).

* Pasul 3 — Implementarea exact a celor 7 Order commands + confirmations/rejects + trade notifications via Hub.

* Pasul 4 — Specificație subordonată pentru exact report command set de recovery/reconciliation, fără a schimba cerința că recovery este parte din v1.

* Pasul 5 — Respectarea exclusions Cross/MM/Deals; market data ESTtrade = read-only; simulator coverage pentru v1 per ADR-009.



---



# 11. Validation



Cum se verifică implementarea deciziei:



* connectivity/session baseline funcțional;

* XSD validation conform ADR-008;

* L1 market data funcțional;

* L2 snapshot + incremental conform protocolului;

* exact cele 7 standard Order commands aprobate;

* confirmations/rejects;

* trade notifications;

* recovery/reconciliation capability;

* exclusions Cross/MM/Deals respectate;

* market data direct ESTtrade = read-only;

* no direct ESTtrade trading;

* Gateway ≠ business SoR;

* simulator testează v1 dar nu este protocol authority;

* report candidates nu sunt tratate ca exact v1 set aprobat fără specificație subordonată.



---



# 12. Related Documents



| Document | Rol                      |

| -------- | ------------------------ |

| ADR-005  | Boundary / MD exception / no SoR — neschimbat |

| ADR-006  | Hub ↔ Gateway communication — neschimbat |

| ADR-007  | Technical persistence — neschimbat |

| ADR-008  | Arena 3.1.4 / XSD — neschimbat |

| ADR-009  | Reuse / simulator — neschimbat |

| Arena PDF 3.1.4 + XSD | Protocol / structural baseline |

| Subordinate integration spec | Exact report command set (ulterior) |



---



# Version History



| Versiune | Data       | Autor               | Modificări     |

| -------- | ---------- | ------------------- | -------------- |

| 1.0      | 2026-10-06 | ESTINVEST \& ChatGPT | Prima versiune |


