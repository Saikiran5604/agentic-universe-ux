# A2 Tracker Content
(Copy each section into the corresponding tab of the Excel/Google Sheet tracker)

---

## TAB: Evidence Log

| ID | Source | Fact | Implication for Product |
|----|--------|------|------------------------|
| EV-01 | T1-03, T1-05, T1-POST | Asha says "March 5th" but lead record is 03/05/2026 (DD/MM = 3 May). Customer confirms "3rd May" (T1-06). | Date format parsing bug. Product must show post-call data alongside transcript so testers can spot data vs. speech mismatches. |
| EV-02 | T2-04, T2-05, T2-06, T2-07 | Customer says "I paid" twice, Asha ignores and keeps pushing payment link. | Conversation flow bug. Product must let testers categorize issue types (flow, data, compliance, audio). |
| EV-03 | T2-08, T2-POST | Customer says "don't call me again" but disposition = "Payment link sent". No DNC flag exists. | Post-call data is silently wrong. Product must surface post-call data for review and flag mismatches. |
| EV-04 | T3-03, cf. T1-03, T2-03 | Hindi call (T3) is missing "This call is recorded" mandatory disclosure present in English calls. | Compliance blocker nobody noticed. Product must support compliance tagging with routing to Deepa. |
| EV-05 | T3-06, T3-07, T3-POST | Agent promises callback but post-call data shows callback_requested = blank. Disposition = "Not interested" instead of callback. | Two post-call data errors in one call. Reinforces need for transcript + data side-by-side review. |
| EV-06 | F01, F05, F07 | Same date bug reported by 3 different testers in 3 different words ("Wrong due date", "Date is wrong said March", "Due date mismatch vs policy"). | Product must support duplicate detection/linking to prevent Priya spending hours reconciling. |
| EV-07 | F03, F05, F01, F10 | Severity uses "P1", "urgent", "High", and blank — no standard scale. | Product must enforce a standardized severity dropdown. |
| EV-08 | E2, E3, F08 | Karthik says "fixed in today's build" (no build #). Sameer retests — works once, fails once. Status: "Fixed?" | Product must track build numbers and require deliberate pass/fail retest verdicts. |
| EV-09 | E6 | Priya: "I'm spending two hours every evening merging feedback... can't tell what's actually been fixed." | Core coordination pain. Product's primary job is reducing this overhead. |
| EV-10 | E4 | Rahul: "Can we get a dashboard with all test calls in charts?" | Sponsor wants go/no-go visibility, not analytics. A readiness summary > charts. |
| EV-11 | E4, E5 | Rahul suggests testers edit scripts directly. Deepa blocks it: compliance must review wording changes first. | Product must NOT allow direct script editing. Compliance issues route to Deepa. |
| EV-12 | F06 | Anjali: "Agent sounds robotic" — no call ID, no severity, no specifics. | Vague reports waste time. Product should require call ID and structured fields. |
| EV-13 | F09 | Priya: "Asked for a human, not sure callback was logged" — shows testers are starting to check post-call data. | Validates the need for transcript + post-call data side-by-side. |

---

## TAB: Jobs to Be Done

### Core Functional Job
**When** I'm leading UAT for an automated voice agent on a tight go-live deadline, **I want to** see every reported issue in one place with its current status, linked evidence, and retest history, **so I can** confidently tell the sponsor whether we're ready to go live — without anything falling through the cracks.
[Evidence: E4, E6, F01-F10]

### Supporting Functional Jobs
1. **When** I finish a test call, **I want to** report what went wrong with the call ID and transcript lines pre-linked, **so I can** log a precise, actionable issue in under 2 minutes.
[Evidence: F06, EV-12]

2. **When** the vendor ships a fix, **I want to** retest it against a specific build and record a clear pass or fail, **so I can** know (and show) that the fix actually works.
[Evidence: E2, E3, F08, EV-08]

3. **When** a new issue is reported, **I want to** see if someone already reported the same thing, **so I can** link duplicates instead of creating noise.
[Evidence: F01/F05/F07, E6, EV-06]

### Emotional Job
**When** my sponsor asks "are we ready to go live?", **I want to** feel certain that every issue has been accounted for, **so I can** answer with confidence instead of anxiety.
[Evidence: E6 — "I need to know nothing is falling through the cracks"]

### Social Job
**When** leadership is deciding on go-live, **I want to** show a clear, professional summary of testing progress, **so I'm** seen as someone who runs a rigorous, trustworthy process.
[Evidence: E4 — Rahul needs to "tell leadership by next Friday"]

---

## TAB: User Stories

| ID | Priority | As a... | I want... | So that... | Linked Job | Evidence |
|----|----------|---------|-----------|------------|------------|----------|
| US-01 | Must | UAT lead (Priya) | to see all reported issues in one dashboard with status, severity, and call ID | I can tell at a glance what's open, what's fixed, and what needs retesting | Core | E6, F01-F10 |
| US-02 | Must | Tester (Sameer/Anjali) | to review a test call transcript alongside its post-call data | I can spot when what Asha says doesn't match what the system records | Functional #1 | T1-03/T1-POST, T2-POST, T3-POST, EV-01, EV-03, EV-05 |
| US-03 | Must | Tester | to report an issue with a standardized severity dropdown and mandatory call ID | every issue has consistent metadata and is traceable to a specific call | Functional #1 | F03/F05 severity mismatch, F06 no call ID, EV-07, EV-12 |
| US-04 | Must | UAT lead (Priya) | to see suggested duplicates when a new issue is reported | I don't spend hours reconciling reports that describe the same bug | Functional #3 | F01/F05/F07, E6, EV-06 |
| US-05 | Must | Tester | to record a retest verdict (pass/fail) linked to a specific build number | the team knows exactly what was tested, on which build, and whether it passed | Functional #2 | E2, E3, F08, EV-08 |
| US-06 | Must | UAT lead (Priya) | to see a go-live readiness summary showing open blockers and resolution progress | I can advise Rahul on go/no-go without assembling it manually | Core, Emotional | E4, E6 |
| US-07 | Must | Tester | to flag when post-call data contradicts what happened in the conversation | data integrity issues are caught during UAT, not after go-live | Functional #1 | T2-POST, T3-POST, EV-03, EV-05 |
| US-08 | Must | UAT lead | to tag compliance-sensitive issues so they route to the compliance officer | regulatory wording changes can't go live without Deepa's review | Functional #1 | E4, E5, T3-03 (missing disclosure), EV-04, EV-11 |
| US-09 | Should | Sponsor (Rahul) | to see a summary of testing progress (issues by severity, resolution rate) | I can report to leadership with confidence | Social | E4 |
| US-10 | Should | Tester | to categorize issues by type (data mismatch, conversation flow, compliance, audio, post-call) | patterns across test calls become visible | Functional #1 | Mixed types in F01-F10 |
| US-11 | Should | UAT lead | to link related issues together | I can track that one root cause may have multiple symptoms | Functional #3 | F01/F05/F07/F08, EV-06 |
| US-12 | Could | Compliance officer (Deepa) | to see issues tagged as compliance-related and record my approval/rejection | I can fulfill my review duty without joining the daily testing process | — | E5 |
| US-13 | Won't | Sponsor (Rahul) | a full analytics dashboard with charts for all test calls | — | — | E4 (Rahul asks for charts, but Priya's real need is tracking, not visualisation per E6) |
| US-14 | Won't | Tester | to edit the agent's script directly | — | — | E4 (Rahul suggests), E5 (Deepa blocks: compliance must review first) |
| US-15 | Won't | Anyone | to build or access a real backend, authentication, or external integrations | — | — | Assignment scope |

### Acceptance Criteria for Must Stories

**US-01: Issue dashboard**
- GIVEN I am logged in as UAT lead, WHEN I open the dashboard, THEN I see all issues with columns: ID, title, severity, status, call ID, reporter, date
- GIVEN issues exist with mixed statuses, WHEN I filter by "Open", THEN only open issues are shown
- GIVEN 3 issues are critical/high and open, WHEN I view the dashboard, THEN the "Open Blockers" count shows 3

**US-02: Call review with transcript + post-call data**
- GIVEN I select a test call (e.g., UAT-0142), WHEN the call review loads, THEN I see the transcript on the left and post-call data (disposition, due_date, callback) on the right
- GIVEN the post-call data contradicts the transcript (e.g., T2-POST disposition vs T2-08), THEN the mismatch is highlighted visually

**US-03: Standardized issue reporting**
- GIVEN I click "Report Issue", WHEN the form loads, THEN severity is a required dropdown with exactly 4 options: Critical, High, Medium, Low
- GIVEN I came from a call review, WHEN the form opens, THEN the call ID is pre-filled
- GIVEN I try to submit without a call ID, THEN the form shows a validation error

**US-04: Duplicate detection**
- GIVEN I am filling out a new issue, WHEN there are existing issues with similar keywords, THEN a "Possible Duplicates" panel shows matching issues
- GIVEN a suggested duplicate matches, WHEN I click "Link as Duplicate", THEN my report is linked to the existing issue instead of creating a new one

**US-05: Retest with build tracking**
- GIVEN I click "Start Retest" on an issue, WHEN the retest form opens, THEN I must enter a build number and select Pass or Fail
- GIVEN I submit Pass, THEN the issue status changes to "Verified"
- GIVEN I submit Fail, THEN the issue status changes to "Reopened" with my notes

**US-06: Go-live readiness summary**
- GIVEN there are open critical/high issues, WHEN I view the readiness screen, THEN it shows "NOT READY" with a list of open blockers
- GIVEN all critical/high issues are verified, WHEN I view readiness, THEN it shows "READY"

**US-07: Post-call data mismatch flagging**
- GIVEN the transcript shows the customer said "don't call me again" (T2-08) but disposition = "Payment link sent" (T2-POST), WHEN I view the call review, THEN the disposition is highlighted as a mismatch

**US-08: Compliance tagging**
- GIVEN I check "This issue involves regulatory wording" on the report form, WHEN I submit, THEN the issue is tagged "Compliance" and a note shows it will be routed to Deepa Iyer
- GIVEN a compliance issue is open, WHEN I view go-live readiness, THEN it appears under "Compliance Items Pending"

---

## TAB: Prioritisation (MoSCoW)

| Story | Priority | Rationale |
|-------|----------|-----------|
| US-01 | **Must** | Without a single issue dashboard, Priya's 2-hour nightly merge continues. Core coordination need. |
| US-02 | **Must** | Post-call data bugs (T2-POST, T3-POST) are invisible without side-by-side view. Nobody caught them. |
| US-03 | **Must** | Inconsistent severity (F03 "P1", F05 "urgent") and missing call IDs (F06) make triage impossible. |
| US-04 | **Must** | Three reports of the same date bug (F01/F05/F07) is exactly the problem Priya describes in E6. |
| US-05 | **Must** | "Fixed?" with a question mark (F08) shows the current process can't distinguish vendor-claimed from tester-verified. |
| US-06 | **Must** | Rahul needs go/no-go by next Friday (E4). Someone has to assemble this picture today — the product should do it. |
| US-07 | **Must** | T2-POST and T3-POST have silent errors that nobody flagged. This is a go-live risk. |
| US-08 | **Must** | Missing disclosure (T3-03) is a compliance blocker that nobody noticed. Must be surfaced and routed. |
| US-09 | **Should** | Useful for Rahul but not essential — the go-live readiness screen (US-06) covers the immediate need. |
| US-10 | **Should** | Helps spot patterns but the core flow works without categories as long as issues are described clearly. |
| US-11 | **Should** | Useful but the duplicate detection (US-04) covers the most critical case of same-root-cause linking. |
| US-12 | **Could** | Deepa doesn't need daily access; tagging + email notification could suffice initially. |
| US-13 | **Won't** | Rahul asked for charts (E4) but Priya's real need is tracking (E6). Readiness summary > analytics. |
| US-14 | **Won't** | Blocked by compliance (E5). Testers cannot edit scripts directly. |
| US-15 | **Won't** | Out of scope per assignment brief. |

---

## TAB: Trade-offs

### Trade-off 1: Readiness summary vs. analytics dashboard
**What we cut:** Full analytics dashboard with charts (Rahul's request, E4)
**Why:** Rahul's real need is a go/no-go decision, not data exploration. Priya explicitly says "I don't need charts. I need to know nothing is falling through the cracks" (E6). We give Rahul a readiness view with open blockers, resolution progress bars, and a clear "Ready / Not Ready" verdict. This takes less development time and directly answers the question he actually needs answered.

### Trade-off 2: Compliance routing vs. compliance editing
**What we cut:** A full compliance review workflow where Deepa edits wording in the tool
**Why:** Deepa's involvement is occasional — she reviews flagged issues, not daily testing (E5). Building a full wording-editor is over-engineering. We tag compliance issues, show them on the readiness screen, and assume Deepa reviews them via the existing communication channel (email). If this becomes a bottleneck after launch, we add a dedicated view later.

### Trade-off 3: Duplicate linking vs. auto-merge
**What we cut:** Automatic duplicate merging
**Why:** Auto-merge risks hiding distinct issues that happen to share keywords. We show suggested duplicates when reporting (so Sameer sees "ISS-001 already covers this date bug") but require a human decision to link. This prevents both the current problem (three separate rows for one bug) and the opposite problem (distinct issues merged incorrectly).

### Trade-off 4: Separate role-based views vs. one shared view
**What we cut:** Separate dashboards per role (tester view, lead view, sponsor view)
**Why:** The team is 3 testers + 1 lead + 1 sponsor. The overhead of building and maintaining separate views isn't justified for a 2-week UAT. One shared dashboard with the go-live readiness accessible as a tab serves everyone. Priya sees the issue table, Rahul clicks "Go-Live Readiness" for his summary. Role-based views are a Could for a future version.

### Trade-off 5: Build tracking as free-text vs. structured build management
**What we cut:** A structured build registry with automatic version comparison
**Why:** The vendor ships builds informally ("today's build" — E2). Enforcing a build registry adds process overhead that won't stick in a 2-week UAT. We use a free-text "Build #" field on retests so there's at least a record of which build was tested. Good enough to answer "was this tested on the latest build?" without adding bureaucracy.

---

## TAB: Screens & Content (for A3)

| Screen ID | Screen Name | Elements | Data Source | Empty State | Error State |
|-----------|-------------|----------|-------------|-------------|-------------|
| S01 | Issue Dashboard | Stat cards (Open Blockers, Awaiting Retest, Verified, Total) · Filter bar (All/Open/Reopened/Fixed/Verified) · Issue table (ID, Title, Category, Severity, Status, Call ID, Reporter, Date) · "Report Issue" button · "Go-Live Status" button | Issue records, aggregated counts | "No issues reported yet. Start by reviewing a test call." | "Failed to load issues. Please refresh." |
| S02 | Call Review | Call selector cards (per call: ID, date, tester, language, issue count) · Transcript panel (line ID, speaker, text; highlighted lines for issues) · Post-call data panel (disposition, due_date, callback; mismatch highlighting) · Linked issues list · "Report Issue from This Call" button | Transcript data, post-call system data, linked issue records | "No test calls available. Calls appear here after a test session." | "Transcript failed to load." |
| S03 | Report Issue | Form: Call ID (dropdown, pre-filled if from S02) · Reporter (auto-filled) · Title (text) · Description (textarea) · Severity (dropdown: Critical/High/Medium/Low) · Category (dropdown: Data/Flow/Compliance/Audio/Post-call) · Affected lines (text) · Compliance checkbox · Submit/Cancel · Duplicate detection panel (right sidebar) | Form input, existing issues for duplicate matching | Duplicate panel: "As you type, we'll check for similar issues." | Form validation errors inline |
| S04 | Issue Detail | Issue header (ID, severity badge, status badge, category) · Title & description · Reporter, date, linked calls · Merged duplicates section · Transcript excerpt (highlighted lines) · Status timeline · Retest history (per retest: tester, date, build, verdict, notes) · "Start Retest" button · Back to dashboard | Issue record, linked transcript excerpts, retest records | Retest history: "No retests yet. Click 'Start Retest' after the vendor ships a fix." | "Issue not found." |
| S05 | Retest Verdict | Issue summary (ID, title, severity, status) · Build number (text input) · Retest call ID (dropdown) · Verdict (Pass/Fail, large toggle buttons) · Notes (textarea) · Previous retests list · Submit/Cancel | Issue record, available call IDs | Previous retests: "This is the first retest." | "Failed to submit verdict. Please retry." |
| S06 | Go-Live Readiness | Readiness banner (READY / NOT READY) · Open blockers list (critical + high) · Compliance items pending · Resolution progress bars (by severity) · UAT summary stats (calls, issues, duplicates merged, builds, testers, deadline) | Aggregated from all issues | "No issues reported yet. Testing hasn't started." | "Failed to calculate readiness." |

---

## TAB: Change Log
(Empty at Phase A submission — to be filled during Phase B if any changes are made after lock)

| Date | What Changed | Reason |
|------|-------------|--------|
| — | — | — |
