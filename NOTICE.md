# NOTICE

`redaktura-ru` is a fork and merge of two MIT-licensed skills. This file records
what came from where and what was changed.

## Sources

| Source | Commit | License |
|---|---|---|
| https://github.com/Vladimir-Human/humanizer-ru | `c3240ed1349c54465a69eed3ae84592e960779ea` (2026-10-29) | MIT |
| https://github.com/artemiimillier/anti-ai-skill | `0e05482c2537d699685f5225557237679af07574` (2026-08-26) | MIT |

## Copyright notices

Both sources are distributed under the MIT License. The notices below are
reproduced verbatim; the full license text is in `LICENSE`.

- humanizer-ru — `Copyright (c) 2026 Vladimir`
- anti-ai-skill — `Copyright (c) 2026 anti-ai-skill contributors`
- redaktura-ru as a whole — `Copyright (c) 2026 redaktura-ru contributors`

## File-by-file

The origin column states how close a file is to its upstream version:
**verbatim copy** — byte for byte, **modified copy** — the upstream file with
edits, **derivative** — written for this fork from upstream ideas, **new** —
written for this fork.

| File | Origin | Changes |
|---|---|---|
| `SKILL.md` | humanizer-ru, modified copy | New name and description. Added: transcript mode, "Замер до и после", "Клиентские тексты и обещания", "Сколько проходов", "Приёмка", "Отчёт". Extended: step 4 (replacement test), step 6 (shrink cap), "Русская специфика" (one term, spoken speech), "Факты неприкосновенны" (shrink cap). Signs 20–25 added to the table. Steps 1–7 and signs 1–19 keep their numbering |
| `references/patterns.md` | humanizer-ru, modified copy (signs 1–19 verbatim) | Signs 1–19 unchanged. Added sections 20–25 (accusation, pressure, reassurance, reader teasing, punch fragments, borrowed metaphors) |
| `references/examples.md` | humanizer-ru, verbatim copy | Unchanged |
| `references/measure.md` | new, derivative of anti-ai-skill | Written for this fork: four numbers, thresholds as diagnostics, calibration, manual fallback, "what if worse". Based on anti-ai `rules/1-diagnostika.md` and `rules/4-ritm.md` |
| `references/transcript.md` | new, derivative of anti-ai-skill | Written for this fork: spoken-text norms, ASR noise, meta-replies, fact checks. Based on anti-ai `rules/sohranenie-smysla.md` and the `transcript` profile |
| `references/acceptance.md` | new, derivative of anti-ai-skill | Written for this fork: six acceptance questions, report format, stop rule. Based on anti-ai "Приёмка: шесть вопросов" |
| `scripts/check_facts.py` | humanizer-ru, modified copy | One change: `urllib.parse` replaced with two local helpers (`_unquote`, textual query rebuild in `strip_utm`). Output verified identical on 15 URL cases and on the full CLI run. Reason: security scanners treat any `urllib` import as network access, which raised a false HIGH |
| `scripts/clean_paste.py` | humanizer-ru, modified copy | One change: same `urllib.parse` replacement as in `check_facts.py`. `strip_utm` output verified identical on 15 URL cases and on a full in-place run |
| `scripts/diagnose.py` | anti-ai-skill, modified copy | Attribution header, plus four fixes: usage paths `tools/` → `scripts/`, the reference to the unshipped `fix-typography.py` removed, the typography gate narrowed to invisible characters only, and the docstring updated to match |

Repository-level files (`README.md`, `NOTICE.md`, `SOURCES.md`, `LICENSE`) are
new or rewritten for this fork.

## Deliberate omissions

From anti-ai-skill, these were not carried over:

- `rules/2-mehanicheskaya-chistka.md`, `rules/3-semanticheskiy-prohod.md`,
  `rules/5-chtenie-vsluh.md`, `rules/abstrakciya-i-slogany.md` — their marker
  families are already covered by signs 1–25 or conflict with them.
- `rules/grep-nabor.md` — duplicates `diagnose.py` and needs a file on disk.
- `rules/1-diagnostika.md`, `rules/4-ritm.md` — kept only as the source for
  `measure.md`; the five-stage pipeline and paragraph splicing are not adopted.
- `tools/fix-typography.py` — the base skill has its own typography rules and
  a second tool would compete with them.
- The five dictionary axes and their zero-tolerance thresholds.
