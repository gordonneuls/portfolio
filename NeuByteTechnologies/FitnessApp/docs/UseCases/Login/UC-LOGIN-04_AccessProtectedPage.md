# UC‑LOGIN‑04 — Access Protected Page

Use Case ID
UC‑LOGIN‑04
Use Case Name
Access Protected Page
Module
Login / Authentication

## Purpose
To ensure that only authenticated users can access protected pages within the application. This use case enforces access control by verifying session validity and redirecting unauthenticated users to the Login page, maintaining security and preventing unauthorized access to sensitive features.

## Primary Actor
User (Authenticated or Unauthenticated)

## Stakeholders & Interests
• 	User — wants secure access to their personal data and features.
• 	System — must enforce access control rules consistently.
• 	Security Team — requires strict session validation to prevent unauthorized access.
• 	Product Owner — wants a seamless redirect experience for unauthenticated users.
• 	Analytics Team — needs to track unauthorized access attempts.

## Preconditions
• 	Protected pages are configured with authentication requirements.
• 	System can validate session tokens.
• 	User may or may not be authenticated.

## Postconditions
Success (Authenticated User)
• 	User is granted access to the protected page.
• 	Page content loads normally.
Failure (Unauthenticated User)
• 	User is redirected to the Login page.
• 	System displays an appropriate message (e.g., “Please log in to continue.”).

## Trigger
User attempts to navigate to a protected page (e.g., Dashboard, Weight Tracking, Program Management).

## Main Success Scenario (Basic Flow — Authenticated User)
1. 	User attempts to access a protected page.
(BR‑Login‑12)
2. 	System checks for a valid session token.
(BR‑Login‑09)
3. 	System verifies that the session token is not expired.
(BR‑Login‑10, BR‑Login‑21)
4. 	System confirms that the user is authenticated.
(BR‑Login‑12)
5. 	System loads the protected page content.
(FS‑GLOBAL‑NAV‑01)
6. 	User interacts with the page normally.
7. 	System logs the successful access event.
(Analytics requirement)

## Alternate Flow — Unauthenticated User (A1)
1. 	User attempts to access a protected page.
2. 	System checks for a valid session token.
3. 	No valid token is found.
4. 	System redirects the user to the Login page.
(BR‑Login‑12)
5. 	System displays a message:
“Please log in to access this page.”
6. 	User may log in (UC‑LOGIN‑01).

## Alternate Flow — Expired Session (A2)
1. 	User attempts to access a protected page.
2. 	System detects that the session token is expired.
(BR‑Login‑21)
3. 	System invalidates the session.
4. 	System redirects the user to the Login page.
5. 	System displays a message:
“Your session has expired. Please log in again.”
6. 	User may log in (UC‑LOGIN‑01).

## Exception Flows
E1 — Session Validation Error
• 	System encounters an error validating the session token.
• 	System logs the error.
• 	System redirects the user to the Login page with a generic error message.
E2 — Token Tampering Detected
• 	System detects an invalid or manipulated token.
• 	System logs a security alert.
• 	System forces logout and redirects to Login.

## Non‑Functional Requirements
• 	Security: All protected pages must enforce session validation. (BR‑Login‑12)
• 	Performance: Access checks must occur instantly.
• 	Reliability: Access control must be consistent across all modules.
• 	Accessibility: Redirect messages must be screen‑reader friendly. (SRS‑A11Y‑01)
• 	Analytics: Unauthorized access attempts must be logged.

## Related UI Screens
• 	UIS‑LOGIN‑01 — Login Page
• 	UIS‑GLOBAL‑HEADER‑01
• 	UIS‑GLOBAL‑FOOTER‑01
• 	UIS‑DASH‑01 — Dashboard (example protected page)

## Flowchart TD

    %% Entry
    A[User Attempts to Access Protected Page] --> B[Check Session Token]

    %% Token Check
    B --> C{Valid Token?}
    C -->|No| Z1[Redirect to Login Page<br/>Display 'Please Log In']

    %% Expiration Check
    C -->|Yes| D{Token Expired?}
    D -->|Yes| Z2[Invalidate Session<br/>Redirect to Login<br/>Display 'Session Expired']

    %% Authenticated
    D -->|No| E[Load Protected Page Content]
    E --> F[User Interacts Normally]

    %% Analytics
    F --> G[Log Access Event]

    %% End
    G --> END[Access Granted]

