# UC‑LOGIN‑FLOW‑04 — Logout + Timeout Sequence

Use Case ID
UC‑LOGIN‑FLOW‑04
Use Case Name
Logout + Timeout Sequence
Module
Login / Authentication
## Purpose
To define the complete sequence by which a user’s authenticated session ends, either through explicit logout or automatic session timeout due to inactivity. This flow ensures secure session termination, consistent user experience, and protection of user data across all devices and tabs.
## Primary Actor
Authenticated User
## Stakeholders & Interests
• 	User — wants a predictable, secure way to end their session or be logged out automatically when inactive.
• 	System — must reliably invalidate sessions and enforce timeout rules.
• 	Security Team — requires strict session termination to prevent unauthorized access.
• 	Product Owner — wants a smooth, non‑disruptive logout experience.
• 	Analytics Team — needs visibility into logout events and timeout occurrences.

## Preconditions
• 	User is authenticated.
• 	User has an active session token.
• 	Session timeout duration is configured (e.g., 15 minutes).
• 	System can invalidate session tokens.

## Postconditions
Success
• 	Session token is invalidated.
• 	Session cookie is cleared or expired.
• 	User is redirected to the Login page with a relevant message.
Failure
• 	If session invalidation fails, system forces logout on next request.

## Trigger
One of the following occurs:
1. 	User selects Logout.
2. 	User remains inactive until the session timeout threshold is reached.

## Main Success Scenario (Basic Flow — Explicit Logout)
1. 	User selects Logout from the global header or menu.
(BR‑Login‑10)
2. 	System receives the logout request.
(FS‑LOGIN‑LOGOUT‑01)
3. 	System invalidates the session token.
(BR‑Login‑09)
4. 	System clears or expires the session cookie.
(BR‑Login‑09)
5. 	System redirects the user to the Login page.
(BR‑Login‑07)
6. 	System displays a confirmation message:
“You have been logged out.”
7. 	System logs the logout event.
(Analytics requirement)

## Alternate Flow A1 — Automatic Session Timeout
1. 	User is authenticated and active.
2. 	System tracks user activity timestamps.
(BR‑Login‑21)
3. 	User becomes inactive.
4. 	Inactivity exceeds the configured timeout threshold.
5. 	System invalidates the session token.
6. 	System expires the session cookie.
7. 	System redirects the user to the Login page.
8. 	System displays a message:
“Your session has expired due to inactivity.”
9. 	System logs the timeout event.

## Alternate Flow A2 — Multiple Tabs Open
1. 	User has multiple tabs open.
2. 	User logs out in one tab.
3. 	System invalidates the session token.
4. 	Other tabs detect invalid session on next request.
5. 	Each tab redirects to Login with message:
“Your session has ended. Please log in again.”

## Alternate Flow A3 — User Closes Browser Without Logging Out
1. 	User closes the browser tab or window.
2. 	Session remains active until timeout.
3. 	No explicit logout event is logged.
4. 	Timeout flow (A1) eventually triggers.

## Exception Flows
E1 — Session Invalidation Error
• 	Step 3 (logout) or step 5 (timeout) fails.
• 	System logs an error.
• 	System forces logout on next request by rejecting the token.
E2 — Network Error During Logout
• 	Logout request fails to reach server.
• 	System clears local session cookie.
• 	On next request, server rejects missing/invalid token and redirects to Login.
E3 — Client‑Side Timeout Triggered but Server Session Still Active
• 	Browser detects inactivity and redirects user to Login.
• 	Server session remains active temporarily.
• 	On next request, server invalidates session and forces reauthentication.

## Non‑Functional Requirements
• 	Security: Session termination must fully invalidate tokens.
• 	Performance: Logout and timeout checks must be instantaneous.
• 	Reliability: Behavior must be consistent across devices and tabs.
• 	Accessibility: Logout and timeout messages must be screen‑reader friendly. (SRS‑A11Y‑01)
• 	Analytics: Logout and timeout events must be logged.

## Related UI Screens
• 	UIS‑GLOBAL‑HEADER‑01 — Logout Link
• 	UIS‑LOGIN‑01 — Login Page
• 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    %% Explicit Logout Path
    A[User Clicks Logout] --> B[System Receives Logout Request]
    B --> C[Invalidate Session Token]
    C --> D{Invalidation Successful?}
    D -->|No| E1[Log Error<br/>Force Logout on Next Request]
    D -->|Yes| E[Expire Session Cookie]
    E --> F[Redirect to Login Page]
    F --> G[Display Logout Confirmation]
    G --> H[Log Logout Event]

    %% Timeout Path
    A2[User Inactive] --> B2[System Tracks Inactivity]
    B2 --> C2{Timeout Threshold Reached?}
    C2 -->|No| B2
    C2 -->|Yes| D2[Invalidate Session Token]
    D2 --> E2[Expire Session Cookie]
    E2 --> F2[Redirect to Login Page]
    F2 --> G2[Display Session Expired Message]
    G2 --> H2[Log Timeout Event]

    %% End
    H --> END
    H2 --> END