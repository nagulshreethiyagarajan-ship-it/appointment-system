# INTELLIGENT PATIENT APPOINTMENT OPTIMIZATION FRAMEWORK USING PREDICTIVE ANALYTICS

A complete, modern, full-stack healthcare SaaS platform and intelligent hospital appointment management system built using pure **Java**, modern web technologies (HTML5, Vanilla CSS3, JavaScript ES6), and a relational **MySQL** database schema.

---

## 🌟 Executive Summary & Key Objectives

Traditional hospital outpatient departments suffer from prolonged patient waiting times, unbalanced physician workloads, unannounced missed appointments (no-shows), and double-booking schedule conflicts. 

This framework solves these critical challenges by integrating:
1. **Dynamic Schedule Optimization**: Automatically checks physician availability and clinic shift hours to eliminate double-booking and schedule collisions.
2. **Predictive Analytics Scoring**: Mines historical patient attendance, lead time in advance, time-of-day peak congestion, and demographics to assess no-show risk scores (0–100%).
3. **AI-Assisted Slot Recommendation**: Computes a multi-criteria Recommendation Score for available candidate slots to balance clinical workload and reduce patient wait times.
4. **Clinical Decision Support**: Provides non-diagnostic operational intelligence for clinic administrators to proactively send confirmations and manage queues.

---

## 🎨 Design System & Visual Palette

The web application is styled as a state-of-the-art Healthcare SaaS dashboard:
- **Deep Navy** (`#0F2747`): Executive headers, navigation sidebar, card borders.
- **Medical Blue** (`#1677FF`): Primary actions, scheduled status badges, key active highlights.
- **Teal** (`#18B7A0`): AI recommendations, optimization scores, success accents.
- **Dark Slate** (`#1E293B`): Clean high-contrast typography.
- **Soft Background** (`#F6F9FC`): Clean, hospital-grade backdrop.
- **Status Accents**: Success (`#22A06B`), Warning (`#F59E0B`), Danger (`#EF4444`).

Typography uses **Inter** and **Plus Jakarta Sans** with glassmorphism overlays, soft multi-layer drop shadows, micro-animations, and responsive layouts across Desktop, Laptop, Tablet, and Mobile.

---

## 🏗️ System Architecture

```
                  ┌────────────────────────────────────────────────────────┐
                  │          100% PURE JAVA DESKTOP APPLICATION            │
                  │   HospitalApp.java  •  MainWindow.java (Java Swing)    │
                  └───────────────────────────┬────────────────────────────┘
                                              │
                      ┌───────────────────────┴────────────────────────┐
                      │                                                │
                      ▼                                                ▼
         [ Pure Java Swing GUI ]                     [ Pure Java HTTP Server (8080) ]
        • Role Auth (Admin/Doc/Pat)                  HospitalHttpServer (JDK standard)
        • Live Analytics Dashboard                                     │
        • Predictive Risk Gauge                      ┌─────────────────┴────────────────┐
        • AI Slot Recommendation Cards               ▼                                  ▼
        • Patient & Doctor Directories         index.html / CSS / JS             REST API Endpoints
                      │                                                        (/api/appointments,
                      │                                                         /api/predict, etc.)
                      │                                                                 │
                      └───────────────────────┬─────────────────────────────────────────┘
                                              ▼
                                  ┌───────────────────────┐
                                  │   Application Core    │
                                  │ • AppointmentService  │
                                  │ • PredictionService   │
                                  │ • OptimizationService │
                                  │ • Patient/DoctorServ. │
                                  └───────────┬───────────┘
                                              ▼
                                  ┌───────────────────────┐
                                  │    Data Layer (DAO)   │
                                  │ • AppointmentDAO      │
                                  │ • DoctorDAO           │
                                  │ • PatientDAO          │
                                  │ • DBConnection        │
                                  └───────────┬───────────┘
                                              ▼
                                  ┌───────────────────────┐
                                  │   MySQL / InMemory    │
                                  │ hospital_appointment_db│
                                  └───────────────────────┘
```

---

## ☕ Java OOP Concepts Demonstrated

This project showcases core Object-Oriented Programming (OOP) and software engineering best practices:

| OOP Concept | Implementation in Project | File Reference |
| :--- | :--- | :--- |
| **Abstraction** | Abstract base class `Person` defines blueprint for individuals in the hospital system without direct instantiation. | [`Person.java`](file:///e:/Java%20programming/backend/src/main/java/com/hospital/appointment/model/Person.java) |
| **Inheritance** | `Patient` and `Doctor` extend `Person`, inheriting shared attributes (`id`, `fullName`, `phone`, `email`) while adding domain-specific fields. | [`Patient.java`](file:///e:/Java%20programming/backend/src/main/java/com/hospital/appointment/model/Patient.java), [`Doctor.java`](file:///e:/Java%20programming/backend/src/main/java/com/hospital/appointment/model/Doctor.java) |
| **Polymorphism** | Abstract method `getRoleDescription()` is overridden polymorphically in `Patient` and `Doctor` classes. | [`Person.java`](file:///e:/Java%20programming/backend/src/main/java/com/hospital/appointment/model/Person.java#L23) |
| **Encapsulation** | Private instance variables with public getter and setter accessors, incorporating business validation rules. | All model classes in `model/` |
| **Collections** | `HashMap` for $O(1)$ fast caching, `ArrayList` for ordered directories, `PriorityQueue` for sorting and ranking optimal slots. | [`OptimizationService.java`](file:///e:/Java%20programming/backend/src/main/java/com/hospital/appointment/service/OptimizationService.java#L39) |
| **Exception Handling** | Custom checked exceptions preventing scheduling collisions and validating doctor duty rosters. | [`AppointmentConflictException.java`](file:///e:/Java%20programming/backend/src/main/java/com/hospital/appointment/exception/AppointmentConflictException.java), [`DoctorUnavailableException.java`](file:///e:/Java%20programming/backend/src/main/java/com/hospital/appointment/exception/DoctorUnavailableException.java) |

---

## 🧠 Predictive Analytics Scoring Algorithm

The `PredictionService` calculates the risk of a patient missing their scheduled visit using a weighted multi-factor heuristic model:

$$\text{Risk Score} = \left( w_1 \cdot \text{NoShowRatio} + w_2 \cdot \text{LeadTimeFactor} + w_3 \cdot \text{PeakCongestion} + w_4 \cdot \text{AgeFactor} \right) \times 100$$

Where:
- $w_1 = 0.45$: Historical patient no-show ratio (past unannounced missed visits / total booked visits).
- $w_2 = 0.25$: Booking lead time in days (longer lag time correlates with forgetting).
- $w_3 = 0.15$: Peak hospital rush hour factor (09:30–11:30 AM & 04:00–06:00 PM).
- $w_4 = 0.15$: Age demographic adjustment (pediatric and elderly care dependencies).

### Risk Categories:
- **Low Risk ($< 35\%$)**: Standard automated notification confirmed.
- **Medium Risk ($35\% - 59\%$)**: 24-hour reminder suggested.
- **High Risk ($\ge 60\%$)**: Warning flag, priority phone call/SMS outreach advised.

> [!NOTE]
> **Decision Support Notice**: Predictions represent operational scheduling guidance to help hospital staff manage queues, and do not constitute clinical medical diagnosis.

---

## ⚡ AI Appointment Optimization Engine

The optimization engine evaluates candidate consultation slots using a 7-step pipeline:

$$\text{Patient Preferences} + \text{Doctor Availability} + \text{Historical Patterns} + \text{Predicted Attendance} = \text{Recommended Slots}$$

### Workflow Pipeline:
1. **Patient Request**: Target clinical department, date, preferred time window (Morning/Afternoon).
2. **Doctor Availability**: Query on-duty physician shift hours and break windows.
3. **Historical Patterns**: Analyze doctor workload percentage and peak congestion windows.
4. **Predict Attendance**: Evaluate projected no-show risk score.
5. **Check Conflicts**: Eliminate any overlapping appointments or double-bookings.
6. **Rank Available Slots**: Compute Composite Recommendation Score (0–100%) using a `PriorityQueue`.
7. **Recommend Best Slots**: Render ranked slot cards with score percentage, physician details, and one-click booking.

---

## 🗄️ Relational Database Schema (`schema.sql`)

The MySQL database contains 6 normalized relational tables:
1. `patients`: Demographics, contact information, attendance history counters, and status.
2. `doctors`: Specialization, department, qualifications, consultation fees, daily capacity, and workload percentage.
3. `doctor_availability`: Weekly consultation shifts, break hours, and on-duty flags.
4. `appointments`: Central booking table connecting patients and doctors with unique constraint `(doctor_id, appointment_date, start_time)` preventing double-bookings.
5. `appointment_history`: Audit trail capturing actions (`Created`, `Rescheduled`, `Completed`, `Cancelled`, `Marked Missed`).
6. `predictions`: Log of predictive scores, risk levels, and decision support notes.

Triggers (`trg_after_appointment_insert`, `trg_after_appointment_update`) automatically maintain patient attendance metrics in real-time.

---

## 🚀 How to Run the 100% Pure Java Application

The entire platform runs natively in Java with **zero external web runtimes, non-Java programs, or complex dependencies required**.

### Option 1: Run 100% Pure Java Desktop GUI (Recommended)
You can launch the complete native Java desktop GUI application by either:

**Double-clicking:**
- `run_gui.bat` (on Windows)

**Or running via Terminal / PowerShell:**
```powershell
.\run_gui.ps1
```

**Or compiling manually with standard `javac` & `java`:**
```powershell
# Compile all Java sources
Get-ChildItem -Path "backend/src/main/java" -Recurse -Filter "*.java" | ForEach-Object { '"' + ($_.FullName -replace '\\', '/') + '"' } | Out-File -Encoding ascii sources.txt
javac -d backend/bin "@sources.txt"
Remove-Item sources.txt

# Launch Desktop GUI
java -cp "backend/bin" com.hospital.appointment.HospitalApp
```

---

### Option 2: Run Java Console Test Harness (OOP & Logic Demos)
To demonstrate the Object-Oriented principles, collections, and algorithms directly in the terminal:
```powershell
java -cp "backend/bin" com.hospital.appointment.Application
```

---

### Option 3: Start the Pure Java HTTP Server & REST API
```powershell
java -cp "backend/bin" com.hospital.appointment.server.HospitalHttpServer 8080 .
```
Access in browser at: `http://localhost:8080/`

---

## 📋 Comprehensive Module Inventory

1. **Login & Role-Based Access**:
   - Split-screen banner with AI branding, role switcher (`Administrator`, `Doctor`, `Patient`), and 1-click demo logins.
2. **Executive Analytics Dashboard**:
   - Hero banner with quick optimization CTAs.
   - 7 Real-time KPI statistics cards.
   - Peak Hours interactive congestion heatmap.
   - Appointments timeline chart, doctor workload bar chart, and status donut chart.
3. **Patient Management**:
   - Search, department filters, and attendance badges (`Active`, `High Attendance`, `⚠️ Missed`).
   - Add/Edit/Delete patient modals.
   - Patient Clinical Profile modal with attendance track and visit timeline.
4. **Doctor Management**:
   - Doctor cards with specialization, workload progress meters, and consultation fees.
   - Weekly Availability Schedule Matrix modal.
5. **Appointment Management**:
   - Dynamic time slot generator (09:00 AM – 05:00 PM).
   - Visual slot cards with automatic disable on booked intervals.
   - Strict schedule conflict prevention.
   - Reschedule, cancel with reason, and complete action controls.
6. **Predictive Analytics Center**:
   - Interactive missed appointment risk assessment card with animated speedometer gauge.
   - Clinical decision support alert disclaimer.
   - Heuristics weight breakdown and department attendance rates.
7. **AI Optimization Module**:
   - 7-step visual workflow pipeline.
   - Interactive optimization parameter controls.
   - Top-ranked slot recommendations with composite score percentages (e.g. 88%, 84%).
8. **Data Management / Admin Database Explorer**:
   - Relational table viewer (`patients`, `doctors`, `appointments`).
9. **Reports & Export Module**:
   - Daily, Weekly, Workload, and Attendance summaries.
   - Live **Export to CSV** file download.
   - Print-ready formatted document view.
10. **Patient-Facing Self-Service Portal**:
    - 4-step streamlined booking wizard.
    - Digital confirmation ticket pass with appointment reference ID.

---

## 🔐 Role-Based Access Control & Demo Credentials

When entering `http://localhost:8080/`, the user is greeted by the modern **Login Screen**. The system implements role-specific permissions and selective views:

| Role | Demo Username / Email | Demo Password | Accessible Modules & Selective Content |
| :--- | :--- | :--- | :--- |
| **Administrator** | `admin` (or `admin@hospital.org`) | `admin123` | Full Hospital Executive Dashboard (7 KPIs, Heatmap, Charts), Patient Registry (can create new patient accounts with Username, Email, Password), Doctor Directory, All Appointments, Predictive Engine, AI Optimization, Database Explorer, Reports & CSV Export. |
| **Doctor** | `dr.arun` (or `DOC001`) | `doctor123` | Physician Clinical Workspace: Today's Consultation Queue for Dr. Arun Kumar, One-click Complete/Reschedule/Cancel actions, Weekly Clinical Duty Shift Table, and Patient Attendance Risk analysis. |
| **Patient** | `kavita` (or `kavita.sharma@example.com` or any patient created by Admin) | `password123` | Patient Self-Service Portal: 4-step Appointment Booking Wizard, "My Scheduled Appointments" pass list with Print Slip, and "My Health Profile" with attendance reliability score and login username. |

> [!TIP]
> **Admin Patient Creation Workflow**:
> 1. Sign in as **Administrator**.
> 2. Go to **Patients** module and click **➕ Add New Patient**.
> 3. Fill in the **🔐 Patient Portal Login Credentials** (`Username`, `Email Address`, and `Password`).
> 4. Fill in demographics and click **Create Patient Account & Profile**.
> 5. Sign out. On the Login screen, select **Patient**, enter the newly registered username and password, and sign into that patient's personal portal!

---

## 🎯 Viva & Project Review Key Talking Points

When presenting this project in a review or viva:
1. **Explain the Motivation**: Highlight how standard clinic scheduling leads to waiting room congestion and idle doctor capacity, which predictive optimization directly resolves.
2. **Demonstrate OOP Polymorphism**: Point to `Person.java` and show how `Patient` and `Doctor` polymorphically implement `getRoleDescription()`.
3. **Demonstrate Conflict Detection**: Show that selecting a booked slot throws `AppointmentConflictException` in Java and displays an immediate conflict warning in the web UI.
4. **Demonstrate the Predictive Engine**: Select patient P001 (low risk: 15%) vs patient P002 (medium/high risk: 30–68%) and explain how past missed visits and lead time contribute to the score.
5. **Demonstrate the Optimization Pipeline**: Show how the formula balances physician workload to avoid doctor fatigue while offering the most convenient slots to patients.
6. **Demonstrate Role-Based Access Control**: Sign in as Admin to create a patient profile with custom credentials, then log in as that Patient to verify selective access and personalized booking history.

#   a p p o i n t m e n t - s y s t e m  
 