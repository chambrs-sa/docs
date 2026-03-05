# 1. PROBLEM DEFINITION

## 1.1 Core Problem

On SANRAL road construction sites:

* Multiple subcontractors
* Shared diesel supply
* Manual tracking
* No real-time accountability
* No digital audit trail

**Result:**

* Systemic diesel leakage (theft + inefficiency + manipulation)
* Financial loss to contractor
* Distrust between parties
* No reliable operational data

The issue is not just theft.
It is **lack of measurable accountability in a distributed environment.**

---

## 1.2 Stakeholder Breakdown

### 1. Plant Owner (You)

* Pays for fuel
* Pays drivers
* Responsible for truck performance
* Margins are thin
* Needs cost control
* Wants operational data for optimization

**Pain:** Fuel cost volatility + unverified consumption.

---

### 2. Main Contractor (SANRAL Project Owner)

* Pays subcontractors
* Responsible for project budget
* Diesel theft impacts entire site profitability
* Wants accountability across all plant

**Pain:** Massive diesel losses with no forensic traceability.

---

### 3. Diesel Manager / Fuel Attendant

* Dispenses fuel
* Records manually
* High temptation + low supervision
* Paper records easy to manipulate

**Pain:** Blame exposure.

---

### 4. Drivers

* Operate trucks
* Sometimes complicit
* Sometimes unfairly blamed
* Incentivised by overtime, not efficiency

---

### 5. Site Managers

* Focused on productivity
* Do not have reliable usage metrics
* Cannot correlate diesel to productivity

---

## 1.3 Financial Impact

Let’s quantify realistically.

Assumptions per 10m³ tipper:

* Avg 30–40 litres/hour heavy operation
* 8–10 hours per day
* ~300 litres/day per truck
* Diesel ≈ R23/litre (variable)

→ R6,900/day per truck
→ R138,000/month per truck

If 10% unaccounted:
→ R13,800/month per truck lost

If 15 trucks:
→ R207,000/month loss

Across a large SANRAL site:
→ R1–3 million/month potential leakage

This is a **multi-million-rand structural problem.**

---

## 1.4 Why Manual Systems Fail

| Weakness               | Result                               |
| ---------------------- | ------------------------------------ |
| Paper-based logs       | No verification                      |
| No sensor validation   | Cannot confirm tank levels           |
| No GPS link            | Cannot correlate fuel to movement    |
| No load tracking       | Cannot relate diesel to productivity |
| No timestamp integrity | Easy manipulation                    |
| No audit trail         | No accountability                    |

Manual systems fail because:

* They rely on trust.
* There is no real-time validation.
* There is no data triangulation.

You need triangulation:
Fuel dispensed ≠ Fuel burned ≠ Distance ≠ Load ≠ Engine hours

If these do not reconcile → something is wrong.

---

# 2. INDUSTRY & LITERATURE SURVEY

## 2.1 Global Fuel & Telematics Players

### Fleet Telematics

* Geotab
* Samsara
* MiX Telematics
* Cartrack

These provide:

* GPS tracking
* Engine data via CAN bus
* Fuel consumption analytics

But:

* Rarely integrate load + site fuel dispensing + subcontractor complexity.

---

## 2.2 Fuel Management Systems (Mining & Construction)

Common in mining:

* Tank level sensors
* RFID-based driver identification
* Automated dispensing pumps
* Reconciliation systems

SA mining companies use high-end systems from:

* Orpak Systems
* Franklin Fueling Systems

Mining is mature.
Construction is not.

Your opportunity: **bring mining-level accountability to road construction.**

---

## 2.3 IoT Technologies Available

* Ultrasonic fuel tank sensors
* Capacitive fuel level probes
* Flow meters on fuel lines
* CAN bus OBD-II data extraction
* On-board weight sensors
* GPS trackers (LTE-M / NB-IoT)
* Edge compute modules

---

## 2.4 South African Context

* Patchy site connectivity
* High theft risk
* Multiple subcontractors
* Diesel delivered in bulk bowsers
* Informal operational processes

Solution must:

* Work offline
* Be rugged
* Be tamper-resistant
* Be simple to use

---

# 3. TECHNICAL REQUIREMENTS

## 3.1 Hardware Components

### On Each Truck

1. GPS Telematics Unit (LTE-enabled)
2. CAN bus interface

   * Engine hours
   * Fuel rate
   * RPM
3. Fuel Tank Level Sensor

   * Capacitive probe inside tank
4. Fuel Line Flow Meter (optional advanced)
5. Load Sensor

   * Air suspension pressure sensors
   * Or axle weight sensors
6. Driver ID system (RFID / PIN)
7. Tamper detection switch
8. Internal battery backup

---

### At Fuel Dispensing Point

1. Smart fuel pump controller
2. Digital flow meter
3. RFID truck authentication
4. Tank level sensor on bulk diesel tank

---

## 3.2 Software Architecture

Cloud-native architecture:

```
Truck Edge Device
      ↓
Secure IoT Gateway
      ↓
Cloud Ingestion API
      ↓
Data Processing Layer
      ↓
Analytics Engine
      ↓
Dashboard + Mobile App
```

Tech stack (practical SA option):

* Device firmware (C / Embedded Linux)
* Cloud: AWS / Azure SA region
* Backend: Spring Boot / Node
* Database:

  * Time-series DB (InfluxDB)
  * Relational DB (Postgres)
* Frontend:

  * React web dashboard
  * Mobile app (Flutter)

---

## 3.3 Data Architecture

### Data Streams

* GPS location (every 10 sec)
* Fuel level (every 30 sec)
* Engine status
* Load weight
* Refuelling event
* Driver ID
* Timestamp

All events must be:

* Time-stamped
* Cryptographically signed
* Stored immutably

---

## 3.4 Security & Tamper Detection

* Sensor disconnection alerts
* Sudden fuel drop detection
* Fuel level change without ignition
* Tank cap open detection
* Device enclosure tamper seal

---

## 3.5 Offline Capability

Edge device must:

* Store minimum 7 days of data locally
* Sync when signal available
* Send critical alerts via SMS fallback

Construction sites often lack stable LTE.

---

# 4. DATA STRATEGY

This is where the real value is.

## 4.1 Core Data to Capture

* Fuel dispensed (litres)
* Fuel tank level before & after
* Engine hours
* Distance travelled
* Idle time
* Load weight per trip
* Trip count
* Route mapping

---

## 4.2 KPIs

1. Litres per km
2. Litres per ton moved
3. Litres per engine hour
4. Idle fuel %
5. Refuel discrepancy %
6. Fuel shrinkage rate
7. Cost per m³ moved

These become operational gold.

---

## 4.3 Anomaly Detection

Examples:

* Fuel drop > 20 litres without ignition ON
* Fuel refill recorded but tank increase < dispensed
* Idle time > 35%
* Engine on but no movement for > 30 min
* Fuel efficiency deviates 25% from historical baseline

Pattern-based detection can identify:

* Theft
* Syphoning
* Collusion at pump
* Mechanical inefficiency

---

## 4.4 AI/ML Potential

Future:

* Predictive fuel consumption per route
* Driver behaviour scoring
* Theft probability scoring
* Predictive maintenance
* Site-wide anomaly clustering

You are building a **data moat.**

---

# 5. MODERN SOLUTION ARCHITECTURE

## High-Level Architecture

```
[Truck Sensors]
    ↓
[Edge Telematics Device]
    ↓ LTE / NB-IoT
[IoT Cloud Gateway]
    ↓
[Event Streaming Layer]
    ↓
[Data Lake + Time Series DB]
    ↓
[Analytics Engine]
    ↓
[Operations Dashboard]
    ↓
[Mobile App for Site Managers]
```

---

## Components

### Edge Layer

* Embedded device
* Data buffering
* Basic rule engine
* Tamper detection

### IoT Layer

* Secure MQTT communication
* Device authentication

### Cloud Backend

* Event-driven microservices
* REST APIs for integration
* Contractor system integration

### Analytics Layer

* Real-time fuel reconciliation engine
* KPI computation service
* Alerting service

### Dashboard

* Fleet overview
* Fuel reconciliation view
* Theft alerts
* Productivity dashboard

---

# 6. BUSINESS MODEL & STRATEGIC POSITIONING

## Step 1: Internal Use

Install on your 10m³ fleet.
Prove:

* 8–15% fuel savings
* Reduced disputes
* Better performance metrics

---

## Step 2: ROI Case to Contractor

Pitch:

“If you are losing R1m/month in diesel, we reduce it by 40%.”

Even 20% reduction:
→ R200k/month saved

Charge:

* Per vehicle monthly fee
* Or % of fuel savings

---

## Strategic Positioning

You are not:

“A tracking company.”

You are:

**Construction Operations Intelligence Platform.**

Expansion areas:

* Tyre usage
* Equipment hours
* Operator productivity
* Carbon emissions reporting
* ESG compliance

---

# 7. RISKS & CONSTRAINTS

## Technical

* Sensor calibration issues
* Harsh site environment
* Connectivity gaps

## Human

* Driver resistance
* Diesel manager sabotage
* Collusion

Mitigation:

* Transparency
* Incentive alignment
* Driver performance bonuses

## Operational

* Installation downtime
* Hardware failure

## Financial

* High upfront capex
* Slow contractor adoption

## Legal

* POPIA compliance (driver data)
* Labour disputes
* Contractual obligations

---

# 8. PHASED IMPLEMENTATION ROADMAP

## Phase 1 – MVP (3–4 Months)

Install on 3–5 trucks:

* GPS
* Fuel level sensor
* CAN bus integration
* Basic dashboard
* Manual refuel logging

Goal:
Validate reconciliation logic.

---

## Phase 2 – Pilot (6 Months)

Add:

* Smart fuel pump integration
* Driver ID
* Tamper alerts
* Automated reporting

Measure:

* Theft reduction %
* Idle reduction
* Fuel efficiency improvement

---

## Phase 3 – Contractor Rollout

Propose:

* Site-wide deployment
* Centralized fuel accountability dashboard
* API integration into contractor ERP

---

## Phase 4 – Expansion

Add:

* Load productivity tracking
* AI anomaly detection
* Cross-site analytics
* SaaS model

---

# Strategic Insight

Diesel tracking is the entry point.

The real asset you are building is:

**A structured, verified, real-time operational dataset for construction sites in South Africa.**

If executed properly:

You evolve from:
Plant hire company

To:
Industrial data platform

To:
Technology company serving infrastructure projects across Africa.

---
