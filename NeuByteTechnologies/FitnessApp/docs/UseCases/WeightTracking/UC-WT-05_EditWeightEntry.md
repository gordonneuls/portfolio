# UC‑WT‑05 — Edit Weight Entry
Use Case ID
UC‑WT‑05
Use Case Name
Edit Weight Entry
# Actor
Authenticated User
## Purpose
To allow users to correct or update a previously recorded weight entry so their weight history remains accurate, consistent, and reflective of their true progress, ensuring downstream analytics, trends, and goal tracking remain reliable.
## Description / Goal
The user updates a previously logged weight entry to correct mistakes or adjust values. The system validates the updated entry, saves the changes, and updates all dependent calculations such as trends, insights, and goal progress.
## Preconditions
- 	User is authenticated.
- 	At least one weight entry exists.
- 	The selected entry is editable (not locked by system rules).
## Postconditions
- 	The weight entry is updated.
- 	Trends, insights, and goal progress are recalculated.
- 	The updated entry is reflected in the weight history list and charts.
- 	An analytics event is logged (UC‑ANL‑03).
## Trigger
User selects an existing weight entry and chooses “Edit.”

## Main Success Scenario
1. 	User opens the Weight Tracking module.
2. 	System displays the list of recorded weight entries.
3. 	User selects an entry to edit.
4. 	System displays the entry details in an editable form.
5. 	User updates the weight value and/or date.
6. 	System validates the updated values.
7. 	System saves the updated entry.
8. 	System recalculates trends and insights.
9. 	System updates the UI to reflect the edited entry.
10. 	System logs the edit event.

## Alternate Flows
A1 — User Cancels Edit
1. 	User selects “Cancel.”
2. 	System discards changes.
3. 	System returns to the weight history list.
A2 — User Changes Only the Date
1. 	User updates the date but not the weight value.
2. 	System validates the new date.
3. 	System saves the updated entry.
4. 	System reorders the history list accordingly.

## Exceptions / Error Handling
E1 — Invalid Weight Value
- 	System displays inline validation (e.g., “Weight must be between X and Y”).
- 	“Save” is disabled until corrected.
E2 — Invalid Date
- 	System displays a validation message (e.g., “Date cannot be in the future”).
- 	No update is saved.
E3 — Save Failure
- 	System displays an error banner (UC‑ERR‑01).
- 	System logs the error event (UC‑ERR‑13).
- 	User may retry (UC‑ERR‑02).

## Business Rules
- 	BR‑WT‑07: Weight values must fall within system‑defined limits.
- 	BR‑WT‑08: Edited entries must maintain chronological consistency.
- 	BR‑WT‑09: Editing an entry must trigger recalculation of trends.
- 	BR‑WT‑10: Edited entries must be logged as analytics events.

## Related Requirements
- 	BR‑WT‑07 → BR‑WT‑10
- 	SRS‑WT‑05.x (editing logic)
- 	SRS‑ANL‑03.x (analytics logging)