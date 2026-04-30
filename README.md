PARKSYS — Parking Lot Management System

DBMS Project | UCS310 | Thapar Institute of Engineering and Technology

A full-stack parking lot management system built with MySQL, Node.js, and React. Supports vehicle entry/exit, automatic slot assignment, fee calculation, and admin reporting.

Team
NameRoll NumberLovish Bansal1024030297Vaibhav Budhia1024030307Aayushi Uniyal1024030308

Tech Stack
LayerTechnologyDatabaseMySQL 8.0BackendNode.js + ExpressFrontendReact + Tailwind CSS

Project Structure
parking-system/
├── database/
│   ├── schema.sql        # 6 tables with all constraints
│   ├── triggers.sql      # 2 triggers (entry + exit)
│   ├── procedures.sql    # 2 stored procedures + 2 views
│   └── seed.sql          # sample data
├── backend/
│   ├── server.js         # Express server
│   ├── db.js             # MySQL connection
│   ├── middleware.js     # JWT auth guard
│   └── routes/
│       ├── auth.js       # login
│       ├── owners.js     # owner CRUD
│       ├── vehicles.js   # vehicle CRUD
│       ├── slots.js      # slot status
│       ├── parking.js    # entry / exit
│       └── reports.js    # revenue + summary
└── frontend/
    └── src/
        ├── pages/
        │   ├── Login.jsx
        │   ├── Operator.jsx
        │   ├── SlotMap.jsx
        │   └── Dashboard.jsx
        └── components/
            └── Navbar.jsx

Database Schema
Six tables connected through foreign keys:
ADMIN ──────────────────────────── (login accounts)
OWNER ──────┐
            ↓
         VEHICLE ──────┐
                       ↓
PARKING_SLOT ──→ PARKING_RECORD ──→ PAYMENT
Tables
TablePurposeADMINLogin accounts for admin and operator staffOWNERVehicle owner detailsVEHICLERegistered vehicles linked to ownersPARKING_SLOTPhysical parking spots (12 slots, 2 floors)PARKING_RECORDOne row per parking sessionPAYMENTPayment record per completed session
PL/SQL Components
ComponentNameWhat it doesTriggertrg_after_entryMarks slot occupied on vehicle entryTriggertrg_before_exitCalculates fee, frees slot on exitProceduresp_vehicle_entryFinds free slot, logs entryProceduresp_vehicle_exitSets exit time, creates payment recordViewvw_occupied_slotsLive snapshot of occupied slotsViewvw_daily_revenueRevenue grouped by day
Fee Structure
Vehicle TypeRateTwo-wheeler₹10 / hourFour-wheeler₹20 / hourHeavy vehicle₹40 / hour
Minimum charge: 1 hour. Fee calculated automatically by trg_before_exit.

Setup & Running
Prerequisites

MySQL 8.0 + MySQL Workbench
Node.js v18+

Step 1 — Database
Open MySQL Workbench and run these files in order:
1. database/schema.sql
2. database/triggers.sql
3. database/procedures.sql
4. database/seed.sql
Step 2 — Backend
bashcd backend
npm install
Open db.js and set your MySQL password on line 6:
jspassword : 'your_mysql_password',
Then start the server:
bashnode server.js
Server runs on http://localhost:5000
Step 3 — Frontend
bashcd frontend
npm install
npm run dev
Frontend runs on http://localhost:5173

Both backend and frontend must be running at the same time.


Login Credentials
RoleUsernamePasswordAdminadminadmin123Operatoroperatoroperator123

Features
Operator Panel

Select a registered vehicle and log entry — slot assigned automatically
Select an active vehicle and log exit — fee calculated and bill generated
View all currently parked vehicles in real time

Slot Map

Color-coded grid of all 12 parking slots
Green = available, Red = occupied
Auto-refreshes every 10 seconds
Filter by status or vehicle type

Admin Dashboard

Summary cards — available slots, occupied slots, active vehicles, today's revenue
Daily revenue table from database view
Complete parking history with amounts and payment modes


API Endpoints
MethodEndpointDescriptionPOST/api/auth/loginLogin and get JWT tokenGET/api/vehiclesList all vehiclesGET/api/slotsList all slots with statusGET/api/slots/occupiedCurrently occupied slotsPOST/api/parking/entryLog vehicle entryPOST/api/parking/exitLog vehicle exit + paymentGET/api/parking/activeAll vehicles currently parkedGET/api/parking/historyCompleted parking sessionsGET/api/reports/summaryDashboard summary numbersGET/api/reports/revenueDaily revenue from DB view

Demo Flow

Log in as admin
Go to Operator Panel — select a vehicle, click Log Entry
Go to Slot Map — see the slot flip to red
Back to Operator Panel — select the vehicle under exit, click Log Exit
Go to Dashboard — see the revenue and history update


Submitted for UCS310 — Database Management Systems, Thapar Institute of Engineering and Technology
