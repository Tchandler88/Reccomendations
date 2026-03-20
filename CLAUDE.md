# CLAUDE.md — Securian AEM Migration Presentation

## Project Overview
Single-file HTML presentation for the Securian Financial AEM Legacy to 
Cloud Service Migration — Track 3 (Metadata & Tagging). Built for 
screen-share delivery during client review calls. All data is in a 
JavaScript data layer at the top of `index.html` (or split files if 
refactored). The UI engine reads those objects — content and rendering 
are fully decoupled.

**Client:** Securian Financial  
**Consultant:** Lasso Consulting Group  
**Track:** Track 3 — Metadata & Tagging  
**Phases covered:** Phase 1 (Tag Tree), Phase 2 (Metadata Schema), 
Phase 4 (Folder Redesign)  
**Delivery format:** Standalone HTML file, no build step required

---

## Repository Contents

### Presentation
- `index.html` (or `Securian_AEM_Migration_Presentation.html`) — 
  the full deliverable

### Source Documents (reference for data accuracy)
| File | Contents |
|------|----------|
| `Securian_Folder_Redesign_Proposed.docx` | Full folder architecture, all 25→12 container decisions, dam: field CV values |
| `Securian_Folder_ChangeGuide_OldToNew.docx` | Cell-level workbook change instructions, confirms dam: field value sets |
| `Securian_Folder_Mapping_OldToNew.csv` | Complete folder-by-folder migration map from JCRUNCH export |
| `Securian_Metadata_Analysis_Current.docx` | 137-field schema audit, all disposition decisions, CV value counts |
| `Securian_Metadata_ChangeGuide_OldToNew.docx` | Phase 2 schema change instructions, ENFORCE/RETIRE/REVIEW actions |
| `Securian_Metadata_Schema_Current.csv` | Actual extracted schema from JCRUNCH — ground truth for field list |
| `Securian_TagTree_Redesign_Proposed.docx` | Full tag tree analysis, namespace dispositions, DQ flags, proposed tree |
| `Securian_TagTree_ChangeGuide_OldToNew.docx` | Tag tree workbook change instructions |
| `Securian_Namespace_ValidationFlags.csv` | Real DQ flag data from JCRUNCH namespace validation |
| `Securian_UserAccess_ExecutiveSummary.csv` | ACL/permissions executive summary |
| `Securian_UserAccess_GroupStats.csv` | AEM group membership statistics |
| `Securian_UserAccess_HighPrivilege.csv` | High-privilege user accounts |
| `Securian_UserAccess_Issues.csv` | Access control issues identified |
| `Securian_UserAccess_PowerUsers.csv` | Power user account list |
| `Securian_Workflow_Audit.csv` | Workflow state audit data |
| `Securian_CSA_CurrentState_v1.docx` | Full Current State Assessment document |
| `Securian_DiscoveryFindings_Mar2026.docx` | Full discovery findings report |
| `Securian_SurveyFindings_Feb2026.pdf` | Stakeholder survey findings |

---

## Architecture

### Data Layer (top of HTML file)
All content lives in JavaScript `const` objects. Edit these to update 
content — never hardcode values in the render functions.

| Object | Purpose |
|--------|---------|
| `SITE_META` | Client name, project name, version, date |
| `FOLDERS` | All 25 legacy containers with change type, justification, risk |
| `LEGACY_TREE` | Full legacy folder hierarchy for tree view |
| `PROPOSED_TREE` | Full proposed folder hierarchy for tree view |
| `TAG_NAMESPACES` | All 10 tag namespaces with dispositions and children |
| `META_TABS` | All schema tabs with every field, disposition, and CV values |
| `DQ_FLAGS` | All 8 data quality flags |
| `ROLES` | Proposed AEM Cloud Service role/permission model |
| `DETAILS` | Right-pane detail content for ACL entries |

### UI Engine (bottom of HTML file)
Render functions read the data objects and build DOM. Key functions:
- `renderHierarchyTree()` — builds the collapsible folder trees
- `renderMetaTabs()` — builds the AEM-style tabbed schema UI
- `buildCVHtml()` — renders field type badge + CV value chips per row
- `renderTagTree()` — builds legacy and proposed tag trees
- `renderDQGrid()` — builds DQ flag cards
- `renderRoles()` — builds the permissions role cards
- `openFolderDetail()` / `openTagDetail()` / `openMetaDetail()` — 
  opens the right-side justification pane

---

## What Still Needs Real Data (Priority Order)

### 1. Read CSVs first before any data work
Before updating any data objects, read these files — they contain 
the actual extracted data and may correct or extend what is currently 
hardcoded:
- `Securian_Folder_Mapping_OldToNew.csv` — use to verify/replace 
  `FOLDERS` and `LEGACY_TREE` data
- `Securian_Metadata_Schema_Current.csv` — use to verify/replace 
  `META_TABS` field list
- `Securian_Namespace_ValidationFlags.csv` — use to verify/replace 
  `DQ_FLAGS`
- `Securian_UserAccess_*.csv` files — use to populate real data in 
  the Roles & Permissions panel

### 2. CV values still marked ⚠ (need admin confirmation)
These fields have placeholder values — do not finalize until confirmed:
- `dc:rights` — exact copyright statement wording (Legal sign-off)
- `xmpRights:UsageTerms` — exact usage terms wording (Legal sign-off)
- `wcmSpecificDistributors` — 24 firm names, known outdated entries
- `wcmDistributorsExcluded` — 4 values, confirm with compliance
- `highspotUsageLabel` — must match live Highspot instance (Q23)
- `homeSpot` / `wcmSpots` — Highspot admin input required (Q24)
- `wcmTfExpenseCategory` — Finance/Accounting team confirmation
- `wcmFulfillmentMethod` — Fulfillment/Print operations confirmation
- `wcmDistributionPlatform` — extract from live tag namespace
- `wcmWebsiteSection` — extract from live securian:website namespace
- `dam:owningTeam` / `dam:owningDivision` — extract from live tags

### 3. Blocked fields (do not implement until dependencies resolved)
- `wcmAdvisorTerritory` — BLOCKED on DQ-06 personalization audit
- `cq:tags-security` retirement — BLOCKED on DQ-07 ACL rebuild
- `wcmAdmasterNumber` mandatory flag — BLOCKED on MS-07 investigation

---

## Panels Summary

| Panel | Nav Label | Status |
|-------|-----------|--------|
| Folder Structure | 📁 Folder Structure | ✅ Complete — real data |
| Tag Tree | 🏷️ Tag Tree | ✅ Complete — real data |
| Metadata Schema | 📋 Metadata Schema | ✅ Complete — some ⚠ values pending |
| Roles & Permissions | 🔐 Roles & Permissions | ⚠ Proposed model — needs real UserAccess CSV data |
| Data Quality Flags | ⚠️ Data Quality Flags | ✅ Complete — 8 flags |

---

## Branding
- Primary green: `#0a3d1f` (dark), `#1a8a42` (mid), `#3dba50` (ring/accent)
- Securian logo uses overlapping ring motif — replicated in topbar
- No dark mode required — light theme only
- Font: Segoe UI / system-ui

## Out of Scope (handled in separate PowerPoint)
- Content Hub instance setup and configuration
- Personalization engine architecture details
- Workfront connector deep-dive
- Go-live sequencing and project timeline
