# UC‑WT‑01 — Track Body Weight
Use Case ID
UC‑WT‑01
Use Case Name
Track Body Weight
Module
Weight Tracking

## Purpose
To define how an authenticated user records, edits, and views their body weight entries within the Weight Tracking module. This use case enables users to maintain a historical log of weight measurements, which supports downstream features such as goal tracking, trend visualization, and insights.
## Primary Actor
Authenticated User
## Stakeholders & Interests
• 	User — wants to easily log weight entries and monitor progress over time.
• 	System — must store entries accurately and update dependent features (trend, insights).
• 	Product Owner — wants a simple, intuitive tracking experience.
• 	UX/UI Team — requires clean input flows and clear historical presentation.
• 	Analytics Team — uses weight data for insights and engagement metrics.

## Preconditions
• 	User is authenticated.
• 	User navigates to the Weight Tracking page.
• 	Weight Tracking feature is enabled.

## Postconditions
Success
• 	Weight entry is saved.
• 	Historical list is updated.
• 	Trend graph is recalculated.
• 	Insights module receives updated data.
Failure
• 	Entry is not saved.
• 	System displays an error message.
• 	No changes are made to historical data.

## Trigger
User navigates to the Weight Tracking page or selects Add Weight Entry.

## Main Success Scenario (Basic Flow — Track Body Weight)
1. 	User navigates to the Weight Tracking page.
(BR‑WT‑01)
2. 	System displays:
• 	Current weight (if available)
• 	Historical weight entries
• 	Weight trend graph
(UIS‑WT‑01)
3. 	User selects Add Weight Entry.
4. 	System displays the Add/Edit Weight Entry form.
(UIS‑WT‑02)
5. 	User enters:
• 	Weight value
• 	Date (default = today)
(BR‑WT‑02)
6. 	System validates the input (numeric, within allowed range).
(BR‑WT‑03)
7. 	User selects Save.
8. 	System stores the new weight entry.
(DATA‑WT‑Entry)
9. 	System updates:
• 	Historical list
• 	Trend graph
• 	Insights data
(FS‑WT‑01)
10. 	System displays the updated Weight Tracking page.

## Alternate Flows
A1 — User Edits an Existing Entry
• 	User selects an entry from the historical list.
• 	System displays the entry in edit mode.
• 	User updates the value or date.
• 	System saves changes and updates dependent components.
A2 — User Deletes an Entry
• 	User selects Delete on an entry.
• 	System confirms deletion.
• 	System removes the entry and updates the trend graph.
A3 — No Weight History Exists
• 	System displays:
“No entries yet. Add your first weight entry.”
• 	Trend graph is hidden or replaced with a placeholder.
(BR‑WT‑06)
A4 — User Cancels Entry
• 	User selects Cancel.
• 	System returns to the Weight Tracking page without saving.

## Exception Flows
E1 — Invalid Weight Value
• 	System displays:
“Enter a valid weight.”
• 	User corrects input.
E2 — Out‑of‑Range Weight
• 	System displays:
“Weight must be between X and Y.”
• 	User corrects input.
E3 — Database Save Failure
• 	System displays:
“Unable to save your entry. Please try again.”
• 	System logs the failure.
E4 — Network Timeout
• 	System displays a retry option.

## Non‑Functional Requirements
• 	Performance: Entry save must complete within 150ms.
• 	Reliability: All entries must be stored with timestamp accuracy.
• 	Security: Only authenticated users may access or modify entries.
• 	Accessibility: Forms must support keyboard navigation and ARIA labels.
• 	Usability: Input must be simple, fast, and mobile‑friendly.
• 	Consistency: UI must match global form and list patterns.

## Related UI Screens
• 	UIS‑WT‑01 — Weight Tracking Page
• 	UIS‑WT‑02 — Add/Edit Weight Entry
• 	UIS‑WT‑03 — Weight Trend Graph
• 	UIS‑WT‑04 — Weight Insights

## Flowchart TD

    A[User Opens Weight Tracking Page] --> B[Display Current Weight & History]
    B --> C[User Selects Add Weight Entry]
    C --> D[Display Add/Edit Form]
    D --> E[User Enters Weight & Date]
    E --> F[Validate Input]
    F --> G{Valid?}

    G -->|No| H[Display Validation Error]
    H --> E

    G -->|Yes| I[Save Weight Entry]
    I --> J[Update History & Trend Graph]
    J --> K[Display Updated Weight Tracking Page]
    K --> END