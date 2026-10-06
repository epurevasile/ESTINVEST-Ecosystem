# ADR-009 – Gateway Connector Reuse and Arena Simulator Strategy



| Proprietate | Valoare                 |

| ----------- | ----------------------- |

| ADR ID      | ADR-009                 |

| Titlu       | Gateway Connector Reuse and Arena Simulator Strategy |

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



ADR-005…ADR-008 (Approved) stabilesc boundary-ul ESTINVEST Gateway, comunicația Hub ↔ Gateway, Technical Store și conformitatea Arena 3.1.4 / XSD.



În ESTtrade există implementări de referință:



* `C:\ESTtrade\05_Gateway\gateway-connector`

* `C:\ESTtrade\05_Gateway\gateway-simulator`



Starea ESTtrade verificată read-only la crearea acestui ADR:



* branch: `main`

* HEAD: `fa5332c822b90dfd3e52ea4acb9166c26d9197cd`

* working tree: NOT CLEAN (modificări/untracked în afara `05_Gateway`; fără impact material asupra deciziei de reuse)

* `gateway-connector` / `gateway-simulator`: prezente (NestJS `src/`)



Findings factuale (motivation/context, nu noi decizii de scope):



* Connector: există asset-uri TCP / framing-buffer / MD5 / session / reconnect / heartbeat / market-data; runtime XSD validation lipsește; Orders/Trades complete nu sunt implementate ca fluxuri complete.

* Simulator: există TCP server / framing / session / market-data parțial; comenzi Orders/Trades/Reports sunt respinse explicit ca „not supported in Phase 1”; Level 2 existent aproximează comportamentul; XSD compliance nu este baseline valid; XML builders folosesc `xsi:type` assumptions.



Este necesară o decizie privind **reuse** și **ownership-ul simulatorului**, fără a muta/copia fișiere în acest ADR.



ADR-009 **nu modifică** ADR-005…ADR-008.



---



# 2. Problem Statement



Trebuie stabilit formal:



1. dacă codul ESTtrade Gateway este implementation/production baseline sau doar reference/reuse source;

2. ce tipuri de asset-uri pot fi candidate pentru reuse selectiv după verificare;

3. ce asset-uri nu se preiau implicit ca baseline (în special protocol/XML);

4. cui aparține Arena Gateway Simulator ca ownership/lifecycle;

5. care este rolul simulatorului față de ADR-008 (test double vs protocol authority);

6. ce criterii conceptuale trebuie îndeplinite înainte de reuse.



Fără aceste clarificări, există riscul copierii „as-is” a comportamentelor legacy neconforme ADR-008 sau al cuplării lifecycle-ului Gateway de ESTtrade.



---



# 3. Decision Drivers



* respectarea ADR-005 (Gateway distinct; repo `ESTINVEST-Gateway`; ≠ business SoR);

* respectarea ADR-006 (contract intern; Arena DTO/XML nu sunt modele BackOffice);

* respectarea ADR-007 (Technical Store separat; fără transfer ownership business);

* respectarea ADR-008 (Arena 3.1.4; XSD outgoing+incoming obligatoriu; simulator ≠ BVB compliance);

* păstrarea investiției tehnice ESTtrade unde este justificată;

* evitarea moștenirii implicite a defectelor/gaps demonstrate;

* ownership corect al simulatorului sub ESTINVEST Gateway;

* fără autorizarea mutării/copierii efective de fișiere în acest ADR.



---



# 4. Considered Options



## Opțiunea A



### Descriere



Mutarea/copierea integrală a `gateway-connector` și `gateway-simulator` din ESTtrade și folosirea lor ca baseline.



### Avantaje



* viteză aparentă de pornire;

* reutilizare maximală a codului existent.



### Dezavantaje



* codul existent are gaps și neconformități demonstrate;

* ar importa legacy assumptions (XML/XSD/namespace/ordering);

* contrazice ADR-008 ca autoritate de protocol.



---



## Opțiunea B



### Descriere



Selective reuse după verificare + reconstrucția zonelor protocol-critical conform Arena 3.1.4 / XSD; simulator owned de ESTINVEST Gateway.



### Avantaje



* reuse fără moștenirea implicită a defectelor;

* aliniere la ADR-008;

* ownership/lifecycle corect pentru simulator;

* testability independentă de ESTtrade.



### Dezavantaje



* fiecare asset trebuie verificat înainte de reuse;

* pot exista componente care trebuie rescrise;

* cost de migrare/refactoring.



---



## Opțiunea C



### Descriere



Rescriere completă fără reutilizarea niciunui asset ESTtrade.



### Avantaje



* start curat fără legacy coupling.



### Dezavantaje



* pierde asset-uri tehnice potențial reutilizabile (TCP/framing/MD5/session etc.) fără motiv suficient.



---



## Opțiunea D



### Descriere



Păstrarea simulatorului ca asset owned de ESTtrade, folosit de Gateway extern.



### Avantaje



* evitarea mutării imediate a simulatorului.



### Dezavantaje



* cuplează lifecycle/test architecture Gateway de ESTtrade;

* ownership-ul simulatorului ar fi în proiectul greșit.



---



# 5. Decision



Opțiunea aprobată:



**Opțiunea B — Selective reuse după verificare; zone protocol-critical reconstruite conform Arena 3.1.4/XSD; simulator owned de ESTINVEST Gateway.**



Motivația alegerii și deciziile consemnate:



### Relația cu ADR-005…008



* ADR-009 respectă ADR-005…008 și **nu le modifică**;

* protocol/XML se aliniază la ADR-008; simulator interoperability nu demonstrează BVB compliance.



### Reuse policy



Codul existent din ESTtrade:



`C:\ESTtrade\05_Gateway\gateway-connector`



este:



* **REFERENCE / REUSE SOURCE**



și **nu** este:



* IMPLEMENTATION BASELINE;

* PRODUCTION BASELINE



pentru ESTINVEST Gateway.



Reuse-ul se face:



* component-by-component;

* numai după verificare;

* numai dacă respectă ADR-005…ADR-008;

* fără copiere implicită a comportamentelor existente.



**ADR-009 nu autorizează mutarea sau copierea efectivă a fișierelor.** Aceasta este implementare ulterioară.



### Potentially reusable assets



Următoarele sunt **candidate for selective reuse after verification** (nu aprobate pentru copiere as-is):



* TCP client/server patterns;

* buffering / framing logic;

* MD5 wire handling;

* reconnect patterns;

* session handling patterns;

* heartbeat handling;

* anumite market-data parsing/mapping patterns;

* anumite simulator infrastructure patterns.



### Assets not to be inherited as-is



Nu se preiau implicit ca baseline:



* XML builders existente;

* Arena DTO/mapping layer existent;

* namespace / `xsi:type` assumptions existente;

* field ordering assumptions;

* XSD assumptions;

* current Level 2 implementation;

* simulator-generated XML ca protocol authority;

* Orders/Trades/Reports/Recovery stubs;

* orice comportament demonstrat neconform ADR-008.



Motiv: auditul/verificarea curentă a demonstrat neconformități XSD (lipsă runtime validation; assumptions XML) și acoperire incompletă (Orders/Trades/Reports complete lipsesc / Phase 1 unsupported).



Noua implementare Arena trebuie să fie derivată din:



* ADR-008;

* Arena Gateway Protocol 3.1.4;

* XSD structural baseline;



nu din comportamentul legacy.



### Simulator ownership



Arena Gateway Simulator aparține ecosistemului de dezvoltare/testare al:



**ESTINVEST Gateway**



și **nu** rămâne dependent structural de ESTtrade ca ownership/lifecycle.



Simulatorul existent:



`C:\ESTtrade\05_Gateway\gateway-simulator`



este, la fel ca connectorul existent:



* reference/reuse source;

* nu baseline de conformitate.



Path-ul exact al simulatorului în noul repository = OUT OF SCOPE.



### Simulator role



Simulatorul nou / evoluat este:



**TEST DOUBLE pentru Arena Gateway**



și **nu**:



* sursă de adevăr protocol;

* dovadă suficientă de compatibilitate BVB;

* substitut pentru XSD;

* substitut pentru mediul oficial/test BVB.



Simulatorul trebuie să fie aliniat la același baseline:



Arena Gateway Protocol 3.1.4 + XSD structural baseline conform ADR-008.



### Simulator validation boundary



* simulatorul trebuie să producă/consume XML conform XSD unde emulează Arena;

* succesul Connector ↔ Simulator nu dovedește singur BVB compatibility;

* comportamentele neacoperite integral de protocol trebuie verificate ulterior față de mediul BVB;

* simulatorul nu poate „normaliza” sau ascunde mesaje pe care Connectorul real le-ar respinge conform ADR-008.



Infrastructura/toolchain-ul concret de test = OUT OF SCOPE.



### Development purpose (test capabilities — nu afirmație de implementare existentă)



Simulatorul poate susține dezvoltarea/testarea pentru:



* connectivity/session;

* market data;

* Orders/rejects;

* Trades;

* Reports;

* reconnect/disconnect;

* duplicates;

* timeout scenarios;

* recovery;

* deterministic test scenarios.



Acestea sunt capabilități/obiective de testare. **Nu** se afirmă că toate sunt deja implementate.



Functional scope-ul obligatoriu v1 al Gateway este definit separat de **ADR-010**.



### Reuse acceptance criteria (conceptual)



Un component legacy poate fi reutilizat dacă:



* este identificat exact;

* responsabilitatea lui este în scope;

* comportamentul este verificat;

* nu contrazice ADR-005…ADR-008;

* testele relevante trec;

* pentru protocol/XML, conformitatea cu ADR-008 este demonstrată.



Nu se inventează coverage percentages sau quality gates numerice.



---



# 6. Consequences



## Beneficii



* reuse fără moștenirea implicită a defectelor legacy;

* păstrează investiția existentă unde este justificată;

* un simulator controlat de proiectul Gateway;

* testability independent de ESTtrade;

* alignment cu ADR-008.



## Limitări



* fiecare asset trebuie verificat înainte de reuse;

* pot exista componente care trebuie rescrise;

* cost de migrare/refactoring;

* path-ul exact în `ESTINVEST-Gateway` rămâne de definit.



## Riscuri



* copiere „as-is” sub eticheta reuse;

* simulator care diverge de Arena;

* tratarea testelor locale ca BVB certification;

* păstrarea accidentală a coupling-ului către ESTtrade.



---



# 7. Impact Analysis



| Domeniu      | Impact |

| ------------ | ------ |

| Business     | Fără transfer ownership business; Gateway rămâne ≠ SoR (ADR-005). |

| Architecture | Selective reuse; protocol-critical reconstruit pe ADR-008; simulator owned de ESTINVEST Gateway. |

| Security     | Fără autorizarea mutării secrets/`.env` din ESTtrade; secrets rămân în afara repository conform STD-SEC. |

| Performance  | N/A la nivel de politică reuse; validarea XSD rămâne obligatorie per ADR-008. |

| Operations   | Lifecycle simulator sub Gateway; ESTtrade nu este dependency runtime obligatorie. |

| Development  | Component-by-component verification; fără copy/move autorizat în acest ADR; ADR-010 definește scope v1. |



---



# 8. Alternatives Rejected



| Alternativă | Motivul respingerii |

| ----------- | ------------------- |

| Opțiunea A — copy/move integral ca baseline | Gaps și neconformități demonstrate; importă legacy assumptions. |

| Opțiunea C — rescriere completă fără reuse | Pierde asset-uri tehnice potențial reutilizabile fără motiv. |

| Opțiunea D — simulator owned de ESTtrade | Cuplează lifecycle/test architecture Gateway de ESTtrade; ownership greșit. |



---



# 9. Dependencies



Documente și componente afectate:



* ADR-005 – System Boundary (Approved; neschimbat)

* ADR-006 – Hub ↔ Gateway Communication (Approved; neschimbat)

* ADR-007 – Technical Persistence (Approved; neschimbat)

* ADR-008 – Arena Protocol Compliance (Approved; neschimbat)

* ESTtrade `05_Gateway/gateway-connector` — reference/reuse source

* ESTtrade `05_Gateway/gateway-simulator` — reference/reuse source

* ADR-010 — Gateway v1 functional scope (viitor)



---



# 10. Implementation Notes



Acțiunile necesare pentru implementarea deciziei:



* Pasul 1 — Aprobarea ADR-009 (trecerea din Proposed în statusul de aprobare prevăzut de procesul ADR).

* Pasul 2 — Inventarierea component-by-component a candidatelor de reuse; aplicarea reuse acceptance criteria.

* Pasul 3 — Reconstrucția / ne-moștenirea zonelor protocol-critical (XML/DTO/XSD/ordering/namespace) conform ADR-008.

* Pasul 4 — Stabilirea ownership-ului simulatorului sub ESTINVEST Gateway (path exact ulterior); aliniere la Arena 3.1.4 + XSD.

* Pasul 5 — ADR-010 pentru functional scope v1; fără a trata acest ADR ca autorizație de copy/move.



---



# 11. Validation



Cum se verifică implementarea deciziei:



* legacy code este tratat ca reference/reuse source;

* niciun protocol-critical component nu este acceptat doar pentru că există în ESTtrade;

* XML/XSD components respectă ADR-008;

* simulator ownership aparține ESTINVEST Gateway;

* simulatorul nu este documentat ca protocol authority;

* simulator success ≠ BVB compliance;

* ESTtrade nu devine dependency runtime obligatorie pentru simulator/Gateway;

* reuse decisions sunt trasabile component-by-component;

* review: ADR-005…008 neschimbate; fără mutare/copiere autorizată de acest ADR.



---



# 12. Related Documents



| Document | Rol                      |

| -------- | ------------------------ |

| ADR-005  | Boundary / repo ESTINVEST-Gateway — neschimbat |

| ADR-006  | Internal contract isolation — neschimbat |

| ADR-007  | Technical Store — neschimbat |

| ADR-008  | Arena 3.1.4 / XSD validation — neschimbat |

| ESTtrade gateway-connector / gateway-simulator | Reference/reuse sources only |

| ADR-010  | Gateway v1 functional scope — out of scope |



---



# Version History



| Versiune | Data       | Autor               | Modificări     |

| -------- | ---------- | ------------------- | -------------- |

| 1.0      | 2026-10-06 | ESTINVEST \& ChatGPT | Prima versiune |


