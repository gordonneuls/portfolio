# UC‑RPT‑07 — Apply Report Parameters (App‑Side Filters)
Use Case ID
UC‑RPT‑07
Use Case Name
Apply Report Parameters (App‑Side Filters)
## Purpose 
To allow users to refine a report by applying application‑side filter parameters that validate user input, update the embedded Power BI configuration, and reload the report with a tailored dataset that reflects the selected criteria
## Actor
Authenticated User
## Description / Goal
The user applies one or more application‑side filters (e.g., program, date range, metric type, category) to refine the dataset before it is passed to the embedded Power BI report.
These filters are applied client‑side and merged into the Power BI embed configuration as report parameters.
## Preconditions
- 	User is authenticated.
- 	User has access to the Reports module.
- 	The selected report supports parameterized filtering.
- 	The report has successfully loaded (UC‑RPT‑06).
## Postconditions
- 	The report is reloaded or updated with the applied parameters.
- 	The UI reflects the active filters.
- 	The system logs the filter application event (UC‑ANL‑03).
## Trigger
The user selects or modifies one or more filter controls in the Reports UI.

## Main Success Scenario
1. 	User opens a report that supports app‑side filtering.
2. 	System displays available filter controls (e.g., dropdowns, date pickers, toggles).
3. 	User selects one or more filter values.
4. 	System validates the filter values (e.g., date range, required fields).
5. 	System merges the selected filter values into the Power BI embed configuration.
6. 	System reloads or updates the embedded report with the new parameters.
7. 	System displays the updated report.
8. 	System visually indicates which filters are active.
9. 	System logs the filter application event.

## Alternate Flows
A1 — User Clears Filters
1. 	User selects “Clear Filters.”
2. 	System resets all filter controls to default values.
3. 	System reloads the report with no parameters.
4. 	System updates the UI to show no active filters.
A2 — User Applies Filters Without Changing Values
1. 	User clicks “Apply” without modifying any filter values.
2. 	System detects no changes.
3. 	System does not reload the report.
4. 	System may show a subtle “No changes to apply” message (optional).

## Exceptions / Error Handling
E1 — Invalid Date Range
- 	System displays an inline validation message.
- 	“Apply” button is disabled until corrected.
- 	No request is sent to Power BI.
E2 — Unsupported Filter Combination
- 	System displays a non‑blocking warning.
- 	System applies only the supported filters.
- 	Unsupported filters are ignored in the embed configuration.
E3 — Power BI Parameter Error
- 	System displays a report‑level error state (UC‑RPT‑10).
- 	System logs the error event (UC‑ERR‑13).

## Business Rules
- 	BR‑RPT‑12: App‑side filters must be validated before being passed to Power BI.
- 	BR‑RPT‑13: Unsupported filters must be ignored, not blocked.
- 	BR‑RPT‑14: Filter changes must be logged as analytics events.
- 	BR‑RPT‑15: Date ranges must not exceed system‑defined limits.
- 	BR‑RPT‑16: Clearing filters must reset the report to its default state.

## Related Requirements
- 	BR‑RPT‑12 → BR‑RPT‑16
- 	SRS‑RPT‑07.x (parameter handling)
- 	SRS‑RPT‑03.x (date validation)
- 	SRS‑ANL‑03.x (analytics logging)