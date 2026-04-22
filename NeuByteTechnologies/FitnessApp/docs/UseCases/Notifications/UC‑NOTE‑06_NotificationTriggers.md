# UC‑NOTE‑06 — Notification Triggers

Use Case ID
UC‑NOTE‑06
Use Case Name
Notification Triggers
Module
Notifications / System Events
## Purpose
To define the system‑level events and conditions that generate notifications for the user. This use case covers automatic notification creation, validation, storage, and delivery to the Notification Center.
## Primary Actor
System
## Secondary Actor
Authenticated User (recipient)
## Stakeholders & Interests
-	User — wants timely, relevant notifications about activity, progress, and system events.
-	System — must generate notifications reliably and securely.
-	Product Owner — wants consistent, meaningful notifications that enhance engagement.
-	UX/UI Team — requires predictable formatting and categorization.
-	Security Team — ensures notifications are only created for valid, authorized events.
-	Analytics Team — tracks notification creation and delivery metrics.

## Preconditions
-	User account exists and is active.
-	Notification service is operational.
-	Triggering event occurs (user action, scheduled event, or system event).
-	Notification type is defined in the business rules.

## Postconditions
Success
-	Notification is created and stored in the data store.
-	Notification is associated with the correct user.
-	Notification appears in the Notification Center (UC‑NOTE‑01).
-	Unread count is updated (UC‑NOTE‑05).
Failure
-	Notification is not created.
-	System logs the failure.
-	No user‑visible notification appears.

## Trigger
Any system‑defined event that requires notifying the user, such as:
-	Completion of a workout
-	New weekly report available
-	Weight trend change
-	Program milestone reached
-	Account or profile updates
-	System alerts or errors
-	Scheduled reminders
(BR‑N‑01 → BR‑N‑07)

## Main Success Scenario (Basic Flow — Generate Notification)
1. 	A triggering event occurs (e.g., workout logged, report generated).
(BR‑N‑01)
2. 	System identifies the user associated with the event.
(BR‑N‑15)
3. 	System determines the appropriate notification type based on business rules.
(BR‑N‑07)
4. 	System constructs a notification object containing:
-	Notification ID
-	User ID
-	Title
-	Message body
-	Timestamp
-	Notification type
-	Read/unread status (default: unread)
(BR‑N‑08)
5. 	System validates the notification payload for completeness and correctness.
(FS‑NOTE‑DATA‑03)
6. 	System stores the notification in the data store.
(BR‑N‑06)
7. 	System increments the user’s unread notification count.
(BR‑N‑02)
8. 	Notification becomes available for display in the Notification Center.
(UC‑NOTE‑01)

## Alternate Flows
A1 — Scheduled Notification Trigger
Examples:
-	Weekly progress report
-	Program reminders
-	Daily workout reminders
Flow:
-	A scheduled job triggers the event.
-	System performs the same steps as the Basic Flow.
A2 — Multi‑User Notifications
If a system event affects multiple users:
-	System identifies all impacted users.
-	System generates a notification for each user.
-	System stores each notification individually.
A3 — Notification Suppression Rules
If user has disabled certain notification types (optional feature):
-	System checks user preferences.
-	System suppresses the notification.
-	No record is created.

## Exception Flows
E1 — Invalid Notification Payload
-	Required fields missing or invalid.
-	System rejects the notification.
-	System logs the error.
-	No notification is created.
E2 — Data Store Failure
-	System cannot save the notification.
-	System logs the failure.
-	Notification is not delivered.
E3 — Unauthorized Trigger
-	Event attempts to generate a notification for a user it does not belong to.
-	System blocks the action.
(BR‑N‑15)
E4 — Duplicate Notification Prevention
If the same event fires multiple times:
-	System detects duplicate conditions.
-	System prevents duplicate notifications (per BR‑N‑14).

## Non‑Functional Requirements
-	Performance: Notification creation must complete within 150ms.
-	Reliability: System must not lose notifications during peak load.
-	Security: Only valid, authorized events may generate notifications.
-	Scalability: Must support high‑volume notification generation.
-	Data Integrity: Notification records must remain consistent across devices.
-	Auditability: All notification creation events must be logged.

## Related UI Screens
-	UIS‑NOTE‑01 — Notification List
-	UIS‑NOTE‑02 — Notification Detail
-	UIS‑GLOBAL‑HEADER‑01 — Notification Icon

## Flowchart TD

    A[Triggering Event Occurs] --> B[Identify User]
    B --> C[Determine Notification Type]
    C --> D[Construct Notification Object]
    D --> E[Validate Payload]
    E --> F{Valid?}

    F -->|No| F1[Log Error<br/>Abort Creation]

    F -->|Yes| G[Store Notification]
    G --> H[Update Unread Count]
    H --> I[Make Available to UI]
    I --> END[Notification Created]