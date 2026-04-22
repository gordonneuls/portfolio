UC‑RPT‑06 — Report Load Failure
Purpose
To define how the system responds when a report fails to load due to a technical issue during initialization or rendering of the embedded Power BI report. This use case ensures the user receives a clear, actionable message and that the system logs the failure for diagnostics.
This UC is distinct from:
- 	UC‑RPT‑04_ReportUnavailable → report removed, deprecated, or offline
- 	UC‑RPT‑05_ReportAccessDenied → user lacks permissions
UC‑RPT‑06 focuses on technical failures during loading.

Use Case ID
UC‑RPT‑06
Use Case Name
Report Load Failure
Module
Reports
Primary Actor
Authenticated User
Stakeholders & Interests
- 	User — wants to understand why the report failed to load and what to do next.
- 	System — must detect load failures and provide a graceful fallback.
- 	Product Owner — wants predictable, user‑friendly error handling.
- 	UX/UI Team — requires consistent error states and retry patterns.
- 	Security Team — ensures failures do not expose sensitive information.
- 	Analytics/DevOps — needs logs for diagnosing failures.

Preconditions
- 	User is authenticated.
- 	User has selected a report (UC‑RPT‑02).
- 	Report metadata exists.
- 	System attempts to initialize the Power BI viewer.

Postconditions
Success (Handled Failure)
- 	System displays a Report Load Failure message.
- 	User is informed of the issue.
- 	User is provided with options to retry or return to the Report List.
- 	System logs the failure.
Failure (Unhandled Failure)
- 	System logs the event.
- 	User may see a generic fallback message.

Trigger
System detects a technical failure while attempting to load the report.

Main Success Scenario (Basic Flow — Report Load Failure)
1. 	User selects a report from the Report List (UC‑RPT‑02).
(BR‑RPT‑01)
2. 	System loads the Report Detail screen.
(UIS‑RPT‑02)
3. 	System attempts to initialize the Power BI viewer.
(FS‑RPT‑DETAIL‑02)
4. 	A technical failure occurs, such as:
- 	Network timeout
- 	Power BI embed error
- 	Token retrieval failure
- 	JavaScript runtime error
- 	Cross‑origin or iframe restrictions
(BR‑RPT‑11)
5. 	System detects the failure and stops the load process.
6. 	System displays the Report Load Failure screen, including:
- 	Clear error message
- 	Optional retry button
- 	“Back to Reports” button
(UIS‑RPT‑05)
7. 	System logs the failure event for diagnostics.
(SRS‑LOG‑02)
8. 	User retries or returns to the Report List.

Alternate Flows
A1 — Retry Succeeds
- 	User selects Retry.
- 	System attempts to reload the report.
- 	Report loads successfully.
- 	Flow continues to UC‑RPT‑03_ViewReportDetail.
A2 — Retry Fails Again
- 	System displays the same error state.
- 	User may retry again or return to the Report List.
A3 — Partial Load Failure
If some visuals load but others fail:
- 	System displays a partial‑load warning:
“Some visuals failed to load.”
- 	User may retry or continue viewing partial data.
A4 — Token Expired
- 	System attempts to refresh the embed token.
- 	If refresh succeeds → report loads normally.
- 	If refresh fails → display load failure screen.

Exception Flows
E1 — Power BI Service Outage
- 	System detects a service‑level outage.
- 	System displays:
“Power BI is currently unavailable.”
E2 — Browser Compatibility Issue
- 	System detects unsupported browser features.
- 	System displays:
“Your browser does not support this report.”
E3 — Script Execution Blocked
- 	Browser blocks Power BI scripts (e.g., due to CSP).
- 	System displays a load failure message.
E4 — Report Metadata Corrupted
- 	System cannot parse embed configuration.
- 	System displays a generic fallback:
“Unable to load this report.”

Non‑Functional Requirements
- 	Performance: Failure state must display within 150ms after detection.
- 	Reliability: All load failures must be logged with error codes.
- 	Security: Error messages must not reveal sensitive internal details.
- 	Accessibility: Error screen must support keyboard navigation and ARIA labels.
- 	Consistency: Error messaging must follow global UI patterns.
- 	Resilience: System should attempt token refresh before failing.

Related UI Screens
- 	UIS‑RPT‑01 — Report List Page
- 	UIS‑RPT‑02 — Report Detail Page
- 	UIS‑RPT‑05 — Report Load Failure Screen
- 	UIS‑GLOBAL‑HEADER‑01
- 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    A[User Selects Report] --> B[Initialize Power BI Viewer]
    B --> C{Load Successful?}

    C -->|Yes| D[Display Report Detail (UC-RPT-03)]
    C -->|No| E[Display Load Failure Screen]

    E --> F[Log Failure]
    F --> G[User Retries or Returns to Report List]
    G --> END