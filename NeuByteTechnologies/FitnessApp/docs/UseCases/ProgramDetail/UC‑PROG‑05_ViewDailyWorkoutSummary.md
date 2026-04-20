# UC‑PROG‑05 — View Daily Workout Summary
Use Case ID
UC‑PROG‑05
Use Case Name
View Daily Workout Summary
Module
Exercise Programs (Daily View)
## Purpose
To define how the user views the exercise summary list for a selected day within a program. This use case allows the user to preview the exercises, equipment needs, and workout structure before starting the workout, helping them determine whether they have the space, equipment, and ability to perform the routine.
This UC is display‑only and does not include logging fields, timers, or full exercise definitions.
## Primary Actor
Authenticated User
## Stakeholders & Interests
• 	User — wants to preview the exercises for the day to determine feasibility (equipment, space, difficulty).
• 	System — must load exercise summaries efficiently and accurately.
• 	Product Owner — wants a clear, intuitive preview experience that supports program selection and adherence.
• 	UX/UI Team — requires consistent list formatting and accessibility compliance.
• 	Security Team — ensures users only access program content they are authorized to view.

## Preconditions
• 	User is authenticated.
• 	Program Detail page is displayed (UC‑PROG‑01).
• 	A week has been selected (UC‑PROG‑02).
• 	A day has been selected (UC‑PROG‑03).
• 	The selected day contains one or more exercises.

## Postconditions
Success
• 	System displays the list of exercises for the selected day.
• 	Each exercise shows a summary‑level preview, including:
• 	Exercise name
• 	Exercise category (Strength, Cardio, Mobility, etc.)
• 	Required equipment (if applicable)
• 	Short descriptor (optional)
• 	Estimated duration or sets/reps preview (optional)
• 	User may choose to start the workout (handled in Workout module).
Failure
• 	Exercise list does not load.
• 	System may display an error message.

## Trigger
User selects “View Workout”, “View Exercises”, or expands the day card to reveal the daily workout summary.

## Main Success Scenario (Basic Flow — View Daily Workout Summary)
1. 	User selects a day (UC‑PROG‑03) and chooses “View Workout Summary”.
(BR‑PROG‑13)
2. 	System receives the request to load the exercise list for the selected day.
(FS‑PROG‑DAY‑02)
3. 	System retrieves the exercise summary data for the day, including:
• 	Exercise name
• 	Category (Strength, Cardio, Mobility, etc.)
• 	Required equipment
• 	Short descriptor (optional)
• 	Estimated duration or sets/reps preview (optional)
(BR‑EX‑01 → BR‑EX‑10)
4. 	System orders the exercises according to the program’s defined sequence.
(BR‑PROG‑18)
5. 	System displays the Daily Workout Summary list, including:
• 	Exercise cards
• 	Summary‑level information
• 	Equipment indicators
(UIS‑PROG‑05)
6. 	System displays a “Start Workout” button at the bottom of the summary.
(BR‑WORK‑01)
7. 	User reviews the exercise list to determine feasibility.

## Alternate Flows
A1 — Day Contains Optional Exercises
• 	System marks optional exercises with a distinct visual indicator.
(BR‑PROG‑17)
A2 — Day Contains Rest or Recovery
• 	System displays a rest‑day message (e.g., “Rest / Recovery”).
• 	No exercise list is shown.
(BR‑PROG‑14)
A3 — User Navigates Back to Program Detail
• 	User selects “Back”.
• 	System returns to the Program Detail view (UC‑PROG‑01).

## Exception Flows
E1 — Exercise Data Not Found
• 	One or more exercises cannot be retrieved.
• 	System displays:
“Unable to load exercises for this day.”
• 	System logs the event.
E2 — Unauthorized Access
• 	User attempts to view exercises for a restricted program.
• 	System blocks access and logs a security event.
(BR‑PROG‑20)
E3 — Data Load Failure
• 	System cannot retrieve exercise summaries.
• 	System displays fallback content or an error state.
• 	System logs the failure.

## Non‑Functional Requirements
• 	Performance: Exercise summary list must load within 200ms.
• 	Accessibility: Exercise cards must support keyboard navigation and ARIA labels.
• 	Responsiveness: Layout must adapt to mobile, tablet, and desktop.
• 	Security: Only authorized users may view exercise summaries.
• 	Consistency: Summary view must match the Program and Workout modules’ visual patterns.

## Related UI Screens
• 	UIS‑PROG‑01 — Program Detail Page
• 	UIS‑PROG‑03 — Day Preview Cards
• 	UIS‑PROG‑05 — Daily Workout Summary
• 	UIS‑WORK‑01 — Start Workout Screen (next module)

## Flowchart TD

    A[User Selects Day] --> B[Request Exercise Summaries]
    B --> C[Retrieve Exercise Summary Data]
    C --> D[Order Exercises by Sequence]
    D --> E[Render Daily Workout Summary]
    E --> F[Display Start Workout Button]
    F --> END[User Reviews Exercises]
