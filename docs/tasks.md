Vitanet - Task Breakdown & Development Roadmap (tasks.md)
Phase 1: Project Initialization & Infrastructure Setup
[ ] Initialize GitHub repository and set up folder structure (app.py, static/, templates/, requirements.txt).

[ ] Configure Python virtual environment and install core packages (Flask, pymongo, google-generativeai, boto3, gunicorn).

[ ] Set up DigitalOcean Managed MongoDB cluster and configure connection URI in environment variables (.env).

[ ] Configure DigitalOcean Spaces object storage and write the boto3 utility script for file uploads.

[ ] Provision and configure a DigitalOcean Droplet as a dedicated Solana RPC proxy node.

Phase 2: Database Schema & Backend Core Setup
[ ] Establish MongoDB collections and indexes for hospitals, users (donors/patients), donations_matching, and reviews.

[ ] Implement Flask application factory and base error-handling routes in app.py.

[ ] Set up user authentication and password hashing utilities.

Phase 3: AI Integration & Hospital Vetting Module
[ ] Build the Hospital Registration Form (HTML/CSS) capturing name, license number, email, and location.

[ ] Integrate Gemini API (gemini-1.5-flash) inside the Flask backend for automated background verification and risk analysis.

[ ] Store generated AI reports alongside hospital profiles in MongoDB with an is_verified status flag.

Phase 4: Donor & Patient Portals (With File Upload & Escrow)
[ ] Create the Donor Registration Portal with form fields for blood group, organ type, contact info, and medical history.

[ ] Implement file upload handling to securely store donor health certificates on DigitalOcean Spaces.

[ ] Create the Patient Registration Portal capturing organ requirements, urgency levels, and Solana wallet details.

[ ] Implement the database state tracker for Solana deposits (Locked in Escrow).

Phase 5: Smart Organ Matching & Hospital Verification Workflow
[ ] Develop the Smart Organ Matching Algorithm matching donors and patients based on blood group, organ compatibility, and urgency.

[ ] Build the Hospital Admin Dashboard displaying pending matches and verification queues.

[ ] Implement the hospital verification action that triggers the backend to release escrow funds via the dedicated Solana RPC proxy node.

Phase 6: Ratings, Reviews & Frontend Polish
[ ] Build the Hospital Rating & Review System allowing users to leave 1–5 star ratings post-operation.

[ ] Apply the unified UI/UX design system (styling, colors, typography, and responsive layouts) across all HTML templates using Tailwind CSS or custom CSS.

[ ] Conduct end-to-end testing of the entire user journey (Hospital registration -> AI check -> Donor/Patient registration -> Escrow lock -> Hospital verification -> Solana payout).

Phase 7: Deployment & Production Launch
[ ] Create Procfile and configure deployment settings for DigitalOcean App Platform.

[ ] Push code to GitHub and verify automatic builds and environment variable injections.

[ ] Perform live smoke tests on the production domain.