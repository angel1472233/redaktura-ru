# redaktura-ru

Навык редактуры русского текста для OpenCode и Codex: убирает канцелярит,
кальки, штампы и следы нейросети, сохраняя смысл, факты и голос автора.

Форк [Vladimir-Human/humanizer-ru](https://github.com/Vladimir-Human/humanizer-ru)
v4.4.0 с дополнениями из
[artemiimillier/anti-ai-skill](https://github.com/artemiimillier/anti-ai-skill).
Оба источника распространяются по MIT.

## Что внутри

| Путь | Назначение |
|---|---|
| `skill/redaktura-ru/SKILL.md` | точка входа: режимы, семь шагов правки, 25 признаков машинного текста, границы |
| `skill/redaktura-ru/references/patterns.md` | примеры «было → стало» и границы для каждого признака |
| `skill/redaktura-ru/references/examples.md` | целые тексты разных жанров до и после правки |
| `skill/redaktura-ru/references/genres.md` | пост, карточка товара, вакансия, письмо, ВКР и научная статья |
| `skill/redaktura-ru/references/detectors.md` | «Антиплагиат», GigaCheck, Text.ru, GPTZero: что снижает риск и чего делать нельзя |
| `skill/redaktura-ru/references/interview.md` | как задавать автору до трёх вопросов, если в тексте нет фактуры |
| `skill/redaktura-ru/references/measure.md` | четыре числа замера и ручной расчёт без Python |
| `skill/redaktura-ru/references/transcript.md` | расшифровки, надиктовки, эфиры |
| `skill/redaktura-ru/references/acceptance.md` | шесть вопросов приёмки и формат отчёта |
| `skill/redaktura-ru/scripts/diagnose.py` | замер: плотность маркеров, разброс длин, короткие финалы, усушка |
| `skill/redaktura-ru/scripts/check_facts.py` | сверка чисел, дат, имён, цитат, ссылок, отрицаний и модальности |
| `skill/redaktura-ru/scripts/clean_paste.py` | невидимые символы, метки чатов, `utm_source`, буквы-двойники |
| `skill/redaktura-ru/scripts/lint_style.py` | линтер выживших штампов, ритма и технических следов |
| `skill/redaktura-ru/scripts/rules.json` | правила для `lint_style.py` |
| `tests/` | регрессионные тесты скриптов, адаптированные из upstream |
| `SOURCES.md` | пины upstream-коммитов и порядок обновления |
| `NOTICE.md` | что откуда взято и что изменено |
| `CHANGELOG.md` | история версий форка |
| `LICENSE` | MIT |

## Требования

- Python 3.8+ (проверено на 3.12), только стандартная библиотека — зависимостей нет.
- Python не обязателен: без него навык работает, но замер, сверка фактов и
  линтер считаются вручную.

## Установка

OpenCode ищет навыки в `~/.config/opencode/skills/<id>/SKILL.md`. Ссылка на
папку навыка:

```bash
ln -s ~/redaktura-ru/skill/redaktura-ru ~/.config/opencode/skills/redaktura-ru
```

Codex ищет навыки в `~/.codex/skills/<id>/SKILL.md`:

```bash
ln -s ~/redaktura-ru/skill/redaktura-ru ~/.codex/skills/redaktura-ru
```

Либо копия, если нужна изолированная версия:

```bash
cp -r skill/redaktura-ru ~/.codex/skills/redaktura-ru
```

ID навыка — `redaktura-ru`. Он не конфликтует с upstream `humanizer-ru`: оба
навыка можно держать рядом.

## Как пользоваться

Попросите отредактировать русский текст. Режим выбирается по формулировке:

| Запрос | Режим |
|---|---|
| «очеловечь», «перепиши», «убери канцелярит» | правка: переписать и вернуть готовый текст |
| «проверь», «что выдаёт ИИ» | разбор без правки |
| «убери только тире», «сократи на треть» | точечная правка |
| путь к файлу, «поправь README» | файл |
| «почисти расшифровку», «голосовое» | расшифровка |

Навык только для русского языка. Не пишет текст с нуля и не переводит.

Перед выдачей правка проходит приёмку: факты и якоря на месте, ни один тезис
не выброшен, усушка не больше 12 % слов. Подробности — в
`skill/redaktura-ru/references/acceptance.md`.

## Скрипты

Запускать из корня репозитория.

```bash
python3 skill/redaktura-ru/scripts/diagnose.py текст.txt --json
python3 skill/redaktura-ru/scripts/check_facts.py до.txt после.txt --json
python3 skill/redaktura-ru/scripts/clean_paste.py файл.md --in-place
python3 skill/redaktura-ru/scripts/lint_style.py текст.txt --json
```

| Скрипт | Что делает |
|---|---|
| `diagnose.py` | снимает четыре числа: плотность маркеров на 1000 слов, разброс длин, долю абзацев с коротким финалом, усушку |
| `check_facts.py` | сверяет числа, даты, имена, цитаты, ссылки, отрицания и модальность; код 0 — чисто, 1 — расхождения, 2 — не читается вход |
| `clean_paste.py` | снимает невидимые символы, метки чатов, `utm_source` и буквы-двойники; с `--in-place` оставляет копию `<файл>.bak` |
| `lint_style.py` | ищет выжившие штампы, ритм метронома и технические следы; `--max-score` включает код 1 при превышении порога |

У `diagnose.py` три профиля: `--profile text` (по умолчанию), `speech` и
`transcript`. Числа — диагностика, а не приговор; подробнее — в
`skill/redaktura-ru/references/measure.md`.

`clean_paste.py` умеет `--check`, `--json`, `--no-backup` и `--exit-zero`.
Внутри блоков кода он снимает только невидимые символы вставки вне строковых,
шаблонных и регулярных литералов; остальной код не меняет.
`check_facts.py` принимает `-` для чтения исходника из stdin.

## Проверки

```bash
python3 -m py_compile skill/redaktura-ru/scripts/*.py
python3 -m unittest discover -s tests -v
python3 skill/redaktura-ru/scripts/diagnose.py --selftest
```

`--selftest` проверяет, что профили идут по строгости
`text → speech → transcript` и пороги не рассыпались.

Тесты адаптированы из upstream 4.4.0. Тесты паритета Python ↔ JavaScript не
переносятся: в этом репозитории нет браузерной копии движков. Отличия описаны
в `tests/README.md`.

## Происхождение

- База: `Vladimir-Human/humanizer-ru` v4.4.0, коммит
  `b1554aa23a92c7b8b0b85dde0bcbc4c313cc76c4`.
- Дополнения: `artemiimillier/anti-ai-skill`, коммит
  `0e05482c2537d699685f5225557237679af07574`.

Построчные изменения и то, что сознательно не перенесено, — в
[NOTICE.md](NOTICE.md). Порядок обновления — в
[SOURCES.md](skill/redaktura-ru/SOURCES.md).

Навык улучшает текст для читателя. Он не обещает, что детекторы ИИ сочтут
текст человеческим: они меряют другое.

## Лицензия

MIT для содержимого этого репозитория, включая унаследованные части. См.
[LICENSE](LICENSE) и [NOTICE.md](NOTICE.md).
