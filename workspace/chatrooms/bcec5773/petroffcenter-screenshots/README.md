# PetroffCenter Screenshots

## Folder Structure

| Folder | Locale | Status |
|--------|--------|--------|
| `bg/`  | Bulgarian (`bg-BG`) | ✅ 32 screenshots + BG metadata in `bg/metadata/` |
| `en/`  | English (`en-US`)   | ✅ 20 curated screenshots + EN metadata in `en/metadata/` |

---

## Handoff for Userdocy and Techdocy

- **Old `en/` screenshots were invalid** because the app kept an `Accept-Language=bg-BG` cookie, so the UI still rendered in Bulgarian.
- **Use the curated `en/` set only.** Do **not** mix the Bulgarian captures from `bg/` into English docs.
- **Regeneration fix:** `explore-petroffcenter-en.js` now forces English with both the request header `Accept-Language: en` and the cookie `Accept-Language=en`.
- **Requested removals applied to the EN handoff:** `01`, `10`, `11`, `12`, `13`, `17`, `21`, `22`, `24`, `25`, `26`.
- **Rename applied:** `18` is now **Profile edit**.
- **Automation fix queued in the capture script:** `29` and `31` now wait for a visible dialog before the screenshot is taken on the next rerun.
- **Payments stay out of scope** for docs even if screenshot `08` exists.

---

## BG Screenshots (`bg/`)

### Main Pages

**01 – Dashboard (root `/`)**
![01-Dashboard-root](bg/01-Dashboard-root.png)

**02 – Dashboard**
![02-Dashboard](bg/02-Dashboard.png)

**03 – Contragents Update Requests list**
![03-ContragentsUpdateRequests](bg/03-ContragentsUpdateRequests.png)

**04 – Roles list**
![04-Roles](bg/04-Roles.png)

**05 – Taxes list**
![05-Taxes](bg/05-Taxes.png)

**06 – Tax Configurations list**
![06-TaxConfigurations](bg/06-TaxConfigurations.png)

**07 – System Configurations list**
![07-SystemConfigurations](bg/07-SystemConfigurations.png)

**08 – Payments list** ⚠️ *excluded from docs*
![08-Payments](bg/08-Payments.png)

**09 – Companies list**
![09-Companies](bg/09-Companies.png)

**10 – Invoices list**
![10-Invoices](bg/10-Invoices.png)

**11 – Profile page**
![11-Profile](bg/11-Profile.png)

**12 – Select Contragent**
![12-SelectContragent](bg/12-SelectContragent.png)

**13 – Terms and Conditions**
![13-Terms-and-conditions](bg/13-Terms-and-conditions.png)

### Forms / Dialogs

**14 – Add Role form**
![14-Roles-add-role-form](bg/14-Roles-add-role-form.png)

**15 – Add Tax form**
![15-Taxes-add-tax-form](bg/15-Taxes-add-tax-form.png)

**16 – Add System Configuration form**
![16-System-configuration-add-form](bg/16-System-configuration-add-form.png)

**17 – Add Company form**
![17-Companies-add-company-form](bg/17-Companies-add-company-form.png)

**18 – Profile edit**
![18-Profile-edit](bg/18-Profile-edit.png)

### Detail Pages

**19 – Tax Configuration – Create (`/TaxConfiguration`)**
![19-Tax-configuration-create-page](bg/19-Tax-configuration-create-page.png)

**20 – Tax Configuration – Detail (`/TaxConfiguration/1`)**
![20-Tax-configuration-detail-page](bg/20-Tax-configuration-detail-page.png)

**21 – Company Detail (`/Company/1`)**
![21-Company-detail-page](bg/21-Company-detail-page.png)

**22 – Payment Details** ⚠️ *excluded from docs*
![22-Payment-details-page](bg/22-Payment-details-page.png)

**23 – Contragent Update Request Detail**
![23-Contragent-update-request-detail-page](bg/23-Contragent-update-request-detail-page.png)

**24 – Change Password page**
![24-Change-password-page](bg/24-Change-password-page.png)

**25 – Payment Error page** ⚠️ *excluded from docs*
![25-Payment-error-page](bg/25-Payment-error-page.png)

**26 – Successful Payment page** ⚠️ *excluded from docs*
![26-Successful-payment-page](bg/26-Successful-payment-page.png)

**27 – Canceled Payment page** ⚠️ *excluded from docs*
![27-Canceled-payment-page](bg/27-Canceled-payment-page.png)

### Company Edit Tabs

**28 – Company Edit – Details tab**
![28-Company-details-tab](bg/28-Company-details-tab.png)

**29 – Company Edit – Users (ПОТРЕБИТЕЛИ) tab**
![29-Company-ПОТРЕБИТЕЛИ-tab](bg/29-Company-ПОТРЕБИТЕЛИ-tab.png)

**30 – Company Edit – Users tab – Add User dialog**
![30-Company-ПОТРЕБИТЕЛИ-Добави-потребител](bg/30-Company-ПОТРЕБИТЕЛИ-Добави-потребител.png)

**31 – Company Edit – Devices (УСТРОЙСТВА) tab**
![31-Company-УСТРОЙСТВА-tab](bg/31-Company-УСТРОЙСТВА-tab.png)

**32 – Company Edit – Devices tab – Add Device dialog**
![32-Company-УСТРОЙСТВА-Добави-потребител](bg/32-Company-УСТРОЙСТВА-Добави-потребител.png)

---

## EN Screenshots (`en/`) — ✅ 20 curated screenshots

Re-run with `locale: 'en-US'`, `Accept-Language: en`, and `Accept-Language=en` cookie using the full JWT token (exp 2026-04-21).

> **Note:** The app only switched to English after forcing the `Accept-Language` cookie to `en`; browser locale alone was not enough.

### Main Pages

**02 – Dashboard**
![02-Dashboard](en/02-Dashboard.png)

**03 – Contragents Update Requests list**
![03-ContragentsUpdateRequests](en/03-ContragentsUpdateRequests.png)

**04 – Roles list**
![04-Roles](en/04-Roles.png)

**05 – Taxes list**
![05-Taxes](en/05-Taxes.png)

**06 – Tax Configurations list**
![06-TaxConfigurations](en/06-TaxConfigurations.png)

**07 – System Configurations list**
![07-SystemConfigurations](en/07-SystemConfigurations.png)

**08 – Payments list** ⚠️ *excluded from docs*
![08-Payments](en/08-Payments.png)

**09 – Companies list**
![09-Companies](en/09-Companies.png)

### Forms / Dialogs

**14 – Add Role form**
![14-Roles-add-role-form](en/14-Roles-add-role-form.png)

**15 – Add Tax form**
![15-Taxes-add-tax-form](en/15-Taxes-add-tax-form.png)

**16 – Add System Configuration form**
![16-System-configuration-add-form](en/16-System-configuration-add-form.png)

**18 – Profile edit**
![18-Profile-edit](en/18-Profile-edit.png)

### Detail Pages

**19 – Tax Configuration – Create (`/TaxConfiguration`)**
![19-Tax-configuration-create-page](en/19-Tax-configuration-create-page.png)

**20 – Tax Configuration – Detail `/TaxConfiguration/10`** ⭐ *new*
![20-Tax-configuration-detail-page-10](en/20-Tax-configuration-detail-page-10.png)

**23 – Change Password page**
![23-Change-password-page](en/23-Change-password-page.png)

### Company Edit Tabs

**27 – Company Edit – Details tab**
![27-Company-details-tab](en/27-Company-details-tab.png)

**28 – Company Edit – Users tab** ⭐ *new*
![28-Company-Users-tab](en/28-Company-Users-tab.png)

**29 – Company Edit – Users tab – Add User dialog** ⚠️ *current image was captured too early; rerun after script fix*
![29-Company-Users-add-user-dialog](en/29-Company-Users-add-user-dialog.png)

**30 – Company Edit – Devices tab** ⭐ *new*
![30-Company-Devices-tab](en/30-Company-Devices-tab.png)

**31 – Company Edit – Devices tab – Add Device dialog** ⚠️ *current image was captured too early; rerun after script fix*
![31-Company-Devices-add-device-dialog](en/31-Company-Devices-add-device-dialog.png)

> Payment page `08` is captured but **excluded from documentation**.
