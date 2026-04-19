#UC‑LOGIN‑MFA‑01 — Multi‑Factor Authentication (Verification)

Use Case ID
UC‑LOGIN‑MFA‑01
Use Case Name
Multi‑Factor Authentication (Verification)
Module
MFA / Authentication
## Purpose
To define the steps a user follows to complete Multi‑Factor Authentication (MFA) after successfully entering valid credentials. This use case covers TOTP code entry, validation, error handling, retry behavior, and redirect to the Dashboard upon success. It ensures secure identity verification and protects against unauthorized access.
## Primary Actor
Authenticated User (credentials validated, MFA required)
## Stakeholders & Interests
• 	User — wants a quick, reliable way to verify identity and complete login.
• 	System — must validate MFA codes securely and consistently.
• 	Security Team — requires strong MFA enforcement, retry limits, and secure TOTP handling.
• 	Product Owner — wants a friction‑balanced MFA experience that enhances security without frustrating users.
• 	Analytics Team — needs visibility into MFA attempts, failures, and completions.

## Preconditions
• 	User has successfully entered valid credentials.
• 	User has MFA enabled on their account.
• 	System has a stored TOTP secret for the user.
• 	MFA verification screen is displayed.

## Postconditions
Success
• 	MFA code is validated.
• 	User is fully authenticated.
• 	User is redirected to the Dashboard.
• 	MFA attempt is logged.
Failure
• 	User remains on the MFA screen.
• 	Error message is displayed.
• 	Retry count may increase.
• 	Excessive failures may trigger lockout (if configured).

## Trigger
User is prompted to enter a 6‑digit MFA code after successful primary authentication.

## Main Success Scenario (Basic Flow — TOTP Verification)
1. 	System displays the MFA Code Entry screen.
(BR‑LOGIN‑MFA‑01)
2. 	User opens their Authenticator App (e.g., Google Authenticator, Microsoft Authenticator).
(User action)
3. 	User enters the 6‑digit TOTP code.
(BR‑LOGIN‑MFA‑03)
4. 	System validates the TOTP code using time‑based verification.
(BR‑LOGIN‑MFA‑03)
5. 	System checks for code expiration (30‑second window).
(BR‑LOGIN‑MFA‑06)
6. 	System confirms the code matches the expected value.
(BR‑LOGIN‑MFA‑03)
7. 	System authenticates the user and establishes a session.
(BR‑LOGIN‑07)
8. 	System redirects the user to the Dashboard.
(BR‑LOGIN‑07)
9. 	System logs the MFA success event.
(Analytics requirement)

## Alternate Flows
A1 — Incorrect MFA Code
• 	Step 4 fails.
• 	System displays an error message:
“Invalid code. Please try again.”
(BR‑LOGIN‑MFA‑05)
• 	User may retry.
A2 — Expired MFA Code
• 	Step 5 fails.
• 	System displays:
“This code has expired. Please enter the latest code from your app.”
• 	User enters a new code.
A3 — User Enters Non‑Numeric Code
• 	System validates input format.
• 	System displays:
“Please enter a valid 6‑digit code.”
• 	User corrects input.
A4 — User Cancels MFA
• 	User selects Cancel or navigates back.
• 	System returns user to Login page.
• 	No authentication occurs.

## Exception Flows
E1 — Time Sync Issue
• 	System detects significant time drift between server and device.
• 	System displays:
“Your device time may be incorrect. Please sync your clock and try again.”
E2 — TOTP Validation Service Unavailable
• 	Step 4 fails due to backend outage.
• 	System displays a generic error message.
(BR‑Login‑20 fallback)
• 	User may retry later.
E3 — Excessive Failed Attempts
• 	User exceeds configured retry limit (e.g., 5 attempts).
• 	System locks MFA verification temporarily.
(Security policy)
• 	System displays:
“Too many failed attempts. Please try again later.”

## Non‑Functional Requirements
• 	Security: TOTP secrets must never be transmitted after enrollment.
• 	Performance: MFA validation must complete within 1 second.
• 	Reliability: TOTP validation must be consistent across time zones and devices.
• 	Accessibility: MFA screen must support screen readers and keyboard navigation. (SRS‑A11Y‑01)
• 	Analytics: MFA attempts, failures, and completions must be logged.

## Related UI Screens
• 	UIS‑MFA‑03 — MFA Code Entry
• 	UIS‑LOGIN‑01 — Login Page
• 	UIS‑DASH‑01 — Dashboard
• 	UIS‑GLOBAL‑HEADER‑01
• 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    A[Display MFA Code Entry Screen] --> B[User Enters 6-Digit Code]
    B --> C[Validate TOTP Code]

    C --> D{Code Valid?}
    D -->|No| D1[Display Error<br/>Retry Allowed]

    D -->|Yes| E[Check Code Expiration]
    E --> F{Expired?}
    F -->|Yes| F1[Display Expired Code Message]

    F -->|No| G[Authenticate User]
    G --> H[Redirect to Dashboard]
    H --> I[Log MFA Success Event]

    I --> END[MFA Verification Complete]