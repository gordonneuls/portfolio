# UC‑LOGIN‑FLOW‑01 — Login Sequence

Use Case ID
UC‑LOGIN‑FLOW‑01
Use Case Name
Login Sequence
Module
Login / Authentication
Purpose
To define the complete end‑to‑end login sequence a user follows when accessing the application, including CAPTCHA validation, credential entry, authentication, optional MFA, and redirection to the Dashboard. This flow ensures a secure, consistent, and user‑friendly authentication experience.

## Primary Actor
End User
## Stakeholders & Interests
• 	User — wants a smooth, secure login experience with clear guidance.
• 	System — must validate identity, enforce security controls, and establish a secure session.
• 	Security Team — requires CAPTCHA, credential validation, MFA (if enabled), and session protection.
• 	Product Owner — wants a friction‑balanced flow that prevents abuse but remains user‑friendly.
• 	Analytics Team — needs visibility into login attempts, failures, and drop‑off points.

## Preconditions
• 	User has an existing account.
• 	System is online and accessible.
• 	CAPTCHA service is available.
• 	Authentication service is available.
• 	MFA is enabled for accounts that require it.

## Postconditions
Success
• 	User is authenticated.
• 	Secure session token is created and stored.
• 	User is redirected to the Dashboard.
Failure
• 	User remains on the Login page.
• 	System displays appropriate error messaging.
• 	No session is created.

## Trigger
User navigates to the Login page to authenticate.

## Main Success Scenario (Basic Flow)
(Full Login Sequence)
1. 	User navigates to the Login page.
(BR‑Login‑01)
2. 	System displays the Login form and CAPTCHA challenge.
(BR‑Login‑16)
3. 	User completes the CAPTCHA challenge.
(BR‑Login‑16)
4. 	System validates the CAPTCHA response.
(BR‑Login‑16)
5. 	User enters email and password.
(BR‑Login‑02)
6. 	System validates required fields and email format.
(BR‑Login‑02, BR‑Login‑03)
7. 	System authenticates credentials against stored values.
(BR‑Login‑04, BR‑Login‑05)
8. 	System checks account status (active, not locked, not disabled).
(BR‑Login‑13)
9. 	If MFA is enabled, system displays MFA challenge.
(BR‑Login‑17)
10. 	User enters MFA code.
(BR‑Login‑17)
11. 	System validates MFA code.
(BR‑Login‑17)
12. 	System creates a secure session token.
(BR‑Login‑08)
13. 	System stores the token in an HTTP‑only cookie.
(BR‑Login‑09)
14. 	System redirects the user to the Dashboard.
(BR‑Login‑07)
15. 	System logs the successful login event.
(Analytics requirement)

## Alternate Flows
A1 — CAPTCHA Failure
• 	Step 4 fails.
• 	System displays CAPTCHA error.
• 	User retries CAPTCHA.
A2 — Missing or Invalid Credentials
• 	Step 6 or 7 fails.
• 	System displays appropriate error message.
(BR‑Login‑06)
• 	User remains on Login page.
A3 — Account Locked or Disabled
• 	Step 8 fails.
• 	System displays account‑specific error.
(BR‑Login‑13)
A4 — MFA Not Completed
• 	User abandons MFA screen.
• 	System does not authenticate user.
• 	User remains on MFA page or returns to Login.
A5 — MFA Failure
• 	Step 11 fails.
• 	System displays MFA error message.
• 	User may retry MFA.

## Exception Flows
E1 — Authentication Service Unavailable
• 	Step 7 fails due to backend outage.
• 	System displays generic error.
(BR‑Login‑20 fallback)
E2 — CAPTCHA Service Unavailable
• 	Step 2 or 4 fails.
• 	System displays fallback message or disables login temporarily.
E3 — Session Token Creation Error
• 	Step 12 fails.
• 	System logs error.
• 	User is not authenticated.

## Non‑Functional Requirements
• 	Security: CAPTCHA, MFA, and session token must meet security standards.
• 	Performance: Login sequence must complete quickly.
• 	Reliability: Authentication must be consistent across browsers and devices.
• 	Accessibility: All steps must support keyboard navigation and screen readers. (SRS‑A11Y‑01)
• 	Analytics: All login attempts, failures, and MFA events must be logged.

## Related UI Screens
• 	UIS‑LOGIN‑01 — Login Page
• 	UIS‑LOGIN‑CAPTCHA‑01 — CAPTCHA Component
• 	UIS‑LOGIN‑MFA‑01 — MFA Challenge
• 	UIS‑DASH‑01 — Dashboard
• 	UIS‑GLOBAL‑HEADER‑01
• 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    %% Entry
    A[User Navigates to Login Page] --> B[Display Login Form + CAPTCHA]

    %% CAPTCHA
    B --> C[User Completes CAPTCHA]
    C --> D{CAPTCHA Valid?}
    D -->|No| C1[Display CAPTCHA Error<br/>Retry CAPTCHA]
    D -->|Yes| E[User Enters Email & Password]

    %% Credential Validation
    E --> F{Fields Valid?}
    F -->|No| F1[Display Missing/Invalid Field Error]

    F -->|Yes| G[Authenticate Credentials]
    G --> H{Credentials Valid?}
    H -->|No| H1[Display Invalid Credentials Error]

    %% Account Status
    H -->|Yes| I{Account Active?}
    I -->|No| I1[Display Account Locked/Disabled]

    %% MFA
    I -->|Yes| J{MFA Required?}
    J -->|No| M[Create Session Token]

    J -->|Yes| K[Display MFA Challenge]
    K --> L[Validate MFA Code]
    L --> M{MFA Valid?}
    M -->|No| L1[Display MFA Error]

    %% Session Creation
    M --> N[Store Token in HTTP‑Only Cookie]

    %% Redirect
    N --> O[Redirect to Dashboard]
    O --> P[Log Successful Login Event]

    %% End
    P --> END[Login Complete]