# Olist Marketplace Analytics

### Turning Delivery Reliability Into Customer Experience

A data analytics project on the **Olist Brazilian E-Commerce Public Dataset**, investigating marketplace performance, delivery reliability, customer satisfaction, seller patterns, geography, category performance, payment behavior, and operational intervention priorities.

---

## 🎯 Business Problem

The objective of this project is to understand:

- How marketplace performance evolved over time
- How delivery reliability is associated with customer satisfaction
- How dissatisfaction changes with delivery-delay severity
- Whether poor delivery performance is concentrated among specific sellers or geographies
- Which product categories and customer-state combinations deserve intervention
- Whether payment and freight characteristics meaningfully relate to customer experience
- Which operational factors show the strongest statistical association with low review scores

The goal is not simply to describe the data, but to translate the evidence into **actionable operational priorities for Olist**.

---

## 🔍 Executive Summary

The strongest observable customer-experience signal in the analysis is **delivery reliability**.

### Key findings

| Finding | Result |
|---|---:|
| Delivered orders analyzed | **96,476** |
| Early-order average rating | **4.28★** |
| Late-order average rating | **2.26★** |
| 1–2★ rate: Early | **9.46%** |
| 1–2★ rate: Late | **62.80%** |
| 4–7 days late: 1–2★ rate | **67.98%** |
| Pre-crisis late rate (Aug–Oct 2017) | **3.83%** |
| Crisis late rate (Feb–Mar 2018) | **16.63%** |
| Late orders: pre-crisis → crisis | **491 → 2,255** |
| Late vs Early odds of low rating | **~16.4×** |
| March 2018 top-10 seller share | **~20%** |
| Highest-priority segment | **RJ × Bed/Bath/Table** |
| RJ × Bed/Bath/Table late + low-rating orders | **147** |

The evidence indicates a sharp relationship between delivery performance and customer satisfaction. The analysis is observational, so these results should be interpreted as **associations rather than definitive causal effects**.

---

## 💡 Core Story

The analysis leads to a simple business narrative:

**Late delivery**
↓  
**Severe customer dissatisfaction**
↓  
**Multi-day delays create disproportionate damage**
↓  
**Reliability deteriorated sharply around late 2017 / early 2018**
↓  
**The issue was broadly distributed across sellers**
↓  
**Specific category × destination segments provide focused intervention opportunities**

---

## 📊 Major Insights

### 1. Delivery reliability is strongly associated with customer satisfaction

Early deliveries averaged **4.28★**, while late deliveries averaged **2.26★**.

The 1–2★ review rate increased from **9.46% for early orders to 62.80% for late orders**.

A Mann–Whitney U test and rank-biserial effect size provide strong statistical evidence of a large difference between the early- and late-delivery review distributions.

---

### 2. Customer dissatisfaction accelerates as delays become severe

The 1–2★ review rate rises sharply with longer delays:

| Delay | 1–2★ rate |
|---|---:|
| Early / On-time | **9.51%** |
| 1–3 days late | **32.46%** |
| 4–7 days late | **67.98%** |
| 8–14 days late | **80.26%** |
| 15+ days late | **78.40%** |

The key operational implication is that preventing a delay from becoming a **multi-day failure** is more important than optimizing average delivery time alone.

---

### 3. A broad reliability deterioration emerged in late 2017 / early 2018

Comparing Aug–Oct 2017 with Feb–Mar 2018:

- Late-delivery rate: **3.83% → 16.63%**
- Late orders: **491 → 2,255**
- Average review score: **4.246★ → 3.830★**

This represents a **12.8 percentage-point increase** in late deliveries and a **359% increase in late-order count**.

The analysis identifies a clear deterioration in marketplace reliability during this period, but does not claim that marketplace growth caused it.

---

### 4. The problem was not limited to a handful of sellers

In the seller-scoped analysis of March 2018, the top 10 sellers by late-order count represented only about **20% of late-order volume**.

This suggests that a marketplace-wide reliability issue requires a **network-level operational response**, supplemented by seller-level exception management.

---

### 5. Specific category × destination segments deserve targeted intervention

The strongest intervention opportunities combine meaningful order volume, elevated late rates, and substantial low-rating burden.

| Segment | Total Orders | Late Rate | Late + Low-Rating Orders |
|---|---:|---:|---:|
| **RJ × Bed/Bath/Table** | 1,333 | **15.08%** | **147** |
| **SP × Health/Beauty** | 3,678 | 5.44% | **103** |
| **SP × Bed/Bath/Table** | 4,260 | 4.13% | **102** |
| **RJ × Sports/Leisure** | 882 | 14.06% | **90** |
| **RJ × Computers/Accessories** | 824 | 12.50% | **81** |

These are **intervention priorities**, not proven causal root causes.

---

## 📈 Statistical Validation

### Mann–Whitney U

Early vs late review-score distributions:

- **U = 475,003,963.5**
- **p ≈ 0**
- **Rank-biserial correlation ≈ −0.64**

This indicates a large difference in review-score distributions.

### Logistic Regression

A converged logistic regression modeled the odds of a **1–2★ review** while controlling for:

- Delivery status
- Log distance
- Log freight value
- Log order value
- Cross-state shipping
- Customer state

Key result:

> **Late vs Early delivery: ~16.4× higher odds of a low review**

A separate **1-star robustness model** produced a nearly identical result.

These are **conditional associations, not causal estimates**.

---

## 🧠 Methodology

The analysis combines nine Olist source tables:

- Orders
- Order Items
- Payments
- Reviews
- Customers
- Products
- Sellers
- Geolocation
- Category Translation

### Important modeling decisions

- Delivery delay is calculated only for delivered orders.
- Review data are aggregated to one order-level score.
- Item and payment records are aggregated before order-level joins.
- Geolocation is aggregated to ZIP-prefix level before coordinate joins.
- Seller-level performance uses single-seller orders for clean attribution.
- Category analysis uses orders with an unambiguous single category.
- Distance is estimated using Haversine straight-line distance between aggregated ZIP-prefix coordinates.
- Statistical models are used as robustness checks rather than as substitutes for business interpretation.

---

## 🧹 Data Quality

The raw data contain genuine real-world irregularities, including:

- Multiple review records for some orders
- Duplicate review IDs
- Multiple items per order
- Multiple payment records per order
- Missing delivery timestamps
- Missing product categories
- Multiple sellers on some orders
- Repeated geolocation observations

These issues were explicitly handled to preserve correct analytical grain and avoid join explosions.

---

## 🚀 Business Recommendations

### 1. PREVENT — Detect delivery risk early

Flag orders approaching their estimated delivery date without confirmed delivery.

**Primary KPI:** Late-delivery rate

---

### 2. PRIORITIZE — Focus on high-burden segments

Target operational attention toward high-risk customer-state × category combinations.

Start with segments such as:

- RJ × Bed/Bath/Table
- RJ × Sports/Leisure
- RJ × Computers/Accessories
- SP × Health/Beauty
- SP × Bed/Bath/Table

**KPI:** Segment-level late rate and 1–2★ rate

---

### 3. ESCALATE — Manage persistent seller exceptions

Monitor sellers with consistently elevated late-delivery rates and meaningful customer impact.

However, seller management should complement—not replace—a network-level reliability program.

**KPI:** Seller late rate + late-order volume

---

## 📁 Repository Structure

```text
Olist-Data-Analytics/
│
├── AroraXSinghal.ipynb
├── Olist_Marketplace_Analytics_Report.docx.pdf
└── README.md
