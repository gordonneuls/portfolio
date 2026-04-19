# UC‑LOGIN‑06 — Navigate to Reset Password

Use Case ID
UC‑LOGIN‑06
Use Case Name
Navigate to Reset Password
Module
Login / Authentication
## Purpose
To allow a user who has forgotten their password to navigate from the Login page to the Reset Password page. This use case ensures users have a clear, intuitive path to initiate the password recovery process.
## Primary Actor
Unauthenticated User
## Stakeholders & Interests
• 	User — wants a simple way to reset their password when they cannot log in.
• 	System — must provide a reliable navigation path to the Reset Password workflow.
• 	Product Owner — wants to reduce friction and support smooth account recovery.
• 	Security Team — ensures no sensitive data is exposed during navigation.
• 	Analytics Team — needs to track how often users initiate password reset.

## Preconditions
• 	User is on the Login page.
• 	System can load the Reset Password page.
• 	User is not authenticated.

## Postconditions
Success
• 	User is redirected to the Reset Password page.
• 	User may begin the password reset process (UC‑PWD‑RESET‑01).
Failure
• 	System displays an error message if navigation fails.
• 	User remains on the Login page.

## Trigger
User selects Forgot Password? on the Login page.

## Main Success Scenario (Basic Flow)
1. 	User navigates to the Login page.
(BR‑Login‑01)
2. 	System displays the Login form and a Forgot Password? link.
(BR‑Login‑11)
3. 	User selects the Forgot Password? link.
(BR‑Login‑11)
4. 	System loads the Reset Password page.
(FS‑LOGIN‑NAV‑02)
5. 	System displays the Reset Password form (email entry).
(BR‑PWD‑01)
6. 	User may begin the password reset process (UC‑PWD‑RESET‑01).
7. 	System logs the navigation event for analytics.
(Analytics requirement)

## Alternate Flows
A1 — User Navigates Back to Login
• 	Steps 1–4 occur.
• 	User selects Back to Login.
• 	System returns user to the Login page.
A2 — User Attempts to Access Reset Password While Authenticated
• 	User is already logged in.
• 	System redirects user to the Dashboard instead of Reset Password.
(BR‑Login‑12)

## Exception Flows
E1 — Reset Password Page Fails to Load
• 	Step 4 fails due to system or network issue.
• 	System displays an error message.
(BR‑Login‑20 fallback)
• 	User remains on the Login page.

## Non‑Functional Requirements
• 	Usability: Link must be clearly visible and intuitive.
• 	Accessibility: Link must support keyboard navigation and screen readers. (SRS‑A11Y‑01)
• 	Performance: Reset Password page must load quickly.
• 	Security: No sensitive data is exposed during navigation.
• 	Analytics: Navigation events must be logged.

## Related UI Screens
• 	UIS‑LOGIN‑01 — Login Page
• 	UIS‑PWD‑01 — Reset Password Page
• 	UIS‑GLOBAL‑HEADER‑01
• 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    %% Entry
    A[User on Login Page] --> B[User Clicks 'Forgot Password?']

    %% Navigation
    B --> C{Reset Password Page Loads?}
    C -->|No| E1[Display Error Message<br/>Stay on Login Page]
    C -->|Yes| D[Display Reset Password Page]

    %% User Interaction
    D --> E[User Begins Reset Password Process (UC‑PWD‑RESET‑01)]

    %% Analytics
    D --> F[Log Navigation Event]

    %% End
    F --> END[Navigation Complete]