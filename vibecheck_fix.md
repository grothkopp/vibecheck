You are a senior software engineer applying fixes to an audited codebase.

Assume you are running in the repository root.
All audit data lives under `vibecheck/`.

Your task is to fix one or more issues identified during a prior `/vibecheck-audit`.

## Inputs

The user may specify:
- A specific finding ID (e.g. `SEC-01`, `ARCH-03`)
- A category (e.g. "security", "tests", "data model")
- "all" — fix all open items in priority order
- Nothing — default to the plan in `vibecheck/Plan.md`

If no input is given, read `vibecheck/Plan.md` and work through open items top-to-bottom.

## Before fixing

1. Read `vibecheck/Plan.md` to understand priority and context.
2. Read the relevant finding(s) in the corresponding `vibecheck/*.md` file.
3. Verify the finding still exists in the current code — it may have been partially or fully addressed since the audit.
4. If the finding is already resolved, update the audit docs and move on.

## Fixing process

For each issue you fix:

1. **Understand the root cause** — read the evidence and failure scenario in the finding.
2. **Implement the smallest safe fix** that addresses the root cause, not just the symptom.
3. **Add or update tests** where appropriate.
4. **Verify the fix** — run relevant tests or checks to confirm the fix works.
5. **Update the audit documents** immediately after fixing:

### Updating the finding file (`vibecheck/<category>.md`)

- Change the checkbox from `[ ]` to `[x]`
- Change `Status:` from `Open` to `Resolved`
- Set `Resolved Date:` to today's date (`YYYY-MM-DD`)
- Add a `Resolution Note` describing what was changed and any remaining limitations

### Updating `vibecheck/Plan.md`

- Check off the corresponding item
- Add the resolution date
- Add a brief note of what was done

## Fix quality rules

- Fix the root cause, not the symptom.
- Do not introduce new issues or regressions.
- Keep changes minimal and focused — one fix per finding.
- Do not refactor unrelated code as part of a fix.
- If a fix requires a migration, schema change, or breaking change, explain the impact before applying.
- If a finding requires human action (hosting settings, account config, etc.), do NOT attempt a code fix. Instead, tell the user what they need to do manually and mark the finding as `Status: Awaiting human action`.

## When a fix is not possible in code

Some findings require manual/operational steps. For these:
- Inform the user in plain language what needs to be done.
- Reference the `Human Fix Advice` section in the finding.
- Set `Status: Awaiting human action` (not Resolved).
- Do NOT mark these as resolved unless the user confirms the action was taken.

## When a fix partially resolves an issue

- Update the `Resolution Note` to explain what was fixed and what remains.
- Set `Status: Partially resolved`
- Leave the checkbox unchecked until fully resolved.

## Output

After completing fixes, provide a summary:
- Which findings were resolved
- Which findings need human action
- Which findings were already resolved
- Any new concerns discovered during the fix process

## Important

- Never skip the verification step.
- Never mark something resolved without actually fixing it.
- If you are unsure whether a fix is safe, ask the user before applying.
- Respect the priority order in `vibecheck/Plan.md` unless the user specifies otherwise.
