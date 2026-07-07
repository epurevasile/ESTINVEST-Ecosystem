\# STD-NAM-001 – Naming Convention Standard



| Proprietate | Valoare |

|-------------|----------|

| Document ID | STD-NAM-001 |

| Titlu | Naming Convention Standard |

| Categorie | Naming Standards |

| Versiune | 1.0 |

| Status | Approved |

| Clasificare | Internal |

| Repository | ESTINVEST-Ecosystem |

| Owner | Enterprise Architecture |

| Ultima actualizare | 2026-07-07 |



\---



\# 1. Purpose



Acest standard definește convențiile de denumire utilizate în toate proiectele din cadrul ESTINVEST Ecosystem.



Obiective:



\- consistență;

\- lizibilitate;

\- mentenabilitate;

\- predictibilitate.



\---



\# 2. General Principles



Denumirile trebuie să fie:



\- descriptive;

\- concise;

\- consecvente;

\- în limba engleză;

\- fără abrevieri neclare.



\---



\# 3. Repository Names



Format:



```

ESTINVEST-Onboarding



ESTINVEST-BackOffice



ESTtrade



ESTINVEST-Ecosystem

```



\---



\# 4. Folder Names



Format:



```

PascalCase

```



Exemple:



```

Business



Security



Database



Architecture



Deployment

```



Pentru structurile numerotate:



```

00\_Project\_Foundation



01\_Business\_Architecture



02\_Security

```



\---



\# 5. Document Names



Format:



```

PREFIX-NNN\_Document\_Name.md

```



Exemple:



```

APP-001\_Application\_Overview.md



STD-SEC-001\_Security\_Standard.md



ADR-003\_API\_Versioning.md



ECO-004\_Integration\_Architecture.md

```



\---



\# 6. Source Code



\## Classes



PascalCase



```

CustomerService



OrderController



TradeRepository

```



\---



\## Interfaces



PascalCase



```

CustomerDto



TradeResponse



LoginRequest

```



\---



\## Methods



camelCase



```

createCustomer()



findById()



calculatePortfolio()

```



\---



\## Variables



camelCase



```

customerId



accountNumber



riskProfile

```



\---



\## Constants



UPPER\_SNAKE\_CASE



```

MAX\_LOGIN\_ATTEMPTS



DEFAULT\_TIMEOUT

```



\---



\# 7. Database



\## Tables



snake\_case



```

customer



customer\_account



trade\_order

```



\---



\## Columns



snake\_case



```

customer\_id



created\_at



updated\_at

```



\---



\## Primary Key



```

id

```



\---



\## Foreign Key



```

customer\_id



account\_id



portfolio\_id

```



\---



\# 8. API



REST resources:



```

/customers



/accounts



/orders



/trades

```



JSON:



camelCase



```

firstName



lastName



customerId

```



\---



\# 9. Git



Branches



```

feature/customer-search



fix/jwt-validation



release/v1.0.0



hotfix/login

```



\---



\# 10. Docker



Images



```

estinvest-backoffice



estinvest-onboarding

```



Containers



```

backoffice-api



postgres-db



gateway-simulator

```



\---



\# 11. Environment Variables



UPPER\_SNAKE\_CASE



```

DATABASE\_URL



JWT\_SECRET



API\_PORT



SMTP\_HOST

```



\---



\# 12. Files



Preferințe:



```

README.md



CHANGELOG.md



LICENSE



docker-compose.yml



.env.example

```



\---



\# 13. Exceptions



Orice abatere de la acest standard trebuie documentată prin ADR.



\---



\# 14. Related Documents



\- STD-COD-001 – Coding Standard

\- STD-DB-001 – Database Design Standard

\- STD-API-001 – API Design Standard

\- STD-GIT-001 – Git Workflow Standard



\---



\# Version History



| Versiune | Data | Autor | Modificări |

|----------|------|-------|------------|

| 1.0 | 2026-07-07 | ESTINVEST \& ChatGPT | Prima versiune |

