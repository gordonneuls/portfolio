# UC‑RP‑01 — Request Password Reset
Use Case ID
UC‑RP‑01
Use Case Name
Request Password Reset
Module
Reset Password
## Purpose
To define how a user initiates a password reset request when they cannot log in. This use case covers entering an email address, validating the account, and sending a secure reset link or verification code to the user.
This UC is the first step in the Reset Password sequence.
## Primary Actor
Unauthenticated User
## Stakeholders & Interests
• 	User — wants to regain access to their account quickly and securely.
• 	System — must validate the request and send a secure reset link or code.
• 	Security Team — requires protection against account enumeration and brute‑force attacks.
• 	Product Owner — wants a simple, intuitive recovery flow.
• 	UX/UI Team — requires clear messaging and accessibility compliance.

## Preconditions
• 	User is not authenticated.
• 	User is on the Login screen.
• 	Reset Password feature is enabled.

## Postconditions
Success
• 	System sends a password reset link or verification code to the user’s email.
• 	System displays a confirmation message without revealing whether the email exists.
• 	Reset request is logged for security auditing.
Failure
• 	Reset request is not processed.
• 	System displays a generic error message.

## Trigger
User selects “Forgot Password?” on the Login screen.

## Main Success Scenario (Basic Flow — Request Reset)
1. 	User selects “Forgot Password?” on the Login screen.
(BR‑RP‑01)
2. 	System displays the Request Password Reset screen.
(UIS‑RP‑01)
3. 	User enters their email address.
(BR‑RP‑02)
4. 	System validates the email format.
(BR‑RP‑03)
5. 	System checks whether the email exists without revealing the result.
(BR‑SEC‑07 — Anti‑Enumeration)
6. 	System generates a secure reset token or verification code.
(BR‑RP‑04)
7. 	System sends a password reset email containing:
• 	Reset link or code
• 	Expiration timestamp
• 	Security instructions
(BR‑RP‑05)
8. 	System displays a confirmation message:
“If an account exists for this email, a reset link has been sent.”
(UIS‑RP‑02)
9. 	System logs the reset request for auditing.
(SRS‑LOG‑01)

## Alternate Flows
A1 — User Submits Empty Email Field
• 	System displays validation message:
“Email is required.”
A2 — Invalid Email Format
• 	System displays validation message:
“Enter a valid email address.”
A3 — Email Exists but Account Is Locked
• 	System still sends reset email.
• 	System logs the lockout state.
• 	User receives standard confirmation message.
A4 — Email Exists but User Has MFA Enabled
• 	System includes MFA‑specific instructions in the email.
(BR‑MFA‑01)

## Exception Flows
E1 — Email Service Failure
• 	System cannot send the reset email.
• 	System displays:
“Unable to process your request. Please try again later.”
• 	System logs the failure.
E2 — Token Generation Failure
• 	System displays a generic error message.
• 	System logs the event.
E3 — Rate Limit Exceeded
• 	User has requested too many resets in a short period.
• 	System displays:
“Too many attempts. Please try again later.”
• 	System logs the rate‑limit event.
(BR‑SEC‑09)

## Non‑Functional Requirements
• 	Security:
• 	No account enumeration.
• 	Reset tokens must be time‑limited and single‑use.
• 	Performance:
• 	Reset request must process within 200ms (excluding email delivery).
• 	Reliability:
• 	Email delivery must be logged and monitored.
• 	Accessibility:
• 	Form must support keyboard navigation and ARIA labels.
• 	Privacy:
• 	System must not reveal whether the email exists.

## Related UI Screens
• 	UIS‑LOGIN‑01 — Login Screen
• 	UIS‑RP‑01 — Request Password Reset
• 	UIS‑RP‑02 — Reset Request Confirmation

## Flowchart TD

    A[User Selects Forgot Password] --> B[Display Request Reset Screen]
    B --> C[User Enters Email]
    C --> D[Validate Email Format]
    D --> E[Check Email Exists (Silent)]
    E --> F[Generate Reset Token]
    F --> G[Send Reset Email]
    G --> H[Display Confirmation Message]
    H --> END[Reset Request Logged]
