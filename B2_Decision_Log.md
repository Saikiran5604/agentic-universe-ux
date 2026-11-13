# Decision Log — Phase B

## Decision 1: Organize by issues, not by test calls
**Chosen:** The dashboard is issue-centric (one row per unique issue, not per test call or per report).
**Rejected:** A call-centric view where you browse calls first and discover issues inside them.
**Why:** Priya's core job is tracking issues to resolution, not reviewing calls. The calls are evidence — the issues are the unit of work. Three testers reported the same date bug (F01/F05/F07); a call-centric view would show them as three separate problems in three calls. An issue-centric view with duplicate linking shows one issue with three pieces of evidence.

## Decision 2: Standardized severity (4 levels) over free-text
**Chosen:** A fixed dropdown with Critical / High / Medium / Low.
**Rejected:** Free-text severity (the current state, which produced "P1", "urgent", "High" and blank in the same sheet — F03, F05, F01, F10). Also rejected a 5-level scale (Critical/Major/Moderate/Minor/Cosmetic) as too granular for a 3-person team in a 2-week UAT.
**Why:** Without consistent severity, Rahul can't triage blockers and Priya can't prioritize. Four levels is enough to distinguish "blocks go-live" from "nice-to-fix."

## Decision 3: Transcript + post-call data side by side
**Chosen:** The call review screen (S02) shows the conversation transcript on the left and the system-recorded post-call data on the right, with mismatch highlighting.
**Rejected:** Showing transcripts alone (the current state — nobody is systematically checking post-call data). Also rejected a separate "data audit" screen, which would add navigation and reduce the chance testers actually compare the two.
**Why:** Two of the three test calls have wrong post-call data (T2-POST, T3-POST) and nobody in the starter pack caught this. The side-by-side layout makes mismatches visible by proximity — the tester reads "don't call me again" on the left and sees "Payment link sent" on the right.

## Decision 4: Separate "Fixed" from "Verified" status
**Chosen:** Five statuses: Open → Fixed (vendor claims fix) → Verified (tester confirms) or Reopened (tester finds it still broken). Verified is the only terminal success state.
**Rejected:** The current two-state model (Open / Fixed) where "Fixed" means either "vendor says fixed" or "tester confirmed" — no one knows which (F08 "Fixed?"). Also rejected adding "In Review" and "In Progress" as additional states — too many states for a small team.
**Why:** The retest of issue #1 showed it still failing intermittently (E3, F08). The status "Fixed?" with a question mark is a design failure — the system must enforce a deliberate verification step. Karthik marks "Fixed," then Sameer retests and marks "Verified" or "Reopened."

## Decision 5: Go-live readiness view over analytics dashboard
**Chosen:** A summary screen (S06) with a clear READY / NOT READY verdict, open blocker list, compliance pending items, and progress bars by severity.
**Rejected:** A full analytics dashboard with charts (Rahul's E4 request). Also rejected embedding the readiness info only as stats on the main dashboard — Rahul needs a dedicated, focused view he can show leadership.
**Why:** Rahul's real question is "can we go live?" (E4), not "show me trends." Priya explicitly says she doesn't need charts (E6). A readiness summary answers Rahul's question directly while respecting Priya's need for tracking over visualization.

**Note on F06 ("Agent sounds robotic"):** This feedback sheet entry was deliberately excluded from the prototype's issue list. It has no call ID, no severity, and no specific transcript reference — it is exactly the kind of vague report the product's structured form (mandatory call ID, severity dropdown) is designed to prevent. In a real implementation, Anjali would be prompted to re-submit with a specific call and transcript lines.

**Note on ISS-003 / F03 ("Pushes payment link after customer said already paid"):** The feedback sheet marks this "Fixed" but Karthik's email (E2) only confirms fixes for #1 and #2, and addresses #4. He never confirmed #3. The prototype shows ISS-003 as Open — this is a conscious decision to reflect what was actually confirmed, not what the sheet assumed.

---

## Tools Used
- **Prototype:** Hand-written HTML/CSS/JavaScript. One self-contained file, no external dependencies, opens offline in Chrome.
- **Research & Tracker:** Google Sheets (tracker template), text documents.
- **Flows & IA:** Hand-drawn on paper, photographed.

## Changes After Lock
None. (Phase A plan was followed as locked.)
