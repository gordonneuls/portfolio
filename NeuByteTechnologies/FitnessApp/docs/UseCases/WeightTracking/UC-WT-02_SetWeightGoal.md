# UC‑WT‑02 — Set Weight Goal

Use Case ID
UC‑WT‑02
Use Case Name
Set Weight Goal
Module
Weight Tracking
## Purpose
To define how an authenticated user sets or updates their personal weight goal within the Weight Tracking module. This use case enables the system to calculate progress, generate insights, and visually represent goal attainment in the trend graph.
## Primary Actor
Authenticated User
## Stakeholders & Interests
- 	User — wants to set a target weight to guide progress and motivation.
- 	System — must store the goal and update dependent features (trend, insights).
- 	Product Owner — wants a simple, intuitive goal‑setting experience.
- 	UX/UI Team — requires clear input patterns and visual goal indicators.
- 	Analytics Team — uses goal data for engagement and progress metrics.

## Preconditions
- 	User is authenticated.
- 	User has navigated to the Weight Tracking page.
- 	Weight Tracking feature is enabled.

## Postconditions
Success
- 	Weight goal is saved.
- 	Trend graph updates to include goal line.
- 	Insights module recalculates progress metrics.
- 	Goal is displayed on the Weight Tracking page.
Failure
- 	Goal is not saved.
- 	System displays an error message.
- 	No changes are made to dependent components.

## Trigger
User selects Set Goal or Edit Goal on the Weight Tracking page.

## Main Success Scenario (Basic Flow — Set Weight Goal)
1. 	User selects Set Weight Goal from the Weight Tracking page.
(BR‑WT‑06)
2. 	System displays the Set Weight Goal form.
(UIS‑WT‑02)
3. 	User enters a target weight value.
(BR‑WT‑07)
4. 	System validates the input (numeric, within allowed range).
(BR‑WT‑03)
5. 	User selects Save Goal.
6. 	System stores the weight goal in the user profile.
(DATA‑WT‑Goal)
7. 	System updates:
- 	Trend graph (goal line)
- 	Progress metrics
- 	Insights module
(FS‑WT‑02)
8. 	System displays the updated Weight Tracking page with the new goal.

## Alternate Flows
A1 — User Edits an Existing Goal
- 	User selects Edit Goal.
- 	System displays the current goal pre‑filled.
- 	User updates the value.
- 	System saves and updates dependent components.
A2 — User Removes Goal (Optional Feature)
If supported:
- 	User selects Remove Goal.
- 	System confirms removal.
- 	System deletes the goal and updates the trend graph.
A3 — User Cancels Goal Entry
- 	User selects Cancel.
- 	System returns to the Weight Tracking page without saving.

## Exception Flows
E1 — Invalid Goal Value
- 	System displays:
“Enter a valid weight.”
- 	User corrects input.
E2 — Goal Out of Range
- 	System displays:
“Goal must be between X and Y.”
- 	User corrects input.
E3 — Database Save Failure
- 	System displays:
“Unable to save your goal. Please try again.”
- 	System logs the failure.
E4 — Network Timeout
- 	System displays a retry option.

## Non‑Functional Requirements
- 	Performance: Goal save must complete within 150ms.
- 	Reliability: Goal must persist across sessions.
- 	Security: Only authenticated users may modify their goal.
- 	Accessibility: Form must support keyboard navigation and ARIA labels.
- 	Usability: Goal entry must be simple and mobile‑friendly.
- 	Consistency: UI must match global form patterns.

## Related UI Screens
- 	UIS‑WT‑01 — Weight Tracking Page
- 	UIS‑WT‑02 — Set/Edit Weight Goal
- 	UIS‑WT‑03 — Weight Trend Graph
- 	UIS‑WT‑04 — Weight Insights

## Flowchart TD

    A[User Selects Set Weight Goal] --> B[Display Goal Entry Form]
    B --> C[User Enters Goal Value]
    C --> D[Validate Input]
    D --> E{Valid?}

    E -->|No| F[Display Validation Error]
    F --> C

    E -->|Yes| G[Save Weight Goal]
    G --> H[Update Trend Graph & Insights]
    H --> I[Display Updated Weight Tracking Page]
    I --> END