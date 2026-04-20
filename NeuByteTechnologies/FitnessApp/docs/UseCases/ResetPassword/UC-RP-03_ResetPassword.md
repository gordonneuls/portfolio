# UC‑RP‑03 — Reset Password
Use Case ID
UC‑RP‑03
Use Case Name
Reset Password
Module
Reset Password
## Purpose
To define how a user creates a new password after successfully validating their reset token. This use case covers password entry, validation, security requirements, and updating the user’s credentials in the system.
This is the third step in the Reset Password sequence.
## Primary Actor
Unauthenticated User
## Stakeholders & Interests
• 	User — wants to securely reset their password and regain access to their account.
• 	System — must enforce password complexity and securely update credentials.
• 	Security Team — requires strong password policies and secure hashing.
• 	Product Owner — wants a smooth, intuitive reset experience.
• 	UX/UI Team — requires clear validation messages and accessibility compliance.

## Preconditions
• 	User is not authenticated.
• 	User has requested a password reset (UC‑RP‑01).
• 	Reset token has been validated (UC‑RP‑02).
• 	Reset Password screen is displayed.

## Postconditions
Success
• 	User’s password is updated.
• 	Reset token is marked as used.
• 	User is redirected to a success confirmation screen (UC‑RP‑04).
• 	System logs the password reset event.
Failure
• 	Password is not updated.
• 	System displays validation or error messages.
• 	Token remains valid until expiration or retry.

## Trigger
User submits the Reset Password form with a new password.

## Main Success Scenario (Basic Flow — Reset Password)
1. 	System displays the Reset Password screen.
(UIS‑RP‑03)
2. 	User enters a new password and confirms it.
(BR‑RP‑09)
3. 	System validates that both fields are populated.
(BR‑RP‑10)
4. 	System validates that the passwords match.
(BR‑RP‑11)
5. 	System validates password complexity requirements, such as:
• 	Minimum length
• 	Uppercase/lowercase
• 	Number
• 	Special character
(BR‑SEC‑10 — Password Policy)
6. 	System hashes the new password using a secure algorithm (e.g., bcrypt, Argon2).
(FS‑RP‑PWD‑01)
7. 	System updates the user’s password in the database.
(DATA‑User.PasswordHash)
8. 	System marks the reset token as used.
(BR‑RP‑07)
9. 	System logs the password reset event.
(SRS‑LOG‑01)
10. 	System redirects the user to the Reset Success screen (UC‑RP‑04).

## Alternate Flows
A1 — User Shows Password
• 	User selects “Show Password.”
• 	System toggles visibility.
• 	Flow continues at Step 2.
A2 — Password Meets Minimum Requirements but Is Weak
• 	System displays a non‑blocking warning:
“Your password is valid but weak.”
• 	User may proceed or choose a stronger password.
A3 — Password Meets Complexity but Violates Reuse Policy
• 	System checks password history (if enabled).
(BR‑SEC‑11 — Password Reuse Prevention)
• 	System displays:
“You cannot reuse a recent password.”

## Exception Flows
E1 — Passwords Do Not Match
• 	System displays:
“Passwords do not match.”
• 	User corrects input.
E2 — Password Too Weak or Invalid
• 	System displays specific validation messages.
• 	User corrects input.
E3 — Token Expired During Submission
• 	System detects expiration.
• 	System displays:
“Your reset link has expired. Please request a new one.”
• 	User is directed to UC‑RP‑01.
E4 — Database Update Failure
• 	System displays:
“Unable to update your password. Please try again later.”
• 	System logs the failure.
E5 — Security Violation Detected
• 	System detects tampering or replay attack.
• 	System displays a generic error.
• 	System logs a security alert.
(SRS‑SEC‑05)

## Non‑Functional Requirements
• 	Security:
• 	Password must be hashed and salted.
• 	Token must be invalidated immediately after use.
• 	No sensitive details in error messages.
• 	Performance:
• 	Password update must complete within 200ms.
• 	Reliability:
• 	All reset attempts must be logged.
• 	Accessibility:
• 	Form must support keyboard navigation and ARIA labels.
• 	Privacy:
• 	No indication of account existence beyond generic messaging.

## Related UI Screens
• 	UIS‑RP‑03 — Reset Password Screen
• 	UIS‑RP‑04 — Reset Success Screen
• 	UIS‑RP‑05 — Reset Failure Screen

## Flowchart TD

    A[Display Reset Password Screen] --> B[User Enters New Password]
    B --> C[Validate Fields Populated]
    C --> D[Validate Passwords Match]
    D --> E[Validate Complexity Requirements]
    E --> F{Valid?}

    F -->|No| G[Display Validation Error]
    G --> B

    F -->|Yes| H[Hash Password]
    H --> I[Update User Record]
    I --> J[Mark Token Used]
    J --> K[Log Reset Event]
    K --> L[Display Reset Success Screen]
    L --> END