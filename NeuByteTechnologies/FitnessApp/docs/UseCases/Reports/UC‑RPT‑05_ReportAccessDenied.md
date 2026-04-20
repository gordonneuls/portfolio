# UC‑RPT‑05 — Report Access Denied
Use Case ID
UC‑RPT‑05
Use Case Name
Report Access Denied
Module
Reports
## Purpose
To define how the system responds when a user attempts to access a report they are not authorized to view. This use case ensures that restricted reports are protected, unauthorized access attempts are logged, and the user receives a clear, secure, and user‑friendly message.
This UC covers authorization failure, not availability or technical failure (those are covered in UC‑RPT‑04 and UC‑RPT‑06).
## Primary Actor
Authenticated User
## Stakeholders & Interests
• 	User — wants to understand why they cannot access a report.
• 	System — must enforce access control and prevent unauthorized viewing.
• 	Product Owner — wants predictable, secure behavior for restricted content.
• 	UX/UI Team — requires consistent error messaging and navigation.
• 	Security Team — requires logging of unauthorized access attempts.
• 	Analytics Team — monitors access failures for licensing or entitlement issues.

## Preconditions
• 	User is authenticated.
• 	User has selected a report from the Report List (UC‑RPT‑02).
• 	System attempts to load the report metadata or Power BI embed.
• 	Report requires permissions the user does not have.

## Postconditions
Success (Handled Failure)
• 	System displays a Report Access Denied message.
• 	User is prevented from viewing the report.
• 	System logs the unauthorized access attempt.
• 	User is provided with navigation options (Back to Report List).
Failure (Unhandled Failure)
• 	System logs the event.
• 	User may see a generic fallback message.

## Trigger
System detects that the user does not have permission to access the selected report.

## Main Success Scenario (Basic Flow — Report Access Denied)
1. 	User selects a report from the Report List (UC‑RPT‑02).
(BR‑RPT‑01)
2. 	System retrieves the report metadata.
(FS‑RPT‑DETAIL‑01)
3. 	System checks the user’s access rights for the selected report.
(BR‑RPT‑10)
4. 	System determines that the user does not have permission to view the report.
5. 	System prevents the Power BI viewer from loading.
6. 	System displays the Report Access Denied screen, including:
• 	Report title (if known)
• 	Clear access‑denied message
• 	Optional guidance (e.g., “Contact your administrator”)
• 	“Back to Reports” button
(UIS‑RPT‑04)
7. 	System logs the unauthorized access attempt.
(SRS‑SEC‑05)
8. 	User returns to the Report List.

## Alternate Flows
A1 — Report Hidden from List
If the system uses “security trimming”:
• 	Restricted reports do not appear in the Report List.
• 	UC‑RPT‑05 is triggered only if the user attempts to access a report via direct link or stale bookmark.
A2 — Report Requires Premium License
• 	System displays:
“This report requires a premium license.”
• 	User is directed back to the Report List.
A3 — Report Requires Elevated Role
• 	System displays:
“This report is restricted to administrators.”
A4 — Report Requires Organizational Access
• 	System displays:
“This report is restricted to your organization.”

## Exception Flows
E1 — Access Check Fails
• 	System cannot validate access due to a backend error.
• 	System displays:
“Unable to verify access. Please try again.”
• 	System logs the event.
E2 — Power BI Token Denied
• 	Power BI rejects the embed token due to insufficient permissions.
• 	System displays the Access Denied screen.
E3 — Report Metadata Missing
• 	System cannot determine whether the user should have access.
• 	System displays a generic fallback:
“Unable to load this report.”

## Non‑Functional Requirements
• 	Security: Access control must be enforced at both the app and Power BI levels.
• 	Performance: Access‑denied state must display within 150ms after failure detection.
• 	Reliability: All unauthorized attempts must be logged.
• 	Accessibility: Error screen must support keyboard navigation and ARIA labels.
• 	Consistency: Messaging must follow global error‑handling patterns.
• 	Privacy: Error messages must not reveal sensitive internal details.

## Related UI Screens
• 	UIS‑RPT‑01 — Report List Page
• 	UIS‑RPT‑02 — Report Detail Page
• 	UIS‑RPT‑04 — Report Access Denied Screen
• 	UIS‑GLOBAL‑HEADER‑01
• 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    A[User Selects Report] --> B[Retrieve Report Metadata]
    B --> C[Check User Access]
    C --> D{Access Allowed?}

    D -->|Yes| E[Load Report Detail (UC-RPT-03)]
    D -->|No| F[Display Access Denied Screen]

    F --> G[Log Unauthorized Attempt]
    G --> H[User Returns to Report List]
    H --> END
