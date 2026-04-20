# UC‑PROG‑03 — Select Day
Use Case ID
UC‑PROG‑03
Use Case Name
Select Day
Module
Exercise Programs
## Purpose
To define how the user selects a specific day within a selected week of an Exercise Program. This use case covers loading day‑level details, displaying the day’s title and scheduled date, and showing a summary of exercises without requiring the user to scroll through the entire program.
## Primary Actor
Authenticated User
## Stakeholders & Interests
• 	User — wants to quickly preview what a specific day contains without navigating away or scrolling excessively.
• 	System — must load day‑level data efficiently and accurately.
• 	Product Owner — wants a clean, intuitive day‑level preview experience.
• 	UX/UI Team — requires consistent card expansion behavior and accessibility compliance.
• 	Security Team — ensures users only access program content they are authorized to view.

## Preconditions
• 	User is authenticated.
• 	Program Detail page is displayed (UC‑PROG‑01).
• 	A week has been selected (UC‑PROG‑02).
• 	The selected week contains one or more days.
• 	Day cards are visible and enabled.

## Postconditions
Success
• 	Selected day becomes active.
• 	System displays the day’s details, including:
• 	Day title
• 	Scheduled date (if applicable)
• 	Exercise summaries (name, type, duration, reps/sets)
• 	UI updates without reloading the entire Program Detail page.
Failure
• 	Day details do not load.
• 	System may display an error message.

## Trigger
User selects a day card within the selected week.

## Main Success Scenario (Basic Flow — Select Day)
1. 	User selects a day card within the active week.
(BR‑PROG‑13)
2. 	System receives the request to load day‑level details.
(FS‑PROG‑DAY‑01)
3. 	System retrieves the day metadata, including:
• 	Day number
• 	Day title
• 	Scheduled date (if program is calendar‑aligned)
(BR‑PROG‑07 → BR‑PROG‑12)
4. 	System retrieves exercise summaries for the selected day, including:
• 	Exercise name
• 	Category (strength, cardio, mobility, etc.)
• 	Duration or sets/reps
(BR‑PROG‑13 → BR‑PROG‑18)
5. 	System expands the selected day card to display the exercise summaries.
(UIS‑PROG‑03)
6. 	System collapses any previously expanded day card (accordion behavior).
(BR‑PROG‑16)
7. 	System sets focus to the expanded day section for accessibility.
(SRS‑A11Y‑01)
8. 	User views the selected day’s details.

## Alternate Flows
A1 — User Selects the Same Day Again
• 	System collapses the day card.
• 	No data reload occurs.
• 	UI returns to the week‑level view.
A2 — Day Is a Rest Day
• 	System displays a rest‑day message (e.g., “Rest / Recovery”).
(BR‑PROG‑14)
• 	No exercise summaries are shown.
A3 — Day Contains Optional Exercises
• 	System displays optional exercises with a distinct visual indicator.
(BR‑PROG‑17)

## Exception Flows
E1 — Day Data Not Found
• 	Day index is invalid or missing.
• 	System displays:
“Unable to load this day. Please try again.”
• 	System logs the event.
E2 — Exercise Summary Load Failure
• 	System cannot retrieve exercise summaries.
• 	System displays fallback content or an error state.
• 	System logs the failure.
E3 — Unauthorized Access
• 	User attempts to view a day in a restricted program.
• 	System blocks access and logs a security event.
(BR‑PROG‑20)

## Non‑Functional Requirements
• 	Performance: Day details must load within 150ms.
• 	Accessibility: Day cards must support keyboard navigation and ARIA roles.
• 	Responsiveness: Day expansion must adapt to mobile, tablet, and desktop.
• 	Security: Only authorized users may view day details.
• 	Consistency: Day selection must not reload the entire page.

## Related UI Screens
• 	UIS‑PROG‑01 — Program Detail Page
• 	UIS‑PROG‑02 — Week Selector
• 	UIS‑PROG‑03 — Day Preview Cards
• 	UIS‑GLOBAL‑HEADER‑01
• 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    A[User Selects Day] --> B[Load Day Metadata]
    B --> C[Load Exercise Summaries]
    C --> D[Expand Day Card]
    D --> E[Collapse Other Day Cards]
    E --> F[Set Accessibility Focus]
    F --> END[Day Details Displayed]