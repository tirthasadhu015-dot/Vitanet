Vitanet - System Architecture
1. High-Level Architecture Diagram
Plaintext
                                [ Users / Hospitals ] (Browser / Client)
                                            │
                                 (HTTPS / Custom Domain)
                                            ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                     DigitalOcean App Platform (PaaS)                        │
│                 (Python Flask Backend + HTML/CSS/JS Frontend)               │
└───────┬───────────────────────┬─────────────────────────┬───────────────────┘
        │                       │                         │
        ▼                       ▼                         ▼
┌──────────────┐        ┌───────────────┐         ┌───────────────────────────┐
│ DO Managed   │        │   DO Spaces   │         │ Dedicated Solana RPC      │
│   MongoDB    │        │(Object Store) │         │ Proxy Node (DO Droplet)   │
│ (Database)   │        │ (Cert / Docs) │         │   (Solana Web3 Engine)    │
└──────────────┘        └───────────────┘         └─────────────┬─────────────┘
                                                                │
                                                                ▼
                                                [ Solana Mainnet / Devnet ]
2. Component Breakdown
2.1 Frontend Tier (Client Side)
Technologies: HTML5, CSS3, JavaScript

Responsibility: Renders responsive user interfaces, forms (Donor, Patient, and Hospital registration), dynamic dashboards, and status trackers. Communicates asynchronously with the backend via RESTful APIs.

2.2 Application Tier (Backend & Business Logic)
Technologies: Python, Flask, Gunicorn

Responsibility:

Manages routing, user authentication, and session handling.

Implements the Smart Organ Matching Algorithm based on blood type compatibility, organ category, urgency levels, and location.

Coordinates API communications with the Gemini AI engine for hospital background vetting.

Manages escrow state transitions and triggers blockchain payout actions.

2.3 Data Storage & Database Tier
Technologies: DigitalOcean Managed MongoDB

Responsibility: Stores flexible NoSQL documents for users (donors and patients), hospital profiles, matching records, audit logs, and review ratings without rigid schema bottlenecks.

2.4 Cloud Object Storage Tier
Technologies: DigitalOcean Spaces (S3-Compatible Object Storage)

Responsibility: Secures and stores sensitive medical documents, such as donor health certificates and hospital verification licenses, providing secure download and retrieval URLs.

2.5 Blockchain & RPC Infrastructure Tier
Technologies: DigitalOcean Droplet (Ubuntu), Solana Web3 SDK

Responsibility: Hosts a Dedicated Solana RPC Proxy Node to eliminate public rate limits, ensure low-latency transactions, and execute secure smart escrow lock and release workflows upon hospital confirmation.

2.6 Artificial Intelligence Tier
Technologies: Gemini API (gemini-1.5-flash)

Responsibility: Automatically parses incoming hospital registration inputs to run preliminary background checks, risk assessments, and legitimacy evaluations.

3. Core Data Flow Workflows
Workflow A: Hospital Registration & AI Vetting
Input: Hospital submits credentials (Name, License Number, Location, Email) via the frontend portal.

AI Analysis: Flask backend forwards the credentials to the Gemini API for automated background simulation.

Persistence: The structured profile and generated AI risk report are saved into the DigitalOcean Managed MongoDB hospitals collection.

Workflow B: Patient Deposit & Solana Escrow
Input: Patient registers and connects their Solana wallet.

Escrow Lock: Patient deposits Solana (SOL) coins into the platform pool.

State Update: Database updates the patient record with deposit_status: "Locked in Escrow".

Workflow C: Verification & Payout Execution
Operation & Review: The transplant operation takes place at the designated verified hospital.

Hospital Sign-off: Hospital admins approve and verify the successful donation through their secure dashboard.

Blockchain Settlement: The backend routes the transaction instruction through the Dedicated Solana RPC Proxy Node to transfer locked SOL funds directly from escrow to the donor's verified wallet address.