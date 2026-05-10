# SAP S/4HANA Supply Chain Planning — Course Notes

> Notes from: Exploring Business Processes in SAP S/4HANA Production Planning
> Course on SAP Learning Hub | April 2026

---

## 1. The Core Problem: Why Do We Need Planning?

- **Lead time**: Total time from purchasing raw materials to finished product (weeks/months)
- **Delivery time**: The time a customer is willing to wait (often just days)
- **The gap**: Lead time is almost always LONGER than delivery time → we must plan and produce in advance

---

## 2. Push, Pull and Hybrid Strategies

### Push (Make-to-Stock)
- Produce in advance based on forecasts
- Products are "pushed" to market before customer orders
- Pros: Short delivery time, stable production, economies of scale
- Cons: Risk of overproduction, obsolete inventory, high storage costs
- Best for: Standard products with predictable demand

### Pull (Make-to-Order)
- Production starts ONLY when customer orders
- The customer "pulls" production
- Pros: No risk of unsold goods, customization possible, lower inventory costs
- Cons: Longer delivery time, requires flexible production systems
- Best for: Customized products

### Hybrid
- Push for components/subassemblies
- Pull for final assembly/customization
- Benefits: Shorter lead time, shorter delivery time, balanced inventory levels

---

## 3. Forecasting

Forecasting = predicting future demand based on historical data.

**Three information sources:**
- Historical sales data — what have we sold before?
- Market intelligence — trends, competitors, customer behavior
- One-off events — trade fairs, campaigns, special events

**Key forecasting models:**

| Model | Used for |
|-------|----------|
| Moving average | General data smoothing |
| Constant model | Products with stable demand |
| Trend model | Products with increasing/decreasing demand |
| Seasonal model | Seasonal products (ice cream, Christmas lights) |
| Exponential smoothing | Gives more weight to recent data |
| Linear regression | Statistical trend analysis |

> **Key exam note:** Market intelligence is part of forecasting, NOT demand planning or production planning.

---

## 4. Demand Planning

Demand planning is the overall process that includes forecasting.

**The flow:**
1. Collect historical data — from sales orders, invoices, deliveries
2. Aggregate the data — compile into a unified view
3. Choose planning level — Location, Product Hierarchy, Customer, Sales Org, Region
4. Create forecast — using models + market intelligence

**Two types of plans:**
- Quantity-based — number of units (e.g., 500 bicycles)
- Value-based — monetary (e.g., 500,000 SEK)

---

## 5. Planned Independent Requirements (PIR)

- **Planned** — planned in advance, not based on actual orders
- **Independent** — independent of a specific customer order
- **Requirements** — represents a need for materials/products

PIR is the output of forecasting, entered into the SAP system.

Example: Forecast says 200 bicycles → PIR = 200 bicycles

---

## 6. Three Types of Demand

| Type | Description | Example |
|------|-------------|---------|
| PIR | Forecast-based needs, no customer order yet | 100 bicycles based on forecast |
| Sales Orders | Actual customer orders | Customer orders 50 branded bicycles |
| Stock Transfer Requirements | Needs from other locations | Distribution center needs 50 bicycles |

**Consumption:** When a sales order comes in, it "consumes" the PIR.
- PIR = 200, customer orders 30 → PIR becomes 170
- The system does this automatically to avoid double production

---

## 7. Planning Strategies in SAP

- **Make-to-Stock (Strategy 10)** = Push — considers BOTH PIR and sales orders
- **Subassembly Planning** = Hybrid — pre-produce parts, assemble on order
- **Make-to-Order** = Pull — production starts ONLY on customer order

---

## 8. SAP IBP (Integrated Business Planning)

SAP IBP is the modern planning tool:
- Cloud-based (built on SAP HANA)
- Real-time dashboards
- Predictive analytics
- Simulation ("what if...?")
- Collaboration features
- Excel integration

Covers: S&OP, Demand Planning, Inventory Planning, Response & Supply Planning, Supply Chain Control Tower

---

## 9. MRP (Material Requirements Planning)

MRP = breaks down PIR into exact material needs.

**MRP does:**
1. BOM explosion — breaks down the product into all individual parts
2. Checks existing inventory
3. Creates planned orders (what to produce?)
4. Creates purchase requisitions (what to buy?)
5. Calculates capacity requirements (enough machines and people?)
6. Determines quantities and dates

Example: PIR = 200 bicycles → MRP calculates: 200 frames, 400 wheels, 200 handlebars, etc.

---

## 10. The Complete Chain — From Forecast to Production

Historical Data + Market Intelligence
↓
Forecasting (in SAP IBP)
↓
Demand Plan
↓
PIR (Planned Independent Requirements) → sent to SAP S/4HANA
↓
Demand Management (+ sales orders + stock transfers)
↓
MRP (Material Requirements Planning)
↓
Production Orders + Purchase Orders
↓
PRODUCTION

*Notes by Melika Ghasivand — Industrial Engineering & Management, Mälardalen University*SAP S/4HANA – MRP-Live, Planning Run & Evaluating Results

Notes from: Exploring Business Processes in SAP S/4HANA Production Planning
Sections: Creating Planned Receipts & Evaluating Planning Results
Date: May 10, 2026


1. MRP – What It Does (Recap)
MRP has one job: ensure the right material is available, in the right quantity, from the right source, at the right time.
It compares:

Demand (PIRs, sales orders, reservations, dependent requirements)
Supply (stock, purchase orders, planned orders, production orders)

If demand > supply → shortage → MRP creates a solution.

2. MRP-Live vs Classic MRP
FeatureClassic MRPMRP-LiveSpeedSlow, takes hoursUp to 10x fasterRuns onTraditional databaseSAP HANA (in-memory)FrequencyOnce a week / overnightCan run frequently, near real-timeData freshnessOutdated after next changeAlways currentMRP ListsCreates MRP listsDoes NOT create MRP listsAvailabilityStill availableNew standard in S/4HANA
MRP-Live eliminates the danger of planning with obsolete data. The system decides internally whether to use classic MRP or MRP-Live.

3. Planning Modes
Total Planning

Plans ALL materials in one or more plants
Can run online or as a background job
Includes BOM explosion for all materials

Single-Item Planning
ModeWhat it doesMulti-levelPlans one material AND all its components (entire BOM tree, top-down)Single-levelPlans ONLY one material, not its componentsInteractivePlans one material, lets you review and change results before saving
MRP-Live

Can plan from one single material up to all materials in multiple plants
Can run interactively or as a background job
Much faster than classic modes


4. Low-Level Code – Planning Sequence
MRP plans materials from TOP to BOTTOM using low-level codes:
Low-level code 000:  Finished products (bicycles)
Low-level code 001:  Assemblies (frame, wheel set)
Low-level code 002:  Components (screws, spokes, tires)
The system plans level 000 first, then 001, then 002. This ensures that when planning components, the system already knows how many finished products are needed.

5. Processing Keys
KeyNameWhat it doesWhen to useNEUPLRegenerative planningPlans ALL MRP-relevant materials regardless of changesFirst run ever, or after technical errorsNETCHNet change for total horizonPlans only materials that CHANGED since last runDaily operations (most common)NETPLNet change in planning horizonPlans only changes within planning horizonNo longer available in MRP-Live (SAP HANA is fast enough)

6. MRP Control Parameters
Create Purchase Requisition (1, 2, 3)

1 = Always create purchase requisitions
2 = Only within the opening period
3 = Never (create planned orders only)

Scheduling Agreement Schedule Lines (1, 2, 3)

Same logic as above but for delivery schedules with fixed suppliers

Create MRP List (1, 2, 3)

1 = Always create MRP list
2 = Only when exceptions/problems occur
3 = Never

Important: MRP-Live does NOT create MRP lists. Use classic MRP if you need them.
Planning Mode (1, 2, 3)

1 = Adapt existing planning data (most common)
2 = Re-explode BOMs and routings
3 = Delete everything and recreate from scratch (use with caution!)


7. Planning File
The planning file tells MRP which materials need to be planned. It contains:

The low-level code of each material
Whether the material has undergone MRP-relevant changes (NETCH indicator)
Whether the BOM needs re-explosion

Important: If materials were created BEFORE activating MRP for a plant, you must manually create entries in the planning file.
MRP-relevant changes that trigger a planning file entry:

Changes to procurement type
Changes to operation times
New sales orders
Stock changes
New dependent requirements


8. What MRP Creates When It Finds a Shortage
For in-house production:
MRP finds shortage
    ↓
Creates Planned Order
    ↓
Convert to Production Order
    ↓
Factory starts manufacturing
For external procurement:
MRP finds shortage
    ↓
Creates Planned Order OR Purchase Requisition
    ↓
Convert to Purchase Order
    ↓
Supplier delivers material
Planned orders and purchase requisitions are internal planning elements – they can be changed, rescheduled, or deleted at any time.

9. Firming – Protecting the Plan
Firming prevents MRP from automatically changing orders that are close in time.
Example: You have a planned order for 50 bicycles due TOMORROW. Everything is prepared in the factory. MRP runs and wants to change it to 45. That would cause chaos! Firming locks the order.
Firming Types
TypeWhat happens0No firming – MRP can change everything1Orders inside firming period are locked automatically. New shortages are covered at the end of the period.2Orders inside firming period are locked. New shortages are NOT covered.3Unlocked orders are moved to the end of the firming period.4Unlocked orders are DELETED.
How firming is set:

Planning time fence – defined in material master or MRP group (number of working days)
Manual firming date – manually set a future date
Manual firming – if you manually change a planned order, it automatically gets firmed


10. Evaluating Planning Results
After MRP runs, the planner needs to check that everything looks good.
Three main tools:
1. MRP Cockpit (Monitor Material Coverage)

Dashboard overview
KPIs and color-coded alerts (red = problem, green = ok)
Shows which materials have shortages
Which customer orders are at risk

2. Manage Material Coverage

Detailed analysis
System suggests solutions
Real-time simulation of "what if" scenarios

3. Stock/Requirements List (MD04)

Most detailed view
Shows all supply and demand for one material
The tool we used in our practical exercises

Old vs New MRP Evaluation
BeforeNow with MRP-LiveMRP ran once a weekRuns frequentlyPaper-based MRP listsReal-time dashboardsOutdated informationAlways currentProblems detected lateIssues identified immediatelyMultiple transactions neededSingle view in MRP Cockpit
What the planner can do:

See which customer orders are affected by shortages
Prioritize – which customer has the highest order value?
Simulate solutions – "what if I change supplier?"
Act quickly – create purchase orders, move stock, change production plan


11. Key SAP Fiori Apps for MRP
AppPurposeMonitor Material CoverageOverview of shortages and KPIsManage Material CoverageDetailed analysis with solution suggestionsSchedule MRP RunsSchedule and execute MRP runsChange MaterialCheck material master data (MRP type, procurement type)Display Bill of MaterialView BOM structure and components

12. Key Transaction Codes for MRP
T-codeFunctionMD01MRP Run – total planning for a plantMD02MRP Run – single materialMD04Stock/Requirements List (individual)MD05MRP ListMD06Collective access to MRP ListMD07Collective access to Stock/Requirements ListMD09Determine pegging requirements

13. Complete MRP Flow
PIR + Sales Orders + Stock Transfer Requirements
        ↓
MRP Run (MRP-Live on SAP HANA)
├── Processing key: NEUPL (all) or NETCH (changes only)
├── Scope: Total (all materials) or Single-item
├── Level: Multi-level (all BOM levels) or Single-level
        ↓
Creates: Planned Orders + Purchase Requisitions
        ↓
Planner evaluates in:
├── MRP Cockpit (overview + KPIs)
├── Manage Material Coverage (details + solutions)
└── MD04 Stock/Requirements List (all details)
        ↓
Planner acts:
├── Convert planned orders → Production orders
├── Convert purchase requisitions → Purchase orders
├── Adjust quantities and dates
└── Simulate alternatives
        ↓
PRODUCTION / PROCUREMENT

14. Quiz Answers from This Section
Q: If materials have been created before activating MRP for a plant, you must create an entry in the planning file.
A: True – The planning file tells MRP which materials to plan.
Q: Which is the main function of MRP?
A: MRP is used to procure the requirement quantities on time both for internal purposes and for sales and distribution.
Q: Which transaction code comes closest to the material list in the MRP Cockpit?
A: MD07 – Collective access to Stock/Requirements List (shows multiple materials).

Course: Exploring Business Processes in SAP S/4HANA Production Planning
Learning Journey: Implementing Manufacturing in SAP S/4HANA Cloud Private Edition
