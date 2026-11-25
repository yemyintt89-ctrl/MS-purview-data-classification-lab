# Microsoft Purview Data Protection Lab for small and medium NGOs

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

## Lab architecture

```text
Microsoft Entra ID Group
├── A-Purview-Pilot-Users
├── A-MEAL-Team
├── A-Finance-Team
├── A-Child-Protection
└── A-Data-Protection
            |
            v
SharePoint Online
├── Publications libaray
├── Internal Operations library
├── Program Confidential library
├── Finance Confidential library
└── Child Protection Restricted library
            |
            v
Microsoft Purview Information Protection
├── INGO - Public
├── INGO - Internal
├── INGO - Confidential
└── INGO - Highly Confidential
```

# Core design principle

```text
SharePoint permissions (Protect the storage location)
              +
Sensitivity-label encryption (Protects supported files after download or movement)
```

Both controls are required. A restricted SharePoint library alone does not provide persistent file protection after an authorized user downloads a document.

## Data Classification model

| Classification | Typical information | Protection intent |
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

Do not use the Global Administrator account for Purview configurations.

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

<img width="416" height="449" alt="image" src="https://github.com/user-attachments/assets/5c645ca3-4eb2-41fd-b21e-c46ff94fe63f" />


## Step 4: Create the security groups

Open:

```text
Microsoft Entra admin center
→ Identity
→ Groups
→ All groups
→ New group
```

Use:

```text
Group type: Security
Membership type: Assigned
```

Create these groups:

| Security group | Members | Purpose |
|---|---|---|
| `A-Purview-Pilot-Users` | All five pilot users | Receives the pilot sensitivity-label policy |
| `A-MEAL-Team` | MEAL Manager and MEAL Officer | Access to confidential program and MEAL information |
| `A-Child-Protection` | Child Protection Manager | Access to highly confidential child-protection information |
| `A-Finance-Team` | Finance Manager | Access to finance and donor information |
| `A-Data-Protection` | Purview Lab Administrator | Data-protection review and incident investigation |

<img width="802" height="369" alt="image" src="https://github.com/user-attachments/assets/1e6e6862-55cd-455b-9150-f948e5585b8b" />


These groups will later control who can open files labeled **Confidential** and **Highly Confidential**.

## Step 5: Create the SharePoint demo site

Open:

```text
SharePoint admin center
→ Active sites
→ Create
→ Team site
```

Configure:

```text
Site name: INGO Data Protection Demo
Site address: /sites/INGODataProtectionDemo
Privacy: Private
Owner: Purview Lab Administrator
External sharing: Disabled during the initial pilot
```
<img width="514" height="467" alt="image" src="https://github.com/user-attachments/assets/4e99d41a-04f1-478f-9279-6a02682d1766" />
<img width="277" height="295" alt="image" src="https://github.com/user-attachments/assets/38f24b7f-0327-4316-b825-fa4b45ce08b9" />
<img width="266" height="353" alt="image" src="https://github.com/user-attachments/assets/9f46ef09-6b32-4277-af20-376d83673ff3" />

## Step 6: Create the document libraries

Create:

```text
01-Publications
02-Internal-Operations
03-Program-Confidential
04-Finance-Confidential
05-Child-Protection-Restricted
```

<img width="880" height="383" alt="image" src="https://github.com/user-attachments/assets/4b8bcf19-0fe6-4f3c-9030-b67cac2b2381" />

### Recommended access model

| Library | Authorized access Group |
|---|---|
| `01-Publications` | `A-Purview-Pilot-Users` |
| `02-Internal-Operations` | `A-Purview-Pilot-Users` |
| `03-Program-Confidential` | `A-MEAL-Team` |
| `04-Finance-Confidential` | `A-Finance-Team` |
| `05-Child-Protection-Restricted` | `A-Child-Protection` and `A-Data-Protection` |

```text
01-Publications Access Group
```
<img width="766" height="425" alt="image" src="https://github.com/user-attachments/assets/f3456fd1-655a-49b0-8bd4-fbd04d89ca59" />

<img width="782" height="421" alt="image" src="https://github.com/user-attachments/assets/b51c8132-f60d-4480-b6dd-5fbb8b149cfd" />

```text
02-Internal-Operations Access Group
```
<img width="779" height="428" alt="image" src="https://github.com/user-attachments/assets/1e322aec-663d-4a45-ae52-596374e2551c" />
<img width="750" height="403" alt="image" src="https://github.com/user-attachments/assets/e1ab484e-edbc-4f73-b873-da426c7c84bd" />

For Confidential libraries, stop permission inheritance and remove the general pilot group before granting the relevant departmental group access.

```text
03-Program-Confidential Access Group
```
<img width="763" height="433" alt="image" src="https://github.com/user-attachments/assets/ad886b81-b7de-46bb-8b49-c53ceea21e13" />
<img width="699" height="325" alt="image" src="https://github.com/user-attachments/assets/d7906d72-5eed-4919-838d-b1680557a5af" />

```text
04-Finance-Confidential Access Group
```
<img width="757" height="429" alt="image" src="https://github.com/user-attachments/assets/c633a40a-9704-4aaf-a282-87c9dd5c05f9" />
<img width="743" height="380" alt="image" src="https://github.com/user-attachments/assets/131c278f-5be8-4d3d-a11f-ad715e5a1f3e" />

```text
05-Child-Protection-Restricted Access Group
```
<img width="766" height="428" alt="image" src="https://github.com/user-attachments/assets/bc2af734-12fc-42c0-a891-2e4f0932458a" />
<img width="723" height="353" alt="image" src="https://github.com/user-attachments/assets/c1f41d1f-d227-4b1b-9a01-d7b5ef23bf85" />

## Step 7: Create synthetic test files

Create the following demonstration files and upload in respective libraries: 

| File | SharePoint library | Future label |
|---|---|---|
| `Annual Impact Report 2026.docx` | `01-Publications` | Public |
| `Staff Travel Procedure.docx` | `02-Internal-Operations` | Internal |
| `Household Monitoring Dataset.docx` | `03-Program-Confidential` | Confidential |
| `Approved Budget.xlsx` | `04-Finance-Confidential` | Confidential |
| `Child Protection Case Summary.docx` | `05-Child-Protection-Restricted` | Highly Confidential |

<img width="587" height="258" alt="image" src="https://github.com/user-attachments/assets/dc64be81-34f0-4fd4-9b4d-37565b314e94" />
<img width="485" height="253" alt="image" src="https://github.com/user-attachments/assets/22c8c8da-7072-4999-a8f3-5f7502f81cf1" />
<img width="777" height="374" alt="image" src="https://github.com/user-attachments/assets/f0c64ff7-a973-48ee-8288-89bb0ab1d9f4" />
<img width="479" height="254" alt="image" src="https://github.com/user-attachments/assets/836b0e82-9cc5-40f8-b9df-b2348bac0e12" />
<img width="477" height="253" alt="image" src="https://github.com/user-attachments/assets/847f0878-f734-4a49-8d26-1e20f2a3bb58" />

## Step 8: Validation tests

### Field Officer
Expected access:
```text
01-Publications: Allowed
02-Internal-Operations: Allowed
03-Program-Confidential: Denied
04-Finance-Confidential: Denied
05-Child-Protection-Restricted: Denied
```

### MEAL Officer
Expected access:
```text
01-Publications: Allowed
02-Internal-Operations: Allowed
03-Program-Confidential: Allowed
04-Finance-Confidential: Denied
05-Child-Protection-Restricted: Denied
```

### Finance Manager
Expected access:
```text
01-Publications: Allowed
02-Internal-Operations: Allowed
03-Program-Confidential: Denied
04-Finance-Confidential: Allowed
05-Child-Protection-Restricted: Denied
```

### Child Protection Manager
Expected access:
```text
01-Publications: Allowed
02-Internal-Operations: Allowed
03-Program-Confidential: Denied
04-Finance-Confidential: Denied
05-Child-Protection-Restricted: Allowed
```
## Step 9: Create the sensitivity labels in Microsoft Purview

Navigate to:
```text
Microsoft Purview portal
→ Solutions
→ Information Protection
→ Sensitivity labels
```
### INGO - Public

| Setting | Configuration |
|---|---|
| Display name | INGO - Public |
| Color | Green |
| Description | Approved for public release. Confirm management or Communications approval. |
| Scope | Files and other data assets; Emails |
| Encryption | None |
| Content marking | Footer: `CRCD Myanmar | PUBLIC` |

### INGO - Internal

| Setting | Configuration |
|---|---|
| Display name | INGO - Internal |
| Color | Blue |
| Description | For staff and approved internal collaboration. External sharing requires approval. |
| Scope | Files and other data assets; Emails |
| Encryption | None during the pilot |
| Content marking | Footer: `CRCD Myanmar | INTERNAL USE ONLY` |

Do not encrypt every Internal document. Excessive encryption can disrupt collaboration, search, automation, and partner workflows.

### INGO - Confidential

| Setting | Configuration |
|---|---|
| Display name | INGO - Confidential |
| Color | Orange |
| Priority | 2 |
| Description | Contains personal, program, financial, donor or HR information. Share only with authorized staff and approved recipients. |
| Scope | Files and other data assets; Emails |
| Items | Control Access | Apply content marking |
| Encryption | Assign permissions to `MEAL-Manager`, `MEAL-Officer`, `Finance-Manager`, `Purview-Admin` |
| Content marking | Header: `CONFIDENTIAL`; footer: `CRCD Myanmar | AUTHORIZED USE ONLY`; watermark: `CONFIDENTIAL` |
| Offline access | Recommended: Never |
<img width="309" height="357" alt="image" src="https://github.com/user-attachments/assets/3fd992d8-e71d-478b-92aa-596985ea5236" />

Authorized users should retain normal operational editing rights during the pilot. 

### INGO - Highly Confidential

| Setting | Configuration |
|---|---|
| Display name | INGO - Highly Confidential |
| Color | Red |
| Description | Strictly restricted information that could cause serious harm if disclosed. |
| Scope | Files and other data assets; Emails |
| Items | Control Access | Apply content marking |
| Encryption | Assign permissions only to `CP Manager` and `Purview-Admin` |
| Content marking | Header: `HIGHLY CONFIDENTIAL`; footer: `CRCD Myanmar | RESTRICTED ACCESS`; watermark: `HIGHLY CONFIDENTIAL` |
| Offline access | Recommended: Never|

<img width="308" height="359" alt="image" src="https://github.com/user-attachments/assets/e57437b6-45e1-4e23-85e0-ec1166103d01" />

Do not authorize all employees or the complete pilot group.

## Step 10: Verify label priority

Order the labels from lowest to highest sensitivity:

| Priority | Label | Meaning |
|---|---|---|
| Lowest | INGO - Public | Approved for public use |
| 1 | INGO - Internal | Routine internal information |
| 2 | INGO - Confidential | Controlled operational information |
| 3 | INGO - Highly Confidential | Strictly restricted information |

<img width="768" height="401" alt="image" src="https://github.com/user-attachments/assets/c73759ef-00e7-407a-a45d-8ed39dab8e09" />
The most restrictive label must have the highest priority.

## Step 11: Publish the labels

Create a publishing policy with these settings:

```text
Policy name: POL-A-Data Classified-Purview
Publish to users and group: A-Purview-Pilot-Users only
Default document label: None
Default email label: None
Require users to apply a label: Enabled
Justification to remove a label or lower its justification : Enabled
```
<img width="699" height="417" alt="image" src="https://github.com/user-attachments/assets/77ed6237-a33e-41b1-974c-b8d7187eb36d" />
<b>Label and policy changes can take several hours and, in some cases, up to 24 hours to appear. Sign out and reopen Office applications before troubleshooting. Do not repeatedly recreate labels.</b>

## Step 12: Run manual-label tests
### Public document test
1. Sign in as the MEAL Manager.
2. Open `Annual Impact Report 2026.docx`.
3. Apply **Public**.
4. Confirm the Public footer appears.
   <img width="953" height="398" alt="image" src="https://github.com/user-attachments/assets/8ae466ee-86c2-4d4b-879f-c1bb06344c33" />
6. Save, close, and reopen the file.
7. Download the file and confirm the label remains.
<img width="547" height="407" alt="image" src="https://github.com/user-attachments/assets/81effd88-0104-446e-befe-6622fb2c09c6" />
<b>Expected result: Public footer and no encryption.</b>

### Internal document test
1. Open `Staff Travel Procedure.docx`.
2. Apply **Internal**.
3. Confirm the Internal footer appears.
4. Verify that pilot users can continue editing the file.
<img width="950" height="451" alt="image" src="https://github.com/user-attachments/assets/e83bd715-70ae-4690-85ac-6869c7cad7d3" />
<b>Expected result: Internal marking and normal pilot-user collaboration.</b>

### Confidential document test
1. Sign in as the MEAL Officer.
2. Open `Household Monitoring Dataset.docx` in `03-Program-Confidential`.
3. Apply **Confidential**.
4. Save and reopen it.
5. Confirm that the authorized MEAL users can open it.
   
<img width="931" height="107" alt="image" src="https://github.com/user-attachments/assets/e6400ab5-82f9-4d45-b687-669478a5de4f" />
<img width="917" height="109" alt="image" src="https://github.com/user-attachments/assets/731ff620-6585-4fc0-806d-ecbcc997a4d5" />

7. Download it and confirm the label and encryption remain.

Expected result: Confidential marking, persistent encryption, and authorized MEAL access.

### Highly Confidential document test

1. Sign in as the Child Protection Manager.
2. Open `Child Protection Case Summary.docx`.
3. Apply **Highly Confidential**.
4. Confirm its header, footer, or watermark.
   
<img width="930" height="407" alt="image" src="https://github.com/user-attachments/assets/2e1148a6-9228-455b-a2b5-d5e400901f8f" />

6. Close and reopen it.
7. Download a copy for the file-level access test.

<img width="947" height="525" alt="image" src="https://github.com/user-attachments/assets/d2eb64b9-7684-4974-8849-d93b1f2060db" />

<b>Expected result: Persistent marking and access for approved Child Protection users.</b>

### File-level protection test

1. As the Child Protection Manager, copy the 'Child Protection Case Summary.docx' to a 02-Internal-Operations Library which accessible to the Field Officer.
2. Sign in as the Field Officer.
3. Attempt to open it.
<img width="950" height="553" alt="image" src="https://github.com/user-attachments/assets/5e5ca5c9-bcc0-402d-afc8-324929d3d3ae" />
<b>Expected result: Access is denied or restricted because the Field Officer is not included in the encryption permissions.</b>

## Important limitations

- Microsoft 365 E5 Developer is a test sandbox, not a client production environment.
- Feature availability depends on licenses, service plans, roles, and tenant configuration.
- Label and policy propagation is not immediate.
- Content marking behavior varies between Word, Excel, PowerPoint, Outlook, and PDF.
- Microsoft Purview supports data-protection controls but does not automatically make an organization compliant with every donor, policy, or legal requirement.
- 
## Author

**Ye Myint Thu**  
B.C.Tech | ITIL 





