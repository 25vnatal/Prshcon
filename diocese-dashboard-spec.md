# Diocese Dashboard — Screen Spec

## Design intent
The Diocese Dashboard must feel fundamentally different from the Parish Dashboard.
A **parish** manages day-to-day operations. A **diocese** provides leadership, oversight,
and coordination across many parishes. This screen must answer three questions:

1. Which parishes need attention?
2. What's happening across the diocese?
3. Are there any issues requiring intervention?

This is an exception-surfacing / oversight view, not an operational to-do list —
avoid replicating the Parish Dashboard's "run today's operations" framing.

## Section hierarchy (locked)

### 1. Diocese Snapshot (top)
A quick pulse of the diocese as a whole.
- Total Parishes
- Registered Families
- Active Parishioners
- Upcoming Diocese Events

### 2. Action Center
Only items that require diocesan-level intervention (not parish-level busywork):
- Parish reports pending
- Parish profile approvals
- New parish onboarding
- Diocese announcements awaiting approval

### 3. Parish Health *(the most important section)*
Top 5 parishes requiring attention, each with a stated reason, e.g.:
- Monthly report overdue
- High pending document requests
- No Mass schedule published
- Large number of pending approvals

Includes a "View All Parishes" link/action.

### 4. Upcoming
- Diocese events
- Bishop's calendar
- Major diocesan celebrations
- Parish feasts (optional)

### 5. Diocese Announcements
Latest 2–3 announcements published by the diocese.

## Notes for build
- Pull "Parish Health" candidates from the real 130-parish directory (`mock-data/parishes.json`)
  rather than inventing placeholder parish names, so it reads as a credible oversight view.
- Diocese Snapshot numbers should be internally consistent with `mock-seed.json`
  (e.g. parishCount: 130) and the parish directory.
- Keep this list at exactly 5 sections — do not add more per this brief.
