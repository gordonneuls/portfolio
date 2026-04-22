# UC‑WT‑06 — Delete Weight Entry
Use Case ID
UC‑WT‑06
Use Case Name
Delete Weight Entry
## Actor
Authenticated User
## Purpose
To allow users to remove an incorrect or unwanted weight entry so their weight history remains clean, accurate, and free of erroneous data, ensuring that trends, insights, and goal progress calculations remain trustworthy and meaningful.
## Description / Goal
The user deletes a previously recorded weight entry that is no longer valid. The system confirms the action, removes the entry, updates dependent calculations, and refreshes the weight history and trend visualizations.
## Preconditions
- 	User is authenticated.
- 	At least one weight entry exists.
- 	The selected entry is eligible for deletion.
## Postconditions
- 	The weight entry is permanently removed.
- 	Trends, insights, and goal progress are recalculated.
- 	The updated history and charts reflect the deletion.
- 	An analytics event is logged (UC‑ANL‑03).
## Trigger
User selects an existing weight entry and chooses “Delete.”

## Main Success Scenario
1. 	User opens the Weight Tracking module.
2. 	System displays the list of recorded weight entries.
3. 	User selects an entry to delete.
4. 	System displays a confirmation dialog.
5. 	User confirms the deletion.
6. 	System removes the selected entry.
7. 	System recalculates trends and insights.
8. 	System updates the UI to reflect the removal.
9. 	System logs the deletion event.

## Alternate Flows
A1 — User Cancels Deletion
1. 	User selects “Delete” on an entry.
2. 	System displays a confirmation dialog.
3. 	User selects “Cancel.”
4. 	System closes the dialog and makes no changes.
A2 — Entry Is Already Deleted (Race Condition)
1. 	User attempts to delete an entry.
2. 	System detects the entry no longer exists.
3. 	System displays a non‑blocking message (e.g., “This entry has already been removed”).
4. 	System refreshes the weight history list.

## Exceptions / Error Handling
E1 — Delete Operation Fails
- 	System displays an error banner (UC‑ERR‑01).
- 	System logs the error event (UC‑ERR‑13).
- 	User may retry (UC‑ERR‑02).
E2 — Entry Cannot Be Deleted Due to Data Integrity Rules
- 	System displays a message explaining the restriction.
- 	No deletion occurs.

## Business Rules
- 	BR‑WT‑08: Deleted entries must be removed from all calculations.
- 	BR‑WT‑09: Deletion must trigger recalculation of trends.
- 	BR‑WT‑10: Deletion events must be logged for analytics.
- 	BR‑WT‑11: User must confirm deletion before removal.

## Related Requirements
- 	BR‑WT‑08 → BR‑WT‑11
- 	SRS‑WT‑06.x (deletion logic)
- 	SRS‑ANL‑03.x (analytics logging)