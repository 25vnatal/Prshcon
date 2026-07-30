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
| `screen-parishioner-dashboard-home.html` | Parishioner | Dashboard (home) | ✅ Done (Community Heart) | Reordered for concision: personalized content leads, Quick Actions/Announcements/Community lower. Slim horizontal **Hero banner** (no pill), CTA reads "View Details". Greeting slimmed (no parish subtext). **Upcoming** (renamed from "Your schedule"): rows show mass timing, then church directly below, then intention detail (if any) on its own line — no combined subtext. **My Requests** card: left "My Requests" label, right "N updates" + chevron, sized identically to the Other Actions card (p-4, min-h-[36px] on both). **Other Actions** swapped Feast RSVP for a Subscription payment reminder, count badges read "N pending." Coral bell icons + a red dot on the avatar flag anything actionable. **Parish Announcements** (renamed) shows parish name next to the week. **Search**: chips only pre-fill the bar (via `fillSearch`); results only appear on Enter or the explicit Search button, headed "Results for '[query]'". **Logo fixed**: was JPEG data mislabeled `.png` (MIME mismatch caused a broken-image icon on strict hosts) — replaced with the text-free logo saved correctly as `assets/pc-logo.jpg`, now styled square (`rounded-lg`) instead of circular. **Upcoming** now collapses to 1 tile by default with a "Show N more" expand toggle; Mass Intention date changed to 9 Aug to differ from Next Mass's 2 Aug. Section headers reduced from `text-lg` to `text-base`. Quick Actions tiles made more compact (smaller padding/icon) |
| `screen-parishioner-documents-request.html` | Parishioner | Documents & Certificates → Request Document | ✅ Done | Guided multi-step flow; "Details" step (renamed from Supporting Documents) now shows per-document-type auto-filled fields (from family/member record) plus parishioner-input fields (Godfather/Godmother, Witnesses/Celebrant, Destination, etc.) per the product owner's field list, alongside supporting doc upload. Delivery step: renamed/clarified options, each expands in-place within its own card (not a separate block) to capture email/address/pickup details |
| `screen-parishioner-documents-myrequests.html` | Parishioner | Documents & Certificates → My Requests | ✅ Done | Status list with filter tabs (New/In Progress/Awaiting Action/Completed), expandable Activity Timeline per request using the granular→display status mapping from mock-data-schema.md. **Sorted by most-recently-updated first** (`lastUpdated` field); requests updated within the last 3 days get a left accent border + "UPDATED" badge so they're visually distinct, not just reordered. 5-item bottom nav added (Services highlighted) |
| `screen-parishioner-documents-mydocuments.html` | Parishioner | Documents & Certificates → My Documents | ✅ Done | Completed/delivered documents only, simulated download; empty state if none. 5-item bottom nav added (Services highlighted) |
| `screen-parish-requests-hub.html` | Parish | Requests hub → Documents & Certificates | ✅ Done | Mini-workflow checklist per request (Verify → Prepare & upload certificate [captures Certificate Number + Issue Date] → Request Approval [note shown directly, no extra reveal click] → Priest approval → method-specific delivery action). Tabs: All/Pending/Awaiting Approval/Deliver/**Completed** (renamed from Archive). Paid pill now sits in the card header; standalone Verification pill removed (conveyed via the Verify workflow step instead). Detail view shows per-document-type "Certificate details" (auto-filled + parishioner-provided fields). Completed requests show their completion date. One request (Marriage Certificate) demonstrates the Post/Courier delivery flow. **New Request** button opens a simple one-page form (not a wizard) for staff to capture walk-in/offline requests |
| `screen-parish-familyrecords-search.html` | Parish | Family Records | ✅ Done | Table + right slide-in drawer, sort/filter (status, ward, subscription) + cross-field search. Header shows Registered/Active family counts. Family members now individually editable (name/relationship/DOB/status) via a per-row Edit action. Subscription tab redesigned: prominent color-coded Total Outstanding hero, "Subscription paid till" label, Record Payment shown as its own bounded card with a live, prominent preview (not subtext), payment confirmation replaces the form in place rather than appearing near history. Wards corrected to Infant Jesus Shrine's real catchment (Viveknagar, Ejipura, Neelasandra, Austin Town, National Games Village), now a fixed dropdown everywhere a ward is set. Register Family asks for husband/wife names, marriage date, and marriage parish. **New "Updates" tab** in the drawer reviews parishioner-submitted Family Update Requests (Approve applies the diff directly to the record; Reject requires a message) — tab label shows a pending-count badge. Supports deep-linking via `?family=ID&tab=updates` (used by the Dashboard priority item) and `?openRegister=1` (used by the "Register New Family" quick action, now actually wired up) |
| `screen-parish-masses-hub.html` | Parish | Mass Services | ✅ Rebuilt | **Corrected data model per the Mass Services Build Specification**: a Booking can contain multiple Intentions, each with its own Mass/date, matching MassActivity → MassInstance → MassBooking → MassIntention. **Manage Parish Masses** tab (renamed from "Mass Schedule"): masses now named with language (English/Tamil/Kannada/Konkani), booking caps raised to 20, three new Feast-type masses added (Assumption, Christmas Midnight, Easter Vigil, Good Friday), custom 5-minute-step time picker (three plain `<select>`s — hour/minute/AM-PM — so every option is visible without scrolling, replacing the native time input), an "Intention & booking settings" separator groups the fee/cap/cutoff fields, and every mass now has a working **Edit** button that reopens the same modal pre-filled and updates in place (verified via a sandboxed script test — no duplicate created). Grouped sections (dynamically derived by day-pattern/date-range, from the earlier round) carried forward. **Upcoming Masses**: badge now shows only the confirmed count, no "x of y"; seeded enough bookings that the first 8 chronological instances each show 3–5 real bookings — found and fixed a second sorting bug in the process (Mass times were compared as plain strings, so "6:00 PM" sorted before "6:45 AM"; fixed with a proper time-to-minutes comparator, verified against the real seed data before and after). **Bookings**: now a flat table (never grouped) showing Booked By (name primary, booking # demoted to secondary text below it) / Mass / Intention type+description / Source / Created By / a single combined **Status** column (Pending Payment / Confirmed / Confirmed (Waived) / Payment Failed — collapsing what were two separate Payment+Status columns into one); Stipend column removed from the table (still visible in the detail view); the Mass filter now lists actual date+time+name instances rather than just activity types, plus a free-text search box; CTA renamed **"New Booking"**; the modal itself was reimagined into a two-pane layout (date + Mass list on the left, intention details on the right) with a **cart** that holds multiple intentions and submits them as one booking with one payment — verified end-to-end in a script test (2 intentions added, exactly 1 booking created, cart cleared after) |
| `screen-parishioner-massbooking-request.html` | Parishioner | Book a Mass | ✅ Rebuilt | Date + Mass merged into one accordion step; cart collapsed by default with a "Review & Pay →" shortcut always available; intention description/help adapts per type; parish search replaced native `<datalist>` with a custom results list; sticky (not fixed) footer + `min-h-[100dvh]` so the button doesn't hide under the mobile keyboard. **Found and fixed the actual cause of the date-grouping bug**: `.toISOString()` converts to UTC, and since Bangalore is UTC+5:30, constructing a local midnight Date and slicing the ISO string rolled the calendar date back by one day for anyone in an ahead-of-UTC timezone — this is why Sunday Masses were grouping under a Saturday header. Replaced with a local-date formatter; same fix applied to `screen-parish-masses-hub.html`. All booking fees now minimum ₹100 (bumped the two Weekday Masses that were ₹50). 5-item bottom nav added, stacked below the Continue button (an explicit exception to the usual wizard-excludes-bottom-nav rule) |
| `screen-parishioner-massbooking-mybookings.html` | Parishioner | My Mass Bookings | ✅ Rebuilt | Now booking-centric, not intention-centric: each card is one Booking (Booking #, total, payment status) that expands to show every Intention inside it (Mass, date, type, description, offered by, stipend). Upcoming/Past classified by whether any intention's date is still ahead. Seeded with a single-intention Paid booking, a Pending-payment booking (working "Pay Now"), a past Parish-Office booking, and a genuine **multi-intention** booking spanning two different Masses, to demonstrate the corrected model. 5-item bottom nav (Services highlighted) |
| `screen-parishioner-family.html` | Parishioner | Me → My Family | ✅ Done | Read-only family overview + member list, plus a simple (non-wizard) "Request an Update" form covering the three most common cases: Update Family Details, Update a Member's Details, Add a Family Member — each pre-fills known values per the Capture Once principle. Submissions appear in "My Update Requests" with status tracking (Submitted/Approved/Rejected + reason if rejected). Linked from the Dashboard's "My Family" bottom-nav item. 5-item bottom nav added (My Family highlighted). Scoped deliberately: Remove Member, Record Birth/Marriage/Death, and Parish Transfer are out of this pass (Transfer already exists staff-initiated in Family Records) |
| `screen-parish-dashboard-home.html` | Parish | Dashboard (home) | ✅ Done (Community Heart) | Sidebar nav, session-aware; non-seeded parish logins render genuine Empty state, proving data isolation. "Register New Family" quick action (and the Empty-state "Register a Family" button) now link to Family Records with `openRegister=1`, auto-opening the Register modal there. Parish Connect logo added to the sidebar header, styled square (`rounded-lg`) |
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
| `specs/mass-services-build-spec.md` | User-provided Mass Services Build Specification — corrected data model (Booking → multiple Intentions, each with its own Mass/date), booking states (ONLINE: Draft→Pending Payment→Paid/Confirmed; PARISH_OFFICE: Draft→Payment Confirmed→Confirmed), and the parish/parishioner use cases that `screen-parish-masses-hub.html` and the two `screen-parishioner-massbooking-*.html` screens are now built against |
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

### Bottom Nav Convention (Parishioner)

The 5-item bottom nav (Home/Services/Give/My Family/Menu) is present on every Parishioner screen **except**:
- `screen-parishioner-login.html` — pre-authentication, no nav context yet
- `screen-parishioner-documents-request.html` — guided wizard flow with a sticky footer Back/Continue bar

`screen-parishioner-massbooking-request.html` now includes **both**, stacked (Continue button on top, 5-item nav below) — added per explicit request despite the earlier general exception for wizards.

New Parishioner screens going forward should include the bottom nav unless they're a similar full-screen guided flow, in which case follow the same footer-bar pattern (or the stacked pattern, if both are wanted) instead.

### Auth / Session Convention

- No real backend — login screens accept any non-empty credentials (Parish login additionally validates the parish name against the real directory).
- Session context (role, name, parish id/name) is passed forward via **URL query parameters** on redirect (e.g. `screen-parish-dashboard-home.html?role=staff&parish=PAR-0002&parishName=...`), not `localStorage`/`sessionStorage` — keeps every screen a plain static file with no browser-storage dependency, works identically whether opened as a file, hosted, or viewed as a Claude artifact.
- Every post-login screen must read these params and reflect them in its header (name/role/parish), with a way to log out back to the correct login screen.
- **Parish data isolation demo**: the Parish login's parish picker accepts any of the 130 real parishes. Logging in as **Infant Jesus Shrine (`PAR-0002`)** shows the fully populated dashboard/data. Logging in as any other parish should render the genuine **Empty** state everywhere (no families, no requests) — proving parishes never see each other's data, without needing a second fully-seeded dataset.
