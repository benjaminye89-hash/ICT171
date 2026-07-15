# Lab 6: Total Cost of Ownership (TCO) Analysis

**Date:** 15/07/2026  
**Duration:** 1.5 hours  
**Status:** Complete  

---

## 🎯 Objective
To understand and apply Total Cost of Ownership (TCO) concepts by comparing cloud vs on-premise infrastructure costs, calculating break-even points, ROI, and 3-year projections.

---

## 🛠️ Tools Used
- Microsoft Excel / Google Sheets
- AWS Pricing Calculator
- Azure Pricing Calculator
- Unit documents on TCO

---

## 📋 Step-by-Step Process

### 1. Understood TCO Concepts
I reviewed the concept of Total Cost of Ownership (TCO) – the total cost of acquiring, deploying, using, and maintaining IT infrastructure over its lifetime.

**Key TCO Components:**

| Cost Category | Examples |
|---------------|----------|
| **Hardware** | Servers, storage, networking equipment |
| **Software** | Operating systems, licenses, applications |
| **Labour** | IT staff, administrators, support |
| **Facilities** | Power, cooling, physical space, rent |
| **Maintenance** | Upgrades, repairs, replacements |
| **Training** | Staff training and certification |
| **Cloud Services** | Compute, storage, networking, support |

### 2. Created a Cost Comparison Spreadsheet
I set up a spreadsheet to compare three scenarios:

| Scenario | Description |
|----------|-------------|
| **On-Premise** | Buying and maintaining my own hardware |
| **AWS** | Using Amazon Web Services (cloud) |
| **Azure** | Using Microsoft Azure (cloud) |

**Spreadsheet Structure:**

| Cost Category | On-Premise (Year 1) | On-Premise (Year 2) | AWS | Azure |
|---------------|---------------------|---------------------|-----|-------|
| Hardware (Servers) | $10,000 | $2,000 (maintenance) | $0 | $0 |
| Software Licenses | $2,000 | $2,000 | $0 | $0 |
| IT Staff | $8,000 | $8,000 | $4,000 | $4,000 |
| Power/Cooling | $1,200 | $1,200 | $0 | $0 |
| Cloud Compute | $0 | $0 | $3,000 | $2,800 |
| Cloud Storage | $0 | $0 | $1,000 | $900 |
| **Total Cost** | **$21,200** | **$13,200** | **$8,000** | **$7,700** |

### 3. Calculated Break-Even Point
I calculated when the cloud investment pays off compared to on-premise:

**Break-Even Formula:**
> Break-Even Point = Initial On-Premise Investment / (Cloud Cost Savings per Year)

**Example Calculation:**
- Initial On-Premise Investment: $10,000
- On-Premise Annual Cost (after Year 1): $10,000/year
- AWS Annual Cost: $4,000/year
- Annual Savings: $10,000 - $4,000 = $6,000/year
- **Break-Even Point: $10,000 / $6,000 = 1.67 years**

### 4. Calculated Return on Investment (ROI)
I calculated ROI to evaluate the profitability of the cloud investment:

**ROI Formula:**
> ROI = (Net Savings / Total Investment) × 100

**Example Calculation:**
- 3-Year On-Premise Cost: $20,000
- 3-Year AWS Cost: $8,000
- Net Savings: $20,000 - $8,000 = $12,000
- **ROI = ($12,000 / $20,000) × 100 = 60%**

### 5. Created 3-Year Projections
I projected costs over three years for each scenario:

**On-Premise 3-Year Projection:**

| Year | Hardware | Software | Staff | Power/Cooling | Maintenance | Total |
|------|----------|----------|-------|---------------|-------------|-------|
| 1 | $10,000 | $2,000 | $8,000 | $1,200 | $1,000 | $22,200 |
| 2 | $2,000 | $2,000 | $8,000 | $1,200 | $1,500 | $14,700 |
| 3 | $2,500 | $2,500 | $8,500 | $1,300 | $2,000 | $16,800 |
| **Total** | **$14,500** | **$6,500** | **$24,500** | **$3,700** | **$4,500** | **$53,700** |

**AWS 3-Year Projection:**

| Year | Compute | Storage | Networking | Support | Staff | Total |
|------|---------|---------|------------|---------|-------|-------|
| 1 | $3,000 | $1,000 | $500 | $500 | $4,000 | $9,000 |
| 2 | $3,200 | $1,100 | $550 | $550 | $4,000 | $9,400 |
| 3 | $3,500 | $1,200 | $600 | $600 | $4,200 | $10,100 |
| **Total** | **$9,700** | **$3,300** | **$1,650** | **$1,650** | **$12,200** | **$28,500** |

**Azure 3-Year Projection:**

| Year | Compute | Storage | Networking | Support | Staff | Total |
|------|---------|---------|------------|---------|-------|-------|
| 1 | $2,800 | $900 | $450 | $450 | $4,000 | $8,600 |
| 2 | $3,000 | $1,000 | $500 | $500 | $4,000 | $9,000 |
| 3 | $3,300 | $1,100 | $550 | $550 | $4,200 | $9,700 |
| **Total** | **$9,100** | **$3,000** | **$1,500** | **$1,500** | **$12,200** | **$27,300** |

### 6. Compared Cloud Providers
I compared AWS and Azure across key dimensions:

| Factor | AWS | Azure |
|--------|-----|-------|
| **Compute Cost (t2.micro)** | $0.0116/hr | $0.0104/hr |
| **Storage Cost (1GB)** | $0.023 | $0.020 |
| **Free Tier** | 12 months | 12 months |
| **Global Regions** | 30+ | 60+ |
| **Key Strength** | Market leader, broad services | Enterprise integration |

### 7. Created Summary Dashboard
I created a summary dashboard with key metrics:

| Metric | On-Premise | AWS | Azure |
|--------|------------|-----|-------|
| **3-Year Total Cost** | $53,700 | $28,500 | $27,300 |
| **Total Savings vs On-Prem** | - | $25,200 | $26,400 |
| **ROI** | - | 60% | 64% |
| **Break-Even Point** | - | 1.67 years | 1.58 years |

---

## 📸 Screenshots
![TCO Spreadsheet](./screenshots/lab6-spreadsheet.png)
*Caption: Excel/Google Sheets showing cost comparison*

![Break-Even Analysis](./screenshots/lab6-breakeven.png)
*Caption: Break-even point calculation and chart*

![3-Year Projections](./screenshots/lab6-projections.png)
*Caption: 3-year projections for on-premise, AWS, and Azure*

---

## ⚠️ Errors Faced

### Error 1: Cloud Pricing Complexity
- **Problem:** Cloud pricing varies significantly based on region, usage, and instance type.
- **How I fixed it:** Used the AWS and Azure pricing calculators for accurate estimates.
- **What I learned:** Always use official calculators; never guess cloud costs.

### Error 2: On-Premise Hidden Costs
- **Problem:** I initially forgot to include power, cooling, and facility costs.
- **How I fixed it:** Added facilities and maintenance costs to the on-premise model.
- **What I learned:** On-premise has many hidden costs that cloud eliminates.

---

## 🧠 Reflection

### What I Learned
- **TCO is more than just purchase price** – it includes hardware, software, staff, power, cooling, maintenance, and training.
- **Cloud computing is often cheaper for small-to-medium workloads** because it eliminates upfront hardware and facility costs.
- **AWS and Azure are financially comparable** – differences are often in specific use cases.
- **Break-even analysis** helps organizations decide whether and when to move to the cloud.
- **ROI calculation** proves the financial justification for cloud migration.
- **Hidden costs** (power, cooling, maintenance) are often overlooked in on-premise budgets.

### Real-World Application
- IT managers use TCO analysis to justify cloud migration to executives.
- Financial decisions are based on 3-5 year projections, not just upfront costs.
- Understanding TCO helps organizations avoid costly mistakes and optimize budgets.
- TCO analysis is a critical skill for IT consultants and cloud architects.

---

## ✅ Checklist
- [x] Reviewed TCO concepts and components
- [x] Created a cost comparison spreadsheet (on-prem vs AWS vs Azure)
- [x] Calculated total costs for each scenario
- [x] Calculated break-even point for cloud migration
- [x] Calculated ROI for each cloud provider
- [x] Created 3-year projections
- [x] Compared AWS vs Azure costs and features
- [x] Created summary dashboard with key metrics
- [x] Screenshots added
- [x] Reflection written

---

## 🔗 Next Steps
This lab prepares me for Lab 7, where I'll provision and secure a cloud VM on AWS EC2.
