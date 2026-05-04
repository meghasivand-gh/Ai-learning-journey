# SAP S/4HANA – Hands-on Practice in the SAP System

## System Information

| Info | Detail |
|------|--------|
| System | SAP S/4HANA (T41) |
| Client | 400 |
| Plant | 1010 – Hamburg |
| Material | FG126 – Extreme 50 |
| Date | May 4, 2026 |
| Access via | SAP Learning Hub Student Edition |

---

## 1. Logging into the SAP System

Logged in via **SAP Logon 800** and connected to system **T41 – SAP S/4HANA**. Used Client 400 and language EN (English).

<!-- Add screenshot here: SAP Logon screen -->
<!-- ![SAP Logon](images/01-sap-logon.png) -->

---

## 2. SAP Fiori Launchpad – Navigation

After logging in, the **SAP Fiori Launchpad** opened – the modern web-based interface in SAP S/4HANA. All apps are organized into tabs:

| Tab | Contents |
|-----|----------|
| Master Data | Change Material (MM02), Display Material (MM03), Multilevel BOM (CS12) |
| Production Planning | Maintain PIRs, Monitor Material Coverage, Check Material Coverage |
| Order Creation | Create production orders |
| Capacity Planning | Capacity planning tools |
| Monitoring & Order Control | Monitoring and order control |
| Material Staging | Material staging for production |
| Confirmation | Production order confirmations |
| Goods Receipt & Order Settlement | Monitor Stock/Requirements List (MD04), Post Goods Movement (MIGO) |
| Inventory Management | Stock, Display Stock Overview (MMBE) |

<!-- Add screenshot here: Fiori Launchpad home screen -->
<!-- ![Fiori Launchpad](images/02-fiori-launchpad.png) -->

---

## 3. My Area of Responsibility – Assigning a Plant

Before working with materials and PIRs, you must assign an **area of responsibility**. This tells the system which plants you are responsible for.

I assigned myself to **Plant 1010 (Hamburg)** via the "My Area of Responsibility" app.

**Why is this needed?**
In a real company, not all employees have access to all plants. A planner in Stockholm doesn't need to see data from a factory in Tokyo. SAP asks: "Which plants are you responsible for?" This is a one-time setup.

<!-- Add screenshot here: My Area of Responsibility -->
<!-- ![Area of Responsibility](images/03-area-of-responsibility.png) -->

---

## 4. Creating PIRs – Planned Independent Requirements (MD61)

### What are PIRs?
Planned Independent Requirements (PIRs) are forecast-based requirements that are NOT linked to a specific customer order. They represent an estimate of future demand.

### What I did
Used transaction **MD61** (Create Planned Independent Requirements) in SAP GUI.

**Settings:**

| Field | Value |
|-------|-------|
| Material | FG126 (Extreme 50) |
| Plant | 1010 (Hamburg) |
| Version | 00 |
| Planning period | Month (M) |
| Planning Horizon | 04.05.2026 – 08.06.2027 |

**Quantities entered:**

| Month | Quantity (pieces) |
|-------|-------------------|
| May 2026 | 50 |
| June 2026 | 50 |
| July 2026 | 50 |
| August 2026 | 50 |
| September 2026 | 50 |
| October 2026 | 50 |

**Result:** System confirmed "Requirement saved" – PIRs were created successfully.

<!-- Add screenshot here: PIR planning table with 50 per month -->
<!-- ![PIR Create](images/04-pir-create.png) -->

<!-- Add screenshot here: Confirmation "Requirement saved" -->
<!-- ![PIR Saved](images/05-pir-saved.png) -->

---

## 5. Stock/Requirements List – Viewing the Shortage (MD04)

### What is MD04?
The Stock/Requirements List shows all requirements and receipts for a material. It is one of the most important views in SAP – here you can see if there is a shortage or surplus.

### What I saw BEFORE MRP

Used transaction **MD04** with material FG126, plant 1010.

| Date | Type | Description | Quantity | Available Qty |
|------|------|-------------|----------|---------------|
| 04.05.2026 | Stock | Warehouse stock | | 0 |
| 04.05.2026 | IndReq | PIR (requirement) | 50- | 50- |
| 01.06.2026 | IndReq | PIR (requirement) | 50- | 100- |
| 01.07.2026 | IndReq | PIR (requirement) | 50- | 150- |

**IndReq** = Independent Requirement = My planned independent requirements (PIRs).

**The minus signs** indicate a shortage! The system is saying: "We need 50 per month but have nothing in stock. The shortage grows each month."

<!-- Add screenshot here: MD04 before MRP showing shortage -->
<!-- ![MD04 Before MRP](images/06-md04-before-mrp.png) -->

---

## 6. MRP Run – Resolving the Shortage (MD01)

### What is MRP?
Material Requirements Planning (MRP) is the system's brain. It checks all requirements, compares with stock, and automatically creates production and purchase orders to cover shortages.

### What I did
Used transaction **MD01** (MRP Run) for the entire plant 1010.

**Settings:**

| Parameter | Value | Description |
|-----------|-------|-------------|
| Plant | 1010 | Hamburg |
| Processing Key | NETCH | Net Change in Total Horizon |
| Create Purchase Req. | 2 | Purchase requisitions in opening period |
| Schedule Lines | 3 | Schedule lines |
| Create MRP List | 1 | MRP list |
| Planning Mode | 1 | Adapt planning data (normal mode) |
| Scheduling | 1 | Determination of Basic Dates for Planned |

**Result:**

| Statistic | Count |
|-----------|-------|
| Materials planned | 15,103 |
| Materials with New Exceptions | 383 |
| Materials with Termination MRP List | 4 |

MRP planned over 15,000 materials across the entire plant 1010. My material FG126 was one of them.

<!-- Add screenshot here: MRP Run result -->
<!-- ![MRP Run](images/07-mrp-run.png) -->

---

## 7. Stock/Requirements List – Shortage Resolved (MD04)

### What I saw AFTER MRP

Went back to **MD04** and saw that MRP had created **planned orders (PldOrd)** to cover the shortage:

| Date | Type | Description | Quantity | Available Qty |
|------|------|-------------|----------|---------------|
| 04.05.2026 | Stock | Warehouse stock | | 0 |
| 04.05.2026 | IndReq | PIR (requirement) | 50- | 50- |
| 05.05.2026 | PldOrd | Planned order | 50 | 0 |
| 01.06.2026 | PldOrd | Planned order | 50 | 50 |

**PldOrd** = Planned Order = MRP's answer to the shortage.

For each IndReq (requirement of -50), MRP created a PldOrd (+50). Result: **shortage resolved!**

<!-- Add screenshot here: MD04 after MRP with planned orders -->
<!-- ![MD04 After MRP](images/08-md04-after-mrp.png) -->

---

## 8. Sales Order Creation – Started (VA01)

### What is VA01?
VA01 is the transaction code for creating a sales order – an actual customer order.

### What I did
Opened VA01 and filled in organizational data:

| Field | Value |
|-------|-------|
| Order Type | OR (Standard Order) |
| Sales Organization | 1010 |
| Distribution Channel | 10 |
| Division | 00 |

**Status:** Not yet completed – will continue in the next session by finding a customer and completing the order.

<!-- Add screenshot here: VA01 Create Sales Order -->
<!-- ![VA01](images/09-va01-sales-order.png) -->

---

## Transaction Codes Learned

| T-code | Function | Description |
|--------|----------|-------------|
| MD61 | Create PIR | Create planned independent requirements |
| MD04 | Stock/Requirements List | View all requirements and receipts for a material |
| MD01 | MRP Run (total) | Run MRP for an entire plant |
| MD02 | MRP Run (single) | Run MRP for a single material |
| VA01 | Create Sales Order | Create a sales order |
| MM02 | Change Material | Change material master data |
| MM03 | Display Material | Display material master data |
| /n | Prefix | Close current transaction and open a new one |

---

## Complete Flow Executed

```
1. Logged into SAP S/4HANA (T41, Client 400)
        ↓
2. Navigated SAP Fiori Launchpad
        ↓
3. Assigned Area of Responsibility (Plant 1010)
        ↓
4. Created PIRs via MD61 (50 pcs/month of FG126)
        ↓
5. Checked shortage in MD04 (50-, 100-, 150-)
        ↓
6. Ran MRP via MD01 (15,103 materials planned)
        ↓
7. Verified MRP resolved shortage in MD04 (PldOrd covers IndReq)
        ↓
8. Started sales order creation via VA01 (to be completed next session)
```

---

## Theory to Practice Mapping

| Theory Concept | What I did in SAP |
|---------------|-------------------|
| PIR (Planned Independent Requirements) | Created in MD61 – 50 pcs/month |
| Shortage | Viewed in MD04 – negative available quantities |
| MRP (Material Requirements Planning) | Executed in MD01 – created planned orders |
| Planned Orders | MRP created them automatically, visible in MD04 |
| Stock/Requirements List | Analyzed in MD04 |
| Sales Order | Started in VA01 |

---

## Remaining Tasks (Next Session)

- [ ] Find a customer in the system (search with * in VA01)
- [ ] Create sales order (FG126, 10 pcs, delivery 15.06.2026)
- [ ] Save the sales order
- [ ] Go to MD04 and observe consumption (PIR decreases)
- [ ] Check material master data (MM02) – view strategy group
- [ ] Take screenshots of everything


---

*Course: Exploring Business Processes in SAP S/4HANA Production Planning*
*System: SAP S/4HANA Training System (T41) via SAP Learning Hub Student Edition*
