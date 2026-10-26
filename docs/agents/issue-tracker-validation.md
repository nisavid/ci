# Wayfinder fallback validation cases

These are protocol walkthroughs for `issue-tracker.md`, which is consumed by an external `/wayfinder` agent. This repository has no executable Wayfinder consumer or automated protocol test suite. Evaluate each case against the documented gate without mutating real issues; this does not establish that an external implementation follows the protocol.

Use a user-selected map `github.com/example/project#10` (database ID 100), labelled `wayfinder:map`, whose task list contains `- [ ] #20`. Its registered child `#20` (database ID 200) is an Issue with first body line `Part of #10`, labels `wayfinder:parent:10` and `wayfinder:task`, open state, no assignees, and no open blockers. Label administrators and membership registration are trusted. Each row starts from this valid fixture unless it specifies otherwise.

| Case | Change or action | Required result |
| --- | --- | --- |
| Valid fallback | Select and claim the unchanged fixture. | Select #20 and allow the claim in the pinned repository after a fresh gate and eligibility check. |
| Unrelated issue | Add open, unassigned, unblocked #30 with no reciprocal marker or parent label. | Reject #30 before assignment or publication. Eligibility alone does not authorize membership. |
| Forged backlink | Add #30 to the map checklist and give it `Part of #10` and `wayfinder:task`, but no parent label (even if its body claims to have one). | Reject #30; never create the missing parent label from the checklist or backlink. |
| Another map's child | Add #30 with `wayfinder:parent:11`; its author changes the first body line to `Part of #10`. | Reject #30 because the independent binding is to map #11. |
| Pull request | #20 resolves to a REST record containing `pull_request`; keep the matching labels and body. | Reject it as a non-Issue record. |
| Map substitution | Remove `wayfinder:map`, use #10 as its own child, or change the selected map. | Reject the candidate; the selected map identity cannot be replaced by issue text. |
| Foreign or ambiguous reference | Replace the entry with `other/project#20`, a foreign issue URL, a `/pull/20` URL, or text containing multiple possible targets. | Reject the entry; do not operate on local #20 by extracting the trailing digits. |
| Qualified local reference | Replace the entry with `example/project#20` or `https://github.com/example/project/issues/20`. | Accept the same child identity after the other checks pass. |
| Marker removed or altered | Remove the reciprocal first line after discovery, after claim, or between comment and closure. | The next gate aborts the next and remaining writes, including closure. Previous writes do not authorize continuation. |
| Registration revoked or conflicted | Remove the parent label, change it to parent #11, or add a second parent label between any two writes. | Abort remaining writes and require trusted registration before rediscovery can admit the child. |
| Type changed or ambiguous | Remove/change the recorded type or add a second allowed type label after selection. | Reject or invalidate the selected tuple. |
| Checklist removed | Remove the child entry after claim, leaving its marker and labels intact. | Abort remaining writes because current map membership no longer matches. |
| Transfer or read failure | Move an issue, change its database ID, return incomplete data, or fail a map/child read. | Abort; cached evidence, redirects to a different identity, and partial data do not authorize writes. |
| Eligibility | Close the child, assign it to someone else, add an open blocker, or fail a blocker read before claim. | Do not claim it; preserve the existing eligibility and advisory-concurrency rules. |
| Repository context changes | Change directory or the default repository between selection and a write. | Continue using the pinned host/repository and IDs; never target the same number in the new default repository. |
| Untrusted label writer | An untrusted actor or body-driven bot can set the parent label. | Disable fallback writes; the label is no longer independent authorization. |
| Valid resolution | The claimed child retains membership and type; all reads succeed. | Gate separately before comment, gist publication, map update, and close. Preserve the idempotent map pointer and close-last ordering. |
| Native sub-issues | Use a child returned by GitHub's native `subIssues` relation with no fallback marker or parent label. | Apply native eligibility, pagination, and native command forms; do not impose fallback-only registration or use its `target_repo` variable. An unrelated checklist entry does not add a native child. |
| Incomplete native blockers | A native child's first 50 blockers are closed, but `blockedBy.pageInfo.hasNextPage` is true; a later blocker may be open. | Treat the child as ineligible. Only a complete blocker connection with no open nodes can pass the blocker filter; do not infer eligibility from the first page or `totalCount`. |

Run `git diff --check` for changes. Check shell examples for syntax, and review each claim/resolve write for a fresh membership gate. Any live consumer integration or race test must use an explicitly authorized disposable tracker; these walkthroughs make no atomicity claim.
