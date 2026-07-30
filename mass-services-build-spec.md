# Mass Services — Build Specification

(Full spec provided by the user — see conversation history for complete use cases.
Key corrections vs. the original implementation:)

- A **Booking** can contain **multiple Intentions**, each with its own Mass/date selection.
- Intention Types (canonical 6): Thanksgiving, Special Intentions, In Memory Of, For the Sick, Anniversary, Birthday.
- Data model: MassActivity (config) → MassInstance (occurrence) → MassBooking (source ONLINE/PARISH_OFFICE) → MassIntention (type/description/offeredBy/stipend/paymentStatus).
- Defaults: Max Intentions 999, Booking Fee ₹100, Booking Cut-off 1 hour before Mass.
- Online booking states: Draft → Pending Payment → Paid/Confirmed (or Payment Failed).
- Parish-office booking states: Draft → Payment Confirmed → Confirmed (created only after payment).
- Pending-payment intentions are excluded from the generated Intention Sheet.
- Staff can manually "Stop Bookings" for a specific Mass date ahead of the automatic cut-off.
- Parishioner online payment methods: UPI, Cards, Net Banking only.
