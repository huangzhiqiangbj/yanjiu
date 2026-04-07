# Changelog

## Setup

**Version name:** `k12-ai-education-frontier-weekly-report-v2`
**Runs per experiment:** `5`
**Run interval:** `every 2 minutes`
**Budget cap:** `none`
**Anchor prompts:** comprehensive readers, officials, principals
**Fixed input pack:** `inputs/input-1-verified-week-pack-20260312-20260320.md`
**Notes:** The original `SKILL.md` remains untouched. All mutations will be applied only to the working copy in this directory. The harness reuses a frozen March 12-20, 2026 research pack to reduce live-search noise during evaluation.

## Eval Suite Reset

**Reason:** The first three-eval baseline saturated at `15/15`, so it could no longer distinguish prompt quality.
**Change:** Tightened the rubric to `4` evals (`20` points max), adding exact title/date compliance and stronger brief-style enforcement while keeping audience bias and evidence-boundary checks.
**Expected impact:** The reset should expose whether the skill keeps reader customization inside the content rather than drifting the report title and date format.

## Experiment 0 - baseline

**Score:** 15/15 (100.0%)
**Change:** No prompt changes. Measured the original skill copy against the frozen March 12-20, 2026 research pack across five audience-rotated runs.
**Reasoning:** Establish a stable baseline before attempting any mutations.
**Result:** All five runs passed all three evals. The current skill consistently preserved the five-part structure, tailored emphasis to officials vs principals vs mixed readers, and stayed within the fixed evidence/time-window boundary.
**Failing outputs:** None at the rubric level. The main watch-out is harness saturation: because the frozen input pack is already highly structured, the current three-eval suite may be too easy to expose finer prompt weaknesses.

## Experiment 0 - tightened baseline

**Score:** 16/20 (80.0%)
**Change:** No prompt changes. Reran the original working copy against the same frozen research pack after tightening the eval suite from `3` to `4` checks.
**Reasoning:** The original harness saturated at `15/15`, so the baseline needed a stricter rubric before further prompt mutations could produce measurable gains.
**Result:** The skill still passed reader bias, brief-style execution, and evidence-boundary checks in all five runs, but only `1/5` runs passed the new exact title/date compliance check. The main failure pattern was no longer content quality; it was formal drift when the user prompt itself contained years or governance-oriented wording.
**Failing outputs:** Four of five runs drifted on the formal header: some rewrote the report title into governance/principal variants, and several preserved the prompt's full-year date format instead of converting it back to the skill's default month-day line.

## Experiment 1 - keep

**Score:** 17/20 (85.0%)
**Change:** Added an explicit output-skeleton rule saying reader customization must stay inside the content, while the formal report title remains `# K-12教育科技与AI前沿周报` and the default date line should be month-day only.
**Reasoning:** The tightened baseline showed that the weakest behavior was formal drift in title and date formatting, not the body content itself. A high-priority skeleton rule should be the smallest targeted fix.
**Result:** Score improved from `16/20` to `17/20`. The title drift largely disappeared, which indicates the new rule is being followed, but the model still often copies full-year dates from the user's prompt into the date line.
**Failing outputs:** The remaining misses were concentrated in the date line. Three of five runs still wrote `2026年3月12日-2026年3月20日` or similar, despite the new month-day rule.

## Experiment 2 - keep

**Score:** 20/20 (100.0%)
**Change:** Added an explicit normalization example stating that even if the user prompt says `2026年3月12日-2026年3月20日`, the final date line should still be rewritten as `3月12日-3月20日` unless the user explicitly asks to keep the year.
**Reasoning:** Experiment 1 showed that the remaining failure was not conceptual misunderstanding but prompt copying. A direct example and anti-copy instruction should be more effective than a generic rule.
**Result:** Score improved from `17/20` to `20/20`. All five runs passed the tightened rubric, which confirms that the strongest remaining failure mode was the year-bearing date line rather than any deeper weakness in reader bias, brief style, or evidence discipline.
**Failing outputs:** None at the rubric level. Residual watch-outs were minor wording differences only.

## Experiment 3 - discard

**Score:** 20/20 (100.0%)
**Change:** Repeated the now-working title/date rule inside the delivery checklist under `质量检查`.
**Reasoning:** This tested whether duplicating the instruction in a second high-priority section would further harden the behavior.
**Result:** Score stayed perfect, which means the extra checklist copy did not improve the measured output quality. The experiment confirmed the core date-normalization example from experiment 2 is doing the real work; the repeated checklist wording only adds prompt length.
**Failing outputs:** None at the rubric level. The added rule was reverted to keep the working copy simpler.

## Experiment 4 - discard

**Score:** 20/20 (100.0%)
**Change:** Added a style-level anti-pattern telling the skill not to copy a year-bearing time window from the prompt into the date line.
**Reasoning:** This tested whether a second phrasing style could harden the same behavior without changing the kept rule set.
**Result:** Score stayed perfect, so the anti-pattern did not add measurable value beyond experiment 2. This gave three consecutive `95%+` experiments (`2`, `3`, `4`), which is enough to stop the loop on diminishing-return grounds.
**Failing outputs:** None at the rubric level. The added anti-pattern was reverted to keep the working copy lean.

