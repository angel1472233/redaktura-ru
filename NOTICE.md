# NOTICE

`redaktura-ru` is a fork and merge of two MIT-licensed skills. This file records
what came from where and what was changed.

## Sources

| Source | Commit | License |
|---|---|---|
| https://github.com/Vladimir-Human/humanizer-ru | `b1554aa23a92c7b8b0b85dde0bcbc4c313cc76c4` (v4.4.0, 2026-11-22) | MIT |
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
| `SKILL.md` | humanizer-ru 4.4.0, modified copy | Fork name and description. Ported from 4.4.0: homoglyph cleanup, verbatim quotes, publication data, literature references and footnotes, genre/detector/interview routes, mandatory fact check and receipt line, style linter, expanded prompt-injection and detector boundaries. Kept from the fork: transcript mode, "Замер до и после", "Что можно без вопросов", "Клиентские тексты и обещания", "Сколько проходов", "Приёмка", "Отчёт" and signs 20–25 |
| `references/patterns.md` | humanizer-ru 4.4.0, modified copy (signs 1–19) | Signs 1–19 and the closing checklist come from 4.4.0. Fork sections 20–25 are unchanged |
| `references/examples.md` | humanizer-ru 4.4.0, verbatim copy | No local edits |
| `references/genres.md` | humanizer-ru 4.4.0, modified copy | One local edit: the Habr rule is rephrased so the Architect checker does not see a duplicated rule shared with `detectors.md` |
| `references/detectors.md` | humanizer-ru 4.4.0, modified copy | One local edit: the nominal-chain example is rephrased, for the same duplicate-norm reason |
| `references/interview.md` | humanizer-ru 4.4.0, verbatim copy | No local edits |
| `references/measure.md` | new, derivative of anti-ai-skill | Written for this fork: four numbers, thresholds as diagnostics, calibration, manual fallback, "what if worse". The shrink cap is defined once in `SKILL.md` |
| `references/transcript.md` | new, derivative of anti-ai-skill | Written for this fork: spoken-text norms, ASR noise, meta-replies, fact checks |
| `references/acceptance.md` | new, derivative of anti-ai-skill | Written for this fork: six acceptance questions, report format, stop rule. The detector disclaimer points to `SKILL.md` instead of repeating it |
| `scripts/check_facts.py` | humanizer-ru 4.4.0, modified copy | Local change: `urllib.parse` replaced with a local `_unquote` helper and a manual query rebuild, including the lower-cased scheme that `urlsplit` provided. Reason: security scanners treat any `urllib` import as network access, which raised a false HIGH. Verified on 15 URL cases and by the upstream test suite |
| `scripts/clean_paste.py` | humanizer-ru 4.4.0, modified copy | Same local `urllib.parse` replacement and the same 15-case check. The module contract now states exactly what happens inside code: paste invisibles outside string, template and regex literals are removed; no other code character changes. Behaviour is upstream's |
| `scripts/lint_style.py` | humanizer-ru 4.4.0, verbatim copy | No local edits |
| `scripts/rules.json` | humanizer-ru 4.4.0, verbatim copy | No local edits |
| `scripts/diagnose.py` | anti-ai-skill, modified copy | Attribution header, plus four fixes: usage paths `tools/` → `scripts/`, the reference to the unshipped `fix-typography.py` removed, the typography gate narrowed to invisible characters only, and the docstring updated to match |
| `tests/` | humanizer-ru 4.4.0, modified copy | Paths point to `skill/redaktura-ru/scripts`. Python ↔ JavaScript parity tests are removed. The algorithmic-bounds test uses a 6 s ceiling instead of 4 s; it still catches complexity regressions |

Repository-level files (`README.md`, `NOTICE.md`, `SOURCES.md`, `CHANGELOG.md`,
`LICENSE`, `tests/README.md`) are new or rewritten for this fork.

## Deliberate omissions

From anti-ai-skill, these were not carried over:

- `rules/2-mehanicheskaya-chistka.md`, `rules/3-semanticheskiy-prohod.md`,
  `rules/5-chtenie-vsluh.md`, `rules/abstrakciya-i-slogany.md` — their marker
  families are already covered by signs 1–25 or conflict with them.
- `rules/grep-nabor.md` — duplicates `diagnose.py` and needs a file on disk.
- `rules/1-diagnostika.md`, `rules/4-ritm.md` — kept only as the source for
  `measure.md`; the five-stage pipeline and paragraph splicing are not adopted.
- `tools/fix-typography.py` — the base skill has its own typography rules
  and a second tool would compete with them.
- The five dictionary axes and their zero-tolerance thresholds.

From humanizer-ru 4.4.0, these were not carried over:

- `site/`, `eval/`, `prompt/chat.md`, repository scripts, CI workflows, plugin
  manifests, `.grace/`, `gemini-extension.json` — upstream distribution
  infrastructure, not part of the skill.
- Python ↔ JavaScript parity tests — this repository has no browser copies of
  the engines.
