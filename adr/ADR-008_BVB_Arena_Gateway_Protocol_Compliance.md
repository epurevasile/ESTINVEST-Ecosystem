# ADR-008 – BVB Arena Gateway Protocol Compliance



| Proprietate | Valoare                 |

| ----------- | ----------------------- |

| ADR ID      | ADR-008                 |

| Titlu       | BVB Arena Gateway Protocol Compliance |

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



ADR-005 (Approved) delimitează ESTINVEST Gateway ca market connectivity specializat față de Integration Hub, fără ownership business SoR.



ADR-006 (Approved) izolează contractul intern ESTINVEST de Arena wire protocol și atribuie Gateway-ului traducerea internal contract ↔ Arena protocol / XML / DTOs.



ADR-007 (Approved) permite starea tehnică de integrare (inclusiv protocol/session state când este necesar), fără a transfera ownership business.



Pentru ESTINVEST Gateway v1 este necesară o decizie privind **baseline-ul de conformitate Arena**, autoritatea PDF vs XSD și politica obligatorie de validare structurală.



Surse locale verificate (BackOffice repository):



* `docs/06_Integrations/BVB/Arena_Gateway/arena-gateway-protocol-3.1.4.pdf`

* `docs/06_Integrations/BVB/Arena_Gateway/arena-gateway-messages.xsd`

* `docs/06_Integrations/BVB/Arena_Gateway/arena-gateway-constraints.xsd`



Verificări factuale asupra perechii XSD: `arena-gateway-messages.xsd` importă `arena-gateway-constraints.xsd`; perechea compilează; este utilizabilă ca structural validation baseline local. **Nu se afirmă** proveniența oficială BVB a XSD-urilor dacă aceasta nu este verificată.



ADR-008 definește BVB Arena Gateway Protocol Compliance. **Nu modifică ADR-005, ADR-006 sau ADR-007.**



---



# 2. Problem Statement



Trebuie stabilit formal:



1. care este baseline-ul de protocol pentru ESTINVEST Gateway v1;

2. ce rol au PDF 3.1.4 și perechea XSD (semantic vs structural);

3. dacă validarea XSD este obligatorie pentru outgoing și incoming;

4. cum se tratează XML well-formed dar non-conform față de XSD;

5. cum se gestionează discrepanțele PDF ↔ XSD fără rezolvare tacitǎ;

6. cum se guvernează upgrade-ul de versiune protocol/schema.



Fără aceste clarificări, implementarea riscă transmiterea XML non-conform, producerea de efecte business din mesaje invalide sau tratarea interoperabilității locale cu simulatorul ca dovadă de conformitate BVB.



---



# 3. Decision Drivers



* respectarea ADR-005 / ADR-006 / ADR-007 (boundary, contract isolation, technical state);

* controlul conformității protocolului Arena pentru Gateway v1;

* prevenirea transmiterii XML non-schema-compliant către Arena;

* prevenirea efectelor business din mesaje incoming invalide structural;

* izolarea BackOffice de Arena wire model;

* detectarea drift-ului după schimbări de protocol/schema;

* evitarea adoptării automate a unor versiuni Arena noi;

* evitarea inventării unei ierarhii absolute PDF > XSD sau XSD > PDF pentru toate aspectele.



---



# 4. Considered Options



## Opțiunea A



### Descriere



XML parsing fără validare XSD runtime.



### Avantaje



* overhead redus;

* implementare aparent mai simplă.



### Dezavantaje



* well-formed / parser success nu garantează schema compliance;

* riscă transmiterea sau procesarea XML non-conform Arena.



---



## Opțiunea B



### Descriere



Validare XSD obligatorie outgoing + incoming, baseline Arena Gateway Protocol 3.1.4.



### Avantaje



* controlează conformitatea structurală înainte de transmitere și înainte de efecte interne;

* detectează drift după schimbări de schema;

* susține izolarea BackOffice de Arena wire model.



### Dezavantaje



* cost runtime de validare;

* management/versionare schema;

* discrepanțele PDF ↔ XSD necesită handling explicit.



---



## Opțiunea C



### Descriere



Validare XSD numai outgoing.



### Avantaje



* protejează calea către Arena la transmitere;

* overhead mai mic pe incoming.



### Dezavantaje



* XML incoming invalid ar putea produce efect intern/business;

* diluează protecția pe calea Hub / BackOffice.



---



## Opțiunea D



### Descriere



Validare bazată pe simulator / interoperabilitate locală, fără schema validation obligatorie.



### Avantaje



* feedback rapid în mediu local;

* aparentă interoperabilitate connector ↔ simulator.



### Dezavantaje



* simulatorul nu este sursă de adevăr pentru protocol;

* local interoperability nu demonstrează conformitate BVB dacă ambele implementează aceeași eroare.



---



# 5. Decision



Opțiunea aprobată:



**Opțiunea B — Validare XSD obligatorie outgoing + incoming, baseline Arena Gateway Protocol 3.1.4.**



Motivația alegerii și deciziile consemnate:



### Baseline



* baseline protocol pentru ESTINVEST Gateway v1: **Arena Gateway Protocol 3.1.4**;

* PDF-ul 3.1.4 este **referința semantică** pentru comportament și semnificația mesajelor/câmpurilor;

* `arena-gateway-messages.xsd` și `arena-gateway-constraints.xsd` sunt **referințele structurale** pentru XML;

* perechea XSD este prezentă, `messages.xsd` importă `constraints.xsd`, compilează și este utilizabilă ca structural validation baseline local;

* **nu** se afirmă proveniența oficială a XSD-urilor dacă aceasta nu este verificată.



### Authority / precedence



* PDF 3.1.4 = semantic reference;

* XSD = structural reference;

* pentru structură XML, conformitatea cu XSD este **obligatorie**;

* pentru semantică, se folosește protocolul PDF;

* discrepanțele sunt escaladate/verificate, nu rezolvate prin presupunere;

* nu se inventează o ierarhie absolută PDF > XSD sau XSD > PDF pentru toate aspectele.



### XSD validation policy — OUTGOING



* orice XML generat de ESTINVEST Gateway pentru Arena trebuie validat față de XSD **înainte de transmitere**;

* dacă validation FAIL: mesajul **NU** se transmite către Arena Gateway; este tratat ca technical integration error; **nu** se transformă într-un business result artificial.



### XSD validation policy — INCOMING



* orice XML relevant primit de la Arena Gateway trebuie validat structural față de XSD **înainte** de a produce: internal ESTINVEST contract message; Hub notification; business effect indirect;

* dacă validation FAIL: mesajul **NU** trebuie să producă business effect; se tratează ca technical/protocol integration error; se păstrează suficientă trasabilitate tehnică conform politicilor aplicabile.



Detalii exacte de error codes / persistence schema / alerting = OUT OF SCOPE.



### Well-formed ≠ XSD-valid ≠ semantic validity



Se disting explicit:



* well-formed XML;

* XSD-valid XML;

* semantic protocol validity.



Faptul că XML poate fi parsabil nu îl face conform Arena.



Local interoperability connector ↔ simulator **nu** demonstrează conformitate BVB dacă ambele implementează aceeași eroare.



### PDF / XSD discrepancies



* diferențele PDF ↔ XSD nu se rezolvă tacit;

* nu se „corectează” arbitrar una dintre surse;

* discrepanța se înregistrează explicit;

* implementarea trebuie să păstreze trasabilitate către sursa folosită;

* dacă discrepanța poate afecta interoperabilitatea cu BVB, necesită verificare în mediul oficial/test BVB înainte de production reliance.



Exemplu verificat (fără a concluziona care sursă este „greșită”):



* FutureContentDto: PDF 3.1.4 include `isd`; XSD include `isd`; XSD conține și `reo` / `rio`; PDF 3.1.4 nu le listează în secțiunea FutureContentDto.



### Known 3.1.4 difference



* Arena Gateway Protocol 3.1.4 introduce `FutureContentDto.isd` = Issue Date;

* aceasta trebuie susținută de implementarea conformă v1 atunci când FutureContentDto este în scope;

* ADR-008 **nu** extinde instrument data model business.



### Version governance



* ESTINVEST Gateway **nu** adoptă automat o versiune Arena nouă;

* orice upgrade de protocol/schema necesită: (1) identificarea versiunii; (2) impact analysis; (3) compatibility review; (4) regression testing; (5) explicit approval înainte de schimbarea baseline-ului.



### ESTtrade legacy code (finding / context — nu decizie de reuse)



Auditul existent a demonstrat că XML-ul tipizat produs de implementarea ESTtrade nu este baseline de conformitate XSD. Exemple verificate: namespace/type naming neconform; `xs:sequence` order mismatches; lipsă runtime XSD validation; FutureContentDto/`isd` absent.



**ADR-008 nu decide politica de reuse.** Aceasta aparține ADR-009.



---



# 6. Consequences



## Beneficii



* control al conformității protocolului;

* previne XML malformed / non-schema-compliant către Arena sau către efecte business;

* detectează drift după schimbări de protocol;

* izolează BackOffice de Arena wire model.



## Limitări



* cost runtime de validare;

* management/versionare schema;

* discrepanțele PDF ↔ XSD necesită handling explicit.



## Riscuri



* tratarea provenienței XSD locale ca oficial verificată 3.1.4 când nu este;

* ocolirea validării pentru performanță;

* alegerea tacitǎ PDF sau XSD la discrepanțe;

* folosirea succesului cu simulatorul ca dovadă de compatibilitate BVB.



---



# 7. Impact Analysis



| Domeniu      | Impact |

| ------------ | ------ |

| Business     | Mesaje XSD-invalid nu produc business effect; fără business result artificial din fail-uri tehnice. |

| Architecture | Baseline Arena 3.1.4; PDF semantic / XSD structural; ADR-005/006/007 neschimbate. |

| Security     | Trasabilitate tehnică la fail-uri de validare; fără inventarea unui secret/auth model aici. |

| Performance  | Overhead de validare XSD runtime pe outgoing și incoming. |

| Operations   | Schema pair management; discrepancy register; upgrade protocol cu aprobare explicită. |

| Development  | Validare obligatorie înainte de transmitere/efect; fixture-uri valid/invalid; fără library concrete selectată aici. |



---



# 8. Alternatives Rejected



| Alternativă | Motivul respingerii |

| ----------- | ------------------- |

| Opțiunea A — parsing fără validare XSD runtime | Well-formed/parser success nu garantează schema compliance. |

| Opțiunea C — validare numai outgoing | Incoming invalid ar putea produce efect intern/business. |

| Opțiunea D — validare prin simulator/interoperabilitate locală | Simulatorul nu este sursă de adevăr pentru protocol; nu demonstrează conformitate BVB. |



---



# 9. Dependencies



Documente și componente afectate:



* ADR-005 – ESTINVEST Gateway System Boundary and Role (Approved; neschimbat)

* ADR-006 – Integration Hub ↔ ESTINVEST Gateway Communication Architecture (Approved; neschimbat)

* ADR-007 – ESTINVEST Gateway Technical Persistence and Reliable Delivery (Approved; neschimbat)

* Surse locale Arena (BackOffice): PDF 3.1.4; `arena-gateway-messages.xsd`; `arena-gateway-constraints.xsd`

* ADR-009 – reuse & simulator (viitor)

* ADR-010 – v1 functional scope (viitor)



---



# 10. Implementation Notes



Acțiunile necesare pentru implementarea deciziei:



* Pasul 1 — Aprobarea ADR-008 (trecerea din Proposed în statusul de aprobare prevăzut de procesul ADR).

* Pasul 2 — Încărcarea exactă a perechii XSD ca structural validation baseline local; confirmarea că perechea compilează în pipeline-ul de implementare.

* Pasul 3 — Implementarea validării XSD obligatorii outgoing (block before transmit) și incoming (block before internal/Hub/business effect).

* Pasul 4 — Introducerea fixture-urilor known valid/invalid; suport `FutureContentDto.isd` unde FutureContentDto este în scope; discrepancy register pentru diferențe PDF ↔ XSD.

* Pasul 5 — Orice upgrade de protocol/schema urmează version governance (identificare → impact → compatibility → regression → explicit approval).



Nu se selectează aici library-ul concret de parser/XSD validation.



---



# 11. Validation



Cum se verifică implementarea deciziei:



* protocol baseline = 3.1.4;

* exact XSD pair loaded;

* XSD pair compiles;

* outgoing invalid XML blocked;

* incoming invalid XML cannot produce internal/business effect;

* validation tests include known valid/invalid fixtures;

* FutureContentDto.isd supported where applicable;

* PDF/XSD discrepancy register exists when discrepancies are encountered;

* protocol upgrade requires explicit approval;

* no assumption that simulator interoperability = BVB compliance;

* review: ADR-005/006/007 neschimbate; fără inventarea provenienței oficiale XSD.



---



# 12. Related Documents



| Document | Rol                      |

| -------- | ------------------------ |

| ADR-005  | Boundary Gateway — neschimbat |

| ADR-006  | Contract isolation / Arena translation — neschimbat |

| ADR-007  | Technical persistence — neschimbat |

| Arena PDF 3.1.4 | Semantic reference (local BackOffice path) |

| arena-gateway-messages.xsd / arena-gateway-constraints.xsd | Structural reference (local; provenance oficială neverificată aici) |

| ADR-009 / ADR-010 | Reuse/simulator; v1 functional scope — out of scope |



---



# Version History



| Versiune | Data       | Autor               | Modificări     |

| -------- | ---------- | ------------------- | -------------- |

| 1.0      | 2026-10-06 | ESTINVEST \& ChatGPT | Prima versiune |


