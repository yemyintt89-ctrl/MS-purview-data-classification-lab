# Microsoft Purview Data Protection Lab for Local NGOs and INGOs

**Microsoft Purview**, **Microsoft 365**, **SharePoint Online**, and **Microsoft Entra ID**

> **Lab use only:** This project uses fictional staff, synthetic identifiers, and demonstration files. Do not upload real beneficiary, child-protection, safeguarding, employee, health, identity, financial, password, token, or client information.
## Project overview

Humanitarian and development organizations handle information that can cause significant harm if it is disclosed, changed, lost, or shared with the wrong person. This lab demonstrates a practical information-protection model for program, beneficiary, HR, finance, donor, child-protection, and public communications data.

The fictional organization used in this project is:
```text
Organization: Community Resilience and Child Development Myanmar
Short name: CRCD Myanmar
Sector: Local humanitarian and development NGO
Environment: Microsoft 365 E5 Developer sandbox
Demo users: 5
```

The project is designed as a **technical portfolio lab** and a **client demonstration package** for local NGO and INGO environments.

---

## Prerequisites

- Microsoft 365 E5 Developer tenant
- At least six available E5 Developer licenses
- Access to the Microsoft 365 admin center
- Access to the Microsoft Entra admin center
- Access to the Microsoft Purview protal
- Access to the SharePoint admin center
- SharePoint Online and Exchange Online provisioned
- Fictional demo identities and synthetic data

# Core design principle

```text
SharePoint permissions
Protect the storage location
              +
Sensitivity-label encryption
Protects supported files after download or movement
```

Both controls are required. A restricted SharePoint library alone does not provide persistent file protection after an authorized user downloads a document.

## Classification model

| Classification | Typical INGO information | Protection intent |
|---|---|---|
| **Public** | Published reports, vacancies, approved press releases | Visible marking; no encryption |
| **Internal** | Policies, procedures, templates, ordinary meeting notes | Internal marking; no encryption in the pilot |
| **Confidential** | Beneficiary datasets, donor budgets, HR and finance working files | Encryption for approved operational groups |
| **Highly Confidential** | Child protection, safeguarding, health, investigation, identity data | Strict group-based encryption and short offline access |

# Step 1: Confirm the E5 Developer subscription
Open:

```text
Microsoft 365 admin center
→ Log in with global administrator role account
→ Billing
→ Your products
```

Confirm:

```text
Product: Microsoft 365 E5 Developer
Status: Active
Available licenses: At least 6
```
<img width="700" height="424" alt="image" src="https://github.com/user-attachments/assets/bd9491d8-8009-4061-9e82-8eb0676eacc4" />

## Step 2: Create a dedicated Purview administrator

Do not use the Global Administrator account for routine lab work.

Create:

```text
Display name: Purview Lab Administrator
Username: purviewadmin@YOURTENANT.org
```
```text
Assign License: Microsoft 365 E5 Developer license
```
<img width="468" height="472" alt="image" src="https://github.com/user-attachments/assets/8a223513-67c0-4fc8-b69f-580f07420b09" />

```text
Assign Roles: Compliance Administrator role, Information Protection Administrator role
```
| Role | Lab purpose |
|---|---|
| Compliance Administrator | General Microsoft Purview configuration |
| Information Protection Administrator | Create and publish sensitivity labels |
<img width="806" height="300" alt="image" src="https://github.com/user-attachments/assets/f2b2b611-b3c9-4bf1-9e03-b9406d3b66ef" />

## Step 3: Create five pilot users

Use fictional cloud-only accounts:

| Demo role | Demo account |
|---|---|
| MEAL Manager | `mealmanager@YOURTENANT.org` |
| MEAL Officer | `mealofficer@YOURTENANT.org` |
| Child Protection Manager | `cpmanager@YOURTENANT.org` |
| Finance Manager | `financemanager@YOURTENANT.org` |
| Field Officer | `field01@YOURTENANT.org` |

Replace `YOURTENANT` with the actual tenant name.

Assign the Microsoft 365 E5 Developer license to each account.

<img width="643" height="456" alt="image" src="https://github.com/user-attachments/assets/ebc3e561-d44e-4818-a0c1-8893e8627e1e" />



