# SOURCES

How this skill was assembled and how to update it.

## Pinned versions

| Part | Source | Commit | Version |
|---|---|---|---|
| Base | `Vladimir-Human/humanizer-ru` | `c3240ed1349c54465a69eed3ae84592e960779ea` | 4.1.6 |
| Additions | `artemiimillier/anti-ai-skill` | `0e05482c2537d699685f5225557237679af07574` | 2026-08-26 |

Both sources are MIT. See the repository `LICENSE` and `NOTICE.md`.

The skill ID is `redaktura-ru`, not `humanizer-ru`. It does not conflict with
the upstream skill: you can install both side by side.

## Language

The skill edits Russian text, so its instructions, references and examples are
written in Russian, same as the upstream base. This is a deliberate exception to
the repository rule that asks for English in new skills. The rule exists for
tool skills; here the subject matter is the Russian language, and translating
the rules would lose precision in terms such as «признак», «якорь» and
«затёртость».

## How to update

1. Check the upstream commit list for both repositories.
2. Diff the files listed in `NOTICE.md` against the new commit.
3. Port changes into the fork file by file. Do not copy `SKILL.md` wholesale:
   the fork renames the skill and adds sections.
4. Keep the numbering of signs 1–19. New signs go after 19.
5. Update the commit hashes in this file and the change table in `NOTICE.md`.
6. Re-run the checks:
   - `python3 -m py_compile scripts/*.py`
   - `python3 scripts/diagnose.py --selftest`
   - `quick_validate.py` and `eval_skill.py` from `skill-conductor`
   - `skillspector scan --no-llm`

## What not to port from anti-ai

The five dictionary axes, their zero thresholds, the grep command set, the
five-stage pipeline and the paragraph-splicing pass. Reasons are in `NOTICE.md`
and in the merge plan.

## What not to port from the base

Nothing. The base is the source of truth for facts, signs 1–19 and the seven
editing steps.
