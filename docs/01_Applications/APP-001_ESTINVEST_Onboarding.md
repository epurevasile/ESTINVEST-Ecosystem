\# APP-001 – ESTINVEST Onboarding



| Proprietate | Valoare |

|-------------|----------|

| Cod document | APP-001 |

| Titlu | ESTINVEST Onboarding |

| Categorie | 01\_Applications |

| Versiune | 1.0 |

| Stare | Draft |

| Autor | Estinvest \& ChatGPT |



\---



\# 1. Scopul aplicației



ESTINVEST Onboarding este aplicația publică destinată deschiderii relației contractuale dintre un client și ESTINVEST.



Aplicația permite:



\- înregistrarea online;

\- colectarea documentelor;

\- identificarea clientului;

\- desfășurarea procesului KYC;

\- aprobarea dosarului;

\- transferul controlat al clientului către BackOffice.



Aplicația NU este platformă de tranzacționare.



Aplicația NU este sistem operațional.



\---



\# 2. Poziția în ecosistem



```text

&#x20;            Client

&#x20;               │

&#x20;               ▼

&#x20;     ESTINVEST Onboarding

&#x20;               │

&#x20;        REST API

&#x20;               │

&#x20;               ▼

&#x20;ESTINVEST BackOffice Core

&#x20;               │

&#x20;               ▼

&#x20;          ESTtrade

```



\---



\# 3. Responsabilități



Aplicația este responsabilă pentru:



\- înregistrarea clientului;

\- colectarea datelor personale;

\- colectarea documentelor;

\- validarea KYC;

\- validarea completitudinii dosarului;

\- aprobarea inițială;

\- generarea dosarului electronic.



\---



\# 4. Ce deține (Owns)



ESTINVEST Onboarding este proprietarul următoarelor informații până la aprobarea clientului:



\- cererea de deschidere cont;

\- datele introduse de client;

\- documentele încărcate;

\- rezultatele verificărilor KYC;

\- istoricul procesului de onboarding;

\- statusurile procesului.



\---



\# 5. Ce consumă (Consumes)



Aplicația poate utiliza:



\- nomenclatoare comune;

\- liste de țări;

\- tipuri de documente;

\- clasificări de risc;

\- liste de sancțiuni (dacă sunt integrate).



\---



\# 6. Ce publică (Publishes)



După aprobarea dosarului, aplicația transmite către BackOffice:



\- datele clientului;

\- documentele;

\- rezultatele verificărilor KYC;

\- profilul clientului;

\- istoricul aprobării.



Transferul se face exclusiv prin API.



\---



\# 7. Ce NU are voie să facă (Must NOT Do)



Aplicația NU:



\- creează conturi de investiții;

\- gestionează portofolii;

\- gestionează solduri;

\- introduce ordine;

\- procesează tranzacții;

\- calculează poziții;

\- administrează Unified Ledger;

\- efectuează settlement.



Aceste responsabilități aparțin exclusiv BackOffice.



\---



\# 8. Date administrate



Principalele categorii de date sunt:



\- persoane fizice;

\- persoane juridice;

\- reprezentanți;

\- beneficiari reali;

\- documente;

\- declarații;

\- informații fiscale;

\- informații KYC;

\- statusuri.



\---



\# 9. Flux operațional



```text

Client



↓



Înregistrare



↓



Completare date



↓



Încărcare documente



↓



Verificări automate



↓



Verificări operator



↓



Aprobare



↓



Transfer către BackOffice

```



\---



\# 10. Integrarea cu BackOffice



Transferul clientului trebuie să fie:



\- atomic;

\- auditat;

\- idempotent;

\- securizat.



BackOffice validează din nou datele înainte de crearea clientului operațional.



\---



\# 11. Baza de date



Implementarea actuală utilizează:



\- MariaDB



Schema bazei de date va fi auditată separat.



\---



\# 12. Arhitectura tehnică



Implementarea actuală:



Backend:

\- JavaScript



Frontend:

\- JavaScript



Bază de date:

\- MariaDB



Această implementare va fi evaluată pentru compatibilitate cu arhitectura ecosistemului.



\---



\# 13. Securitate



Aplicația trebuie să respecte:



\- HTTPS;

\- RBAC pentru utilizatorii interni;

\- Audit Trail;

\- protecția documentelor;

\- jurnalizarea accesului;

\- politici de retenție a datelor.



\---



\# 14. Audit



Va fi efectuat un audit complet privind:



\- arhitectura;

\- codul;

\- baza de date;

\- securitatea;

\- API-urile;

\- performanța;

\- compatibilitatea cu BackOffice.



\---



\# 15. Obiective de evoluție



Pe termen mediu aplicația trebuie să permită:



\- integrare completă cu BackOffice;

\- integrare cu ESTtrade;

\- eliminarea duplicării datelor;

\- API standard REST;

\- audit complet.



\---



\# 16. Relația cu celelalte documente



Acest document completează:



\- ECO-001 – Ecosystem Context

\- ECO-002 – System Landscape

\- ECO-003 – Data Ownership Matrix

\- ECO-004 – Integration Architecture



și va constitui baza auditului aplicației existente.



\---



\# Istoric versiuni



| Versiune | Data | Modificări |

|----------|------|------------|

| 1.0 | Iulie 2026 | Prima versiune |

