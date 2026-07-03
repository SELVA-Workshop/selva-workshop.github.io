# Workshop Information Update Design

## Scope

Update only `index.html` in the published `selva-workshop.github.io` repository. Do not change the untracked legacy copy at the workspace root or introduce new runtime dependencies.

## Schedule

Enable the existing schedule section and replace its template content with the finalized morning program:

- 09:00 — Opening
- 09:00–09:20 — Ruihao Gong (Online)
- 09:20–09:40 — Michele Magno (Online)
- 09:40–10:00 — Priya Panda (Online)
- 10:00–10:10 — Oral Paper 1 (Video)
- 10:10–10:20 — Oral Paper 2 (Video)
- 10:20–10:30 — Break
- 10:30–10:50 — Huanrui Yang (Video)
- 10:50–11:10 — Yukang Chen (Online)
- 11:10–11:20 — Oral Paper 3 (Video)
- 11:20–11:30 — Oral Paper 4 (Video)
- 11:30 — Awarding & Closing

Use the existing three-column table. The third column records delivery mode where applicable.

## Program Committee

Replace the partial committee list with every reviewer in `../info/ACL-SELVA 2026 Program Committee Status.csv`, except Weilun Feng. Reviewers with `num assigned Submissions > 0` appear first; reviewers with zero assignments appear second. Preserve CSV order within each group. Show names only because most CSV affiliation fields are empty.

## Speakers

Remove Kurt Keutzer's speaker card. Change the section heading from `Invited Speakers (TBD)` to `Invited Speakers` and remove tentative wording. Keep all other speaker cards unchanged.

## Verification

Use automated content assertions to confirm the schedule entries, reviewer ordering and exclusions, and speaker removal. Parse the final HTML to catch malformed markup, then inspect the Git diff to ensure no unrelated changes.
