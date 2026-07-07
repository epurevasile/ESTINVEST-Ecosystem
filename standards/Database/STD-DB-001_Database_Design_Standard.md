\# STD-DB-001 – Database Design Standard



| Proprietate | Valoare |

|-------------|----------|

| Document ID | STD-DB-001 |

| Titlu | Database Design Standard |

| Categorie | Database Standards |

| Versiune | 1.0 |

| Status | Approved |

| Clasificare | Internal |

| Repository | ESTINVEST-Ecosystem |

| Owner | Enterprise Architecture |

| Ultima actualizare | 2026-07-07 |



\---



\# 1. Purpose



Acest standard definește regulile generale pentru proiectarea bazelor de date în cadrul ESTINVEST Ecosystem.



Se aplică pentru:



\- Onboarding;

\- BackOffice;

\- ESTtrade;

\- Reporting;

\- Gateway;

\- orice aplicație viitoare.



\---



\# 2. Core Principles



Bazele de date trebuie proiectate conform următoarelor principii:



\- Single Source of Truth;

\- integritate referențială;

\- audit by design;

\- soft delete unde este necesar;

\- migrații controlate;

\- documentare obligatorie;

\- securitate by design.



\---



\# 3. Approved Database Technologies



| Utilizare | Tehnologie |

|----------|------------|

| Aplicații noi | PostgreSQL |

| Aplicații existente | MariaDB |

| ORM standard | Prisma |

| Migrații | Prisma Migrate / SQL Migration |



\---



\# 4. Naming Conventions



\## Tables



Tabelele se denumesc cu litere mici, `snake\_case`.



Exemple:



```text

customer

account

portfolio

trade

audit\_log

