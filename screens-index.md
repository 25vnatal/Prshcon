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
| `screen-parishioner-dashboard-home.html` | Parishioner | Dashboard (home) | ✅ Done (Community Heart) | Reordered for concision: personalized content (attention cards, schedule) now leads; Quick Actions/Announcements/Community moved lower. New **Hero banner** (St. Mary's Basilica Feast) in celebratory gold/coral gradient. "Things needing your attention" redone as two focused cards: **My Requests** (count + preview + View More → My Requests) and **Other Actions** (collapsible — Mass booking payment pending, Feast RSVP), each item flagged with a coral bell/notification icon. A red notification dot appears on the account avatar whenever anything needs action. **Announcements** redone as a "This Week at [Parish]" bulletin list (4 short bullet items) instead of a single long post. **Search** is now functional: overlay with suggestion chips (Churches near me / Mass timings / Parish contact / Novena schedule) returning real canned results from parish data |
| `screen-parishioner-documents-request.html` | Parishioner | Documents & Certificates → Request Document | ✅ Done | Guided multi-step flow; "Details" step (renamed from Supporting Documents) now shows per-document-type auto-filled fields (from family/member record) plus parishioner-input fields (Godfather/Godmother, Witnesses/Celebrant, Destination, etc.) per the product owner's field list, alongside supporting doc upload. Delivery step: renamed/clarified options, each expands in-place within its own card (not a separate block) to capture email/address/pickup details |
| `screen-parishioner-documents-myrequests.html` | Parishioner | Documents & Certificates → My Requests | ✅ Done | Status list with filter tabs (New/In Progress/Awaiting Action/Completed), expandable Activity Timeline per request using the granular→display status mapping from mock-data-schema.md |
| `screen-parishioner-documents-mydocuments.html` | Parishioner | Documents & Certificates → My Documents | ✅ Done | Completed/delivered documents only, simulated download; empty state if none |
| `screen-parish-requests-hub.html` | Parish | Requests hub → Documents & Certificates | ✅ Done | Mini-workflow checklist per request (Verify → Prepare & upload certificate [captures Certificate Number + Issue Date] → Request Approval [note shown directly, no extra reveal click] → Priest approval → method-specific delivery action). Tabs: All/Pending/Awaiting Approval/Deliver/**Completed** (renamed from Archive). Paid pill now sits in the card header; standalone Verification pill removed (conveyed via the Verify workflow step instead). Detail view shows per-document-type "Certificate details" (auto-filled + parishioner-provided fields). Completed requests show their completion date. One request (Marriage Certificate) demonstrates the Post/Courier delivery flow. **New Request** button opens a simple one-page form (not a wizard) for staff to capture walk-in/offline requests |
| `screen-parish-familyrecords-search.html` | Parish | Family Records | ✅ Done | Table + right slide-in drawer, sort/filter (status, ward, subscription) + cross-field search. Header shows Registered/Active family counts. Family members now individually editable (name/relationship/DOB/status) via a per-row Edit action. Subscription tab redesigned: prominent color-coded Total Outstanding hero, "Subscription paid till" label, Record Payment shown as its own bounded card with a live, prominent preview (not subtext), payment confirmation replaces the form in place rather than appearing near history. Wards corrected to Infant Jesus Shrine's real catchment (Viveknagar, Ejipura, Neelasandra, Austin Town, National Games Village), now a fixed dropdown everywhere a ward is set. Register Family asks for husband/wife names, marriage date, and marriage parish. **New "Updates" tab** in the drawer reviews parishioner-submitted Family Update Requests (Approve applies the diff directly to the record; Reject requires a message) — tab label shows a pending-count badge. Supports deep-linking via `?family=ID&tab=updates` (used by the Dashboard priority item) and `?openRegister=1` (used by the "Register New Family" quick action, now actually wired up) |
| `screen-parish-masses-hub.html` | Parish | Masses | ✅ Done | Two modules: **Mass Schedule** (6 seeded recurring Masses for Infant Jesus Shrine — Sunday 6:30/8:00/10:00 AM, weekday, Saturday Vigil, Novena — with a working "Add Mass Schedule" form) and **Mass Intentions** (auto-generates the next 4 weeks of bookable instances from the weekly schedules, each expandable to show booked intentions with payment status, "Print Sheet" simulated, "Add Booking" opens a walk-in booking form). Sidebar "Masses" link and the Dashboard's "Schedule a Mass" quick action now wired up across all Parish screens |
| `screen-parishioner-massbooking-request.html` | Parishioner | Book a Mass | ✅ Done | Guided flow: choose Mass (next 3 weeks, generated from the same schedule as the Parish side; already-booked Masses show live capacity and go "Full" when at max) → intention type/for/message → offering + payment method → review/submit → confirmation with reference. Linked from the Dashboard's "Book a Mass" quick action |
| `screen-parishioner-massbooking-mybookings.html` | Parishioner | My Mass Bookings | ✅ Done | Upcoming/Past filter tabs; seeded with the Aug 2 Thanksgiving booking already referenced elsewhere in the app plus a past Memorial booking, for continuity. Linked from the Dashboard's "Your schedule" section |
| `screen-parishioner-family.html` | Parishioner | Me → My Family | ✅ Done | Read-only family overview + member list, plus a simple (non-wizard) "Request an Update" form covering the three most common cases: Update Family Details, Update a Member's Details, Add a Family Member — each pre-fills known values per the Capture Once principle. Submissions appear in "My Update Requests" with status tracking (Submitted/Approved/Rejected + reason if rejected). Linked from the Dashboard's "My Family" bottom-nav item. Scoped deliberately: Remove Member, Record Birth/Marriage/Death, and Parish Transfer are out of this pass (Transfer already exists staff-initiated in Family Records) |
| `screen-parish-dashboard-home.html` | Parish | Dashboard (home) | ✅ Done (Community Heart) | Sidebar nav, session-aware; non-seeded parish logins render genuine Empty state, proving data isolation. "Register New Family" quick action (and the Empty-state "Register a Family" button) now link to Family Records with `openRegister=1`, auto-opening the Register modal there |
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
| Parish | Masses | `screen-parish-masses-hub.html` | ✅ Built |
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
