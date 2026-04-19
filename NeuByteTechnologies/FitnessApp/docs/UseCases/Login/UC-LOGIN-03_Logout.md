# UC‑LOGIN‑03 — Logout

Use Case ID
UC‑LOGIN‑03
Use Case Name
Logout
Module
Login / Authentication
## Purpose
To allow an authenticated user to securely end their session, ensuring that no further actions can be taken under their account until they log in again. This protects user data and prevents unauthorized access on shared or unattended devices.
## Primary Actor
Authenticated User
## Stakeholders & Interests
• 	User — wants to securely exit the application and protect their account.
• 	System — must reliably terminate sessions and clear authentication tokens.
• 	Security Team — requires proper session invalidation to prevent unauthorized access.
• 	Product Owner — wants a clear, predictable logout experience.
• 	Analytics Team — needs to track logout events.

## Preconditions
• 	User is authenticated.
• 	User has an active session token.
• 	Logout option is available in the global header or menu.

## Postconditions
Success
• 	Session token is invalidated.
• 	Session cookie is cleared or expired.
• 	User is redirected to the Login page.
• 	User sees a confirmation message.
Failure
• 	If session cannot be invalidated, system logs an error and forces logout on next request.

## Trigger
User selects Logout from the global header or menu.

## Main Success Scenario (Basic Flow)
1. 	User selects Logout from the global header or menu.
(BR‑Login‑10)
2. 	System receives the logout request.
(FS‑LOGIN‑LOGOUT‑01)
3. 	System invalidates the user’s session token.
(BR‑Login‑09)
4. 	System clears or expires the session cookie.
(BR‑Login‑09)
5. 	System redirects the user to the Login page.
(BR‑Login‑07)
6. 	System displays a message:
“You have been logged out.”
7. 	System logs the logout event.
(Analytics requirement)

## Alternate Flows
A1 — User Closes Browser Instead of Logging Out
• 	User closes the browser tab or window.
• 	Session remains active until timeout (UC‑LOGIN‑02).
• 	No explicit logout event is logged.
A2 — User Logs Out from Multiple Tabs
• 	User selects Logout in one tab.
• 	System invalidates the session.
• 	Other tabs detect invalid session on next request and redirect to Login.

## Exception Flows
E1 — Session Invalidation Error
• 	Step 3 fails due to backend or token store issue.
• 	System logs an error.
• 	System forces logout on next request by rejecting the session token.
E2 — Network Error During Logout
• 	Logout request fails to reach server.
• 	System clears local session cookie.
• 	On next request, server rejects the missing/invalid token and redirects to Login.

## Non‑Functional Requirements
• 	Security: Logout must fully invalidate the session token. (BR‑Login‑09)
• 	Performance: Logout must complete instantly.
• 	Reliability: Logout must work consistently across devices and tabs.
• 	Accessibility: Logout option must be keyboard‑accessible. (SRS‑A11Y‑01)
• 	Analytics: Logout events must be logged.

## Related UI Screens
• 	UIS‑GLOBAL‑HEADER‑01 — Logout Link
• 	UIS‑LOGIN‑01 — Login Page
• 	UIS‑GLOBAL‑FOOTER‑01