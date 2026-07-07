\# ECO-003 – Data Ownership Matrix



| Proprietate | Valoare |

|-------------|----------|

| Cod document | ECO-003 |

| Titlu | Data Ownership Matrix |

| Categorie | 00\_Ecosystem\_Foundation |

| Versiune | 1.0 |

| Stare | Draft |

| Autor | Estinvest \& ChatGPT |



\---



\# 1. Scopul documentului



Acest document definește proprietatea asupra fiecărui tip de informație utilizat în ecosistemul software ESTINVEST.



Pentru fiecare categorie de date sunt definite:



\- aplicația care creează informația;

\- aplicația care o deține oficial (System of Record);

\- aplicațiile care o pot modifica;

\- aplicațiile care o consumă;

\- metoda de integrare.



Acest document elimină ambiguitățile privind responsabilitatea asupra datelor și previne existența mai multor surse de adevăr.



\---



\# 2. Principii generale



Fiecare informație are un singur proprietar.



Doar aplicația proprietară poate modifica informația.



Celelalte aplicații:



\- citesc informația;

\- transmit cereri de modificare;

\- sincronizează datele prin API.



Nu este permis accesul direct la baza de date a altei aplicații.



\---



\# 3. Definiții



\## Creator



Aplicația care generează inițial informația.



\---



\## Owner (System of Record)



Aplicația responsabilă pentru păstrarea versiunii oficiale.



\---



\## Updater



Aplicația autorizată să modifice informația.



\---



\## Consumer



Aplicația care utilizează informația fără a o modifica.



\---



\## Synchronization



Modalitatea prin care informația circulă între aplicații.



\---



\# 4. Data Ownership Matrix



| Informație | Creator | Owner | Modifică | Consumă | Sincronizare |

|------------|---------|--------|----------|----------|--------------|

| Cerere deschidere cont | Onboarding | Onboarding | Onboarding | BackOffice | REST API |

| Documente încărcate | Onboarding | Onboarding | Onboarding | BackOffice | REST API |

| Status KYC | Onboarding | Onboarding | Onboarding | BackOffice | REST API |

| Client activ | BackOffice | BackOffice | BackOffice | ESTtrade | REST API |

| Cont principal | BackOffice | BackOffice | BackOffice | ESTtrade | REST API |

| Solduri cash | BackOffice | BackOffice | Unified Ledger | ESTtrade | REST API |

| Portofolii | BackOffice | BackOffice | BackOffice | ESTtrade | REST API |

| Poziții instrumente financiare | BackOffice | BackOffice | Unified Ledger | ESTtrade | REST API |

| Ordine | ESTtrade / Broker | BackOffice | BackOffice | ESTtrade | REST API |

| Execuții | Gateway BVB | BackOffice | BackOffice | ESTtrade | REST API |

| Settlement | BackOffice | BackOffice | BackOffice | Reporting | REST API |

| Unified Ledger | BackOffice | BackOffice | BackOffice | Reporting | Intern |

| Audit | Toate aplicațiile | Aplicația care generează evenimentul | Nu se modifică | Audit | Intern |

| Utilizatori interni | BackOffice | BackOffice | BackOffice | Toate | REST API |

| Roluri și permisiuni | BackOffice | BackOffice | BackOffice | Toate | REST API |

| Notificări | Notification Service | Notification Service | Notification Service | Toate | API |



\---



\# 5. Fluxul de viață al unui client



```text

Client

&#x20;   │

&#x20;   ▼

ESTINVEST Test

&#x20;   │

&#x20;   │  Creează:

&#x20;   │  - Cerere

&#x20;   │  - Documente

&#x20;   │  - KYC

&#x20;   │

&#x20;   ▼

Client Aprobat

&#x20;   │

&#x20;   ▼

BackOffice

&#x20;   │

&#x20;   │ Creează:

&#x20;   │ - Client

&#x20;   │ - Cont principal

&#x20;   │ - Portofolii

&#x20;   │ - Unified Ledger

&#x20;   │

&#x20;   ▼

ESTtrade

&#x20;   │

&#x20;   │ Consumă:

&#x20;   │ - Client

&#x20;   │ - Portofolii

&#x20;   │ - Solduri

&#x20;   │ - Ordine

```



\---



\# 6. Principii de sincronizare



\## Onboarding → BackOffice



Transferul se realizează numai după aprobarea procesului KYC.



BackOffice validează datele înainte de crearea clientului.



\---



\## BackOffice → ESTtrade



ESTtrade nu modifică informațiile oficiale despre client.



Primește exclusiv date publicate de BackOffice.



\---



\## Gateway BVB → BackOffice



Execuțiile sunt preluate de BackOffice.



BackOffice actualizează Unified Ledger.



\---



\## BackOffice → Reporting



Reporting utilizează exclusiv date validate și reconciliate.



\---



\# 7. Reguli obligatorii



\## DO-001



Fiecare tip de informație are un singur Owner.



\---



\## DO-002



Nu există două aplicații care modifică aceeași informație.



\---



\## DO-003



Aplicațiile comunică exclusiv prin API.



\---



\## DO-004



Nu este permis accesul direct la baza de date a unei alte aplicații.



\---



\## DO-005



Unified Ledger reprezintă sursa oficială pentru toate mișcările financiare.



\---



\## DO-006



ESTtrade nu stochează permanent informații operaționale care aparțin BackOffice-ului.



Cache-ul temporar este permis doar pentru optimizarea performanței și nu devine sursă oficială de date.



\---



\## DO-007



Onboarding nu administrează clienți activi.



După aprobarea clientului și transferul în BackOffice, responsabilitatea operațională este preluată integral de BackOffice.



\---



\# 8. Excepții



Orice excepție privind proprietatea asupra datelor trebuie documentată printr-un Architecture Decision Record (ADR).



\---



\# 9. Relația cu celelalte documente



Acest document completează:



\- ECO-001 – Ecosystem Context

\- ECO-002 – System Landscape



și reprezintă baza pentru:



\- ECO-004 – Integration Architecture

\- APP-001 – Onboarding

\- APP-002 – BackOffice

\- APP-003 – ESTtrade



\---



\# 10. Istoric versiuni



| Versiune | Data | Modificări |

|----------|------|------------|

| 1.0 | Iulie 2026 | Prima versiune |

