# UC‑PROG‑02 — Select Week
Use Case ID
UC‑PROG‑02
Use Case Name
Select Week
Module
Exercise Programs
## Purpose
To define how the user selects a specific week within an Exercise Program. This use case covers loading week‑level data, displaying day titles and dates, and updating the Program Detail view without requiring the user to scroll through the entire program.
## Primary Actor
Authenticated User
## Stakeholders & Interests
- 	User — wants to quickly navigate to a specific week without excessive scrolling.
- 	System — must load week‑specific data efficiently.
- 	Product Owner — wants a clean, intuitive program navigation experience.
- 	UX/UI Team — requires a week selector that updates content without page reload.
- 	Security Team — ensures users only access programs they are authorized to view.

## Preconditions
- 	User is authenticated.
- 	Program Detail page is displayed (UC‑PROG‑01).
- 	Program contains one or more weeks.
- 	Week selector is visible and enabled.

## Postconditions
Success
- 	Selected week becomes active.
- 	System displays the days for that week, including:
- 	Day number
- 	Day title
- 	Scheduled date (if applicable)
- 	UI updates without reloading the entire Program Detail page.
Failure
- 	Week does not load.
- 	System may display an error message.

## Trigger
User selects a week from the week selector (e.g., Week 1, Week 2, Week 3).

## Main Success Scenario (Basic Flow — Select Week)
1. 	User selects a week from the week selector.
(BR‑PROG‑11)
2. 	System receives the request to load the selected week.
(FS‑PROG‑WEEK‑01)
3. 	System retrieves the week‑level data, including:
- 	Week number
- 	Days in the week
- 	Day titles (e.g., “Upper Body Strength”, “Rest Day”)
- 	Scheduled dates (if program is calendar‑aligned)
(BR‑PROG‑07 → BR‑PROG‑12)
4. 	System retrieves day‑level summaries for the selected week.
(BR‑PROG‑13 → BR‑PROG‑18)
5. 	System updates the Program Detail page to display:
- 	Active week highlight
- 	Day cards for the selected week
(UIS‑PROG‑02)
6. 	System sets focus to the top of the week section for accessibility.
(SRS‑A11Y‑01)
7. 	User views the selected week’s days.

## Alternate Flows
A1 — User Selects the Same Week Again
- 	System detects the week is already active.
- 	No data reload occurs.
- 	UI remains unchanged.
A2 — Program Has Variable Week Length
- 	Some programs may have 4, 6, 8, or 10 weeks.
- 	System dynamically renders the correct number of week tabs.
(BR‑PROG‑07)
A3 — Week Contains Rest Days
- 	System displays rest days with a distinct visual style.
(BR‑PROG‑14)

## Exception Flows
E1 — Week Data Not Found
- 	Week index is invalid or missing.
- 	System displays:
“Unable to load this week. Please try again.”
- 	System logs the event.
E2 — Data Load Failure
- 	System cannot retrieve week data.
- 	System displays fallback content or error state.
- 	System logs the failure.
E3 — Unauthorized Access
- 	User attempts to view a week in a restricted program.
- 	System blocks access and logs a security event.
(BR‑PROG‑20)

## Non‑Functional Requirements
- 	Performance: Week data must load within 200ms.
- 	Accessibility: Week selector must support keyboard navigation and ARIA roles.
- 	Responsiveness: Week layout must adapt to mobile, tablet, and desktop.
- 	Security: Only authorized users may view program weeks.
- 	Consistency: Week selection must not reload the entire page.

## Related UI Screens
- 	UIS‑PROG‑01 — Program Detail Page
- 	UIS‑PROG‑02 — Week Selector
- 	UIS‑PROG‑03 — Day Preview Cards
- 	UIS‑GLOBAL‑HEADER‑01
- 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    A[User Selects Week] --> B[Load Week Metadata]
    B --> C[Load Day Titles & Dates]
    C --> D[Load Day Summaries]
    D --> E[Update Program Detail UI]
    E --> F[Set Accessibility Focus]
    F --> END[Week Displayed]