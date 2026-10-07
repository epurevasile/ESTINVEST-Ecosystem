 # APP-002_ESTtrade_Family

| Proprietate | Valoare |
|-------------|----------|
| Document | APP-002_ESTtrade_Family |
| Document ID | APP-002 |
| Titlu | ESTtrade Family |
| Proiect | ESTINVEST Ecosystem |
| Versiune | 1.1 |
| Status | Approved |
| Domeniu | Applications |
| Data | Iulie 2026 |
| Ultima actualizare | 2026-10-06 |

---

# Change Log

| Versiune | Data | Modificări |
|----------|------|------------|
| 1.0 | Iulie 2026 | Prima versiune |
| 1.1 | 2026-10-06 | Aliniere cu ADR-005 privind traseul business și excepția market data read-only |

---

# 1. Scop

Acest document definește arhitectura funcțională a familiei de aplicații **ESTtrade**.

ESTtrade reprezintă ansamblul canalelor prin care clienții și utilizatorii interni interacționează cu serviciile de tranzacționare și operațiuni financiare ale ESTINVEST.

---

# 2. Viziune

ESTtrade este punctul unic de acces pentru:

- tranzacționare;
- administrarea portofoliului;
- operațiuni financiare;
- informații de piață;
- notificări.

Funcționalitățile business utilizează serviciile furnizate de BackOffice Core. Pentru market data read-only se aplică excepția aprobată în ADR-005.

---

# 3. Componente

Familia ESTtrade este alcătuită din:

## ESTtrade Web

Canalul principal destinat clienților.

Funcționalități:

- introducere ordine;
- modificare ordine;
- anulare ordine;
- vizualizare portofoliu;
- operațiuni financiare;
- rapoarte.

---

## ESTtrade Mobile

Aplicația mobilă.

Oferă aceleași funcționalități esențiale ca versiunea Web, adaptate dispozitivelor mobile.

---

## Broker Console

Interfață dedicată brokerilor.

Permite:

- administrarea clienților alocați;
- introducerea ordinelor;
- modificarea ordinelor;
- monitorizarea activității.

---

## Trader Console

Interfață destinată traderilor autorizați.

Permite:

- gestionarea ordinelor;
- transmiterea către piață;
- monitorizarea execuțiilor;
- gestionarea incidentelor operaționale.

---

# 4. Module funcționale

## Trading

- introducere ordine;
- modificare ordine;
- anulare ordine;
- istoric ordine.

---

## Portfolio

- dețineri;
- evaluare;
- alocarea activelor;
- performanță.

---

## Financial Operations

- alimentare cont;
- retragere numerar;
- schimb valutar;
- transferuri interne;
- istoric operațiuni.

---

## Market Information

- cotații;
- grafice;
- știri;
- simboluri.

---

## Notifications

- execuții;
- confirmări;
- mesaje;
- alerte.

---

## Reports

- extras de cont;
- portofoliu;
- tranzacții;
- situații fiscale.

---

# 5. Responsabilități

ESTtrade:

- colectează comenzile utilizatorului;
- validează datele de intrare;
- transmite cererile către BackOffice;
- afișează rezultatele.

ESTtrade nu implementează logica de business.

---

# 6. Relația cu BackOffice

BackOffice furnizează servicii pentru:

- Order Management;
- Trade Management;
- Settlement;
- Cash Management;
- Portfolio Management;
- Reporting.

ESTtrade consumă aceste servicii prin API.

---

# 7. Relația cu Integration Hub

Pentru operațiile business / trading, traseul este:

ESTtrade → BackOffice → Integration Hub → ESTINVEST Gateway → BVB / Arena Gateway

ESTtrade nu comunică direct cu Gateway / BVB pentru business.

ESTtrade nu comunică direct cu:

- BVB (pentru trading / business);
- Depozitarul Central;
- banca de decontare;
- custozi externi.

Comunicațiile externe de business sunt realizate prin BackOffice și Integration Hub.

Excepție aprobată (ADR-005): ESTtrade poate consuma direct din ESTINVEST Gateway numai market data read-only.

Această excepție nu autorizează order submission, cancel/change, Trade creation, mutații Portfolio / Buying Power, scrieri Settlement / Ledger și nu reprezintă un bypass general al BackOffice sau Integration Hub.

---

# 8. Securitate

Autentificarea și autorizarea sunt furnizate de IAM.

ESTtrade nu gestionează direct identitățile utilizatorilor.

---

# 9. Principii

- API First;
- Thin Client;
- Stateless;
- Event Driven;
- Responsive Design;
- Mobile First (unde este aplicabil).

---

# 10. Extensibilitate

Familia ESTtrade poate fi extinsă cu:

- Open API;
- AI Trading Assistant;
- Portal Parteneri;
- integrare cu alte canale digitale.

Aceste extensii trebuie să utilizeze aceleași servicii BackOffice.

---

# 11. Relația cu celelalte documente

Acest document completează:

- SYS-001_Ecosystem_Architecture;
- ADR-006_Trading_Channel_Architecture (BackOffice);
- documentația proiectului ESTtrade.

---

# 12. Concluzii

ESTtrade reprezintă familia oficială a canalelor de tranzacționare și operațiuni financiare din ecosistemul ESTINVEST.

Separarea dintre interfața utilizator și logica de business permite reutilizarea serviciilor BackOffice, dezvoltarea independentă a canalelor și integrarea facilă a unor noi modalități de acces.

---

# Version History

| Versiune | Data | Modificări |
|----------|------|------------|
| 1.0 | Iulie 2026 | Prima versiune |
| 1.1 | 2026-10-06 | Aliniere cu ADR-005 privind traseul business și excepția market data read-only |
