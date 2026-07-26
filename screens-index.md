# Parish Connect — Screens Index

Design system: **Community Heart v1** (locked — supersedes "Midnight Digital v1"). Source files: `design-tokens.css`, `head-snippet.html`, `style-guide.html`.

Every screen below must:
- Use the exact `head-snippet.html` block in its `<head>`
- Pull components from `style-guide.html` rather than inventing new markup
- Implement empty / loading / populated / error states
- Reflect the Family → Member data model without duplicate entry

| File | Interface | Flow | Status | Notes |
|---|---|---|---|---|
| `style-guide.html` | Shared | Design system reference | ✅ Done (Community Heart) | Not a product screen — component library only |
| `index.html` | Shared | Demo launcher | ✅ Done (Community Heart) | Links to each interface's login |
| `screen-parishioner-login.html` | Parishioner | Login | ✅ Done (Community Heart) | Realistic email/password form, accepts any non-empty input |
| `screen-parish-login.html` | Parish | Login | ✅ Done (Community Heart) | Parish picker (all 130 parishes); non-seeded parishes → empty dashboard, proving data isolation |
| `screen-diocese-login.html` | Diocese | Login | ✅ Done (Community Heart) | Single diocese-admin account; brand panel uses deep blue, not black, per "no dark backgrounds" rule |
| `screen-parishioner-dashboard-home.html` | Parishioner | Dashboard (home) | ✅ Done (Community Heart) | All 4 states, 5-item bottom nav, illustration in Empty state + header, session-aware header/logout; "Request a Document" quick action and Baptism Certificate action-center item now link to the real screens below |
| `screen-parishioner-documents-request.html` | Parishioner | Documents & Certificates → Request Document | ✅ Done | Guided multi-step flow; "Details" step (renamed from Supporting Documents) now shows per-document-type auto-filled fields (from family/member record) plus parishioner-input fields (Godfather/Godmother, Witnesses/Celebrant, Destination, etc.) per the product owner's field list, alongside supporting doc upload. Delivery step: renamed/clarified options, each expands in-place within its own card (not a separate block) to capture email/address/pickup details |
| `screen-parishioner-documents-myrequests.html` | Parishioner | Documents & Certificates → My Requests | ✅ Done | Status list with filter tabs (New/In Progress/Awaiting Action/Completed), expandable Activity Timeline per request using the granular→display status mapping from mock-data-schema.md |
| `screen-parishioner-documents-mydocuments.html` | Parishioner | Documents & Certificates → My Documents | ✅ Done | Completed/delivered documents only, simulated download; empty state if none |
| `screen-parish-requests-hub.html` | Parish | Requests hub → Documents & Certificates | ✅ Done | Mini-workflow checklist per request (Verify → Prepare & upload certificate [captures Certificate Number + Issue Date] → Request Approval [note shown directly, no extra reveal click] → Priest approval → method-specific delivery action). Tabs: All/Pending/Awaiting Approval/Deliver/**Completed** (renamed from Archive). Paid pill now sits in the card header; standalone Verification pill removed (conveyed via the Verify workflow step instead). Detail view shows per-document-type "Certificate details" (auto-filled + parishioner-provided fields). Completed requests show their completion date. One request (Marriage Certificate) demonstrates the Post/Courier delivery flow. **New Request** button opens a simple one-page form (not a wizard) for staff to capture walk-in/offline requests |
| `screen-parish-familyrecords-search.html` | Parish | Family Records | ✅ Done | Rebuilt as sortable/filterable table (click column headers to sort; status + subscription filters; search spans name/head/record no./mobile/email/ward/address/member names) with a right slide-in drawer for detail. Subscription tab computes months-outstanding and total-outstanding from a real "paid through" month, supports flexible-period payment collection (fixed From month, monthly-step To picker, live preview), shows a payment-history list, and confirms status changes after collection. Edit (persistent icon in the drawer header, works from any tab) and Print are both globally accessible. Register Family now asks for husband/wife names, marriage date, and marriage parish. Ward moved into Overview; record number now sits under the family name in the header. All date fields use native date/month pickers |
| `screen-parish-dashboard-home.html` | Parish | Dashboard (home) | ✅ Done (Community Heart) | Sidebar nav, session-aware; non-seeded parish logins render genuine Empty state, proving data isolation |
| `screen-diocese-dashboard-home.html` | Diocese | Dashboard (home) | ✅ Done (Community Heart) | Sidebar nav; follows the locked Diocese Dashboard spec (Snapshot → Action Center → Parish Health → Upcoming → Announcements); Parish Health uses real parishes from the directory |

---

## Mock-Data Foundation

| File | Purpose |
|---|---|
| `mock-data/mock-data-schema.md` | Canonical entity/field/status schema for every module, derived from the feature design briefs |
| `mock-data/mock-seed.json` | Instantiated seed data for the Rodrigues family + one sample record per module |
| `mock-data/parishes.json` | Real 130-parish Bangalore Archdiocese directory |

## Screen Specs (beyond the module briefs)

| File | Purpose |
|---|---|
| `specs/diocese-dashboard-spec.md` | Diocese Dashboard section hierarchy (Snapshot / Action Center / Parish Health / Upcoming / Announcements) |

## Hosting & Linking Conventions

This entire set is built to be deployed to a static web host (Netlify/Vercel/GitHub Pages/etc.) as one flat folder, so it can be demoed on a phone (Parishioner flows) and a laptop (Parish/Diocese flows) from the same hosted URL.

- **`index.html`** is the demo launcher — links to each interface's home dashboard. Open the hosted root URL to get there.
- All screens are flat files (no subfolders) so relative links (`href="screen-parish-dashboard-home.html"`) resolve correctly regardless of host.
- **Build discipline**: a nav item links to a real file (`<a href="...">`) only once that target screen actually exists. Until then it calls `toast('Label')` as a placeholder. When a target screen is built, go back and swap any `toast(...)` calls that were meant to reach it into real `<a href>`/`onclick=location.href=...` links. This avoids broken links on a live hosted demo while still letting us build incrementally.

### Top-level Screen Map (locked filenames)

| Interface | Nav item | Filename | Status |
|---|---|---|---|
| Parishioner | Login | `screen-parishioner-login.html` | ✅ Built |
| Parishioner | Dashboard (home) | `screen-parishioner-dashboard-home.html` | ✅ Built |
| Parishioner | Services (hub) | `screen-parishioner-services-hub.html` | Planned |
| Parishioner | Donate (hub) | `screen-parishioner-donate-hub.html` | Planned |
| Parishioner | Me (hub) | `screen-parishioner-me-hub.html` | Planned |
| Parish | Login | `screen-parish-login.html` | ✅ Built |
| Parish | Dashboard (home) | `screen-parish-dashboard-home.html` | ✅ Built |
| Parish | Family Records | `screen-parish-familyrecords-search.html` | ✅ Built |
| Parish | Committees | `screen-parish-committees-hub.html` | Planned |
| Parish | Requests (hub) | `screen-parish-requests-hub.html` | ✅ Built |
| Parish | Masses | `screen-parish-masses-hub.html` | Planned |
| Parish | Communication | `screen-parish-communication-hub.html` | Planned |
| Parish | Finance | `screen-parish-finance-hub.html` | Planned |
| Parish | Administration | `screen-parish-administration-hub.html` | Planned |
| Parish | Reports | `screen-parish-reports-hub.html` | Planned |
| Parish | Settings | `screen-parish-settings-hub.html` | Planned |
| Diocese | Login | `screen-diocese-login.html` | ✅ Built |
| Diocese | Dashboard (home) | `screen-diocese-dashboard-home.html` | ✅ Built |
| Diocese | Parishes | `screen-diocese-parishes-directory.html` | Planned |
| Diocese | Ministry | `screen-diocese-ministry-hub.html` | Planned |
| Diocese | Communication | `screen-diocese-communication-hub.html` | Planned |
| Diocese | Reports | `screen-diocese-reports-hub.html` | Planned |
| Diocese | Settings | `screen-diocese-settings-hub.html` | Planned |

Deeper flow-specific screens (e.g. within Requests → Documents & Certificates) get added as rows here once we build that stage, following the same `screen-[interface]-[flow]-[name].html` pattern.

### Auth / Session Convention

- No real backend — login screens accept any non-empty credentials (Parish login additionally validates the parish name against the real directory).
- Session context (role, name, parish id/name) is passed forward via **URL query parameters** on redirect (e.g. `screen-parish-dashboard-home.html?role=staff&parish=PAR-0002&parishName=...`), not `localStorage`/`sessionStorage` — keeps every screen a plain static file with no browser-storage dependency, works identically whether opened as a file, hosted, or viewed as a Claude artifact.
- Every post-login screen must read these params and reflect them in its header (name/role/parish), with a way to log out back to the correct login screen.
- **Parish data isolation demo**: the Parish login's parish picker accepts any of the 130 real parishes. Logging in as **Infant Jesus Shrine (`PAR-0002`)** shows the fully populated dashboard/data. Logging in as any other parish should render the genuine **Empty** state everywhere (no families, no requests) — proving parishes never see each other's data, without needing a second fully-seeded dataset.
