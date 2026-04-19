# UC‑LOGIN‑02 — Session Timeout
Use Case ID
UC‑LOGIN‑02
Use Case Name
Session Timeout
Module
Login / Authentication

## Purpose
To automatically log out inactive users after a defined period of inactivity, ensuring account security and preventing unauthorized access to protected features when a user leaves the application unattended.

## Primary Actor
Authenticated User

## Stakeholders & Interests
- User — wants their account protected if they forget to log out.
- Security Team — requires enforcement of inactivity timeouts to prevent unauthorized access.
- System — must reliably detect inactivity and terminate sessions.
- Product Owner — wants a secure but non‑disruptive timeout experience.
- Analytics Team — needs to track session timeout events.

## Preconditions
- User is authenticated.
- User has an active session token.
- System session timeout duration is configured (e.g., 15 minutes).

## Postconditions
Success
- User session is invalidated.
- User is redirected to the Login page.
- User sees a message indicating the session expired.
Failure
- If session cannot be invalidated, system logs an error and forces logout on next request.

## Trigger
User remains inactive for the configured timeout duration.

## Main Success Scenario (Basic Flow)
- User logs in and begins an authenticated session.
(BR‑Login‑07, BR‑Login‑08)
- System tracks user activity timestamps.
(BR‑Login‑21)
- User becomes inactive (no clicks, navigation, or API calls).
(BR‑Login‑21)
- System detects that inactivity has exceeded the configured timeout threshold.
(BR‑Login‑21)
- System invalidates the user’s session token.
(BR‑Login‑09)
- System clears or expires the session cookie.
(BR‑Login‑09)
- System redirects the user to the Login page.
(BR‑Login‑07)
- System displays a message:
“Your session has expired due to inactivity.”
(BR‑Login‑21)
- System logs the session timeout event.
(Analytics requirement)

## Alternate Flows
A1 — User Returns Before Timeout
- Steps 1–3 occur.
- User interacts with the system before timeout threshold.
- System resets the activity timer.
- Session continues normally.
A2 — User Has Multiple Tabs Open
- System tracks activity across all tabs.
- Activity in any tab resets the timer.
- Timeout only occurs if all tabs are inactive.

## Exception Flows
E1 — Session Invalidation Error
- Step 5 fails due to backend or token store issue.
- System marks the session as invalid on next request.
- User is forced to reauthenticate.
E2 — Client‑Side Timeout Triggered but Server Session Still Active
- Browser detects inactivity and redirects user to Login.
- Server session remains active temporarily.
- On next request, server invalidates session and forces reauthentication.

## Non‑Functional Requirements
- Security: Session timeout must comply with security policies. (BR‑Login‑21)
- Performance: Timeout checks must not degrade system performance.
- Reliability: Session invalidation must be consistent across devices and tabs.
- Accessibility: Timeout message must be screen‑reader friendly. (SRS‑A11Y‑01)
- Analytics: Timeout events must be logged.

## Related UI Screens
- UIS‑LOGIN‑01 — Login Page
- UIS‑GLOBAL‑HEADER‑01
- UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    %% Entry
    A[User Authenticated] --> B[System Tracks Activity Timestamp]

    %% Inactivity
    B --> C[User Becomes Inactive]
    C --> D{Timeout Threshold Reached?}
    D -->|No| B
    D -->|Yes| E[Invalidate Session Token]

    %% Session End
    E --> F[Expire Session Cookie]
    F --> G[Redirect to Login Page]
    G --> H[Display Session Expired Message]

    %% Analytics
    H --> I[Log Session Timeout Event]

    %% End
    I --> END[Session Terminated]
