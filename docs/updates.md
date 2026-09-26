Vitanet - Feature Updates & Requirements Patch (updates.md)
1. Donor Authentication & Status Management
Donor Login Portal: Inside the "Donate" section, add a secure Donor Login option alongside registration. Donors can log in using unique credentials (e.g., generated username/ID and password) to check their real-time donation status.

Registration Cancellation: Authenticated donors must have the ability to cancel their organ donation registration from their dashboard if they change their mind, which updates their status in the SQLite database and removes them from active matching pools.

2. Hospital-Exclusive "Find an Organ" Search & 4-Digit ID Generation
Unique Hospital ID Generation: Upon successful hospital registration and verification, the system must automatically generate a unique 4-digit random numeric ID (e.g., 4821) assigned to that hospital.

Restricted Search Access: The "Find an organ" feature/search page must be strictly restricted to verified registered hospitals only. Non-hospital users cannot access or execute searches.

Mandatory Hospital ID Input: To perform an organ search on the platform, the hospital must enter their unique 4-digit hospital registration ID for authorization and auditing.

3. Scope & UI Consistency Guidelines
Minimal Scope Changes: Only implement the specified requirements (Donor login/cancellation, 4-digit hospital ID generation, and hospital-restricted organ search). Avoid any major architectural rehauls.

UI Preservation: Maintain the exact existing UI design, styling, color palette, and layout as shown in the production interface (Deep Medical Blue background, teal action buttons, and clean component hierarchy) without altering the core visual framework.
