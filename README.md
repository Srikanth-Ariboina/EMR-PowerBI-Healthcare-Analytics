# EMR Healthcare Analytics – Power BI Dashboard

## 📊 Project Overview

**EMR Healthcare Analytics** is an end-to-end Power BI project designed to analyze Electronic Medical Record (EMR) and hospital operational data.

The dashboard provides management-level insights into **patient activity, OPD visits, IPD admissions, laboratory performance, pharmacy operations, and hospital billing**.

The project demonstrates practical skills in **Power BI, DAX, Power Query, data modeling, SQL, data visualization, and healthcare analytics**.

---

##  Business Objective

The objective of this project is to provide a centralized analytics solution that helps hospital management monitor:

* Patient volume and encounter trends
* OPD and follow-up activity
* IPD admissions and length of stay
* Ward and ICU activity
* Pharmacy sales and medicine utilization
* Hospital billing and collections
* Doctor and department performance

---

## 🛠️ Technologies Used

| Technology          | Purpose                                |
| ------------------- | -------------------------------------- |
| Power BI Desktop    | Dashboard development and reporting    |
| DAX                 | KPI calculations and time intelligence |
| Power Query         | Data cleaning and transformation       |
| SQL / Oracle SQL    | Data extraction and analysis           |
| Excel               | Sample EMR dataset                     |
| Star Schema         | Data modeling                          |
                   |

---

## 🏥 Healthcare Domain

The project simulates an EMR environment containing:

* Patient registration
* Doctor information
* OPD admissions
* Emergency visits
* IPD admissions
* Discharges
* Laboratory orders
* Pharmacy transactions
* Hospital billing

---

#  Project Structure

```text
EMR-PowerBI-Healthcare-Analytics/
│
├── README.md
│
├── Dashboard/
│   ├── 01_Executive_Overview.png
│   ├── 02_Patient_OPD_Analytics.png
│   ├── 03_IPD_Admission_Analytics.png
│   ├── 04_Laboratory_Analytics.png
│   ├── 05_Pharmacy_Analytics.png
│   └── 06_Billing_Finance.png
│
├── PowerBI/
│   ├── EMR_Analytics_Dashboard.pbix
│   └── EMR_1920x1080_Dashboard_Specification.txt
│
├── DAX/
│   └── EMR_Complete_DAX_Measures.txt
│
├── Data/
│   └── EMR_PowerBI_Sample_Data.xlsx
│
├── SQL/
│   ├── Patient_Analytics.sql
│   ├── OPD_Analytics.sql
│   ├── IPD_Analytics.sql
│   ├── Laboratory_Analytics.sql
│   └── Billing_Analytics.sql
│
└── Documentation/
    ├── Data_Model.png
    ├── Business_Requirements.md
    └── Dashboard_Documentation.md
```

---

#  Data Model

The project follows a **Star Schema** architecture.

### Dimension Tables

* `Patients`
* `Doctors`
* `Departments`
* `DateDim`

### Fact Tables

* `Encounters`
* `Admissions`
* `LabOrders`
* `Pharmacy`
* `Billing`

### Model Design


                    ┌──────────────┐
                    │   DateDim    │
                    └──────┬───────┘
                           │
                           │
┌────────────┐       ┌─────▼──────┐       ┌────────────┐
│  Patients  │──────►│ Encounters │◄──────│  Doctors   │
└────────────┘       └────────────┘       └────────────┘
      │
      │
      ├──────────────► Admissions
      │
      ├──────────────► LabOrders
      │
      ├──────────────► Pharmacy
      │
      └──────────────► Billing
```

Relationships are primarily **one-to-many** from dimensions to fact tables.

---

#  Dashboard Pages

# 1. EMR Executive Overview

# KPI Cards

* Total Patients
* Total Encounters
* Total Admissions
* Total Hospital Revenue

# Visualizations

* Monthly Hospital Revenue Trend
* Encounters by Department
* Revenue by Service
* Admissions by Ward
* Top Doctors by Revenue
* Patient Visit Status

# Slicers

* Date
* Department
* Doctor
* Encounter Type

---

## 2. Patient & OPD Analytics

### KPIs

* Total Patients
* OPD Visits
* Follow-up Visits
* Average Consultation Fee

# Visualizations

* Monthly OPD Visit Trend
* New vs Follow-up Patients
* Visits by Department
* Doctor-wise Encounters
* Patient Age Distribution
* Visit Status

---

## 3. IPD / Admission Analytics

### KPIs

* Total Admissions
* Average Length of Stay
* ICU Admissions
* Bed Revenue

### Visualizations

* Monthly Admission Trend
* Admissions by Ward
* Average LOS by Ward
* Admission Type
* Bed Revenue by Ward
* Monthly Discharges

---

## 4. Laboratory Analytics

### KPIs

* Total Lab Orders
* Completed Lab %
* Average TAT
* Lab Revenue

### Visualizations

* Monthly Lab Orders
* Top Laboratory Tests
* Average TAT by Test
* Result Status
* Laboratory Revenue Trend
* Pending Lab Orders

### Operational Analysis

Laboratory turnaround time can be monitored using:

Completed Lab Orders
Average Lab TAT
Pending Orders
Delayed Orders

## 5. Pharmacy Analytics

### KPIs

* Total Prescriptions
* Medicine Quantity
* Pharmacy Revenue
* Average Prescription Value

### Visualizations

* Monthly Pharmacy Revenue
* Top Medicines
* Medicine-wise Revenue
* Payment Status
* Quantity Sold
* Doctor-wise Prescriptions

---

## 6. Billing & Finance

### KPIs

* Gross Billing
* Total Discount
* Net Billing
* Collection Rate

### Visualizations

* Monthly Net Billing
* Gross vs Net Billing
* Revenue by Bill Type
* Payment Mode
* Payment Status
* Discount Trend



#  Key DAX Measures

# Total Patients

DAX
Total Patients =
DISTINCTCOUNT(Encounters[PatientID])
```

# Total Encounters

`DAX
Total Encounters =
COUNTROWS(Encounters)
```

# Total Admissions

DAX
Total Admissions =
COUNTROWS(Admissions)
```

# Average Length of Stay

```DAX
Average LOS =
AVERAGE(Admissions[LOS_Days])
```

# Total Hospital Revenue

```DAX
Total Hospital Revenue =
[Total Bed Revenue]
    + [Total Lab Revenue]
    + [Total Pharmacy Revenue]
    + [Net Billing]
```

### Revenue YTD

```DAX
Revenue YTD =
TOTALYTD(
    [Total Hospital Revenue],
    DateDim[Date]
)
```

### Previous Year Revenue

DAX
Revenue Previous Year =
CALCULATE(
    [Total Hospital Revenue],
    SAMEPERIODLASTYEAR(DateDim[Date])
)


# Revenue YoY %

DAX
Revenue YoY % =
DIVIDE(
    [Total Hospital Revenue] - [Revenue Previous Year],
    [Revenue Previous Year],
    0
)


# Collection Rate

DAX
Collection Rate % =
DIVIDE(
    [Paid Billing],
    [Net Billing],
    0
)


### Average Lab TAT

```DAX
Average Lab TAT =
AVERAGE(LabOrders[TAT_Minutes])
```

### Revenue Per Patient

```DAX
Revenue Per Patient =
DIVIDE(
    [Total Hospital Revenue],
    [Total Patients],
    0
)
```

---

#  Power Query Transformations

The project uses Power Query for:

* Data type correction
* Null value handling
* Duplicate removal
* Date transformation
* Column renaming
* Data cleansing
* Creating calculated columns
* Standardizing department names
* Standardizing patient and doctor attributes

---

# 🧮 SQL Analysis

SQL can be used as the source-layer analysis before loading data into Power BI.

Typical analysis includes:

```sql
-- Patient count
SELECT COUNT(DISTINCT patientid) AS total_patients
FROM encounters;
```


-- Department-wise encounters
SELECT
    department,
    COUNT(*) AS encounter_count
FROM encounters
GROUP BY department
ORDER BY encounter_count DESC;
```

-- Average length of stay
SELECT
    ward,
    ROUND(AVG(los_days), 2) AS average_los
FROM admissions
GROUP BY ward;
```


---

#  Dashboard Design

The dashboard uses a clean healthcare-oriented design:

* Light gray background
* Blue analytical visuals
* Minimal visual clutter
* Interactive slicers
* Cross-filtering
* Drill-through analysis
* Report tooltips

---

#  Interactive Features

The dashboard supports:

* Date filtering
* Department filtering
* Doctor filtering
* Ward filtering
* Encounter-type filtering
* Cross-filtering between visuals
* Drill-through to patient details
* Drill-through to doctor details
* Report-page tooltips

---

#  Patient Drill-through

The patient detail page can contain:

* Patient ID
* Patient Name
* Age
* Gender
* Blood Group
* Encounter History
* Admission History
* Laboratory History
* Pharmacy History
* Billing History

---

#  Doctor Drill-through

The doctor detail page can contain:

* Doctor Name
* Specialty
* Encounter Count
* Admissions
* Lab Orders
* Revenue
* Average Consultation Fee
* Monthly Performance

---

#  Business Value

This dashboard helps hospital management:

* Monitor patient demand
* Identify high-volume departments
* Track doctor performance
* Monitor inpatient utilization
* Identify laboratory TAT issues
* Analyze pharmacy performance
* Monitor billing and collections
* Identify revenue trends
* Support data-driven operational decisions

---

#  Project Workflow

```text
Hospital / EMR Data
        ↓
     SQL
        ↓
Data Extraction
        ↓
  Power Query
        ↓
Data Cleaning
        ↓
   Data Model
        ↓
   Star Schema
        ↓
      DAX
        ↓
 KPI Calculations
        ↓
 Power BI Visuals
        ↓
 Interactive Dashboard
        ↓
 Management Insights
```

---

#  Skills Demonstrated

### Power BI

* Dashboard Development
* Data Modeling
* Star Schema
* Power Query
* DAX
* Time Intelligence
* Drill-through
* Tooltips
* Slicers
* Interactive Visualizations

### DAX

* `CALCULATE`
* `SUM`
* `AVERAGE`
* `DISTINCTCOUNT`
* `DIVIDE`
* `FILTER`
* `RANKX`
* `TOTALYTD`
* `TOTALMTD`
* `SAMEPERIODLASTYEAR`
* `RANKX`

### SQL

* SELECT
* WHERE
* GROUP BY
* HAVING
* JOIN
* CASE
* Aggregate Functions
* Subqueries
* CTEs
* Window Functions

### Healthcare Analytics

* EMR
* OPD
* IPD
* Patient Analytics
* Laboratory Analytics
* Pharmacy Analytics
* Billing Analytics
* Hospital Operations



#  Data Privacy

This repository uses **synthetic/sample EMR data** for demonstration and portfolio purposes.

No real patient information, personally identifiable information, medical records, or confidential hospital data should be uploaded to this repository.



#  Project Role

**Role:** Power BI Developer / Data Analyst

# Responsibilities

* Designed the EMR analytical data model
* Created Power Query transformations
* Developed DAX measures
* Designed interactive Power BI dashboards
* Created healthcare KPIs
* Analyzed OPD/IPD operations
* Developed laboratory and pharmacy analytics
* Developed billing and revenue analytics
* Implemented drill-through and interactive filtering
* Created management-level reporting



#  How to Use

1. Download the sample dataset.
2. Open Power BI Desktop.
3. Import `EMR_PowerBI_Sample_Data.xlsx`.
4. Load the required tables.
5. Create the recommended relationships.
6. Add the DAX measures.
7. Import the Power BI theme.
8. Build the six dashboard pages.
9. Add slicers and interactions.
10. Save the final report as:

### EMR_Analytics_Dashboard.pbix

#  Project Highlights

* End-to-end Power BI healthcare analytics solution
* Star-schema data model
* 5 interactive dashboard pages
* 20+ DAX measures
* Patient and hospital operational analytics
* Pharmacy revenue analysis
* Billing and collection analysis
* Drill-through analysis


---

 📞 Contact

**Srikanth Ariboina**


Power BI Developer | Data Analyst

Skills: Power BI | DAX | SQL | PL/SQL | Power Query | Data Modeling 
