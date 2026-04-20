# UC‑PROG‑04 — Select Program
Use Case ID
UC‑PROG‑04
Use Case Name
Select Program
Module
Exercise Programs / Program Detail
## Purpose
To define how the user selects (activates) an Exercise Program from the Program Detail screen. This use case covers confirming the user’s choice, updating the user’s active program, and ensuring the system reflects the new selection across the dashboard and related modules.
## Primary Actor
Authenticated User
## Stakeholders & Interests
• 	User — wants to choose a program and begin following it.
• 	System — must update the user’s active program reliably and securely.
• 	Product Owner — wants a smooth, intuitive program selection experience.
• 	UX/UI Team — requires clear confirmation patterns and consistent navigation.
• 	Analytics Team — tracks program selection and engagement.
• 	Security Team — ensures users can only activate programs they are authorized to access.

## Preconditions
• 	User is authenticated.
• 	Program Detail page is displayed (UC‑PROG‑01).
• 	User has navigated to a specific program.
• 	Program is eligible for selection (not expired, not restricted).

## Postconditions
Success
• 	Selected program becomes the user’s active program.
• 	System stores the active program ID and start date.
• 	Dashboard and related modules reflect the new active program.
• 	User is redirected to the Dashboard or confirmation screen.
Failure
• 	Program remains unselected.
• 	System may display an error message.

## Trigger
User selects the “Select Program” or “Activate Program” button on the Program Detail screen.

## Main Success Scenario (Basic Flow — Select Program)
1. 	User selects “Select Program” on the Program Detail page.
(BR‑PROG‑19)
2. 	System receives the request to activate the program.
(FS‑PROG‑ACTIVATE‑01)
3. 	System validates that the program exists and is eligible for activation.
(BR‑PROG‑20)
4. 	System retrieves the program metadata (duration, start date rules, etc.).
(BR‑PROG‑02 → BR‑PROG‑06)
5. 	System sets the selected program as the user’s active program, storing:
• 	Program ID
• 	Activation timestamp
• 	Calculated start date
(BR‑PROG‑19)
6. 	System updates the user profile with the active program reference.
(DATA‑UserProfile.ProgramID)
7. 	System recalculates the user’s upcoming schedule (weeks/days).
(BR‑PROG‑11)
8. 	System returns a success response.
9. 	UI displays a confirmation message and redirects the user to the Dashboard.
(UIS‑PROG‑04)

## Alternate Flows
A1 — Confirmation Dialog (Recommended UX)
• 	System displays:
“Start this program? Your current program will be replaced.”
• 	User selects Yes → continue Basic Flow.
• 	User selects No → cancel operation.
A2 — User Already Has an Active Program
• 	System displays a warning:
“Selecting this program will replace your current active program.”
• 	User confirms → continue Basic Flow.
• 	User cancels → no changes made.
A3 — Program Has a Delayed Start Date
If the program begins on a future date:
• 	System calculates the start date.
• 	System displays:
“This program will begin on [date].”

## Exception Flows
E1 — Program Not Found
• 	Program ID is invalid or missing.
• 	System displays:
“This program is no longer available.”
• 	System logs the event.
E2 — Program Not Eligible
Examples:
• 	Restricted program
• 	Premium program without access
• 	Program expired
System displays:
“You do not have access to this program.”
E3 — Data Store Failure
• 	System cannot update the user’s active program.
• 	System displays:
“Unable to select program. Please try again.”
• 	Program remains unselected.
E4 — Network Timeout
• 	System cannot complete the request.
• 	UI may retry or show fallback messaging.

## Non‑Functional Requirements
• 	Performance: Program activation must complete within 200ms.
• 	Security: Only authorized users may activate programs.
• 	Reliability: Activation must persist across sessions and devices.
• 	Data Integrity: User’s active program must remain consistent across modules.
• 	Accessibility: Activation button must support keyboard navigation and ARIA labels.
• 	Consistency: Activation flow must match other selection flows in the app.

## Related UI Screens
• 	UIS‑PROG‑01 — Program Detail Page
• 	UIS‑PROG‑04 — Program Activation Confirmation
• 	UIS‑DASH‑01 — Dashboard
• 	UIS‑GLOBAL‑HEADER‑01
• 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    A[User Selects Program] --> B[Validate Program Eligibility]
    B --> C[Retrieve Program Metadata]
    C --> D[Set Program as Active]
    D --> E[Update User Profile]
    E --> F[Recalculate Schedule]
    F --> G[Return Success Response]
    G --> H[UI Shows Confirmation]
    H --> END[Redirect to Dashboard]