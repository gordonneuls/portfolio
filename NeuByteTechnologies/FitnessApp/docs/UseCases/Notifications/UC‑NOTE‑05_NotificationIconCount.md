# UC‑NOTE‑05 — Notification Icon Count
Use Case ID
UC‑NOTE‑05
Use Case Name
Notification Icon Count
Module
Notifications / Global Header
## Purpose
To define how the system calculates and displays the unread notification count in the global header. This use case covers retrieving unread notifications, updating the count in real time, handling read/unread changes, and ensuring accuracy across sessions and devices.
## Primary Actor
System
## Secondary Actor
Authenticated User
## Stakeholders & Interests
-	User — wants an accurate, real‑time indicator of unread notifications.
-	System — must calculate unread counts efficiently and consistently.
-	Product Owner — wants a clean, intuitive notification experience.
-	UX/UI Team — requires consistent badge behavior and accessibility compliance.
-	Security Team — ensures counts reflect only the user’s own notifications.
-	Analytics Team — tracks unread counts and engagement patterns.

## Preconditions
-	User is authenticated.
-	Notification service is available.
-	Notifications exist in the data store (read or unread).

## Postconditions
Success
-	System calculates the unread notification count.
-	Notification icon displays the correct count.
-	Count updates dynamically when notifications are read or new ones arrive.
Failure
-	Count is not displayed or is inaccurate.
-	System may display a fallback state (e.g., no badge).

## Trigger
Triggered when:
-	User logs in
-	User opens any authenticated screen
-	Notifications are loaded (UC‑NOTE‑02)
-	A notification is marked as read (UC‑NOTE‑04)
-	A new notification is created (UC‑NOTE‑06)

## Main Success Scenario (Basic Flow — Calculate and Display Unread Count)
1. 	System identifies the authenticated user.
(BR‑N‑15)
2. 	System queries the notification data store for all unread notifications belonging to the user.
(BR‑N‑06)
3. 	System counts the number of unread notifications.
(BR‑N‑02)
4. 	System updates the notification icon badge with the unread count.
(BR‑N‑02)
5. 	UI displays the count in the global header.
(UIS‑GLOBAL‑HEADER‑01)
6. 	Count updates automatically when:
-	A notification is marked read (UC‑NOTE‑04)
-	A new notification is created (UC‑NOTE‑06)
-	Notifications are refreshed (UC‑NOTE‑02)

## Alternate Flows
A1 — No Unread Notifications
-	Step 2 returns zero unread notifications.
-	System hides the badge or displays “0” depending on UI spec.
(BR‑N‑12)
A2 — User Has a Large Number of Unread Notifications
-	If unread count exceeds UI threshold (e.g., 99+):
-	System displays “99+” or similar compact format.
(UIS‑GLOBAL‑HEADER‑01)
A3 — Real‑Time Updates (Optional Feature)
If push updates are enabled:
-	System receives a push event for a new notification.
-	System increments the unread count.
-	UI updates the badge without page reload.

## Exception Flows
E1 — Notification Service Unavailable
-	System cannot retrieve unread notifications.
-	System displays no badge or a fallback state.
-	System logs the failure.
(BR‑N‑13)
E2 — Unauthorized Access Attempt
-	User ID does not match notification owner.
-	System blocks the request and logs a security event.
(BR‑N‑15)
E3 — Data Corruption
-	Notification records contain invalid read/unread states.
-	System excludes invalid records.
-	System logs the issue.

## Non‑Functional Requirements
-	Performance: Unread count must load within 100ms.
-	Accuracy: Count must always reflect the true unread state.
-	Security: Only the user’s notifications may be counted.
-	Scalability: Must support large notification histories.
-	Accessibility: Badge must include ARIA labels (e.g., “3 unread notifications”).
-	Consistency: Badge behavior must match mobile, tablet, and desktop layouts.

## Related UI Screens
-	UIS‑GLOBAL‑HEADER‑01 — Header with Notification Icon
-	UIS‑NOTE‑01 — Notification List
-	UIS‑NOTE‑02 — Notification Detail

## Flowchart TD

    A[Trigger: Login / Refresh / Read / New Notification] --> B[Query Unread Notifications]
    B --> C[Count Unread Items]
    C --> D{Unread Count > 0?}

    D -->|No| D1[Hide Badge or Show 0]

    D -->|Yes| E[Update Badge with Count]
    E --> F[Display in Global Header]
    F --> END[Unread Count Updated]