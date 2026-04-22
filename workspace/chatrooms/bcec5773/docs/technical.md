# PetroffCenter — Technical Documentation

**Application:** PetroffCenter  
**Version:** 1.0.0.0  
**Staging URL:** `https://staging.center.petroffsoft.bg/`  
**Vendor:** PetroffSoft Ltd.  
**Framework:** Blazor WebAssembly (MudBlazor component library)  

---

## Table of Contents

1. [Architecture Overview](#architecture-overview)
2. [Authentication](#authentication)
3. [Application Routes (Page API)](#application-routes-page-api)
4. [Form Data Models](#form-data-models)
5. [Role Permission Flags](#role-permission-flags)
6. [External API Integrations](#external-api-integrations)
7. [Payment System](#payment-system)
8. [Exploration Scripts (Scout Artifacts)](#exploration-scripts-scout-artifacts)
9. [Localization](#localization)

---

## Architecture Overview

PetroffCenter is a Blazor WebAssembly single-page web application. The UI runs client-side in the browser and communicates with an ASP.NET Core API back-end. Navigation follows a client-side SPA routing pattern.

**UI Framework:** MudBlazor (Material Design)  
**Observed component selectors:** `.mud-input-control`, `.mud-dialog`, `.mud-table`, `[role=tab]`  
**Viewport target:** 1440×1000 (desktop)  

---

## Authentication

Authentication is token-based. During staging exploration, authenticated requests were observed to rely on browser-managed session state. This is an environment-specific observation only and should not be interpreted as implementation guidance or a recommended security configuration.

For authoritative implementation details, refer to the deployed environment configuration and server-side authentication settings.

> The staging token observed during Scout's exploration session had an expiry of 2026-04-21.

---

## Application Routes (Page API)

All routes are relative to the base URL `https://staging.center.petroffsoft.bg/`.

### Main Navigation Routes

| Route | EN Title | BG Title | Description |
|---|---|---|---|
| `/` or `/Dashboard` | Dashboard | Табло | Main landing page. Shows navigation cards for all modules. |
| `/ContragentsUpdateRequests` | Contragent update requests | Заявки за промяна на контрагент | List of pending/processed contragent update requests. |
| `/ContragentsUpdateRequests/{id}` | Contragent update request | Заявка за промяна на контрагент | Detail view for a single request. |
| `/Roles` | Roles | Роли | List of all roles and their permissions. |
| `/Taxes` | Taxes | Такси | List of all tax definitions. |
| `/TaxConfigurations` | Tax configurations | Конфигурации на такси | List of all tax configurations. |
| `/TaxConfiguration` | Tax configuration items | Конфигурация на такси | Create a new tax configuration. |
| `/TaxConfiguration/{id}` | Tax configuration items | Конфигурация на такси | Edit an existing tax configuration by ID. |
| `/SystemConfigurations` | System Configurations | Системни Конфигурации | List all system key-value configuration entries. |
| `/Payments` | Payments | Плащания | List of all payments. ⚠️ *Excluded from user-facing docs per Scout handoff.* |
| `/PaymentDetails/{id}` | Payment details | Детайли за плащане | Detail view for a single payment. |
| `/Companies` | Companies | Компании | List of all registered companies. |
| `/Company/0` | New company | Нова компания | Create a new company (Add company form at route `/Company/0`). |
| `/Company/{id}` | Company detail | Детайли за компания | Edit an existing company. Has three tabs: Details, Users, Devices. |
| `/Invoices` | Invoices | Фактури | List of all invoices. |
| `/Profile` | Edit | Редакция | Current user's profile edit page. |
| `/SelectContragent` | — | — | Redirects to `/Dashboard`. Used for contragent context switching. |
| `/ChangePassword` | Change password | Смяна на парола | Dedicated change-password page. |
| `/Terms-and-conditions` | Terms and conditions | Общи условия | Full text of the terms of service (Version 3.0, effective 01.03.2026). |
| `/PaymentError` | Payment error | Грешка при плащане | Shown after a failed payment. |
| `/SuccessfulPayment` | — | — | Redirects to Dashboard after successful payment. |
| `/Canceled` | — | — | Redirects to Dashboard after canceled payment. |

---

## Form Data Models

### Role (`POST /Roles` — Add role dialog)

| Field | Type | Required | Notes |
|---|---|---|---|
| `Name` | `text` | ✅ | Role display name |
| `Description` | `textarea` | ❌ | Free-text description |
| `Permissions` | `checkbox[]` | ❌ | See [Role Permission Flags](#role-permission-flags) |

### Tax (`POST /Taxes` — Add tax dialog)

| Field | Type | Required | Notes |
|---|---|---|---|
| `Name` | `text` | ✅ | Tax display name |
| `Tax type` | `select` (hidden enum) | ✅ | Default: `NONE`. Observed value: `PERCENT` (Add percent) |
| `Value` | `number` | ✅ | Default: `0`. E.g. `20.000` for 20% |
| `Accounting code` | `text` | ❌ | Code used in accounting software integration |

### Tax Configuration (`PUT /TaxConfiguration/{id}` — detail/create page)

| Field | Type | Required | Notes |
|---|---|---|---|
| `Name` | `text` | ✅ | Configuration name (e.g., "20% ДДС") |
| `Tag` | `text` | ❌ | Format: `Value1#Value2#Value3`. Observed value: `#1#2#3` |
| `Fiscal Group` | `select` (hidden enum) | ❌ | Values: `Unspecified`, `TaxGroup2` (Group 2) |
| Tax items table | inline editor | ❌ | Columns: Tax, Priority, Tax type, Value, Accounting code |

### System Configuration (`POST /SystemConfigurations` — Add form dialog)

| Field | Type | Required | Notes |
|---|---|---|---|
| `Key` | `text` | ✅ | Configuration key identifier |
| `Value` | `textarea` | ✅ | Configuration value (can be multi-line) |

**Known system configuration keys observed in staging:**

| Key | Value |
|---|---|
| `CapellaPrintServiceBaseUrlAddress:1` | `http://<print-service-host>:<port>/api/v1/` |
| `CapellaNSSINight...` | *(truncated in capture)* |

### Company (`POST /Company/0` or `PUT /Company/{id}` — Details tab)

| Field | Type | Required | Notes |
|---|---|---|---|
| `Name` | `text` | ✅ | Company display name |
| `Bulstat` | `text` | ✅ | Bulgarian company registration number |
| `VAT registration number` | `text` | ❌ | e.g. `BG203751602` |
| `Country` | `text/select` | ✅ | Default: `BULGARIA` |
| `Time zone` | `text/select` | ✅ | Default: `Europe/Sofia` |
| `Address` | `text` | ❌ | Physical address |
| `МОЛ` (Materially Responsible Person) | `text` | ❌ | BG legal requirement for financial responsibility |
| `Bank` | `text` | ❌ | Bank name |
| `IBAN` | `text` | ❌ | Bank account IBAN |
| `BIC` | `text` | ❌ | Bank SWIFT/BIC code |
| `Phone number` | `text` | ❌ | Contact phone |
| `Email` | `text` | ❌ | Contact email |
| Photo | `file` | ❌ | Company logo / image |

### Company User (`POST /Company/{id}/Users` — Add user dialog)

| Field | Type | Notes |
|---|---|---|
| `Name` | `text` | Full name |
| `Username` | `text` | Login username |
| `Email` | `text` | Email address |
| `Employee position` | `text` | Job title or position |
| `POS Number` | `text` | Point-of-sale terminal identifier |
| `Roles` | `multiselect` | Assign one or more roles |

### Company Device (`POST /Company/{id}/Devices` — Add device dialog)

| Field | Type | Notes |
|---|---|---|
| `Description` | `text` | Device description / identifier |

### Profile Edit (`PUT /Profile`)

| Field | Type | Notes |
|---|---|---|
| `Name` | `text` | Display name |
| `Username` | `text` | Login username |
| `Phone number` | `text` | Contact phone |
| `Email` | `text` | **Disabled** — cannot be changed via this form |
| `Gender` | `select` (hidden enum) | Values: `MALE`, `FEMALE` |
| `Accounting user code` | `text` | Code used in accounting integration |
| `POS Number` | `text` | POS terminal ID |
| `Access code` | `text` | PIN/access code for POS |
| `Additional info` | `textarea` | Free-text notes |
| Photo | `file` | Profile avatar |

### Change Password (`POST /ChangePassword`)

| Field | Type | Notes |
|---|---|---|
| `Old password` | `password` | Current password for verification |
| `New password` | `password` | New password |
| `Repeat new password` | `password` | Confirmation field |

---

## Role Permission Flags

Permissions are boolean checkboxes assigned per role. Observed flags (EN / BG):

| EN Label | BG Label | Notes |
|---|---|---|
| Main access | Основен достъп | Base platform access |
| Access code | Достъп чрез код | Access via PIN/code |
| Create contragent update requests | Създаване на заявка за промяна на контрагент | Submit contragent changes |
| Make payments | Прави плащания | Initiate payment transactions |
| Access invoices | Достъп до фактури | View invoice list |
| Administrator | Администратор | Admin-level access |
| Sys admin | Системен администратор | System administration |
| Reverses orders in Friday via external systems | Сторнира поръчки във Friday чрез външни системи | Integration permission for Friday POS |
| Creates accounts in Capella via external systems | Създава сметки в Capella чрез външни системи | Integration permission for Capella accounting |

---

## External API Integrations

### Capella Print Service

The application connects to a local print service (likely for fiscal receipt printing).

| Property | Value |
|---|---|
| Base URL key | `CapellaPrintServiceBaseUrlAddress:{instance}` |
| Observed endpoint | `http://192.168.0.166:7879/api/v1/` |
| Protocol | HTTP REST (`api/v1/`) |
| Scope | Local network (non-public IP) |

> Instance suffix (`:1`) suggests multiple print service instances may be configured per environment.

### Capella Accounting Integration

Referenced by role permission flags: creating accounts in Capella via external systems. No direct API URL observed in staging configuration.

### Friday POS Integration

Referenced by role permission flag: reversing orders in Friday via external systems. No direct API URL observed in staging configuration.

---

## Payment System

| Property | Value |
|---|---|
| Provider | EPAY (ePay.bg — Bulgarian payment gateway) |
| Payment status enum | `PENDING` (В изчакване) |
| Request status field | `<UNKNOWN>` / `<НЕОПРЕДЕЛЕН>` when unset |
| Transaction type | `EPAY` |
| Success redirect | `/SuccessfulPayment` → redirects to Dashboard |
| Cancel redirect | `/Canceled` → redirects to Dashboard |
| Error page | `/PaymentError` |
| Policy page | Payment, Cancellation, and Refund Policy (linked from Payments page) |

Payments list columns: `№`, `Amount`, `Payment Status`, `Paid on`, `Reason`, `Request status`, `Transaction Type`.

---

## Exploration Scripts (Scout Artifacts)

Scout created three Playwright-based exploration scripts as external handoff artifacts. They are **not versioned in this repository**. All require the env variable `PETROFF_AUTH_TOKEN`.

The script names and output locations below are included for reference only, based on the Scout delivery artifacts.

### `explore-petroffcenter.js`

**Purpose:** Initial BG-locale exploration of all main routes.  
**Artifact output (external to this repo):** `petroffcenter-screenshots/bg/` + `bg/metadata/exploration.json`  
**Locale:** `bg-BG`  
**Routes captured:** `/`, `/Dashboard`, `/ContragentsUpdateRequests`, `/Roles`, `/Taxes`, `/TaxConfigurations`, `/SystemConfigurations`, `/Payments`, `/Companies`, `/Invoices`, `/Profile`, `/SelectContragent`, `/Terms-and-conditions`

### `explore-petroffcenter-en.js`

**Purpose:** Curated EN-locale exploration including forms, dialogs, and company tabs.  
**Artifact output (external to this repo):** `petroffcenter-screenshots/en/` + `en/metadata/exploration.json`  
**Locale:** `en-US` with `Accept-Language: en` header **and** `Accept-Language=en` cookie.  

> **Critical note:** Setting only the browser locale is insufficient. The application reads the `Accept-Language` cookie and overrides locale from it. Both the HTTP header and the cookie must be set to `en` to render the UI in English.

**Routes captured:**

| ID | Route | Label |
|---|---|---|
| 02 | `/Dashboard` | Dashboard |
| 03 | `/ContragentsUpdateRequests` | ContragentsUpdateRequests |
| 04 | `/Roles` | Roles |
| 05 | `/Taxes` | Taxes |
| 06 | `/TaxConfigurations` | TaxConfigurations |
| 07 | `/SystemConfigurations` | SystemConfigurations |
| 08 | `/Payments` | Payments *(excluded from docs)* |
| 09 | `/Companies` | Companies |
| 14 | `/Roles` (after "Add role") | Roles-add-role-form |
| 15 | `/Taxes` (after "Add tax") | Taxes-add-tax-form |
| 16 | `/SystemConfigurations` (after "Add system configuration") | System-configuration-add-form |
| 18 | `/Profile` (after "Change password" click) | Profile-edit |
| 19 | `/TaxConfiguration` | Tax-configuration-create-page |
| 20 | `/TaxConfiguration/10` | Tax-configuration-detail-page-10 |
| 23 | `/ChangePassword` | Change-password-page |
| 27 | `/Company/1` (Details tab) | Company-details-tab |
| 28 | `/Company/1` (Users tab) | Company-Users-tab |
| 29 | `/Company/1` (Add user dialog) | Company-Users-add-user-dialog ⚠️ *image captured before dialog was visible; fix pending* |
| 30 | `/Company/1` (Devices tab) | Company-Devices-tab |
| 31 | `/Company/1` (Add device dialog) | Company-Devices-add-device-dialog ⚠️ *same issue* |

### `explore-petroffcenter-dialogs.js`

**Purpose:** Extended BG-locale capture of forms, dialogs, and detail pages not covered in the initial run.  
**Output:** `petroffcenter-screenshots/bg/` (appends to existing screenshots, IDs 14–32) + `bg/metadata/dialogs.json`  
**Locale:** `bg-BG`  

**Actions captured:**

| Route | Action | Label |
|---|---|---|
| `/Roles` | Click "Добави роля" | Roles add role form |
| `/Taxes` | Click "Добави такса" | Taxes add tax form |
| `/SystemConfigurations` | Click "Добави конфигурация на системата" | System configuration add form |
| `/Companies` | Click "Добави компания" | Companies add company form |
| `/Profile` | Click "Смяна на парола" | Profile edit |
| `/TaxConfiguration` | Direct navigation | Tax configuration create page |
| `/TaxConfiguration/1` | Direct navigation | Tax configuration detail page |
| `/Company/1` | Direct navigation | Company detail page |
| `/PaymentDetails/1` | Direct navigation | Payment details page |
| `/ContragentsUpdateRequests/1` | Direct navigation | Contragent update request detail page |
| `/ChangePassword` | Direct navigation | Change password page |
| `/PaymentError` | Direct navigation | Payment error page |
| `/SuccessfulPayment` | Direct navigation | Successful payment page |
| `/Canceled` | Direct navigation | Canceled payment page |

---

## Localization

The application supports Bulgarian (`bg-BG`) and English (`en-US`).

| Mechanism | Notes |
|---|---|
| `Accept-Language` HTTP header | Sent via `extraHTTPHeaders` in Playwright context |
| `Accept-Language` cookie | Required in addition to the header; takes precedence |
| Browser locale | Set via `locale` in Playwright browser context; insufficient alone |

> English rendering was only reliably achieved after Scout added the `Accept-Language=en` cookie alongside the `Accept-Language: en` header.

---

*Generated from Scout's Playwright exploration of `https://staging.center.petroffsoft.bg/` on 2026-04-21.*
