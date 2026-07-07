\# STD-GIT-001 – Git Workflow Standard



| Proprietate | Valoare |

|-------------|----------|

| Document ID | STD-GIT-001 |

| Titlu | Git Workflow Standard |

| Categorie | Git Standards |

| Versiune | 1.0 |

| Status | Approved |

| Clasificare | Internal |

| Repository | ESTINVEST-Ecosystem |

| Owner | Enterprise Architecture |

| Ultima actualizare | 2026-07-07 |



\---



\# Change Log



| Versiune | Data | Modificări |

|----------|------|------------|

| 1.0 | 2026-07-07 | Prima versiune |



\---



\# 1. Purpose



Acest standard definește modul de utilizare a Git și GitHub în cadrul ESTINVEST Ecosystem.



Obiective:



\- trasabilitate;

\- colaborare;

\- controlul modificărilor;

\- calitate;

\- managementul versiunilor.



\---



\# 2. General Principles



Toate proiectele utilizează:



\- Git

\- GitHub

\- Pull Requests

\- Code Review

\- Semantic Versioning



\---



\# 3. Repository Organization



Fiecare aplicație are propriul repository.



Exemple:



\- ESTINVEST-Ecosystem

\- ESTINVEST-Onboarding

\- ESTINVEST-BackOffice

\- ESTtrade



\---



\# 4. Branch Strategy



\## Main



Conține exclusiv cod stabil.



\---



\## Develop



Integrarea dezvoltărilor curente.



\---



\## Feature



Format:



```

feature/<nume-functionalitate>

```



Exemple:



```

feature/customer-search



feature/jwt-authentication



feature/order-import

```



\---



\## Hotfix



Format:



```

hotfix/<descriere>

```



\---



\## Release



Format:



```

release/v1.2.0

```



\---



\# 5. Commit Messages



Format recomandat:



```

<tip>: <descriere>

```



Exemple:



```

feat: add customer search



fix: correct JWT validation



docs: update API documentation



refactor: simplify order service



test: add onboarding integration tests



chore: update dependencies

```



\---



\# 6. Commit Rules



Fiecare commit trebuie să fie:



\- mic;

\- coerent;

\- compilabil;

\- testabil.



Nu se recomandă commit-uri foarte mari.



\---



\# 7. Pull Requests



Fiecare Pull Request trebuie să includă:



\- descriere;

\- scop;

\- impact;

\- teste efectuate;

\- documentație actualizată (dacă este cazul).



\---



\# 8. Code Review



Orice modificare importantă trebuie revizuită înainte de integrare.



Revizuirea urmărește:



\- corectitudine;

\- securitate;

\- performanță;

\- lizibilitate;

\- respectarea standardelor.



\---



\# 9. Versioning



Se utilizează Semantic Versioning.



Format:



```

Major.Minor.Patch

```



Exemple:



```

1.0.0



1.1.0



1.1.3



2.0.0

```



\---



\# 10. Tags



Fiecare versiune oficială trebuie etichetată.



Exemplu:



```

v1.0.0

```



\---



\# 11. Releases



Fiecare Release trebuie să includă:



\- versiunea;

\- data;

\- modificările principale;

\- eventualele incompatibilități.



\---



\# 12. Documentation



Modificările care afectează arhitectura, API-ul sau baza de date trebuie însoțite de actualizarea documentației relevante.



\---



\# 13. AI Assisted Development



Codul generat cu ajutorul AI trebuie:



\- revizuit;

\- testat;

\- înțeles de dezvoltator înainte de integrare.



Utilizarea AI nu elimină responsabilitatea dezvoltatorului pentru calitatea codului.



\---



\# 14. Repository Maintenance



Repository-urile trebuie să conțină:



\- README.md

\- LICENSE (dacă este cazul)

\- .gitignore

\- documentație actualizată



\---



\# 15. Exceptions



Orice abatere de la acest standard trebuie justificată și documentată.



\---



\# 16. Related Documents



\- STD-DEV-001 – Development Workflow

\- STD-APP-001 – Application Documentation Standard

\- ECO-007 – Development Roadmap



\---



\# Version History



| Versiune | Data | Autor | Modificări |

|----------|------|-------|------------|

| 1.0 | 2026-07-07 | ESTINVEST \& ChatGPT | Prima versiune |

