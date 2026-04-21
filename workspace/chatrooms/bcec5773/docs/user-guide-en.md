# PetroffCenter — User Guide

**Version:** 1.0.0.0  
**Vendor:** PetroffSoft Ltd.  
**Support:** support@petroffsoft.bg  

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Dashboard](#dashboard)
3. [Contragent Update Requests](#contragent-update-requests)
4. [Roles](#roles)
5. [Taxes](#taxes)
6. [Tax Configurations](#tax-configurations)
7. [System Configurations](#system-configurations)
8. [Companies](#companies)
9. [Invoices](#invoices)
10. [Profile](#profile)
11. [Change Password](#change-password)
12. [Terms and Conditions](#terms-and-conditions)

---

## Getting Started

PetroffCenter is a web-based management platform by PetroffSoft Ltd. It lets you manage companies, users, roles, taxes, invoices, and system settings from a single interface.

Open your browser and go to the PetroffCenter URL provided by your system administrator. You will need a valid access token or login credentials to sign in.

---

## Dashboard

**Route:** `/Dashboard`

The Dashboard is the home screen after login. It shows navigation cards for every module you have access to:

| Card | What it does |
|---|---|
| **Contragent update requests** | Approve or reject contragent update requests |
| **Roles** | Create, edit, or delete roles and manage their permissions |
| **Taxes** | Create, edit, or delete taxes |
| **Tax configurations** | Create, edit, or delete tax configurations |
| **System Configurations** | Create, edit, or delete system configurations |
| **Payments** | View previous and current payments |
| **Companies** | Manage registered companies |

Click any card to navigate to that module.

---

## Contragent Update Requests

**Route:** `/ContragentsUpdateRequests`

This page lists all requests submitted to update contragent (counterparty) data.

### List columns

| Column | Description |
|---|---|
| Name | Name of the contragent |
| Request status | Current status of the request |
| Date | Date the request was created |
| Creator user name | Username of the person who submitted the request |
| Contragent requester | Company that submitted the request |
| Processing date | Date the request was processed |

### Filtering

Use the **Contragent** and **Request status** filter chips at the top of the table to narrow the list. Click **×** on a filter chip to remove it. Click **Search** to apply filters.

### Viewing a request

Click a row to open the detail view for that request (`/ContragentsUpdateRequests/{id}`).

---

## Roles

**Route:** `/Roles`

Roles define what actions a user can perform in PetroffCenter. Each role has a name, a description, and a set of permission flags.

### Roles list

The table shows **Name**, **Permission** (summary), and **Description** for each role. Use the **Search** box to filter by any column. Click column headers to sort.

### Adding a role

1. Click **Add role**.
2. Fill in the form:

   | Field | Required | Notes |
   |---|---|---|
   | Name | ✅ | Display name for the role |
   | Description | ❌ | Free-text description |
   | Permissions | ❌ | Check each permission the role should have (see below) |

3. Click **Add** to save, or **Cancel** to discard.

### Available permissions

| Permission | What it grants |
|---|---|
| Main access | Basic access to the platform |
| Access code | Access via PIN/code |
| Create contragent update requests | Submit requests to update contragent data |
| Make payments | Initiate payment transactions |
| Access invoices | View the invoice list |
| Administrator | Full administrative access |
| Sys admin | System administration capabilities |
| Reverses orders in Friday via external systems | Reverse Friday POS orders through external system integration |
| Creates accounts in Capella via external systems | Create Capella accounting records through external system integration |

---

## Taxes

**Route:** `/Taxes`

Taxes are the individual tax rates used across the system (e.g., 20% VAT).

### Taxes list

The table shows **Name**, **Tax type**, **Value**, and **Accounting code**. Use **Search** to filter. Pagination controls are at the bottom.

### Adding a tax

1. Click **Add tax**.
2. Fill in the form:

   | Field | Required | Notes |
   |---|---|---|
   | Name | ✅ | e.g. "20%" |
   | Tax type | ✅ | Select from dropdown (e.g. "Add percent") |
   | Value | ✅ | Numeric value, e.g. `20` |
   | Accounting code | ❌ | Code used in your accounting software |

3. Click **Save** to create, or **Cancel** to discard.

---

## Tax Configurations

**Route:** `/TaxConfigurations`

Tax configurations group one or more taxes together with additional fiscal settings. They are typically assigned to products or services.

### Configurations list

Shows all saved configurations as cards. Each card shows the configuration name. Click a card to open and edit it. Click **Add tax configuration** to create a new one.

### Creating or editing a tax configuration

**Route:** `/TaxConfiguration` (new) or `/TaxConfiguration/{id}` (edit)

| Field | Required | Notes |
|---|---|---|
| Name | ✅ | e.g. "20% ДДС" |
| Tag | ❌ | Format: `Value1#Value2#Value3` (e.g. `#1#2#3`) |
| Fiscal Group | ❌ | Select from dropdown (e.g. "Group 2") |

After setting the header fields, use the **Add tax** button to add individual tax entries to the configuration. The tax items table shows: Tax, Priority, Tax type, Value, Accounting code.

Click **Save** to save the configuration.

---

## System Configurations

**Route:** `/SystemConfigurations`

System configurations are key-value pairs that control application behavior, integrations, and service endpoints.

> ⚠️ This section is typically managed by system administrators only.

### Configurations list

The table shows **Key** and **Value** for each entry. Use pagination to browse.

### Adding a system configuration

1. Click **Add system configuration**.
2. Enter a **Key** and a **Value**.
3. Click **Save** to apply, or **Cancel** to discard.

---

## Companies

**Route:** `/Companies`

Companies are the top-level organisational units in PetroffCenter. Each company can have its own users and devices.

### Companies list

Companies are shown as cards with **Name**, **Bulstat**, and other details. Click a company card to open it. Click **Add company** to create a new one.

### Company details (Details tab)

**Route:** `/Company/{id}`

Fill in the company's information:

| Field | Required | Notes |
|---|---|---|
| Name | ✅ | Company display name |
| Bulstat | ✅ | Bulgarian company registration number |
| VAT registration number | ❌ | e.g. `BG203751602` |
| Country | ✅ | Default: BULGARIA |
| Time zone | ✅ | Default: Europe/Sofia |
| Address | ❌ | Physical address |
| Materially responsible person (МОЛ) | ❌ | Person legally responsible for assets |
| Bank | ❌ | Bank name |
| IBAN | ❌ | Bank account IBAN |
| BIC | ❌ | Bank SWIFT/BIC code |
| Phone number | ❌ | Company phone |
| Email | ❌ | Company email |
| Photo | ❌ | Upload a company logo |

Click **Save** to apply changes or **Cancel** to discard. Click **Clear** to reset individual fields.

### Company Users tab

Click the **USERS** tab on the company page.

The table shows: **Name**, **Username**, **Email**, **Employee position**, **POS Number**, **Roles**.

To add a user to the company:

1. Click **Add user**.
2. Fill in the user details:

   | Field | Notes |
   |---|---|
   | Name | Full name |
   | Username | Login username |
   | Email | Email address |
   | Employee position | Job title or role within the company |
   | POS Number | POS terminal identifier |
   | Roles | Assign one or more roles to the user |

3. Confirm to save.

### Company Devices tab

Click the **DEVICES** tab on the company page.

The table shows all registered devices (**Description**). To register a new device:

1. Click **Add this device**.
2. Enter a **Description** for the device.
3. Confirm to save.

---

## Invoices

**Route:** `/Invoices`

The invoices page lists all invoices. The table shows: **№**, **Name**, **Amount**, **Note**, **Date**, **Sent invoice**.

Use the **Sent invoice**, **Recipient name**, **Date**, and **Created on** filters to narrow the list. Use the **Search** box for full-text search. Pagination controls are at the bottom.

---

## Profile

**Route:** `/Profile`

The Profile page lets you view and update your personal account information.

| Field | Notes |
|---|---|
| Name | Your display name |
| Username | Your login username |
| Phone number | Your contact phone |
| Email | Read-only — cannot be changed here |
| Gender | Select from dropdown |
| Accounting user code | Your code in the connected accounting software |
| POS Number | Your POS terminal identifier |
| Access code | Your PIN/access code for POS |
| Additional info | Free-text notes |
| Photo | Upload a profile picture |

Click **Save** to apply changes.

To change your password, click **Change password** — this will navigate you to the Change Password page.

---

## Change Password

**Route:** `/ChangePassword`

Use this page to update your account password.

| Field | Notes |
|---|---|
| Old password | Your current password |
| New password | The new password you want to set |
| Repeat new password | Type the new password again to confirm |

Click **Change password** to apply. Click **Clear** to reset the form.

---

## Terms and Conditions

**Route:** `/Terms-and-conditions`

The full text of PetroffSoft's Terms of Service.

**Version:** 3.0  
**Effective date:** 01.03.2026

A link to the Terms and conditions is available in the footer of every page. A link to the **Payment, Cancellation, and Refund Policy** is also available on the Payments page.

---

*PetroffCenter 1.0.0.0 — Copyright © PetroffSoft Ltd.*
