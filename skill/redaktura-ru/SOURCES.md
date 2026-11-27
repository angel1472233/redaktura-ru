# SOURCES

How this skill was assembled and how to update it.

## Pinned versions

| Part | Source | Commit | Version |
|---|---|---|---|
| Base | `Vladimir-Human/humanizer-ru` | `b1554aa23a92c7b8b0b85dde0bcbc4c313cc76c4` | 4.4.0 |
| Additions | `artemiimillier/anti-ai-skill` | `0e05482c2537d699685f5225557237679af07574` | 2026-08-26 |

Both sources are MIT. See `LICENSE` and `NOTICE.md` in the repository root.

The skill ID is `redaktura-ru`, not `humanizer-ru`. It does not conflict with
the upstream skill: you can install both side by side.

## Language

The skill edits Russian text, so its instructions, references and examples are
written in Russian, same as the upstream base. This is a deliberate exception to
the repository rule that asks for English in new skills. The rule exists for
tool skills; here the subject matter is the Russian language, and translating
the rules would lose precision in terms such as «признак», «якорь» and
«затёртость».

## File map

| File | Origin |
|---|---|
| `SKILL.md` | humanizer-ru 4.4.0, modified: fork name, transcript mode, measurement, acceptance, report, signs 20–25 |
| `references/patterns.md` | humanizer-ru 4.4.0 (signs 1–19) plus fork sections 20–25 |
| `references/examples.md` | humanizer-ru 4.4.0, verbatim |
| `references/genres.md` | humanizer-ru 4.4.0, one local duplicate-norm edit |
| `references/detectors.md` | humanizer-ru 4.4.0, one local duplicate-norm edit |
| `references/interview.md` | humanizer-ru 4.4.0, verbatim |
| `references/measure.md` | fork: derivative of anti-ai-skill |
| `references/transcript.md` | fork: derivative of anti-ai-skill |
| `references/acceptance.md` | fork: derivative of anti-ai-skill |
| `scripts/check_facts.py` | humanizer-ru 4.4.0, local `urllib.parse` replacement |
| `scripts/clean_paste.py` | humanizer-ru 4.4.0, local `urllib.parse` replacement; code contract clarified |
| `scripts/lint_style.py` | humanizer-ru 4.4.0, verbatim |
| `scripts/rules.json` | humanizer-ru 4.4.0, verbatim |
| `scripts/diagnose.py` | anti-ai-skill, modified copy: header and four fixes |
| `tests/` | humanizer-ru 4.4.0, adapted: fork paths, no Python ↔ JS parity tests |

## How to update

1. Check the upstream commit list and tags for both repositories.
2. Diff the files in the file map against the new commit.
3. Port changes file by file. Do not copy `SKILL.md` wholesale: the fork renames
   the skill and keeps its own sections.
4. Keep the numbering of signs 1–19. Fork signs stay after 19.
5. Reapply the local `urllib.parse` replacement in `check_facts.py` and
   `clean_paste.py`. Check it on URL cases with `utm_source` and with a query
   string that must survive.
6. Keep the local duplicate-norm edits in `genres.md` and `detectors.md` if the
   upstream text returns; the Architect checker treats a long shared shingle as
   a duplicated rule.
7. Update the commit hashes here, the table in `NOTICE.md` and `CHANGELOG.md`.
8. Re-run the checks:
   - `python3 -m py_compile skill/redaktura-ru/scripts/*.py`
   - `python3 -m unittest discover -s tests -v`
   - `python3 skill/redaktura-ru/scripts/diagnose.py --selftest`
   - `quick_validate.py`, `eval_skill.py` and `check_skill.py` from the
     `make-skill` environment
   - `skillspector scan --no-llm` and the full scan through the project adapter

## What not to port from anti-ai

The five dictionary axes, their zero thresholds, the grep command set, the
five-stage pipeline and the paragraph-splicing pass. Reasons are in `NOTICE.md`.

## What not to port from the base

The fork keeps its own process sections: transcript mode, measurement,
acceptance, report and signs 20–25. Upstream prose changes inside the seven
steps, the fact rules and the boundaries are ported.

These parts of the upstream repository are out of scope: `site/`, `eval/`,
`prompt/chat.md`, repository scripts (`scripts/build_*`, `scripts/check_*`),
CI workflows, plugin manifests, `.grace/`, `gemini-extension.json` and the
Python ↔ JavaScript parity tests. They belong to the upstream distribution,
not to this skill.

## License

MIT. The full text is in `LICENSE` at the repository root; provenance and the
degree of copying are in `NOTICE.md`.

- `Copyright (c) 2026 redaktura-ru contributors`
- `Copyright (c) 2026 Vladimir` (humanizer-ru)
- `Copyright (c) 2026 anti-ai-skill contributors` (anti-ai-skill)
