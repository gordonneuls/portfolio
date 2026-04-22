# UC‑RPT‑04 — Report Unavailable
Use Case ID
UC‑RPT‑04
Use Case Name
Report Unavailable
Module
Reports

## Purpose
To define how the system responds when a user attempts to access a report that is temporarily or permanently unavailable. This use case covers error detection, fallback messaging, and user guidance. It ensures the user receives a clear, actionable response instead of a broken or confusing experience.

## Primary Actor
Authenticated User

## Stakeholders & Interests
- 	User — wants to understand why a report cannot be accessed and what to do next.
- 	System — must detect unavailability conditions and provide a graceful fallback.
- 	Product Owner — wants predictable, user‑friendly error handling.
- 	UX/UI Team — requires consistent error states and messaging patterns.
- 	Analytics Team — tracks report failures for monitoring and improvement.
- 	Security Team — ensures restricted reports do not leak information.

## Preconditions
- 	User is authenticated.
- 	User has selected a report from the Report List (UC‑RPT‑02).
- 	System attempts to load the report metadata or Power BI embed.

## Postconditions
Success (Handled Failure)
- 	System displays a clear “Report Unavailable” message.
- 	User is informed of the reason (if appropriate).
- 	User is provided with navigation options (Back to Report List, Retry).
Failure (Unhandled Failure)
- 	System logs the error.
- 	User may see a generic fallback message.

## Trigger
System detects that the selected report cannot be loaded.

## Main Success Scenario (Basic Flow — Report Unavailable)
1. 	User selects a report from the Report List (UC‑RPT‑02).
(BR‑RPT‑01)
2. 	System attempts to load the report metadata or Power BI embed.
(FS‑RPT‑DETAIL‑01)
3. 	System detects that the report is unavailable due to one of the following:
- 	Report has been removed
- 	Report is temporarily offline
- 	Power BI dataset is unavailable
- 	Report is deprecated
- 	Report is under maintenance
(BR‑RPT‑11)
4. 	System prevents the report viewer from loading.
5. 	System displays the Report Unavailable screen, including:
- 	Report title (if known)
- 	Clear error message
- 	Optional reason (if appropriate)
- 	“Back to Reports” button
(UIS‑RPT‑03)
6. 	System logs the unavailability event for monitoring.
(SRS‑LOG‑02)
7. 	User returns to the Report List or retries.

## Alternate Flows
A1 — Report Temporarily Offline
- 	System displays:
“This report is temporarily unavailable. Please try again later.”
- 	User may retry.
A2 — Report Deprecated
- 	System displays:
“This report is no longer supported.”
- 	System may show a link to a replacement report.
A3 — Report Removed
- 	System displays:
“This report is no longer available.”
- 	Report may be removed from the list on next refresh.
A4 — Power BI Service Outage
- 	System displays:
“Power BI is currently unavailable.”
- 	User may retry or return to the Report List.

## Exception Flows
E1 — Report Metadata Missing
- 	System cannot retrieve report metadata.
- 	System displays a generic fallback:
“Unable to load this report.”
- 	System logs the event.
E2 — Unauthorized Access (Handled Separately)
If the issue is access‑related, system triggers UC‑RPT‑05_ReportAccessDenied instead.
E3 — Network Timeout
- 	System displays:
“Network issue. Please try again.”

## Non‑Functional Requirements
- 	Performance: Error state must display within 150ms after failure detection.
- 	Reliability: System must handle all Power BI and metadata failures gracefully.
- 	Security: Error messages must not reveal sensitive internal details.
- 	Accessibility: Error screen must support keyboard navigation and ARIA labels.
- 	Consistency: Error messaging must follow global UI patterns.

## Related UI Screens
- 	UIS‑RPT‑01 — Report List Page
- 	UIS‑RPT‑02 — Report Detail Page
- 	UIS‑RPT‑03 — Report Unavailable Screen
- 	UIS‑GLOBAL‑HEADER‑01
- 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD
    A[User Selects Report] --> B[Attempt to Load Report]
    B --> C{Report Available?}

    C -->|Yes| D[Load Report Detail (UC-RPT-03)]
    C -->|No| E[Display Report Unavailable Screen]

    E --> F[Log Error]
    F --> G[User Returns to Report List or Retries]
    G --> END