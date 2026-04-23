# FS‑DASH — Dashboard Functional Specification
## 1. Purpose
The Dashboard module provides the user with a consolidated, real‑time overview of their fitness activity, program progress, weight trends, and daily workout plan. It acts as the primary landing page after authentication and serves as the central navigation hub for the application.

## 2. Scope
This FS covers:
-	Dashboard display
-	User identity and context
-	Program progress
-	Weight summary and trend
-	Today’s workout summary
-	Recent activity
-	Progress summary cards
-	Notifications
-	Data refresh behavior
-	Responsive layout
This module does not include editing workouts, logging weight, or viewing program details — those are handled in their respective modules.

## 3. References
-	Business Requirements: BR‑DASH‑01 → BR‑DASH‑15
-	Use Cases: UC‑DASH‑01 (View Dashboard), UC‑DASH‑02 (Start Workout), UC‑DASH‑03 (View Recent Activity)
-	RTM: Dashboard Module Rows
-	FS‑AUTH (Authentication)
-	FS‑PROGRAM (Program Detail)
-	FS‑WEIGHT (Weight Tracking)

## 4. Functional Requirements (Text‑Only)
FR‑DASH‑01 — Display Dashboard
Description:
The system shall display the Dashboard immediately after successful authentication.
Required Fields:
- user_id
- last_login_timestamp
- selected_program_id
- current_weight
- weight_trend
- today_workout_summary
- recent_activity_list
- progress_summary_metrics
Maps to: BR‑DASH‑01

FR‑DASH‑02 — Display User Identity
Description:
The system shall display the logged‑in user’s email address.
Required Fields:
- email
Maps to: BR‑DASH‑02

FR‑DASH‑03 — Display Last Login Timestamp
Description:
The system shall display the user’s last login date and time.
Required Fields:
- last_login_timestamp	
Maps to: BR‑DASH‑03

FR‑DASH‑04 — Display Selected Exercise Program
Description:
The system shall display the name of the user’s currently selected exercise program.
Required Fields:
- program_name
Maps to: BR‑DASH‑04

FR‑DASH‑05 — Display Program Progress
Description:
The system shall display the user’s current week and/or day within the selected program.
Required Fields:
- current_week
- current_day
Maps to: BR‑DASH‑05

FR‑DASH‑06 — Display Current Weight
Description:
The system shall display the user’s most recently recorded weight.
Required Fields:
- current_weight	
- weight_unit
Maps to: BR‑DASH‑06

FR‑DASH‑07 — Display Weight Trend
Description:
The system shall calculate and display the user’s weight trend.
Required Fields:
- weight_trend (values: increase, decrease, no_change)
- trend_period (e.g., last 7 days)
Maps to: BR‑DASH‑07

FR‑DASH‑08 — Display Today’s Workout Summary
Description:
The system shall display a summary of the workout scheduled for the current day.
Required Fields:
- exercise_count
- estimated_duration_minutes
- workout_title
Maps to: BR‑DASH‑08

FR‑DASH‑09 — Start Workout Action
Description:
The system shall provide a “Start Workout” button that navigates the user to the workout logging workflow.
Required Fields:
- start_workout_button
- workout_id
Maps to: BR‑DASH‑09

FR‑DASH‑10 — Display Recent Activity
Description:
The system shall display the user’s most recent 3–5 logged workouts.
Required Fields:
- activity_date
- activity_type
- completion_status
- activity_id
Maps to: BR‑DASH‑10

FR‑DASH‑11 — Quick Access to Activity Details
Description:
The system shall allow users to select a recent activity entry to view or edit its details.
Required Fields:
- activity_id
- view_activity_action
Maps to: BR‑DASH‑11

FR‑DASH‑12 — Display Progress Summary Cards
Description:
The system shall display summary cards showing weekly, monthly, and/or streak‑based progress metrics.
Required Fields:
- weekly_workouts_comppleted
- monthly_workouts_completed
- current_streak_days
Maps to: BR‑DASH‑12

FR‑DASH‑13 — Display Notifications
Description:
The system shall display system‑generated notifications relevant to the user.
Required Fields:
- notification_id
- notification_message
- notification_type
- timestamp
Maps to: BR‑DASH‑13

FR‑DASH‑14 — Responsive Layout
Description:
The system shall render the Dashboard in a responsive layout optimized for mobile, tablet, and desktop.
Required Fields:
- layout_breakpoint	
- component_visibility_rules
Maps to: BR‑DASH‑14

FR‑DASH‑15 — Data Refresh
Description:
The system shall refresh Dashboard data upon page load and after any user action that modifies underlying data.
Required Fields:
- refresh_trigger
- refresh_timestamp
Maps to: BR‑DASH‑15

## 5. Non‑Functional Requirements
-	Dashboard must load in < 2 seconds on broadband
-	Must support low‑bandwidth fallback
-	Must not block rendering while fetching secondary data
-	Must support accessibility requirements (see FS‑A11Y)
-	Must support mobile and desktop layouts

## 6. Dependencies
-	Authentication module
-	Program Detail module
-	Weight Tracking module
-	Activity Logging module
-	Notification module
-	Analytics module

## 7. Acceptance Criteria
-	Dashboard loads immediately after login
-	All required fields display correctly
-	Weight trend is calculated accurately
-	Today’s workout summary appears when applicable
-	Recent activity list shows correct entries
-	Start Workout button navigates correctly
-	Notifications display in correct order
-	Layout adapts to screen size
-	Data refresh triggers after updates

## 8. Open Questions
-	Should the Dashboard support user‑customizable widgets?
-	Should we include a “Last 7 Days” mini‑chart for weight?
-	Should notifications auto‑dismiss after a timeout?