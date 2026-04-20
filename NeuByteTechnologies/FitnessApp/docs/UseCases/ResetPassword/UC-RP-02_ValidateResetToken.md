# UC‑RP‑02 — Validate Reset Token
Use Case ID
UC‑RP‑02
Use Case Name
Validate Reset Token
Module
Reset Password
## Purpose
To define how the system validates a password reset token or verification code when the user clicks the reset link sent to their email. This UC ensures the token is authentic, unexpired, unused, and associated with a valid user account before allowing the user to proceed to the password reset screen.
This is a system‑driven validation step triggered by a user action (clicking the link).
## Primary Actor
Unauthenticated User
## Stakeholders & Interests
• 	User — wants a seamless and secure password reset experience.
• 	System — must ensure the token is valid and secure before allowing password changes.
• 	Security Team — requires strong token validation to prevent unauthorized access.
• 	Product Owner — wants a reliable, user‑friendly recovery flow.
• 	UX/UI Team — requires clear messaging for invalid or expired tokens.

## Preconditions
• 	User is not authenticated.
• 	User has requested a password reset (UC‑RP‑01).
• 	System has generated and sent a reset token.
• 	User clicks the reset link or enters a verification code.

## Postconditions
Success
• 	Token is validated.
• 	System displays the Reset Password screen (UC‑RP‑03).
• 	Token is marked as “in use” or “pending” to prevent reuse.
Failure
• 	System displays an error message (expired, invalid, or already used).
• 	User is directed to request a new reset link.
• 	System logs the failure.

## Trigger
User clicks the password reset link in their email or enters a reset code manually.

## Main Success Scenario (Basic Flow — Validate Token)
1. 	User clicks the reset link or enters the reset code.
(BR‑RP‑05)
2. 	System extracts the token from the link or input.
(FS‑RP‑TOKEN‑01)
3. 	System verifies that the token exists in the database.
(BR‑RP‑06)
4. 	System checks that the token has not expired.
(BR‑SEC‑08 — Token Expiration)
5. 	System checks that the token has not already been used.
(BR‑RP‑07)
6. 	System checks that the token matches the correct user account.
(BR‑RP‑08)
7. 	System marks the token as “validated” or “pending reset.”
(FS‑RP‑TOKEN‑02)
8. 	System displays the Reset Password screen (UC‑RP‑03).
(UIS‑RP‑03)

## Alternate Flows
A1 — Token Provided via Manual Entry
• 	User enters a verification code instead of clicking a link.
• 	System validates the code using the same logic.
• 	Flow rejoins at Step 3.
A2 — Token Already Validated but Not Used
• 	System checks token state.
• 	If token is still within valid time window, system allows user to proceed.
• 	Flow continues at Step 8.

## Exception Flows
E1 — Token Expired
• 	System detects expiration.
• 	System displays:
“This reset link has expired. Please request a new one.”
• 	System logs the event.
• 	User is directed to UC‑RP‑01_RequestReset.
E2 — Token Invalid
• 	Token does not exist or is malformed.
• 	System displays:
“Invalid reset link.”
• 	System logs the event.
E3 — Token Already Used
• 	Token is marked as used.
• 	System displays:
“This reset link has already been used.”
• 	User is directed to request a new reset link.
E4 — Token Tampered With
• 	Token signature or hash does not match.
• 	System displays a generic error message.
• 	System logs a security alert.
(SRS‑SEC‑05)
E5 — Database or Service Failure
• 	System cannot validate the token.
• 	System displays:
“Unable to process your request. Please try again later.”
• 	System logs the failure.

## Non‑Functional Requirements
• 	Security:
• 	Token must be cryptographically secure.
• 	Token must be single‑use and time‑limited.
• 	No sensitive information may be revealed in error messages.
• 	Performance:
• 	Token validation must complete within 150ms.
• 	Reliability:
• 	All validation attempts must be logged.
• 	Accessibility:
• 	Error messages must be screen‑reader friendly.
• 	Privacy:
• 	System must not reveal whether an account exists.

## Related UI Screens
• 	UIS‑RP‑02 — Reset Request Confirmation
• 	UIS‑RP‑03 — Reset Password Screen
• 	UIS‑RP‑04 — Invalid or Expired Token Screen

## Flowchart TD

    A[User Clicks Reset Link] --> B[Extract Token]
    B --> C[Validate Token Exists]
    C --> D{Token Valid?}

    D -->|No| E[Display Invalid/Expired Token Message]
    E --> F[Log Failure]
    F --> END

    D -->|Yes| G[Check Expiration]
    G --> H{Expired?}

    H -->|Yes| E
    H -->|No| I[Check Token Not Used]

    I --> J{Already Used?}
    J -->|Yes| E
    J -->|No| K[Mark Token Validated]
    K --> L[Display Reset Password Screen]
    L --> END
