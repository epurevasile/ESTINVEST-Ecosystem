\# Architecture Audit Notes



| Proiect | ESTINVEST Ecosystem |

|----------|---------------------|

| Scop | Decizii temporare rezultate din auditul de arhitectură |

| Status | Working Document |



\---



\# Audit 01 – Business Architecture



\## BA-001 – Rolul Onboarding



Status: Aprobat



Decizie:



Onboarding este aplicația responsabilă pentru inițierea relației contractuale cu clientul.



Nu este proprietarul Master Data.



La aprobarea clientului, responsabilitatea este transferată către BackOffice.



Impact:



\- APP-001\_ESTINVEST\_Onboarding

\- SYS-002\_Logical\_Architecture



\---



\## BA-002 – Actualizarea datelor clientului



Status: Aprobat



Decizie:



După activarea clientului, modificările sunt inițiate prin Onboarding, dar sunt validate și aplicate exclusiv în BackOffice.



Onboarding nu modifică direct Master Data.



Impact:



\- APP-001\_ESTINVEST\_Onboarding

\- DATA-001\_Data\_Architecture\_Principles

\- SYS-002\_Logical\_Architecture



\---



\## BA-003 – Generarea parolei inițiale



Status: Aprobat



Decizie:



După activarea clientului:



\- ESTtrade creează contul de acces.

\- ESTtrade generează parola temporară.

\- ESTtrade transmite parola temporară către Onboarding prin API securizat.

\- Onboarding transmite e-mailul de activare către client.

\- După prima autentificare, parolele sunt administrate exclusiv de ESTtrade.



Impact:



\- APP-001\_ESTINVEST\_Onboarding

\- APP-002\_ESTtrade\_Family

\- SEC-001\_IAM



\---



\## BA-004 – Introducerea ordinelor



Status: Aprobat



Decizie:



Brokerii din sucursale și agenții introduc ordine exclusiv prin interfața internă ESTtrade.



Brokerii și traderii din sediul central, conectați în rețeaua internă, pot introduce ordine atât prin ESTtrade, cât și prin BackOffice.



BackOffice nu reprezintă un canal public de tranzacționare.



Impact:



\- APP-002\_ESTtrade\_Family

\- ADR-006\_Trading\_Channel\_Architecture

\- DM-006\_Order\_Data\_Model



\---

