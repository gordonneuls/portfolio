# UC‑WT‑03 — View Weight Trend

Use Case ID
UC‑WT‑03
Use Case Name
View Weight Trend
Module
Weight Tracking
## Purpose
To define how an authenticated user views their historical weight trend over time. This use case retrieves historical entries, generates a chronological trend graph, and displays progress relative to the user’s weight goal (if set). It supports user motivation, progress tracking, and downstream insights.
## Primary Actor
Authenticated User
## Stakeholders & Interests
• 	User — wants to visually understand weight progress over time.
• 	System — must retrieve historical data and generate an accurate trend graph.
• 	Product Owner — wants a clear, motivating visualization.
• 	UX/UI Team — requires consistent graphing patterns and responsive design.
• 	Analytics Team — uses trend data for insights and engagement metrics.

## Preconditions
• 	User is authenticated.
• 	User has navigated to the Weight Tracking page.
• 	Weight Tracking feature is enabled.
• 	Historical weight data may or may not exist.

## Postconditions
Success
• 	Trend graph is displayed.
• 	Historical entries are shown in chronological order.
• 	Goal line is displayed (if a goal exists).
• 	Insights module receives updated trend data.
Failure
• 	Graph is not displayed.
• 	System shows a fallback message.
• 	No changes are made to stored data.

## Trigger
User navigates to the Weight Tracking page or selects View Trend.

## Main Success Scenario (Basic Flow — View Weight Trend)
1. 	User navigates to the Weight Tracking page.
(BR‑WT‑01)
2. 	System retrieves historical weight entries.
(DATA‑WT‑Entry)
3. 	System sorts entries chronologically.
(FS‑WT‑03)
4. 	System retrieves the user’s weight goal (if set).
(DATA‑WT‑Goal)
5. 	System generates the Weight Trend Graph, including:
• 	Chronological data points
• 	Line or curve visualization
• 	Goal line (if applicable)
(UIS‑WT‑03)
6. 	System displays the trend graph and historical list.
7. 	User reviews the trend and compares progress to the goal.

## Alternate Flows
A1 — No Weight History Exists
• 	System detects zero entries.
• 	System displays:
“No data available. Add your first weight entry to view your trend.”
• 	Graph area shows a placeholder.
(BR‑WT‑06)
A2 — Goal Not Set
• 	System displays the trend graph without a goal line.
• 	System may show a prompt:
“Set a weight goal to track your progress.”
A3 — Partial Data (e.g., only one entry)
• 	System displays a minimal graph or placeholder.
• 	System prompts user to add more entries.

## Exception Flows
E1 — Data Retrieval Failure
• 	System cannot retrieve historical entries.
• 	System displays:
“Unable to load your trend. Please try again later.”
• 	System logs the failure.
E2 — Graph Rendering Error
• 	System fails to generate the graph (e.g., JS error).
• 	System displays a fallback message.
• 	Historical list still loads.
E3 — Corrupted Data
• 	System detects invalid or malformed entries.
• 	System excludes corrupted data and logs the issue.
• 	System displays a partial graph or fallback.

## Non‑Functional Requirements
• 	Performance: Trend graph must render within 200ms after data retrieval.
• 	Reliability: Graph must accurately reflect all stored entries.
• 	Security: Only authenticated users may view their own data.
• 	Accessibility: Graph must include ARIA labels and keyboard navigation.
• 	Usability: Graph must be mobile‑friendly and responsive.
• 	Consistency: Visualization must follow global charting standards.

## Related UI Screens
• 	UIS‑WT‑01 — Weight Tracking Page
• 	UIS‑WT‑03 — Weight Trend Graph
• 	UIS‑WT‑04 — Weight Insights

## Flowchart TD

    A[User Opens Weight Tracking Page] --> B[Retrieve Historical Entries]
    B --> C[Sort Entries Chronologically]
    C --> D[Retrieve Weight Goal]
    D --> E[Generate Trend Graph]
    E --> F{Graph Generated?}

    F -->|No| G[Display Fallback Message]
    G --> H[Log Error]
    H --> END

    F -->|Yes| I[Display Trend Graph & History]
    I --> J[User Reviews Trend]
    J --> END