# A4 User Flows

## Core Flow: Report → Track → Retest → Sign Off

```
START: Priya opens Dashboard (S01)
    |
    v
Sees issue list with stats
    |
    +---> Clicks "Review Calls" ---> Call Review (S02)
    |                                    |
    |                              Reviews transcript + post-call data
    |                                    |
    |                              Spots a problem
    |                                    |
    |                              Clicks "Report Issue from This Call"
    |                                    |
    |                                    v
    +---> Clicks "Report Issue" ---> Report Issue (S03)
                                        |
                                   Fills form (call ID pre-filled if from S02)
                                   Checks for duplicates in sidebar
                                        |
                                   +--- Is it a duplicate? ---+
                                   |                          |
                                  YES                        NO
                                   |                          |
                              Link to existing           Submit new issue
                              issue                          |
                                   |                          v
                                   +----> Back to Dashboard (S01)
                                              |
                                         Issue appears in table
                                              |
                                    [Vendor ships fix, marks "Fixed"]
                                              |
                                         Tester clicks issue row
                                              |
                                              v
                                       Issue Detail (S04)
                                              |
                                    Sees status: "Fixed"
                                    Clicks "Start Retest"
                                              |
                                              v
                                       Retest Verdict (S05)
                                              |
                                    Enters build #, call ID
                                    Selects Pass or Fail
                                              |
                                       +-----+-----+
                                       |           |
                                     PASS        FAIL
                                       |           |
                                  Status →     Status →
                                  "Verified"   "Reopened"
                                       |           |
                                       v           v
                                  Issue Detail  Issue Detail
                                  (updated)    (updated, awaits new fix)
                                       |
                                       v
                              Back to Dashboard (S01)
                                       |
                              All blockers resolved?
                                       |
                                  +----+----+
                                  |         |
                                 YES       NO
                                  |         |
                                  v         v
                            Go-Live      Continue
                            Readiness    testing loop
                            (S06):
                            "READY"
```

## Edge Case 1: Duplicate Issue Detected During Reporting

```
Tester opens Report Issue (S03)
    |
Starts typing issue title
    |
Duplicate panel shows: "ISS-001: Wrong due date — Reopened"
    |
    +--- Tester recognizes it's the same bug ---+
    |                                            |
   YES                                          NO (different issue)
    |                                            |
Clicks "Link as Duplicate"                  Continues filling form
    |                                            |
Redirected to ISS-001                       Submits new issue
with their call added                            |
as evidence                                Back to Dashboard
```

## Edge Case 2: Compliance Issue Flagged

```
Tester reports issue (S03)
    |
Checks "Involves regulatory wording"
    |
Banner appears: "Will be routed to Deepa Iyer for compliance review"
    |
Submits issue
    |
    v
Dashboard (S01): Issue tagged "Compliance"
    |
    v
Go-Live Readiness (S06): Issue appears under "Compliance Items Pending"
    |
[Deepa reviews externally, confirms fix is OK]
    |
Priya updates issue status
    |
Compliance item cleared from readiness
```

## Edge Case 3: Retest Fails (Intermittent Bug)

```
Sameer opens ISS-001 (S04)
    |
Status: "Fixed" (vendor claims)
    |
Clicks "Start Retest" → S05
    |
Tests call 1: date correct ✓
Tests call 2: date WRONG ✗
    |
Selects verdict: FAIL
Notes: "Works on first call, fails on second. Intermittent."
    |
Submits → ISS-001 status changes to "Reopened"
    |
Back to Dashboard: ISS-001 shows red "Reopened" badge
    |
Go-Live Readiness: ISS-001 back in "Open Blockers"
```

## Edge Case 4: Post-Call Data Mismatch Noticed

```
Tester opens Call Review (S02)
    |
Selects UAT-0157
    |
Reads transcript: customer says "don't call me again" (T2-08)
Sees post-call data: Disposition = "Payment link sent" ← highlighted red
    |
Mismatch is visually flagged
    |
Clicks "Report Issue from This Call" → S03
    |
Reports: "Post-call disposition wrong — says 'Payment link sent' 
          but customer refused and said don't call again"
Category: Post-Call Data
    |
Submits → New issue in dashboard
```
