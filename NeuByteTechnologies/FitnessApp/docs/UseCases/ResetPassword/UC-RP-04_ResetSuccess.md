# UC‑RP‑04 — Reset Success
Use Case ID
UC‑RP‑04
Use Case Name
Reset Success
Module
Reset Password
# Purpose
To define how the system confirms a successful password reset and guides the user back to the login process. This UC ensures the user receives clear confirmation, understands that their password has been updated, and can proceed to authenticate with their new credentials.
This is the fourth step in the Reset Password sequence.
## Primary Actor
Unauthenticated User
## Stakeholders & Interests
• 	User — wants confirmation that their password has been successfully updated.
• 	System — must clearly communicate success and provide a path back to login.
• 	Security Team — requires confirmation that the reset token is invalidated.
• 	Product Owner — wants a smooth, reassuring end to the reset flow.
• 	UX/UI Team — requires consistent success messaging and navigation patterns.

## Preconditions
• 	User has successfully reset their password (UC‑RP‑03).
• 	Reset token has been marked as used.
• 	System has updated the user’s password in the database.

## Postconditions
Success
• 	System displays a confirmation message.
• 	User is directed to log in with their new password.
• 	System ensures the reset token cannot be reused.
Failure
• 	Not applicable — this UC only occurs after a successful reset.

## Trigger
System completes the password update process and transitions to the Reset Success screen.

## Main Success Scenario (Basic Flow — Reset Success)
1. 	System completes the password update process.
(FS‑RP‑PWD‑01)
2. 	System invalidates the reset token to prevent reuse.
(BR‑RP‑07)
3. 	System displays the Reset Success screen, including:
• 	Confirmation message
• 	Instructions to log in
• 	“Return to Login” button
(UIS‑RP‑04)
4. 	User selects “Return to Login.”
5. 	System navigates the user to the Login screen.
(UIS‑LOGIN‑01)

## Alternate Flows
A1 — Automatic Redirect (Optional)
If auto‑redirect is enabled:
• 	System displays success message for a short duration (e.g., 5 seconds).
• 	System automatically redirects user to the Login screen.
A2 — User Closes Browser
• 	User closes the tab or browser.
• 	No further action is required.
• 	User may log in later using their new password.

## Exception Flows
None — this UC only occurs after a successful password reset.

## Non‑Functional Requirements
• 	Security:
• 	Token must be fully invalidated before showing success.
• 	No sensitive information may be displayed.
• 	Performance:
• 	Success screen must load within 100ms.
• 	Accessibility:
• 	Screen must support keyboard navigation and ARIA labels.
• 	Usability:
• 	Messaging must be clear, concise, and reassuring.
• 	Consistency:
• 	Success screen must follow global UI patterns for confirmation states.

## Related UI Screens
• 	UIS‑RP‑03 — Reset Password Screen
• 	UIS‑RP‑04 — Reset Success Screen
• 	UIS‑LOGIN‑01 — Login Screen

## Flowchart TD

    A[Password Reset Completed] --> B[Invalidate Reset Token]
    B --> C[Display Reset Success Screen]
    C --> D[User Selects Return to Login]
    D --> E[Navigate to Login Screen]
    E --> END