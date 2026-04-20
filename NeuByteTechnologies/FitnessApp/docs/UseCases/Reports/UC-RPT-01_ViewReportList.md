# UC‑RPT‑01 — View Report List
Use Case ID
UC‑RPT‑01
Use Case Name
View Report List
Module
Reports
## Purpose
To define how the user views the list of available reports within the Reports module. This use case covers loading report metadata, displaying the report list, and ensuring the user can navigate to individual report detail screens.
## Primary Actor
Authenticated User
## Stakeholders & Interests
• 	User — wants quick access to progress and performance insights.
• 	System — must retrieve report definitions and availability efficiently.
• 	Product Owner — wants a clear, intuitive reporting experience that drives engagement.
• 	UX/UI Team — requires consistent list formatting and accessibility compliance.
• 	Analytics Team — tracks which reports users access most frequently.
• 	Security Team — ensures users only access reports they are authorized to view.

## Preconditions
• 	User is authenticated.
• 	Reports module is enabled for the user.
• 	At least one report type exists in the system.

## Postconditions
Success
• 	System displays the list of available reports.
• 	Each report card shows:
• 	Report title
• 	Short description
• 	Report category (Progress, Trends, History, etc.)
• 	Optional: last generated date
• 	User may select a report to view its details (UC‑RPT‑02).
Failure
• 	Report list does not load.
• 	System may display an error message.

## Trigger
User navigates to the Reports section from the main menu or dashboard.

## Main Success Scenario (Basic Flow — View Report List)
1. 	User selects Reports from the main menu or dashboard.
(BR‑RPT‑01)
2. 	System receives the request to load the report list.
(FS‑RPT‑LIST‑01)
3. 	System retrieves the list of available reports for the user, including:
• 	Report ID
• 	Report title
• 	Report description
• 	Report category
• 	Report availability rules
(BR‑RPT‑02 → BR‑RPT‑06)
4. 	System retrieves optional metadata (if applicable):
• 	Last generated date
• 	Data freshness indicators
(BR‑RPT‑07)
5. 	System renders the Report List page, displaying:
• 	Report cards
• 	Categories or grouping (optional)
(UIS‑RPT‑01)
6. 	System sets focus to the top of the Report List screen for accessibility.
(SRS‑A11Y‑01)
7. 	User views the list of available reports.

## Alternate Flows
A1 — No Reports Available
• 	System detects zero available reports.
• 	System displays an empty state message:
“No reports available at this time.”
(BR‑RPT‑12)
A2 — Reports Grouped by Category
If grouping is enabled:
• 	System organizes reports into categories (e.g., Progress, Trends, History).
• 	UI displays category headers.
(UIS‑RPT‑01)
A3 — Reports with Data Freshness Warnings
• 	System displays a badge (e.g., “Outdated”) if report data is stale.
(BR‑RPT‑07)

## Exception Flows
E1 — Report Metadata Not Found
• 	System cannot retrieve report definitions.
• 	System displays:
“Unable to load reports. Please try again.”
• 	System logs the event.
E2 — Unauthorized Access
• 	User attempts to access a restricted report.
• 	System hides the report or displays an access‑denied message.
(BR‑RPT‑10)
E3 — Data Store Failure
• 	System cannot retrieve report list.
• 	System displays fallback content or an error state.
• 	System logs the failure.

## Non‑Functional Requirements
• 	Performance: Report list must load within 300ms.
• 	Accessibility: Report cards must support keyboard navigation and ARIA labels.
• 	Responsiveness: Layout must adapt to mobile, tablet, and desktop.
• 	Security: Only authorized reports may be displayed.
• 	Scalability: Must support large numbers of report types.
• 	Consistency: Report list must match the visual patterns of the Reports module.

## Related UI Screens
• 	UIS‑RPT‑01 — Report List Page
• 	UIS‑GLOBAL‑HEADER‑01
• 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    A[User Navigates to Reports] --> B[Load Report Metadata]
    B --> C[Retrieve Report List]
    C --> D[Retrieve Optional Metadata]
    D --> E[Render Report List Page]
    E --> F[Set Accessibility Focus]
    F --> END[Report List Displayed]
