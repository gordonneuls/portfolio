# UC‑LOGIN‑FLOW‑02 — Password Reset Sequence
## Purpose
To define the complete end‑to‑end password reset sequence a user follows when they cannot access their account. This includes navigating to the Reset Password page, submitting their email, receiving a reset link or code, creating a new password, and returning to the Login page. This flow ensures a secure, reliable, and user‑friendly account recovery experience.

Use Case ID
UC‑LOGIN‑FLOW‑02
Use Case Name
Password Reset Sequence
Module
Login / Authentication
## Purpose
To define the complete end‑to‑end password reset sequence a user follows when they cannot access their account. This includes navigating to the Reset Password page, submitting their email, receiving a reset link or code, creating a new password, and returning to the Login page. This flow ensures a secure, reliable, and user‑friendly account recovery experience.
## Primary Actor
Unauthenticated User
## Stakeholders & Interests
• 	User — wants a simple, secure way to regain access to their account.
• 	System — must verify identity and enforce password security rules.
• 	Security Team — requires secure token handling, expiration, and strong password policies.
• 	Product Owner — wants a friction‑balanced recovery process that reduces support tickets.
• 	Analytics Team — needs visibility into reset attempts, failures, and completions.

## Preconditions
• 	User has an existing account.
• 	User is not authenticated.
• 	System can send email (SMTP or email service available).
• 	Reset token generation service is available.
• 	Reset Password page is accessible.

## Postconditions
Success
• 	User’s password is successfully updated.
• 	Reset token is invalidated.
• 	User is redirected to the Login page with a confirmation message.
Failure
• 	Password is not changed.
• 	User remains on the Reset Password page or sees an error.
• 	Reset token may be invalidated depending on failure type.

## Trigger
User selects Forgot Password? on the Login page.

## Main Success Scenario (Basic Flow — Full Reset Sequence)
1. 	User selects Forgot Password? on the Login page.
(BR‑Login‑11)
2. 	System loads the Reset Password page.
(FS‑LOGIN‑NAV‑02)
3. 	User enters their email address.
(BR‑PWD‑01)
4. 	System validates email format and required field.
(BR‑PWD‑01, BR‑Login‑03)
5. 	System checks whether the email exists in the system.
(BR‑PWD‑02)
6. 	System generates a secure, time‑limited reset token.
(BR‑PWD‑03)
7. 	System sends a password reset email containing the reset link or code.
(BR‑PWD‑04)
8. 	User opens the email and selects the reset link.
(BR‑PWD‑04)
9. 	System validates the reset token (not expired, not used, matches user).
(BR‑PWD‑05)
10. 	System displays the Create New Password page.
(BR‑PWD‑06)
11. 	User enters a new password and confirms it.
(BR‑PWD‑06)
12. 	System validates password strength and matching fields.
(BR‑PWD‑07)
13. 	System securely updates the user’s password (salted + hashed).
(BR‑PWD‑08)
14. 	System invalidates the reset token.
(BR‑PWD‑05)
15. 	System redirects the user to the Login page with a success message.
(BR‑PWD‑09)
16. 	System logs the password reset completion event.
(Analytics requirement)

## Alternate Flows
A1 — Email Not Found
• 	Step 5 fails.
• 	System displays a generic message:
“If this email exists, a reset link has been sent.”
(BR‑PWD‑02)
• 	User remains on the Reset Password page.
A2 — User Does Not Open Email
• 	Steps 1–7 occur.
• 	User never opens the email.
• 	Token expires automatically.
(BR‑PWD‑05)
A3 — User Opens Email After Token Expiration
• 	Step 9 fails.
• 	System displays:
“This reset link has expired.”
• 	User is prompted to request a new reset link.
A4 — User Cancels Reset
• 	User closes the browser or returns to Login.
• 	No password changes occur.

## Exception Flows
E1 — Email Delivery Failure
• 	Step 7 fails due to SMTP or provider issue.
• 	System displays a generic error message.
(BR‑Login‑20 fallback)
• 	User remains on Reset Password page.
E2 — Token Tampering Detected
• 	Step 9 fails due to invalid or manipulated token.
• 	System logs a security alert.
• 	System displays:
“Invalid reset link.”
E3 — Password Update Error
• 	Step 13 fails due to database or backend issue.
• 	System displays a generic error message.
• 	User may retry.

## Non‑Functional Requirements
• 	Security: Reset tokens must be single‑use, time‑limited, and cryptographically secure.
• 	Performance: Email delivery should occur within seconds.
• 	Reliability: Token validation must be consistent across devices.
• 	Accessibility: All pages must support screen readers and keyboard navigation. (SRS‑A11Y‑01)
• 	Analytics: Reset attempts, failures, and completions must be logged.

## Related UI Screens
• 	UIS‑LOGIN‑01 — Login Page
• 	UIS‑PWD‑01 — Reset Password Page
• 	UIS‑PWD‑02 — Create New Password Page
• 	UIS‑GLOBAL‑HEADER‑01
• 	UIS‑GLOBAL‑FOOTER‑01

## Flowchart TD

    %% Entry
    A[User Clicks 'Forgot Password?'] --> B[Display Reset Password Page]

    %% Email Entry
    B --> C[User Enters Email]
    C --> D{Email Valid?}
    D -->|No| D1[Display Email Error]

    D -->|Yes| E[Check Email Exists]
    E --> F{Email Exists?}
    F -->|No| F1[Display Generic Message<br/>Stay on Page]

    %% Token Generation
    F -->|Yes| G[Generate Reset Token]
    G --> H[Send Reset Email]

    %% Email Interaction
    H --> I[User Opens Email & Clicks Link]
    I --> J[Validate Reset Token]

    J --> K{Token Valid?}
    K -->|No| K1[Display Expired/Invalid Link]

    %% New Password
    K -->|Yes| L[Display Create New Password Page]
    L --> M[User Enters New Password]

    M --> N{Password Valid?}
    N -->|No| N1[Display Password Error]

    %% Update
    N -->|Yes| O[Update Password]
    O --> P[Invalidate Token]

    %% Redirect
    P --> Q[Redirect to Login Page<br/>Display Success Message]

    %% Analytics
    Q --> R[Log Reset Completion]

    %% End
    R --> END[Password Reset Complete]

