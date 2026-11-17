# SmartMed – Complete Medicine Stock & Expiry Management System

> **B.Tech College CEP Project Demonstration**  
> **Technology Stack:** PHP 8+ | MySQL / MariaDB | Bootstrap 5 | JavaScript (ES6+) | Chart.js | Bootstrap Icons | XAMPP

---

## Project Overview

**SmartMed** is a full-stack clinical pharmacy inventory and expiry management web application developed for hospital pharmacies, community health clinics, and dispensary environments. It provides real-time stock control, multi-batch tracking, intelligent First-Expired First-Out (FEFO) dispensing guidance, automated dynamic expiry alerts, low-stock reorder thresholds, and comprehensive reporting with 1-click CSV export.

All inventory records, transactions, audit logs, and settings are stored in a relational **MySQL** database (`smartmed_db`) without relying on browser `localStorage` as primary storage.

---

## Key Features

1. **160 Reference Generic Indian Medicines Catalog:**
   - 160 genuine reference medicine records commonly stocked across Indian pharmacies.
   - Spanning **20 distinct clinical categories**: General medicines, Fever and pain relief, Headache and migraine, Cold, cough and allergy, Acidity and digestion, Antibiotics, Antifungal medicines, Antiviral medicines, Diabetes medicines, Blood pressure medicines, Cardiovascular medicines, Respiratory and asthma medicines, Gastrointestinal medicines, Neurological medicines, Skin care medicines, Women's healthcare, Pediatric medicines, Vitamins and supplements, Eye and ear care, Emergency and clinic supplies.
   - Genuine generic names, verified Indian brand names (e.g. *Dolo 650, Augmentin 625, Pan-D, Shelcal, Montair LC*), standard Indian pharmaceutical manufacturers (Cipla, Sun Pharma, Dr. Reddy's, Glenmark, Alkem, Abbott India, Torrent), dosage forms, strengths, and standard `MED-0001` format IDs.

2. **Multi-Batch Inventory Tracking:**
   - Supports multiple packaging batches per medicine with distinct packaging batch numbers, purchase dates, expiry dates, supplier associations, unit costs, and shelf locations (e.g. *Rack A-01, Cold Chain Cooler 1*).
   - Clear sample data badges distinguishing demonstration records.

3. **Dynamic Traffic-Light Expiry Management:**
   - Real-time date arithmetic using PHP and MySQL `CURDATE()`.
   - **Green (SAFE):** Expiry date is greater than the warning threshold (> 90 days).
   - **Yellow (EXPIRING SOON):** Expiry date falls within the warning period (<= 90 days) with countdown of days remaining.
   - **Red (EXPIRED):** Expiry date has passed. **Strictly blocked from outward dispensing** with authorized hospital disposal logging.
   - Configurable warning threshold via Admin Settings (default: 90 days).

4. **Stock In Module (`stock_in.php`):**
   - Atomic database transactions receiving incoming consignments.
   - Creates or updates batches, updates medicine stock balances, records permanent ledger transactions, and logs audit events.

5. **Stock Out with Strict FEFO Protocol (`stock_out.php`):**
   - Enforces **First Expire, First Out (FEFO)** to prevent inventory wastage.
   - Database row-locking (`SELECT ... FOR UPDATE`) prevents concurrent negative balances.
   - Automatically recommends the earliest-expiring batch; prompts for override confirmation if a staff member attempts to issue from a later consignment.
   - Hard blocks expired batches from dispensation.

6. **Low Stock Management (`low_stock.php`):**
   - Automatically compares live available batch inventory against minimum safety stock thresholds.
   - Identifies critical deficits and calculates recommended reorder quantities.

7. **Interactive Professional Dashboard (`dashboard.php`):**
   - Live KPI statistic cards (Total Medicines, Available Units, Low Stock Items, Expiring Soon, Expired Batches, Suppliers, Today's Inflow, Today's Dispensed).
   - **5 Dynamic Chart.js visualizations** driven by MySQL:
     1. *Medicines by Category (Doughnut Chart)*
     2. *Expiry Status Distribution (Traffic-Light Doughnut)*
     3. *Highest Stocked Medicines (Bar Chart)*
     4. *Monthly Stock Movements In vs Out (Grouped Bar Chart)*
     5. *Low-Stock Reorder Deficit (Bar Chart)*
   - Active operational alerts and recent chronological transaction table.

8. **Permanent Movement Ledger (`transactions.php`):**
   - Permanent immutable audit trail of all `STOCK IN`, `STOCK OUT`, `ADJUSTMENT`, and `EXPIRED` events.
   - Multi-parameter filtering by date range, transaction type, medicine, and batch number.

9. **Authorized Supplier Management (`suppliers.php`):**
   - Full vendor CRUD operations, wholesale drug license numbers, contact details, and supplied batch counts.
   - Safe deletion validation prevents deleting vendors with active inventory consignments.

10. **Reports & 1-Click CSV Export (`reports.php`):**
    - 9 comprehensive report types: Complete Inventory Summary, Batch-Wise Expiry Intelligence, Expired Quarantine, Low Safety Stock, Stock Movement In/Out, Supplier Performance, Consignment Intake, Prescription Dispensation, and System Audit Logs.
    - Direct 1-click RFC 4180 CSV export download and clean print-to-PDF styles (`@media print`).

11. **Role-Based Access Control & User Management (`users.php`):**
    - Secure session authentication with `session_regenerate_id()`.
    - Passwords securely hashed with **Bcrypt** (`password_hash` & `password_verify`).
    - Granular permissions: `admin` (master record management, user accounts, system settings, expired batch disposal) vs `staff` (dispensing, stock intake, inventory lookup, reports).

12. **Database Diagnostic & Health Wizard (`setup.php`):**
    - Automated 1-click database synchronization and diagnostic suite.
    - Safely creates `smartmed_db`, missing tables, missing columns, and seeds 160 generic Indian medicines without deleting existing records.

---

## 8-Step Localhost Setup Guide (XAMPP & VS Code)

### Step 1: Install & Launch XAMPP
Ensure you have **XAMPP** installed with PHP 8+ and MySQL/MariaDB.
1. Open the **XAMPP Control Panel**.
2. Click **Start** next to **Apache**.
3. Click **Start** next to **MySQL**.

### Step 2: Place Project in `htdocs`
Copy the project folder into your XAMPP web root directory:
```
C:\xampp\htdocs\SmartMed
```

### Step 3: Open in VS Code
Open the folder `C:\xampp\htdocs\SmartMed` in Visual Studio Code to review and inspect source code.

### Step 4: Open the Setup Wizard
Open your browser and navigate to:
```
http://localhost/SmartMed/setup.php
```
The wizard checks your PHP version, PDO MySQL driver, and server connection.

### Step 5: 1-Click Initialize / Sync Database
Click the button: **"Run Safe Database Setup & Migration"**.
The wizard will:
- Automatically create the database `smartmed_db`.
- Create all 11 database tables safely (`CREATE TABLE IF NOT EXISTS`).
- Populate the 160 reference generic Indian medicines across 20 categories.
- Seed demo suppliers, categories, settings, and sample batches (Safe, Expiring Soon, Expired, Low Stock).
- Seed demo admin and staff accounts.
*(Existing records are preserved during upgrades).*

### Step 6: Access Application
Navigate to:
```
http://localhost/SmartMed/
```
The system will route you directly to the login interface (`login.php`).

### Step 7: Log In With Demo Credentials
Use either of the pre-configured accounts:

| Role | Username | Password | Privileges |
|---|---|---|---|
| **Administrator** | `admin` | `admin123` | Full access (Master records, User management, Settings, Batch disposal) |
| **Healthcare Staff** | `staff` | `staff123` | Operational access (Stock In, Stock Out with FEFO, Catalog lookup, Reports) |

### Step 8: Ready for College CEP Project Demonstration
You can now demonstrate:
- Real-time KPI cards and dynamic Chart.js visualizations on the **Dashboard**.
- Searching and filtering through 160 generic medicines on the **Medicines Catalog**.
- Multi-batch tracking with traffic-light status on **Batches**.
- Adding new inventory via **Stock In**.
- Dispensing medication with **FEFO recommendation & expired batch safety blocks** on **Stock Out**.
- Traffic-light filtered quarantine on **Expiry Alerts** with admin safe disposal.
- Deficit calculations on **Low Stock**.
- Downloading reports with 1-click CSV on **Reports**.
- Managing user roles and permissions on **Users**.

---

## Technology Architecture & Folder Structure

```
SmartMed/
├── api.php                     # Central JSON REST API gateway
├── config.php                  # Global configuration, RBAC, expiry calculation & audit helpers
├── db.php                      # PDO MySQL singleton with auto-database creation
├── index.php                   # Primary route controller (redirects to dashboard or login)
├── login.php                   # Authentication controller with Bcrypt verification
├── logout.php                  # Session destruction controller
├── dashboard.php               # Professional clinical dashboard with 5 Chart.js visualizations
├── medicines.php               # 160 Reference medicines catalog with multi-attribute filtering & modals
├── batches.php                 # Batch-wise inventory ledger with packaging expiry & location tracking
├── stock_in.php                # Inventory intake controller with atomic batch updates
├── stock_out.php               # Outward dispensing controller with FEFO guidance & row locks
├── expiry_alerts.php           # Traffic-light expiry monitoring (Red / Yellow / Green) & disposal
├── low_stock.php               # Safety stock comparison, deficit calculation & reorder shortcuts
├── suppliers.php               # Pharmaceutical vendor management with active batch dependency check
├── transactions.php            # Permanent immutable movement ledger with date & type filters
├── reports.php                 # 9 Clinical inventory reports with 1-click CSV export & print PDF
├── users.php                   # User account management with Bcrypt hashing (Admin only)
├── profile.php                 # Current user profile editing & secure password update
├── settings.php                # System configuration editor (expiry warning threshold, clinic name)
├── setup.php                   # 1-Click automated database setup & health diagnostics wizard
├── init_db.php                 # CLI & web database migration entrypoint
├── test_smartmed.php           # Automated verification test suite
│
├── database/
│   ├── schema.sql              # Raw SQL schema for phpMyAdmin manual import
│   ├── migrate.php             # Safe procedural migration script (CREATE IF NOT EXISTS & ALTERs)
│   └── seed_medicines_data.php # 160 Reference generic Indian medicines across 20 categories
│
└── assets/
    ├── css/
    │   └── style.css           # Healthcare design system (Dark navy `#0d1b2a`, teal `#32b5b3`, print styles)
    └── js/
        ├── app.js              # Sidebar renderer, theme manager, session sync, toasts
        ├── auth.js             # Client route guards & session synchronization
        ├── dashboard.js        # Dynamic Chart.js loaders fetching live MySQL datasets
        ├── medicines.js        # Catalog search, filter, details modal & CRUD AJAX
        ├── batches.js          # Batch inventory search & admin disposal AJAX
        ├── stock.js            # Stock-in & Stock-out with FEFO recommendation and override alert
        ├── suppliers.js        # Vendor lookup, active consignment validation & modal AJAX
        ├── transactions.js     # Multi-parameter movement ledger filtering AJAX
        └── storage.js          # Legacy state fallback & background sync
```

---

## Automated Verification Suite

To verify system health and catalog integrity from the command line:
```powershell
C:\xampp\php\php.exe test_smartmed.php
```
**Test Results:**
- Reference catalog verification: 160 medicines covering 20 distinct categories.
- Unique `MED-0001` format ID verification without duplicates.
- Dynamic date calculation algorithm (+150d = Safe, +45d = Expiring Soon, -10d = Expired).
- Medical disclaimer constant verification.
- Zero PHP syntax errors across all application files (`php -l`).

---

## Medical Disclaimer

> **IMPORTANT:** SmartMed is developed strictly for educational and demonstration purposes as a B.Tech college CEP project demonstration. All clinical records, inventory numbers, and FEFO suggestions are provided for inventory and reference purposes only, not a substitute for professional medical advice, diagnosis, or treatment. Medicines must be dispensed solely according to qualified healthcare professionals' prescriptions and regulatory guidelines.
#   N e w _ M e d t e c h  
 #   m e d T e c h  
 #   m e d T e c h  
 