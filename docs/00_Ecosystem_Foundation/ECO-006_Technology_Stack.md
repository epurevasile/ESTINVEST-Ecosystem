\# ECO-006 – Technology Stack



| Proprietate | Valoare |

|-------------|----------|

| Document ID | ECO-006 |

| Titlu | Technology Stack |

| Categorie | Ecosystem Foundation |

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



Acest document definește tehnologiile standard aprobate pentru dezvoltarea aplicațiilor din cadrul \*\*ESTINVEST Ecosystem\*\*.



Scopul este:



\- standardizarea dezvoltării;

\- reducerea complexității;

\- simplificarea mentenanței;

\- reutilizarea competențelor și componentelor.



\---



\# 2. Architecture Principles



Tehnologiile sunt selectate conform următoarelor principii:



\- Open Source atunci când este posibil;

\- Enterprise Ready;

\- Scalabile;

\- Ușor de întreținut;

\- Comunitate activă;

\- Documentație completă;

\- Compatibilitate cu AI-assisted development.



\---



\# 3. Backend Technologies



| Componentă | Standard |

|------------|----------|

| Runtime | Node.js LTS |

| Language | TypeScript |

| Framework | NestJS |

| API | REST |

| Validation | class-validator |

| Authentication | JWT |

| Authorization | RBAC |

| Documentation | Swagger / OpenAPI |



\---



\# 4. Frontend Technologies



| Componentă | Standard |

|------------|----------|

| Framework | React |

| Meta Framework | Next.js |

| Language | TypeScript |

| Styling | Tailwind CSS |

| Components | Shadcn UI (unde este potrivit) |

| Charts | Recharts |



\---



\# 5. Database Technologies



| Componentă | Standard |

|------------|----------|

| Primary Database | PostgreSQL |

| Legacy Support | MariaDB |

| ORM | Prisma |

| Migrations | Prisma Migrate |



\---



\# 6. Infrastructure



| Componentă | Standard |

|------------|----------|

| Containers | Docker |

| Reverse Proxy | Caddy |

| Networking | Docker Network |

| Remote Access | Tailscale |

| OS | Ubuntu Server LTS |



\---



\# 7. Development Tools



| Componentă | Standard |

|------------|----------|

| IDE | Cursor |

| Secondary IDE | VS Code |

| Version Control | Git |

| Repository | GitHub |

| API Testing | Bruno |

| Database Client | DBeaver |



\---



\# 8. AI Development Tools



| Componentă | Standard |

|------------|----------|

| Architecture | ChatGPT |

| Code Assistance | Cursor |

| Alternative Code Assistance | Claude |

| Local LLM | Ollama |

| RAG Platform | Open WebUI |



\---



\# 9. Security Technologies



| Componentă | Standard |

|------------|----------|

| Authentication | JWT |

| Password Hashing | bcrypt |

| TLS | HTTPS |

| Secrets | Environment Variables |

| Audit | Audit Log |



\---



\# 10. Integration Technologies



| Componentă | Standard |

|------------|----------|

| Internal APIs | REST |

| External Trading | FIX Protocol |

| Market Connectivity | BVB Gateway |

| Messaging | JSON |

| File Exchange | CSV / XML / PDF |



\---



\# 11. Reporting



Standarde:



\- PDF

\- Excel

\- CSV



\---



\# 12. Logging \& Monitoring



Standarde:



\- Structured Logging

\- Health Checks

\- Audit Logs

\- Performance Monitoring



\---



\# 13. Version Policy



Se utilizează întotdeauna versiuni LTS sau versiuni stabile recomandate.



Actualizarea tehnologiilor se face controlat și documentat.



\---



\# 14. Exceptions



Utilizarea altor tehnologii este permisă numai dacă:



\- există o justificare tehnică;

\- există un ADR aprobat;

\- nu afectează standardizarea ecosistemului.



\---



\# 15. Related Documents



\- ECO-001 – Ecosystem Context

\- ECO-002 – System Landscape

\- ECO-004 – Integration Architecture

\- STD-APP-001 – Application Documentation Standard



\---



\# Version History



| Versiune | Data | Autor | Modificări |

|----------|------|-------|------------|

| 1.0 | 2026-07-07 | ESTINVEST \& ChatGPT | Prima versiune |

