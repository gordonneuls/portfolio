# FS‑REPORTS — Reports Functional Specification
## 1. Purpose
The Reports module provides authenticated users with access to analytical insights derived from their workouts, trends, and program activity. This module governs the listing of available reports, report selection, parameter passing (dates + user context), and embedding of Power BI reports within the application.
## 2. Scope
This FS covers:
- 	Report list display
- 	Report selection
- 	Report parameterization (dates + user ID)
- 	Power BI embedding
- 	Loading and error states
- 	Accessibility behaviors
- 	Session‑aware behavior
- 	Performance considerations
This module does not define:
- 	Report visual design (Power BI)
- 	Report filtering, exporting, or drill‑through (handled by Power BI)
- 	Data pipeline refresh logic (covered in DE‑PIPE‑01 Daily Refresh Pipeline FS)
## 3. References
- 	Business Requirements: BR‑RPT‑01 → BR‑RPT‑XX
- 	Use Cases: UC‑RPT‑01 → UC‑RPT‑03
- 	RTM: Reports Module Rows
- 	WCAG 2.1 AA (for accessibility behaviors)
- 	Power BI Embedded Integration Guide

## 4. Functional Requirements
FR‑RPT‑01 — Load Report List
Description:
The system shall load the list of available reports for the authenticated user.
Details:
- 	Each report entry must include: ReportID, Title, Description, Icon
- 	List must be sorted by predefined order
- 	Skeleton loading states must appear until data is available
- 	Errors must be displayed in a non‑blocking message
Maps to: BR‑RPT‑01, UC‑RPT‑01

FR‑RPT‑02 — Display Report List
Description:
The system shall display all available reports on the Reports page.
Details:
- 	Each report must be keyboard‑focusable
- 	Each report must include accessible labels
- 	Selecting a report navigates to Report Detail
- 	List must support screen‑reader navigation
Maps to: BR‑RPT‑02, UC‑RPT‑01

FR‑RPT‑03 — Navigate to Report Detail
Description:
The system shall navigate to the Report Detail page when a report is selected.
Details:
- 	Navigation must pass ReportID
- 	Navigation must close the menu if open
- 	Focus must move to the Report Detail header
Maps to: BR‑RPT‑03, UC‑RPT‑02

FR‑RPT‑04 — Display Report Metadata
Description:
The system shall display metadata for the selected report.
Details:
- 	Title
- 	Description
- 	Supported date ranges
- 	Last refresh timestamp (from Power BI dataset)
- 	Metadata must be screen‑reader accessible
Maps to: BR‑RPT‑04, UC‑RPT‑02

FR‑RPT‑05 — Date Filter Controls
Description:
The system shall provide date filter controls for the report.
Details:
- 	Preset ranges: Last Week, Last Month, Last Quarter
- 	Custom range: StartDate + EndDate
- 	Default selection: Last Week
- 	Date inputs must be validated before loading the report
Maps to: BR‑RPT‑05, UC‑RPT‑02

FR‑RPT‑06 — Apply Report Parameters
Description:
The system shall apply report parameters before embedding the report.
Details:
- 	Parameters include: UserID, StartDate, EndDate
- 	Parameters must be passed securely to Power BI
- 	Invalid parameters must block report load and show an error
Maps to: BR‑RPT‑06, UC‑RPT‑02

FR‑RPT‑07 — Embed Power BI Report
Description:
The system shall embed the Power BI report within the Report Detail page.
Details:
- 	Embedding must use Power BI secure embed token
- 	Report must load inside a responsive container
- 	Loading indicator must display until the report is ready
- 	Errors must be displayed in an accessible error region
Maps to: BR‑RPT‑07, UC‑RPT‑03

FR‑RPT‑08 — Refresh Embedded Report
Description:
The system shall refresh the embedded report when parameters change.
Details:
- 	Refresh must not reload the entire page
- 	Refresh must show a loading indicator
- 	Focus must remain on the report container
Maps to: BR‑RPT‑08, UC‑RPT‑03

FR‑RPT‑09 — Accessibility Requirements
Description:
The Reports module shall fully support accessibility behaviors.
Details:
- 	All controls must be keyboard‑reachable
- 	Screen readers must announce date filter changes
- 	Errors must be announced via ARIA live regions
- 	Focus must be managed after navigation and refresh
Maps to: BR‑RPT‑09, UC‑RPT‑01 → UC‑RPT‑03

FR‑RPT‑10 — Error Handling
Description:
The system shall handle report errors gracefully.
Details:
- 	Parameter errors → inline validation message
- 	Power BI load errors → non‑blocking error message
- 	Missing report → redirect to Report List
- 	Errors must be logged for diagnostics
Maps to: BR‑RPT‑10, UC‑RPT‑03

FR‑RPT‑11 — Performance Requirements
Description:
The Reports module shall load efficiently.
Details:
- 	Report list must load within 300ms
- 	Embedded report must load within Power BI SLA
- 	Date filter changes must refresh within 500ms
- 	Skeleton states must appear for loads > 200ms
Maps to: BR‑RPT‑11, UC‑RPT‑03

FR‑RPT‑12 — Security Requirements
Description:
The system shall ensure secure access to reports.
Details:
- 	Users may only access reports scoped to their UserID
- 	Embed tokens must be short‑lived
- 	Parameters must not expose sensitive data
- 	All report access requires authentication
Maps to: BR‑RPT‑12, UC‑RPT‑03

## 5. Non‑Functional Requirements
- 	Must meet WCAG 2.1 AA
- 	Must support keyboard‑only workflows
- 	Must support screen readers
- 	Must load within defined performance thresholds
- 	Must function without client‑side JavaScript (SSR/HTMX compatible)
- 	Must maintain secure Power BI embedding practices

## 6. Dependencies
- 	Power BI Embedded
- 	Report metadata table
- 	Session management
- 	Routing system
- 	Accessibility module
- 	Performance module

## 7. Acceptance Criteria
- 	Report list loads and displays correctly
- 	Report Detail displays correct metadata
- 	Date filters validate and apply correctly
- 	Embedded report loads with correct parameters
- 	Refresh behavior works without full reload
- 	Errors are displayed and announced
- 	Module supports full keyboard navigation
- 	Screen readers announce filter and report changes

## 8. Open Questions
- 	Should users be able to save favorite date ranges?
- 	Should we support exporting parameters for audit logs?
- 	Should Report Detail support deep‑linking with parameters?
- 	Should we add a “Compare Periods” enhancement in a future release?