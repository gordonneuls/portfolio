# UC‑RPT‑02 — Select Report
## Purpose
To define how the user selects a report from the Report List and navigates to the Report Detail screen. This use case covers identifying the selected report, loading its metadata, and launching the embedded Power BI report viewer.
This UC does not define filtering, exporting, or interacting with the report — those are handled natively by Power BI.
Use Case ID
UC‑RPT‑02
Use Case Name
Select Report
Module
Reports
## Primary Actor
Authenticated User
## Stakeholders & Interests
• 	User — wants to open a report and view insights quickly.
• 	System — must load the correct report and pass the correct embed URL or token.
• 	Product Owner — wants a smooth, intuitive reporting experience.
• 	UX/UI Team — requires consistent navigation and loading states.
• 	Analytics Team — tracks which reports users open.
• 	Security Team — ensures users only access reports they are authorized to view.

## Preconditions
• 	User is authenticated.
• 	Report List is displayed (UC‑RPT‑01).
• 	Selected report exists and is available to the user.
• 	Power BI embed configuration is valid.

## Postconditions
Success
• 	System loads the selected report’s metadata.
• 	System opens the Report Detail screen.
• 	Power BI report loads in the embedded viewer.
• 	User can interact with the report using Power BI’s built‑in features.
Failure
• 	Report does not load.
• 	System may display an error message.

## Trigger
User selects a report card from the Report List.

## Main Success Scenario (Basic Flow — Select Report)
1. 	User selects a report from the Report List.
(BR‑RPT‑01)
2. 	System receives the request to load the selected report.
(FS‑RPT‑DETAIL‑01)
3. 	System retrieves the report metadata, including:
• 	Report ID
• 	Report title
• 	Report description
• 	Power BI embed URL or Publish‑to‑Web URL
(BR‑RPT‑02 → BR‑RPT‑06)
4. 	System validates that the user has access to the report.
(BR‑RPT‑10)
5. 	System loads the Report Detail screen.
(UIS‑RPT‑02)
6. 	System initializes the Power BI viewer using the embed URL.
(FS‑RPT‑DETAIL‑02)
7. 	Power BI loads the report.
(Power BI handles filtering, exporting, drill‑downs)
8. 	User views and interacts with the report.

## Alternate Flows
A1 — Report Requires Authentication Token (Embed Mode)
• 	System retrieves a Power BI embed token.
• 	System injects the token into the viewer.
• 	Report loads normally.
A2 — Report Uses Publish‑to‑Web
• 	System loads the public embed URL.
• 	No authentication token is required.
A3 — Report Has a Data Freshness Warning
• 	System displays a badge or message (optional):
“Last refreshed: 2 days ago.”

## Exception Flows
E1 — Report Not Found
• 	Report ID is invalid or missing.
• 	System displays:
“This report is no longer available.”
• 	System logs the event.
E2 — Unauthorized Access
• 	User attempts to open a restricted report.
• 	System blocks access and logs a security event.
(BR‑RPT‑10)
E3 — Power BI Embed Failure
• 	Power BI viewer fails to load.
• 	System displays:
“Unable to load report. Please try again.”
• 	System logs the failure.
E4 — Network Timeout
• 	System cannot load the report.
• 	UI may retry or show fallback messaging.

## Non‑Functional Requirements
• 	Performance: Report metadata must load within 200ms.
• 	Security: Only authorized users may access reports.
• 	Reliability: Embed URLs and tokens must be valid and refreshed as needed.
• 	Accessibility: Report Detail screen must support keyboard navigation and ARIA labels.
• 	Responsiveness: Power BI viewer must adapt to mobile, tablet, and desktop.
• 	Consistency: Navigation must match the rest of the Reports module.

## Related UI Screens
• 	UIS‑RPT‑01 — Report List Page
• 	UIS‑RPT‑02 — Report Detail Page
• 	UIS‑GLOBAL‑HEADER‑01
• 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    A[User Selects Report] --> B[Load Report Metadata]
    B --> C[Validate Access]
    C --> D[Load Report Detail Screen]
    D --> E[Initialize Power BI Viewer]
    E --> F[Power BI Loads Report]
    F --> END[User Views Report]