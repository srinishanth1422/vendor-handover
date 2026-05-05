# Management Handover Document
## Vendor Repush Automation – AstraZeneca

| Field | Detail |
|---|---|
| **Prepared On** | 28 April 2026 |
| **Scope** | Vendor Creation & Repush Automation (Workday → SAP PI → SAP FI) |
| **Source Documents** | Vendor Use Case · Vendor Repush Automation SOP · Incident Trend Analysis |

---

## 1. Executive Summary

The Vendor Repush Automation initiative addresses the end-to-end lifecycle of vendor creation within AstraZeneca's ERP ecosystem—spanning **Workday, Snowflake (HR RAW), SnapLogic, SAP PI, and SAP FI**.

**Current Operational Status:**

- The automation is **live and operational** via the Azimuth Incident Classifier application, enabling Genpact operators to trigger vendor repushes directly—eliminating the previous dependency on the SnapLogic team for manual ticket-based repushes.
- Between **March 2025 and March 2026**, the system processed **1,610 vendor-related incidents**, averaging **7–8 incidents per day**.
- The peak incident volume occurred in **Q4 2025** (Oct: 330, Nov: 294, Dec: 253), correlating with year-end vendor payment cycles and bank-detail update surges.
- Approximately **40% of incidents are recurring**, primarily caused by persistent SAP bank-detail validation failures, STOP-table entries, and employee leave/termination statuses.

**Key Achievement:** The automation reduced per-ticket handling time from **15–20 minutes of manual SnapLogic effort** (with next-day response) to a **self-service, near-real-time process** with multi-PRID batch capability.

---

## 2. Project / Operational Ledger

### ✅ Completed Milestones

| # | Milestone | Status |
|---|---|---|
| 1 | Vendor creation workflow defined and operationalized across 4 teams (Genpact → SnapLogic → SAP PI → SAP FI) | ✅ Complete |
| 2 | Bank-detail validation logic implemented (Account No. = 17 chars, IBAN ≠ null, IFSC ≠ null, all mandatory fields) | ✅ Complete |
| 3 | Snowflake HR RAW data cache integrated (1-hour full refresh from Workday) | ✅ Complete |
| 4 | SnapLogic pipeline for automated vendor push to SAP PI | ✅ Complete |
| 5 | Azimuth web application deployed with Vendor Repush button (SSO via Azure SAML) | ✅ Complete |
| 6 | Multi-PRID batch processing capability | ✅ Complete |
| 7 | Auto-retrieval of latest vendor push timestamp from Common-Infra database | ✅ Complete |

### ⏳ Pending / Open Items

| # | Item | Priority | Owner |
|---|---|---|---|
| 1 | **Recurring incident root-cause resolution** — 644 recurring incidents (40%) remain unmitigated; root causes include SAP bank-detail validation failures, STOP table entries, and leave/termination status conflicts | 🔴 High | SAP FI / HR RAW |
| 2 | **Preventive controls implementation** — Current controls are reactive, not preventive; proactive validation at Workday entry stage is absent | 🔴 High | SnapLogic / SAP |
| 3 | **Q4 demand spike planning** — No documented surge-capacity or runbook for Q4 peak (3x normal volume) | 🟡 Medium | All Teams |
| 4 | **SLA formalization** — No documented SLA targets for end-to-end vendor creation turnaround | 🟡 Medium | Genpact / Management |

---

## 3. Financial Snapshot

> [!NOTE]
> The source documents do not contain explicit financial figures (budgets, rental agreements, or invoice amounts). The following operational cost indicators are derived from the data provided.

### Operational Cost Indicators

| Metric | Value | Implication |
|---|---|---|
| **Total incidents (12 months)** | 1,610 | Volume baseline for resourcing |
| **Avg. resolution time per incident** | 48 hours (end-to-end across teams) | Extended cycle time driving vendor payment delays |
| **Total team-hours consumed** | 77,280 hours/year (1,610 × 48 hrs) | Significant operational overhead |
| **Effort distribution** | SnapLogic 50% · SAP 40% · Genpact 10% | SnapLogic bears the majority of operational burden |
| **Pre-automation manual effort** | 15–20 min/ticket + next-day response lag | Automation ROI benchmark |

### Cost-Saving Opportunity

Reducing the **644 recurring incidents** through preventive validation at the Workday data-entry stage would eliminate approximately **40% of operational volume**, translating to an estimated **30,912 team-hours annually**.

---

## 4. Stakeholder & Vendor Directory

### Team Responsibilities

| Team | Role | Assignment Group |
|---|---|---|
| **Genpact** | Ticket creation, initiation, and final closure | — |
| **SnapLogic** | Data retrieval from HR RAW, bank-detail verification, vendor push execution via pipeline | `AppSupport-EU-Integration` |
| **SAP PI** | Vendor data ingestion validation; confirms vendor receipt in SAP | `Appsupport-ECS-SAP-Integrations` |
| **SAP FI** | Final financial review and ticket status update | `AppSupport-ECS-SAP-FI` |
| **HR RAW** | Snowflake data integrity; source-of-truth for Workday banking data | — |

### Key Contact Points

| Team | Contact / Distribution List |
|---|---|
| SnapLogic Support | azeusnaplogicsupport@astrazeneca.com |
| HR RAW / Data Support | HREUITDataSupport@astrazeneca.com |

### System Touchpoints

| System | Function |
|---|---|
| **Workday** | Source system for employee banking details |
| **Snowflake (HR RAW)** | Data cache layer; refreshes hourly from Workday |
| **SnapLogic** | Integration/orchestration platform; executes vendor push pipelines |
| **SAP PI** | Middleware; receives and validates vendor data |
| **SAP FI** | Financial module; final vendor record creation |
| **Azimuth App** | Self-service web interface for Genpact operators |
| **Common-Infra DB** | Stores latest vendor push timestamps |

---

## 5. Risk & Mitigation Register

### 🔴 Red Flag Items — Immediate Management Attention Required

| # | Risk | Impact | Current State | Recommended Mitigation |
|---|---|---|---|---|
| 1 | **40% recurring incidents unresolved** — SAP bank-detail validation failures, STOP table entries, and leave/termination statuses cause the same tickets to recur daily | Vendor payment delays; SLA breaches; vendor trust erosion | Reactive — tickets are re-processed each time without addressing root cause | Implement upstream validation in Workday to block incomplete submissions; automate STOP table clearance workflows |
| 2 | **Q4 volume surge (3x baseline)** — Oct–Dec 2025 saw 877 incidents (54% of annual volume in 3 months) | Team capacity overwhelmed; extended resolution times; cascading payment delays | No documented surge capacity plan | Establish a Q4 runbook with pre-allocated resources, proactive vendor data cleansing before Oct, and escalation thresholds |
| 3 | **48-hour average resolution time** — End-to-end vendor creation spans 2 full business days across 4 teams | Vendor payment delays; repeated user follow-ups; operational friction | Sequential, multi-team handoff with no parallel processing | Explore SLA targets per team (e.g., 4-hour SnapLogic SLA, 8-hour SAP PI SLA); implement automated escalation alerts |
| 4 | **Data dependency on HR RAW 1-hour refresh** — Stale data window of up to 60 minutes creates re-work when users update Workday and immediately request vendor creation | Wasted processing cycles; user frustration; false-negative validation results | 1-hour full-load refresh is the only data sync mechanism | Evaluate event-driven or incremental refresh for banking detail changes; add a "last refreshed" indicator in the Azimuth app |
| 5 | **Single-channel visibility** — SnapLogic team cannot view user bank details directly; must rely on HR RAW team for data issues | Diagnostic delays; increased ticket bounce between teams | Manual escalation to HR RAW for data-related failures | Provide read-only bank-detail view to SnapLogic support (masked/tokenized for compliance) |

---

## 6. Standard Operating Procedures (SOPs)

### SOP 1: Vendor Creation – End-to-End Workflow

```
Step 1 │ Genpact creates a vendor creation ticket
        ↓
Step 2 │ Ticket is routed to SnapLogic team
        ↓
Step 3 │ SnapLogic retrieves employee bank details from HR RAW (Snowflake)
        ↓
Step 4 │ Bank detail validation:
        │   • Bank Account Number = 17 characters
        │   • IBAN ≠ null
        │   • IFSC Code ≠ null
        │   • All mandatory fields populated
        ↓
Step 5 │ IF validation fails:
        │   → Contact user to update bank details in Workday
        │   → Wait for HR RAW refresh (up to 1 hour)
        │   → Re-verify in HR RAW
        ↓
Step 6 │ IF validation passes:
        │   → SnapLogic executes vendor creation push
        ↓
Step 7 │ Vendor data sent to SAP PI for ingestion verification
        ↓
Step 8 │ SAP PI confirms receipt → Ticket assigned to SAP FI
        ↓
Step 9 │ SAP FI performs final financial review → Updates ticket status
        ↓
Step 10│ Genpact closes the ticket
```

### SOP 2: Vendor Repush via Azimuth Application (Self-Service)

**Application Access:**
- **URL:** `https://dev-azimuth-incident-classifier.paas-bronze.astrazeneca.net/chat`
- **Authentication:** SSO via Azure SAML (per AZ policy)

**Procedure:**

| Step | Action | Detail |
|---|---|---|
| 1 | **Login** | Authenticate via SSO; verify your PRID is displayed (top-right corner) |
| 2 | **Initiate Repush** | Click the **Vendor Repush** button (bottom-left corner of landing page) |
| 3 | **Enter PRIDs** | When prompted, type or paste the PRID(s). For multiple PRIDs, enter as comma-separated values |
| 4 | **Submit** | Click **Submit**. System confirms the PRIDs and displays "Getting relevant data…" |
| 5 | **Wait for Processing** | SnapLogic pipeline triggers automatically. Vendor creation and database update takes ~5 minutes |
| 6 | **Retrieve Timestamp** | System fetches the latest vendor push timestamp from Common-Infra database |
| 7 | **Assign to SAP PI** | Share the PRID and timestamp with SAP PI team (`Appsupport-ECS-SAP-Integrations`) |
| 8 | **SAP PI Confirms** | Once SAP PI confirms vendor receipt, request them to assign to SAP FI (`AppSupport-ECS-SAP-FI`) |

### SOP 3: Escalation Matrix for Common Failure Scenarios

| Failure Scenario | Root Cause | Immediate Action | Escalation To |
|---|---|---|---|
| "User found in STOP table" | User record flagged in SAP | Remove from STOP table and repush | SAP FI team |
| "SAP bank validation failure" | Incomplete/invalid bank details in Workday | Contact user to correct bank details; re-verify after HR RAW refresh | Genpact → User |
| "User on leave/termination status" | Employee status prevents vendor creation | Verify employment status; coordinate with HR | HR RAW team (HREUITDataSupport@astrazeneca.com) |
| "Vendor not created – data issue" | HR RAW data discrepancy | SnapLogic cannot view bank details directly; escalate with SAP failure comments | HR RAW team |
| Timestamp not available | Common-Infra DB delay | Wait 5 minutes; retry. If persistent, contact SnapLogic support | SnapLogic (azeusnaplogicsupport@astrazeneca.com) |

---

## 7. Explanation — Strategic Context for New Management

### Why This Automation Exists

The Vendor Repush Automation was implemented to eliminate a **critical operational bottleneck**: every vendor bank-detail update in Workday required Genpact to raise a manual ticket, which the SnapLogic team processed one-by-one at **15–20 minutes each**, often with **next-day turnaround**. At 7–8 tickets/day, this was unsustainable—particularly during Q4 surges.

### Why the Current Architecture Uses a Layered Approach

The Workday → Snowflake → SnapLogic → SAP PI → SAP FI chain exists because:
- **Workday** is the HR system of record but does not natively integrate with SAP for vendor creation.
- **Snowflake (HR RAW)** provides a queryable, hourly-refreshed data layer that decouples Workday from downstream consumers.
- **SnapLogic** serves as the integration orchestrator, transforming and routing data.
- **SAP PI/FI** are the financial systems of record where vendor records must ultimately reside.

### Why 40% Recurring Incidents Persist

The recurring incidents are a **systemic issue, not an automation failure**. The automation correctly processes each request; however, the same vendor records fail repeatedly because the underlying data quality issues (invalid bank details, STOP table entries, leave/termination status) are never resolved at the source. **The fix must come from upstream data governance**, not from re-engineering the automation.

### Why Q4 Is Uniquely Risky

Q4 sees a 3x volume spike driven by year-end vendor payment processing deadlines and employee banking detail updates. The current reactive model absorbs this surge without pre-positioning resources, creating cascading delays. **Proactive data cleansing in September** would significantly reduce Q4 incident volume.

---

## Appendix: Incident Volume Data (Mar 2025 – Mar 2026)

| Month | Total Incidents | Est. Recurring (40%) | Est. New (60%) |
|---|---|---|---|
| Mar 2025 | 38 | 15 | 23 |
| Apr 2025 | 50 | 20 | 30 |
| May 2025 | 69 | 28 | 41 |
| Jun 2025 | 51 | 20 | 31 |
| Jul 2025 | 70 | 28 | 42 |
| Aug 2025 | 99 | 40 | 59 |
| Sep 2025 | 120 | 48 | 72 |
| **Oct 2025** | **330** | **132** | **198** |
| **Nov 2025** | **294** | **118** | **176** |
| **Dec 2025** | **253** | **101** | **152** |
| Jan 2026 | 86 | 34 | 52 |
| Feb 2026 | 64 | 26 | 38 |
| Mar 2026 | 86 | 34 | 52 |
| **Total** | **1,610** | **644** | **966** |

---

*Document prepared for management transition. All data sourced from operational records covering March 2025 – March 2026.*
