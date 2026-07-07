\# STD-API-001 – API Design Standard



| Proprietate | Valoare |

|-------------|----------|

| Document ID | STD-API-001 |

| Titlu | API Design Standard |

| Categorie | API Standards |

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



Acest standard definește regulile pentru proiectarea, implementarea și documentarea API-urilor utilizate în cadrul ESTINVEST Ecosystem.



Obiective:



\- consistență;

\- interoperabilitate;

\- securitate;

\- mentenabilitate;

\- documentare standardizată.



\---



\# 2. API Principles



Toate API-urile trebuie să respecte următoarele principii:



\- API First

\- RESTful Design

\- Stateless Communication

\- Versioning

\- Security by Design

\- Backward Compatibility (unde este posibil)

\- Documentare OpenAPI



\---



\# 3. API Style



Standardul oficial este:



\- REST API

\- JSON

\- HTTPS



\---



\# 4. URI Naming Convention



Se folosesc substantive, nu verbe.



Corect:



```

/customers

/accounts

/orders

/trades

/portfolios

```



Nu:



```

/createCustomer

/getOrders

/deleteTrade

```



\---



\# 5. HTTP Methods



| Metodă | Utilizare |

|----------|-----------|

| GET | Citire |

| POST | Creare |

| PUT | Înlocuire completă |

| PATCH | Actualizare parțială |

| DELETE | Ștergere logică sau fizică |



\---



\# 6. Versioning



Toate API-urile trebuie versiunate.



Exemplu:



```

/api/v1/customers

/api/v1/orders

```



Versiunile majore incompatibile vor utiliza un nou prefix (`v2`, `v3`).



\---



\# 7. Request Format



Datele sunt transmise în format JSON.



Exemplu:



```json

{

&#x20; "firstName": "Ion",

&#x20; "lastName": "Popescu",

&#x20; "email": "ion@example.ro"

}

```



\---



\# 8. Response Format



Răspunsurile trebuie să fie consecvente.



Exemplu:



```json

{

&#x20; "success": true,

&#x20; "data": { },

&#x20; "message": "",

&#x20; "errors": \[]

}

```



\---



\# 9. Error Handling



Codurile HTTP trebuie utilizate corect.



| Cod | Semnificație |

|------|--------------|

| 200 | OK |

| 201 | Created |

| 204 | No Content |

| 400 | Bad Request |

| 401 | Unauthorized |

| 403 | Forbidden |

| 404 | Not Found |

| 409 | Conflict |

| 422 | Validation Error |

| 500 | Internal Server Error |



\---



\# 10. Authentication



Standard:



\- JWT Bearer Token



Header:



```

Authorization: Bearer <token>

```



\---



\# 11. Authorization



Controlul accesului se realizează prin RBAC.



Fiecare endpoint trebuie să definească rolurile autorizate.



\---



\# 12. Pagination



Colecțiile mari trebuie paginate.



Parametri recomandați:



```

?page=1

\&pageSize=50

```



\---



\# 13. Filtering



Exemplu:



```

?status=ACTIVE



?customerId=123



?fromDate=2026-01-01

```



\---



\# 14. Sorting



Exemplu:



```

?sort=createdAt



?sort=-createdAt

```



\---



\# 15. Idempotency



Operațiile PUT și DELETE trebuie să fie idempotente.



Pentru operațiile POST critice se recomandă utilizarea unui \*\*Idempotency Key\*\*.



\---



\# 16. API Documentation



Toate API-urile trebuie documentate folosind OpenAPI (Swagger).



Documentația trebuie să includă:



\- endpoint-uri;

\- parametri;

\- modele de date;

\- exemple de request și response;

\- coduri de eroare.



\---



\# 17. Security



Toate API-urile trebuie să utilizeze:



\- HTTPS;

\- autentificare JWT;

\- validarea datelor;

\- limitarea accesului pe roluri;

\- audit pentru operațiuni critice.



\---



\# 18. Logging



Operațiunile importante trebuie jurnalizate.



Se recomandă logarea:



\- utilizatorului;

\- endpoint-ului;

\- metodei HTTP;

\- rezultatului;

\- duratei execuției.



\---



\# 19. Performance



API-urile trebuie proiectate pentru:



\- timp redus de răspuns;

\- paginare;

\- optimizarea interogărilor;

\- evitarea transferului inutil de date.



\---



\# 20. Related Documents



\- ECO-006 – Technology Stack

\- STD-SEC-001 – Security Standard

\- STD-DB-001 – Database Design Standard

\- APP-005 – API Catalogue



\---



\# Version History



| Versiune | Data | Autor | Modificări |

|----------|------|-------|------------|

| 1.0 | 2026-07-07 | ESTINVEST \& ChatGPT | Prima versiune |

