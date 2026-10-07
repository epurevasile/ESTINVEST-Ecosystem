# SEC-001_IAM

| Proprietate | Valoare |
|-------------|----------|
| Document | SEC-001_IAM |
| Document ID | SEC-001 |
| Titlu | IAM |
| Proiect | ESTINVEST Ecosystem |
| Versiune | 1.0 |
| Status | Approved |
| Domeniu | Enterprise Security |
| Data | Iulie 2026 |

---

# 1. Scop

Acest document definește arhitectura serviciului Identity & Access Management (IAM) utilizat în întregul ecosistem ESTINVEST.

IAM este serviciul central responsabil pentru autentificare, autorizare și administrarea identităților.

Toate aplicațiile ecosistemului utilizează același serviciu IAM.

---

# 2. Viziune

IAM este un serviciu Enterprise.

Nu aparține unei aplicații individuale.

El deservește toate aplicațiile ecosistemului.

---

# 3. Obiective

IAM urmărește:

- autentificare unică;
- autorizare centralizată;
- administrarea utilizatorilor;
- administrarea rolurilor;
- administrarea permisiunilor;
- audit;
- Single Sign-On (SSO);
- suport Multi-Factor Authentication (MFA).

---

# 4. Aplicații consumatoare

Serviciul IAM este utilizat de:

- BackOffice Core;
- ESTtrade;
- Onboarding;
- Mobile App;
- AI Assistant;
- Reporting Services;
- Integration Hub;
- aplicațiile viitoare.

---

# 5. Funcționalități

## Authentication

- Login
- Logout
- Password Reset
- MFA
- Session Management

---

## Authorization

- RBAC
- Permissions
- Resource Access
- Policy Evaluation

---

## Identity Management

- utilizatori;
- grupuri;
- roluri;
- organizații;
- service accounts.

---

## Token Management

- Access Token
- Refresh Token
- Expiration
- Revocation

---

## Audit

- autentificări;
- deconectări;
- schimbări de rol;
- modificări de permisiuni;
- parole resetate.

---

# 6. Model logic

```text
                  IAM

          Authentication
                 │
                 ▼
          Authorization
                 │
                 ▼
          Roles & Permissions
                 │
                 ▼
            Applications
```

---

# 7. Model RBAC

Utilizatorii primesc unul sau mai multe roluri.

Rolurile acordă permisiuni.

Permisiunile controlează accesul la resurse.

```text
User

↓

Role

↓

Permission

↓

Application Resource
```

---

# 8. Principii de securitate

IAM respectă:

- Zero Trust;
- Least Privilege;
- Need to Know;
- Separation of Duties;
- Defense in Depth.

---

# 9. Single Sign-On

Arhitectura permite implementarea unui mecanism SSO.

Utilizatorul se autentifică o singură dată și poate accesa aplicațiile autorizate.

---

# 10. Multi-Factor Authentication

MFA este obligatorie pentru:

- administratori;
- brokeri;
- traderi;
- operatori BackOffice;
- utilizatori privilegiați.

Poate fi activată opțional pentru clienți.

---

# 11. API Security

IAM furnizează:

- JWT;
- OAuth2 (viitor);
- OpenID Connect (viitor);
- API Keys pentru integrare;
- Service Accounts.

---

# 12. Audit

Toate operațiunile IAM sunt auditate.

Se păstrează:

- utilizator;
- aplicație;
- IP;
- dispozitiv (unde este disponibil);
- dată și oră;
- rezultat;
- Correlation ID.

---

# 13. Relația cu celelalte documente

Acest document completează:

- SYS-001_Ecosystem_Architecture;
- DATA-001_Data_Architecture_Principles;
- INT-001_Integration_Hub.

Documentația internă a fiecărei aplicații descrie doar integrarea cu IAM, nu mecanismele de autentificare și autorizare.

---

# 14. Scalabilitate

Modelul permite integrarea viitoare cu:

- Identity Providers externi;
- Active Directory / LDAP;
- Google Workspace;
- Microsoft Entra ID;
- OpenID Connect Providers.

---

# 15. Concluzii

IAM reprezintă serviciul Enterprise pentru administrarea identităților și controlul accesului în ecosistemul ESTINVEST.

Toate aplicațiile utilizează acest serviciu pentru autentificare, autorizare și audit, asigurând o administrare unitară și un nivel ridicat de securitate.
