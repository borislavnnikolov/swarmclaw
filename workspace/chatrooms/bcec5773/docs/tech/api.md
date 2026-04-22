# API / HTTP reference

This document lists the HTTP requests and routes that were explicitly observed during the Playwright-driven exploration used to produce this reference.

> Note: The PetroffCenter web application likely performs additional internal API calls (XHR/fetch) while rendering pages. Those backend endpoints are **not** documented here and were **not logged** by the capture used for this reference.
>
> The sections below **Application Routes** and **Application Data Models** document what Scout's exploration revealed about each page's content, form fields, and known system integrations. This supplements the HTTP capture reference above.

## Base URL

- `baseUrl`: `https://staging.center.petroffsoft.bg/`

All navigations are implemented as `page.goto(new URL(route, baseUrl).toString(), ...)`, which produces an HTTP `GET` to `baseUrl + route`.

## Authentication (request-shaping)

The scripts do not perform an explicit login request. They authenticate by injecting a JWT token into the browser context.

### Environment variable

| Name | Required | Meaning |
|---|---:|---|
| `PETROFF_AUTH_TOKEN` | yes | JWT used as auth for staging access. |

### Cookies set (all scripts)

The following cookies are set for `staging.center.petroffsoft.bg` with the JWT token value:

- `Authorization`, `authorization`
- `token`
- `accessToken`, `AccessToken`
- `PetroffCookie`
- `PETROFF_accessToken`, `PETROFF_accessToken_`

### Headers set (English script)

`explore-petroffcenter-en.js` sets these headers for all requests in its browser context:

| Header | Value |
|---|---|
| `Accept-Language` | `en` |
| `Authorization` | `<PETROFF_AUTH_TOKEN>` |

### Localization cookie (English script)

`explore-petroffcenter-en.js` also sets:

- Cookie `Accept-Language=en`

## Routes visited (HTTP `GET`)

### `explore-petroffcenter.js` (BG main routes)

| Method | Path | Notes |
|---|---|---|
| `GET` | `/` | Dashboard (root). |
| `GET` | `/Dashboard` | Dashboard. |
| `GET` | `/ContragentsUpdateRequests` | Contragents Update Requests list. |
| `GET` | `/Roles` | Roles list. |
| `GET` | `/Taxes` | Taxes list. |
| `GET` | `/TaxConfigurations` | Tax Configurations list. |
| `GET` | `/SystemConfigurations` | System Configurations list. |
| `GET` | `/Payments` | Payments list (captured, excluded from docs by convention). |
| `GET` | `/Companies` | Companies list. |
| `GET` | `/Invoices` | Invoices list. |
| `GET` | `/Profile` | Profile page. |
| `GET` | `/SelectContragent` | Select Contragent page. |
| `GET` | `/Terms-and-conditions` | Terms and Conditions page. |

### `explore-petroffcenter-dialogs.js` (BG forms + detail routes)

| Method | Path | Notes |
|---|---|---|
| `GET` | `/Roles` | Opens “Add role” dialog via UI click, then screenshots. |
| `GET` | `/Taxes` | Opens “Add tax” dialog via UI click, then screenshots. |
| `GET` | `/SystemConfigurations` | Opens “Add system configuration” via UI click, then screenshots. |
| `GET` | `/Companies` | Opens “Add company” form via UI click, then screenshots. |
| `GET` | `/Profile` | Opens “Change password” flow via UI click, then screenshots. |
| `GET` | `/TaxConfiguration` | Tax configuration create page. |
| `GET` | `/TaxConfiguration/1` | Tax configuration detail page (ID `1`). |
| `GET` | `/Company/1` | Company detail page (ID `1`). |
| `GET` | `/PaymentDetails/1` | Payment details page (ID `1`, excluded from docs by convention). |
| `GET` | `/ContragentsUpdateRequests/1` | Contragent update request detail page (ID `1`). |
| `GET` | `/ChangePassword` | Change password page. |
| `GET` | `/PaymentError` | Payment error page (excluded from docs by convention). |
| `GET` | `/SuccessfulPayment` | Successful payment page (excluded from docs by convention). |
| `GET` | `/Canceled` | Canceled payment page (excluded from docs by convention). |

### `explore-petroffcenter-en.js` (EN curated set)

| Method | Path | Notes |
|---|---|---|
| `GET` | `/Dashboard` | Main list page. |
| `GET` | `/ContragentsUpdateRequests` | Main list page. |
| `GET` | `/Roles` | Main list page + opens “Add role” form via UI click. |
| `GET` | `/Taxes` | Main list page + opens “Add tax” form via UI click. |
| `GET` | `/TaxConfigurations` | Main list page. |
| `GET` | `/SystemConfigurations` | Main list page + opens “Add system configuration” form via UI click. |
| `GET` | `/Payments` | Captured but excluded from docs by convention. |
| `GET` | `/Companies` | Main list page. |
| `GET` | `/Profile` | Captures “Profile edit” state via UI click. |
| `GET` | `/TaxConfiguration` | Tax configuration create page. |
| `GET` | `/TaxConfiguration/10` | Tax configuration detail page (ID `10`). |
| `GET` | `/ChangePassword` | Change password page. |
| `GET` | `/Company/1` | Captures “Details” tab, then navigates Users/Devices tabs and captures dialogs. |

## Browser dialogs (Playwright `dialog` events)

All scripts attach `page.on('dialog', ...)` to record (and dismiss) browser-level dialogs (e.g., `alert`, `confirm`). Any captured entries are written to `metadata/exploration.json` under `dialogs`.


---

## Application Routes

All routes listed here are relative to the base URL `https://staging.center.petroffsoft.bg/`.

| Route | EN Title | BG Title | Description |
|---|---|---|---|
| `/` or `/Dashboard` | Dashboard | Табло | Home page. Navigation cards for all modules. |
| `/ContragentsUpdateRequests` | Contragent update requests | Заявки за промяна на контрагент | List of pending/processed contragent update requests. |
| `/ContragentsUpdateRequests/{id}` | — | — | Detail view for a single request. |
| `/Roles` | Roles | Роли | List of roles and permissions. |
| `/Taxes` | Taxes | Такси | List of tax definitions. |
| `/TaxConfigurations` | Tax configurations | Конфигурации на такси | List of tax configurations. |
| `/TaxConfiguration` | Tax configuration items | — | Create new tax configuration. |
| `/TaxConfiguration/{id}` | Tax configuration items | — | Edit existing tax configuration. |
| `/SystemConfigurations` | System Configurations | Системни Конфигурации | Key-value system configuration. |
| `/Payments` | Payments | Плащания | Payments list. ⚠️ excluded from user docs. |
| `/PaymentDetails/{id}` | — | — | Single payment detail. |
| `/Companies` | Companies | Компании | List of companies. |
| `/Company/0` | — | — | Create new company (Add form). |
| `/Company/{id}` | — | — | Edit company. Tabs: Details, Users, Devices. |
| `/Invoices` | Invoices | Фактури | Invoices list. |
| `/Profile` | Edit | Редакция | Current user profile edit. |
| `/SelectContragent` | — | — | Redirects to `/Dashboard`. |
| `/ChangePassword` | Change password | Смяна на парола | Change password page. |
| `/Terms-and-conditions` | Terms and conditions | Общи условия | ToS text. Version 3.0, effective 01.03.2026. |
| `/PaymentError` | — | — | Failed payment error page. |
| `/SuccessfulPayment` | — | — | Redirects to Dashboard after success. |
| `/Canceled` | — | — | Redirects to Dashboard after cancel. |

---

## Application Data Models

### Role

Fields in the Add/Edit Role form:

| Field | Type | Required | Notes |
|---|---|---|---|
| `Name` | text | ✅ | Role display name |
| `Description` | textarea | ❌ | Free-text description |
| `Permissions` | checkbox[] | ❌ | See Role Permission Flags below |

#### Role Permission Flags

| EN Label | BG Label |
|---|---|
| Main access | Основен достъп |
| Access code | Достъп чрез код |
| Create contragent update requests | Създаване на заявка за промяна на контрагент |
| Make payments | Прави плащания |
| Access invoices | Достъп до фактури |
| Administrator | Администратор |
| Sys admin | Системен администратор |
| Reverses orders in Friday via external systems | Сторнира поръчки във Friday чрез външни системи |
| Creates accounts in Capella via external systems | Създава сметки в Capella чрез външни системи |

### Tax

| Field | Type | Required | Notes |
|---|---|---|---|
| `Name` | text | ✅ | Display name |
| `Tax type` | select (enum) | ✅ | Default: `NONE`; observed: `PERCENT` |
| `Value` | number | ✅ | Default: `0` |
| `Accounting code` | text | ❌ | Accounting software code |

### Tax Configuration

| Field | Type | Required | Notes |
|---|---|---|---|
| `Name` | text | ✅ | e.g. "20% ДДС" |
| `Tag` | text | ❌ | Format: `Value1#Value2#Value3` |
| `Fiscal Group` | select (enum) | ❌ | Values: `Unspecified`, `TaxGroup2` |

Tax items sub-table columns: Tax, Priority, Tax type, Value, Accounting code.

### System Configuration

| Field | Type | Required |
|---|---|---|
| `Key` | text | ✅ |
| `Value` | textarea | ✅ |

Known keys in staging:

| Key | Value |
|---|---|
| `CapellaPrintServiceBaseUrlAddress:1` | `http://192.168.0.166:7879/api/v1/` |

### Company

| Field | Type | Required | Notes |
|---|---|---|---|
| `Name` | text | ✅ | |
| `Bulstat` | text | ✅ | Bulgarian company registration number |
| `VAT registration number` | text | ❌ | e.g. `BG203751602` |
| `Country` | text/select | ✅ | Default: `BULGARIA` |
| `Time zone` | text/select | ✅ | Default: `Europe/Sofia` |
| `Address` | text | ❌ | |
| `МОЛ` (Materially Responsible Person) | text | ❌ | Bulgarian legal requirement |
| `Bank` | text | ❌ | |
| `IBAN` | text | ❌ | |
| `BIC` | text | ❌ | |
| `Phone number` | text | ❌ | |
| `Email` | text | ❌ | |
| Photo | file | ❌ | Company logo |

### Company User (Add user dialog)

| Field | Type |
|---|---|
| `Name` | text |
| `Username` | text |
| `Email` | text |
| `Employee position` | text |
| `POS Number` | text |
| `Roles` | multiselect |

### Company Device (Add device dialog)

| Field | Type |
|---|---|
| `Description` | text |

### Profile Edit

| Field | Type | Notes |
|---|---|---|
| `Name` | text | |
| `Username` | text | |
| `Phone number` | text | |
| `Email` | text | **Disabled** — read-only |
| `Gender` | select (enum) | Values: `MALE`, `FEMALE` |
| `Accounting user code` | text | |
| `POS Number` | text | |
| `Access code` | text | PIN for POS access |
| `Additional info` | textarea | |
| Photo | file | Profile avatar |

### Change Password

| Field | Type |
|---|---|
| `Old password` | password |
| `New password` | password |
| `Repeat new password` | password |

---

## External Service Integrations

### Capella Print Service

| Property | Value |
|---|---|
| Config key | `CapellaPrintServiceBaseUrlAddress:{instance}` |
| Observed endpoint | `http://<capella-print-host>:<port>/api/v1/` |
| Protocol | HTTP REST |
| Scope | Local network (non-public IP) |

### Payment System (EPAY)

| Property | Value |
|---|---|
| Provider | EPAY (ePay.bg) |
| Payment status enum | `PENDING` |
| Transaction type | `EPAY` |
| Success redirect | `/SuccessfulPayment` → Dashboard |
| Cancel redirect | `/Canceled` → Dashboard |
| Error page | `/PaymentError` |

### Capella Accounting / Friday POS

Referenced via role permissions. No API URLs discovered in staging configuration.
