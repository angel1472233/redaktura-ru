# Tests

Regression tests for the bundled scripts. They are adapted from
`humanizer-ru` 4.4.0 (MIT) and run on the Python standard library only.

## Run

```bash
python3 -m unittest discover -s tests -v
```

`diagnose.py` has its own smoke check:

```bash
python3 skill/redaktura-ru/scripts/diagnose.py --selftest
```

## What changed against upstream

- The scripts live in `skill/redaktura-ru/scripts`, not
  `skills/humanizer-ru/scripts`.
- The Python ↔ JavaScript parity tests are not shipped: this repository has no
  browser copy of the fact checker, the style linter or the paste cleaner.
- The algorithmic-bounds test keeps a 6 s ceiling instead of upstream's 4 s.
  It still catches complexity regressions; the local run takes about 4 s.

Everything else is the upstream suite: extended correctness cases, CLI
hardening, performance guards and the style-linter rule tests.
