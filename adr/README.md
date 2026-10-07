# Architecture Decision Records (ADR)

## Scop

Folderul **adr** conține toate **Architecture Decision Records (ADR)** ale proiectului **ESTINVEST Ecosystem**.

Un ADR documentează o decizie importantă de arhitectură, împreună cu contextul în care a fost luată, alternativele analizate și justificarea alegerii.

Scopul ADR-urilor este păstrarea istoricului deciziilor arhitecturale și asigurarea trasabilității acestora pe întreaga durată de viață a proiectului.

---

# Ce este un ADR?

Un **Architecture Decision Record** este un document scurt și structurat care răspunde la întrebările:

* Ce decizie a fost luată?
* De ce a fost necesară?
* Ce alternative au fost analizate?
* De ce a fost aleasă această soluție?
* Care sunt consecințele asupra sistemului?

---

# Când se creează un ADR?

Se creează un ADR pentru orice decizie care poate influența arhitectura ecosistemului.

Exemple:

* alegerea tehnologiilor principale;
* modificarea arhitecturii;
* introducerea unui nou serviciu;
* schimbarea modelului de integrare;
* modificarea modelului de date;
* schimbarea politicilor de securitate;
* adoptarea unor standarde cu impact major.

---

# Structura folderului

```text id="2wyww9"
adr
│
├── ADR-001_...
├── ADR-002_...
├── ADR-003_...
└── ...
```

Fiecare document utilizează șablonul:

```text id="jpn1kj"
templates/ADR_Template.md
```

---

# Convenții de denumire

Formatul recomandat este:

```text id="zbp85c"
ADR-001_Scurta_Descriere.md
```

Exemple:

* ADR-001_Onboarding_Uses_MariaDB.md
* ADR-002_BackOffice_Uses_PostgreSQL.md
* ADR-003_Client_Model.md
* ADR-004_Unified_Ledger.md

---

# Ciclul de viață al unui ADR

```text id="4onh3v"
Problemă identificată
        │
        ▼
Analiză opțiuni
        │
        ▼
Decizie
        │
        ▼
Creare ADR
        │
        ▼
Implementare
        │
        ▼
Revizuire (dacă este necesar)
```

---

# Reguli generale

* Fiecare ADR are un identificator unic.
* Numerotarea este secvențială și nu se reutilizează.
* Un ADR aprobat nu se șterge.
* Dacă o decizie este schimbată, se creează un ADR nou care îl înlocuiește sau îl completează pe cel anterior.
* ADR-urile trebuie să fie concise, clare și bine argumentate.

---

# Relația cu celelalte documente

* **docs/** descrie arhitectura și funcționalitatea sistemului.
* **standards/** definesc regulile care trebuie respectate.
* **templates/** oferă șabloanele utilizate pentru redactarea ADR-urilor.
* **prompts/** conțin prompturile reutilizabile pentru analiză și documentare.

ADR-urile completează aceste documente prin explicarea deciziilor de arhitectură care au condus la forma actuală a ecosistemului.

---

# Obiectiv

Scopul acestui folder este documentarea și păstrarea istoricului tuturor deciziilor importante de arhitectură, oferind o referință clară pentru dezvoltarea și evoluția **ESTINVEST Ecosystem**.
