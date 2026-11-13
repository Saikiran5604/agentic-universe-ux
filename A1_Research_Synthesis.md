# Research Synthesis — Asha UAT Testing Loop

## Key Insights

**1. The date bug is a systemic format problem, not a one-off error.**
Asha reads 03/05/2026 as "March 5th" instead of "3 May" — a DD/MM vs MM/DD parsing error. The customer confirms "3rd May" (T1-06), and the lead record stores DD/MM (T1-POST). This same root cause surfaces in F01, F05, F07, and F08 — three testers reported it in different words, and the sheet treats them as separate issues. Priya spends hours reconciling exactly this (E6). → Need duplicate linking so three reports of one bug collapse into a single issue. [T1-03, T1-05, T1-POST, F01, F05, F07, F08, E3, E6]

**2. Post-call data is silently wrong, and nobody is testing it.**
Everyone focuses on what Asha *says*, but the data the system *records* is wrong too — and nobody flagged it. T2-POST records "Payment link sent" when the customer refused and said "don't call me again" (T2-08). T3-POST records "Not interested" when the customer asked for a callback (T3-06), and callback_requested is blank despite Asha promising one (T3-07). → Post-call data needs to sit next to the transcript so testers actually see the mismatch. [T2-04–T2-08, T2-POST, T3-06, T3-07, T3-POST, F09, F10]

**3. The mandatory recording disclosure is missing on the Hindi call — a compliance blocker nobody noticed.**
T1-03 and T2-03 both include "This call is recorded for quality purposes." T3-03 does not. Nobody in the email thread or feedback sheet flagged this. Given Deepa's stance that compliance wording must go through her team (E5), a missing disclosure is a go-live blocker. → Compliance issues need first-class treatment: tagged, routed to Deepa, blocking go-live until cleared. [T1-03, T2-03, T3-03, E5]

**4. The process lacks a retest protocol, so "Fixed" is meaningless.**
Karthik says "#1 and #2 are fixed" (E2) with no build number. Sameer retests — one call correct, one wrong (E3). He marks it "Fixed?" because there's no pass/fail protocol. → We need to separate "vendor claims fixed" from "tester verified fixed" and require a deliberate verification step with a build reference. [E2, E3, F08]

**5. Priya's real job is coordination, not testing — but the tools only support logging.**
Priya spends "two hours every evening" merging feedback from three testers (E6). Rahul wants dashboards (E4) but Priya needs confidence nothing falls through the cracks. → Prioritize deduplication, status tracking, and a clear go/no-go picture over analytics visualizations. [E4, E6]

## Users

**Primary — Priya Nair (UAT Lead):** Runs testing day-to-day, collates all feedback, reports to sponsor. The product is built for her coordination job.
**Secondary — Sameer Khan & Anjali Rao (Testers):** Execute test calls, report issues. Need fast, guided reporting with consistent severity and duplicate checking.
**Stakeholder — Rahul Mehta (Sponsor):** Decides go-live by next Friday. Needs a readiness summary, not a full dashboard.
**Peripheral — Deepa Iyer (Compliance):** Reviews compliance-tagged issues before go-live (E5). Occasional involvement, not daily access.

## Reference Teardown

**TestRail (Gurock)** — Test management with retest-per-build workflows. *Borrow:* the structured retest model where each verification is tied to a specific build number, producing a clear Pass/Fail — this prevents the "Fixed?" ambiguity in F08. *Avoid:* heavyweight test case authoring with preconditions and expected results — UAT testers need lightweight issue reporting, not QA templates.

**Linear** — Issue tracking with explicit status states and duplicate detection. *Borrow:* the clear pipeline (Open → Fixed → Verified / Reopened) with no ambiguous states, and the duplicate-suggestion feature that would have prevented F01, F05, and F07 from being three separate rows. *Avoid:* the engineering-centric model (sprints, cycles) — Priya's team thinks in test sessions and builds.

**Marker.io** — In-context bug reporting where metadata is auto-filled from the thing being reviewed. *Borrow:* the concept of reporting an issue *from within the call review*, with call ID and transcript line pre-linked — ensuring every report has structured evidence, unlike F06 (no call ID, no specifics). *Avoid:* the visual annotation model (built for web QA, not call review) — adapt to transcript-anchored reporting instead.
