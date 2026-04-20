# UC‑RPT‑03 — View Report Detail
Use Case ID
UC‑RPT‑03
Use Case Name
View Report Detail
Module
Reports
## Purpose
To define how the user views the selected report in the Report Detail screen. This use case covers loading the embedded Power BI report, handling access validation, and presenting the report viewer. All report interactions (filtering, exporting, drill‑downs) are handled natively by Power BI and are not part of this use case.
## Primary Actor
Authenticated User
## Stakeholders & Interests
• 	User — wants to view insights and analytics in a clear, interactive report.
• 	System — must load the correct report and initialize the Power BI viewer.
• 	Product Owner — wants a seamless reporting experience with minimal friction.
• 	UX/UI Team — requires consistent loading states and responsive layout.
• 	Analytics Team — tracks report usage and engagement.
• 	Security Team — ensures users only access authorized reports.

## Preconditions
• 	User is authenticated.
• 	User has selected a report from the Report List (UC‑RPT‑02).
• 	Report metadata exists and includes a valid Power BI embed URL or token.
• 	Power BI service is available.

## Postconditions
Success
• 	Report Detail screen is displayed.
• 	Power BI viewer loads the selected report.
• 	User can interact with the report using Power BI’s built‑in features.
Failure
• 	Report viewer does not load.
• 	System may display an error message or fallback state.

## Trigger
User selects a report from the Report List (UC‑RPT‑02).

## Main Success Scenario (Basic Flow — View Report Detail)
1. 	User selects a report from the Report List.
(BR‑RPT‑01)
2. 	System loads the Report Detail screen.
(UIS‑RPT‑02)
3. 	System retrieves the selected report’s metadata, including:
• 	Report ID
• 	Report title
• 	Power BI embed URL or Publish‑to‑Web URL
(BR‑RPT‑02 → BR‑RPT‑06)
4. 	System validates that the user has access to the report.
(BR‑RPT‑10)
5. 	System initializes the Power BI viewer using the embed URL and, if required, an embed token.
(FS‑RPT‑DETAIL‑02)
6. 	Power BI loads the report inside the viewer.
7. 	System displays a loading indicator until the report is fully rendered.
(UIS‑RPT‑02)
8. 	User views and interacts with the report using Power BI’s native controls.

## Alternate Flows
A1 — Report Requires Embed Token
• 	System requests a Power BI embed token.
• 	System injects the token into the viewer.
• 	Report loads normally.
A2 — Report Uses Publish‑to‑Web
• 	System loads the public embed URL.
• 	No authentication token is required.
A3 — Report Has a Data Freshness Warning
• 	System displays a badge or message (optional):
“Last refreshed: [timestamp].”
A4 — User Navigates Back
• 	User selects “Back”.
• 	System returns to the Report List (UC‑RPT‑01).

## Exception Flows
E1 — Report Not Found
• 	Report ID is invalid or missing.
• 	System displays:
“This report is no longer available.”
• 	System logs the event.
E2 — Unauthorized Access
• 	User attempts to view a restricted report.
• 	System blocks access and logs a security event.
(BR‑RPT‑10)
E3 — Power BI Viewer Fails to Load
• 	Power BI returns an error.
• 	System displays:
“Unable to load report. Please try again.”
• 	System logs the failure.
E4 — Network Timeout
• 	System cannot load the report.
• 	UI may retry or show fallback messaging.

## Non‑Functional Requirements
• 	Performance: Report viewer must initialize within 300ms (excluding Power BI load time).
• 	Security: Only authorized users may access reports.
• 	Reliability: Embed URLs and tokens must be valid and refreshed as needed.
• 	Accessibility: Report Detail screen must support keyboard navigation and ARIA labels.
• 	Responsiveness: Power BI viewer must adapt to mobile, tablet, and desktop.
• 	Consistency: Viewer layout must match the Reports module’s visual patterns.

## Related UI Screens
• 	UIS‑RPT‑01 — Report List Page
• 	UIS‑RPT‑02 — Report Detail Page
• 	UIS‑GLOBAL‑HEADER‑01
• 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    A[User Selects Report] --> B[Load Report Detail Screen]
    B --> C[Retrieve Report Metadata]
    C --> D[Validate Access]
    D --> E[Initialize Power BI Viewer]
    E --> F[Power BI Loads Report]
    F --> END[User Views Report]
