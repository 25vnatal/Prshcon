# Parish Connect — Canonical Mock-Data Schema

Source of truth for every entity, field, status, and relationship used across screens.
Derived from the Navigation Hierarchy + the per-module Feature Design Briefs.
Every screen must pull from this schema rather than inventing field names or statuses ad hoc.

---

## 0. Canonical Seed Anchors (locked)

These IDs/names must be reused identically across every screen and mock dataset.

| Entity | Value |
|---|---|
| Diocese | Bangalore Archdiocese (`DIO-0001`) |
| Parish | Infant Jesus Shrine, Viveknagar (`PAR-0002`) — Post Box 4712, Viveknagar, Bengaluru 560 047 |
| Parish Priest | Fr. John Anthony |
| Family | Rodrigues Family (`FAM-1001`) |
| Head of Family | John Rodrigues |
| Spouse | Grace Rodrigues |
| Son | Kevin Rodrigues |
| Daughter | Clara Rodrigues |
| Parish Directory | 130 parishes — `mock-data/parishes.json` |

ID convention: `{ENTITY}-{4-digit-sequence}`, e.g. `FAM-1001`, `MEM-2001`, `REQ-3001`, `MASS-4001`. Sequences are namespaced per entity type so IDs never collide across modules.

---

## 1. Global Entity Map

```
Diocese
└── Parish
    ├── Family
    │   ├── Family Member
    │   ├── Subscription (Ledger, Payments, Receipts)
    │   ├── Update Request
    │   └── Transfer Request
    ├── Document Type → Document Request → Generated Document
    ├── Home Service → Home Service Request
    ├── Sacrament → Sacrament Registration
    ├── Document (Workspace) → Template, Recipient, Attachments
    ├── Announcement
    ├── Donation Cause → Donation
    ├── Event → Event Registration
    ├── Mass Schedule → Mass Instance → Mass Booking → Intention
    ├── Financial Category / Account → Financial Transaction → Report
    └── Parish Profile (aggregates all of the above for public display)
```

Every "Request"-shaped entity (Document Request, Home Service Request, Sacrament Registration, Event Registration, Family Update/Transfer Request) shares the same underlying lifecycle — see **Section 3, Universal Request Lifecycle** — and should render with the same status-badge/timeline components from `style-guide.html`.

---

## 2. Core Entities

### 2.1 Diocese
| Field | Type | Notes |
|---|---|---|
| id | string | `DIO-0001` |
| name | string | "Bangalore Archdiocese" |
| totalCatholicPopulation | number | ~360,561 (from directory sizing methodology) |
| parishCount | number | 130 |

### 2.2 Parish
| Field | Type | Notes |
|---|---|---|
| id | string | `PAR-0002` |
| name | string | |
| address | string | |
| sizeTier | enum | Large / Medium / Small |
| estFamilies | string | range, e.g. "925 - 2,300" |
| parishPriest | string | |
| assistantPriests | string[] | |
| yearEstablished | number \| null | |
| phone / email / website | string \| null | |
| patronSaint, feastDay, motto | string | Parish Profile fields |
| officeHours | { weekday, saturday, sunday, weeklyHoliday } | |
| worshipLocations | Array<{name, address, status}> | |

Full 130-parish directory: `mock-data/parishes.json`.

### 2.3 Family
| Field | Type | Notes |
|---|---|---|
| id | string | `FAM-1001` |
| familyRecordNumber | string | e.g. "IJS/FAM/1001" |
| familyName | string | "Rodrigues Family" |
| headOfFamilyId | string | → Member.id |
| registeredParishId | string | → Parish.id |
| registrationDate | date | |
| bccWardZone | string | |
| address | { houseNo, street, locality, city, state, pinCode, landmark } | |
| mobile, email, preferredLanguage | string | |
| marriageDate, marriageChurch | string \| null | |
| subscriptionStatus | enum | see 2.7 |
| internalNotes | string | staff-only |
| status | enum | **Active / Temporarily Away / Transferred / Archived** |

### 2.4 Family Member
| Field | Type | Notes |
|---|---|---|
| id | string | `MEM-2001` |
| familyId | string | |
| fullName, preferredName | string | |
| gender | enum | |
| dateOfBirth, placeOfBirth | date/string | |
| relationship | enum | Head / Spouse / Son / Daughter / Parent / Other |
| fatherName, motherName | string | |
| maritalStatus | enum | |
| mobile, email | string \| null | |
| occupation, education | string | |
| membershipStatus | enum | **Active / Away for Studies / Away for Work / Transferred / Deceased** |
| specialCategories | string[] | e.g. senior citizen, differently-abled |
| sacramentalSummary | Array<{sacrament, date, church}> | Baptism, First Communion, Confirmation, Marriage |

### 2.5 Family Update / Transfer Request
| Field | Type | Notes |
|---|---|---|
| id | string | `REQ-3001` |
| type | enum | Update Family / Update Member / Add Member / Remove Member / Record Birth-Marriage-Death / Parish Transfer |
| familyId, memberId | string | |
| updatedInformation | object | field-diff payload |
| supportingDocuments | string[] | |
| comments | string | |
| requestDate | date | system |
| status | enum | see **Universal Request Lifecycle** |

### 2.6 Subscription Policy / Ledger
| Entity | Field | Notes |
|---|---|---|
| Subscription Policy | minimumAmount, billingFrequency (Monthly/Yearly), effectiveDate, gracePeriod, status (Active/Inactive) | parish-level config |
| Subscription Ledger row | familyId, billingPeriod, minimumAmountDue, amountPaid, outstandingAmount, lastPaymentDate, status | one row per family per period |
| Payment | amount, method, receiptNumber, paymentDate, status | |

### 2.7 Subscription Status (family-facing)
`Paid` · `Pending` · `Overdue`

---

## 3. Universal Request Lifecycle

The briefs use a more granular status set than the Navigation Hierarchy's simplified 5-stage model. **Both are valid — the granular set is the underlying data model; the 5-stage set is the simplified view surfaced in the Parish "Requests" hub.** Mapping:

| Nav Hierarchy (display) | Granular status (data) |
|---|---|
| **New** | Draft, Submitted |
| **In Progress** | Under Review, Pending Approval |
| **Awaiting Action** | Awaiting Information (from requester) |
| **Completed** | Approved → [module-specific fulfillment] → Completed |
| **Rejected** | Rejected, Cancelled |

Full granular status list (used internally, e.g. in Request Details / Activity Timeline):
`Draft → Submitted → Under Review → Awaiting Information → Pending Approval → Approved → [Document Generated / Ready for Delivery / Scheduled] → Completed`
Exit states at any point: `Rejected`, `Cancelled`.

Every request entity also carries independent sub-statuses that must **not** be conflated with the main request status:
- **Payment Status**: Not Required / Pending / Paid (or Successful) / Failed / Refunded
- **Document Verification Status**: Pending / Verified / Rejected
- **Delivery Status** (documents only): Not Ready / Ready for Delivery / Delivered

Workflow actors, consistent across every module: **Requester (Parishioner or Parish Staff on their behalf) → Parish Staff (verify, recommend) → Parish Priest (approve/reject) → System (execute + notify)**.

---

## 4. Module Entities

### 4.1 Document Type (config) → Document Request → Generated Document
- **Document Type**: name, category, description, status, adminCharge, paymentRequired, requiredSupportingDocs[], deliveryOptions[] (In-app Download/Email/Collect at the Parish/Post / Courier), expectedProcessingTime, processingInstructions
- **Document Request**: id, documentTypeId, requesterId, familyId, purpose, uploadedDocuments[], deliveryMethod, adminCharge, paymentMethod, additionalInfo, requestStatus (universal lifecycle), paymentStatus, documentVerificationStatus, deliveryStatus, submissionDate, requestReference, internalNotes, capturedFields (see below)
- **Generated Document**: linked file, certificateNumber, issueDate, generatedDate, deliveredDate

**Per-document-type field requirements** (not in the original feature briefs — provided directly by the product owner). Each type distinguishes fields already known from the family/member record (auto-filled, Capture Once) from fields only the parishioner can provide (captured at request time) and fields the parish staff fills in when preparing the certificate (system/output fields, not requested from the parishioner):

| Document Type | Auto-filled (from family/member record) | Parishioner must provide | Parish staff fills in at generation |
|---|---|---|---|
| Baptism Certificate | Full Name, Date of Birth, Father's Name, Mother's Name, Baptism Date, Baptism Parish | Godfather, Godmother | Certificate Number, Issue Date |
| First Communion Certificate | Full Name, Date of Birth, First Communion Date, Parish | — | Certificate Number, Issue Date |
| Confirmation Certificate | Full Name, Date of Birth, Confirmation Date, Parish | — | Certificate Number, Issue Date |
| Marriage Certificate | Groom Name, Bride Name, Marriage Date, Marriage Parish | Witness 1, Witness 2, Celebrant | Certificate Number, Issue Date |
| No Objection / Good Standing Certificate | Full Name, Family Record Number, Address, Membership Status | Destination (Parish/Institution/Organization) | Certificate Number, Issue Date |
| Parish Transfer Certificate | Family Record Number, Family Name, Head of Family, Family Members | Destination Parish, Transfer Date | Certificate Number, Issue Date |

Baptism Date/Parish, Communion Date/Parish, and Confirmation Date/Parish are resolved from the Family Member's `sacramentalSummary` when available; if no matching sacrament entry exists on file, the field renders empty and editable rather than blocking the request.

### 4.2 Home Service (config) → Home Service Request
- **Home Service types** (fixed list): Home Communion, House Blessing, Sick Visit/Anointing, Funeral Visit/Prayer, Confession at Home, Family Prayer/Rosary, Blessing of Vehicle/Business/Office, Other Pastoral Visit — each with active/inactive, availability, approvalRequired, instructions, emergencyAllowed (where applicable)
- **Home Service Request**: id, serviceType, familyId, familyMemberId (nullable), preferredDate, preferredTime, notes, status (universal lifecycle + Scheduled/Completed), service-specific fields (occasion, isEmergency, deceasedName, blessingType, description)

### 4.3 Sacrament (config) → Sacrament Registration
- **Sacrament config**: name, description, image, status, registrationRequired, registrationMode (Always Open/Registration Window), registrationOpens/Closes, celebrationDate, venue, minAge, parishMembershipRequired, previousSacramentsRequired, requiredDocuments[], feeRequired, registrationFee, instructions
- **Registration**: id, sacramentId, candidateMemberId, familyId, requiredDocuments[], uploadedDocuments[], registrationFee, paymentMethod, additionalInfo, applicant, registrationStatus (universal lifecycle), paymentStatus, documentVerificationStatus, submissionDate, registrationReference, internalNotes

### 4.4 Document (Workspace)
- id, templateId, recipientId, attachments[], deliveryMethod, activityLog[], basicInfo{title, type, generatedFor}

### 4.5 Announcement
- id, title, content, image (optional), attachment (optional), status (**Draft / Published / Archived / Deleted**), createdBy, createdDate, lastUpdated, publishedDate

### 4.6 Donation Cause → Donation
- **Cause**: id, name, description, bannerImage, institutionId (Parish or Diocese), donationType (Open/Goal-based), suggestedAmounts[], allowCustomAmount, targetAmount (goal-based), autoCloseOnTarget, status (**Draft / Active / Closed / Archived**)
- **Donation**: id, causeId, donorId, familyId, institutionId, amount, donorMessage, paymentMethod, transactionReference, receiptNumber, donationDate, paymentStatus (**Pending / Successful / Failed / Refunded**), internalNotes, receiptStatus

### 4.7 Event (config) → Event Registration
- **Event**: id, name, description, category, bannerImage, status, eventDate, startTime, endTime, venue, registrationRequired, registrationOpens/Closes, capacity, paidEvent, registrationFee, attendanceTrackingEnabled
- **Registration**: id, eventId, participantMemberIds[], registrationFee, paymentMethod, additionalInfo, registrationReference, registrationDate, registrationStatus (**Draft / Submitted / Pending Approval / Approved / Waitlisted / Rejected / Cancelled / Completed / No Show**), attendanceStatus (**Not Recorded / Present / Absent**), paymentStatus (**Not Required / Pending / Paid / Failed / Refunded**), internalNotes

### 4.8 Mass Schedule → Mass Instance → Mass Booking
- **Mass Schedule (config)**: id, massName, massType (Regular/Sunday/Feast/Novena/Special), church, startDate, endDate, recurrence (One-Time/Daily/Weekly/Monthly), daysOfWeek[], dateWeekRule, massTime, onlineBookingEnabled, bookingOpensDaysBefore, bookingClosesBefore, maxIntentions, allowMultipleIntentions, suggestedOffering, status
- **Mass Instance**: id, massScheduleId, date, time — the bookable occurrence generated from the recurrence rule
- **Mass Booking (Intention)**: id, massInstanceId, familyId, familyMemberId (optional), requestedBy, mobile, email, intentionType (Thanksgiving/Birthday/Wedding Anniversary/Memorial/Death Anniversary/Healing/Special Intention/Souls in Purgatory/Other), intentionFor, intentionMessage, offeringAmount, paymentMethod, notes, bookingReference, paymentStatus, receiptNumber
- **Intention Sheet**: generated per Mass Instance — sequenceNo, intentionType, intentionFor, intentionMessage, requestedBy, remarks

### 4.9 Financial Category / Account → Financial Transaction → Report
- **Category**: name, type (Income/Expense), parentCategory, status
- **Account**: name, type (Cash/Bank), openingBalance, status
- **Transaction**: id, type (Income/Expense), date, amount, accountId, categoryId, description, referenceNumber, attachments[], paymentMethod, internalNotes, transactionNumber, createdBy, createdDate, status (**Draft / Pending Approval / Posted / Voided**), paymentStatus (**Pending / Completed / Cancelled**)
- **Reports**: Cash Book, Income Report, Expense Report, Income vs Expense, Category Summary, Monthly/Annual Summary, Transaction Register

### 4.10 Parish Profile
Editable: basicInfo (name, shortName, logo, coverImage, patronSaint, feastDay, yearEstablished, motto), contactInfo (phone, whatsapp, email, website, social), address, clergy[], parishOffice, officeHours, worshipLocations[], aboutParish.
Aggregated (read-only, sourced from other modules): Today's/Weekly/Special Masses, Announcements, Upcoming Events, Ministries, Home Services, Giving Options.

---

## 5. Search View Field Sets (for Search-as-Navigation screens)

- **Families**: Record Number, Family Name, Head of Family, Mobile, Ward, Status
- **Members**: Member Name, Family, Relationship, Mobile, Status
- **Parishes** (Diocese Directory): Name, Address/Locality, Size Tier, Est. Families, Parish Priest — from `parishes.json`

---

## 6. Notes for Screen-Builders

- Always resolve a `familyId`/`memberId` reference back to the canonical Rodrigues family (`FAM-1001`) unless a screen specifically needs to demonstrate a *different* family (e.g. a Parish-side "search results" screen showing multiple families).
- Every Request-shaped entity must show: current status badge (using the mapped Nav-hierarchy stage, not the raw granular status, in list views) + an Activity Timeline (using the granular statuses) in its detail view.
- Payment Status, Delivery Status, and Document Verification Status are **independent** of the main request status — never collapse them into a single badge.
- New parishes for Parish/Diocese-side screens should be drawn from `mock-data/parishes.json` rather than invented, to stay consistent with the real directory.
