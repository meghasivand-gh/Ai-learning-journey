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

*Notes by Melika Ghasivand — Industrial Engineering & Management, Mälardalen University*
