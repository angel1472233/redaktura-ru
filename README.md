# redaktura-ru

Навык OpenCode для редактуры русского текста: убирает канцелярит, кальки, штампы и следы нейросети, сохраняя смысл, факты и голос автора.

Форк [Vladimir-Human/humanizer-ru](https://github.com/Vladimir-Human/humanizer-ru) v4.1.6 с дополнениями из [artemiimillier/anti-ai-skill](https://github.com/artemiimillier/anti-ai-skill). Оба источника распространяются по MIT.

## Что внутри

| Путь | Назначение |
|---|---|
| `skill/redaktura-ru/SKILL.md` | точка входа: режимы, семь шагов правки, 25 признаков машинного текста, границы |
| `skill/redaktura-ru/references/` | методика: примеры по признакам, целые тексты, замер, расшифровки, приёмка |
| `skill/redaktura-ru/scripts/` | замер, сверка фактов, чистка мусора вставки |
| `skill/redaktura-ru/SOURCES.md` | пины upstream-коммитов и порядок обновления |
| `NOTICE.md` | что откуда взято и что изменено |
| `LICENSE` | MIT |

## Требования

- Python 3.8+ (проверено на 3.12), только стандартная библиотека — зависимостей нет.
- Python не обязателен: без него навык работает, но замер и сверка фактов считаются вручную.

## Установка

Скиллы OpenCode ищутся в `~/.config/opencode/skills/<id>/SKILL.md`. Ссылка на папку навыка:

```bash
ln -s ~/redaktura-ru/skill/redaktura-ru ~/.config/opencode/skills/redaktura-ru
```

Либо копия, если нужна изолированная версия:

```bash
cp -r skill/redaktura-ru ~/.config/opencode/skills/redaktura-ru
```

ID навыка — `redaktura-ru`. Он не конфликтует с upstream `humanizer-ru`: оба навыка можно держать рядом.

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

Перед выдачей правка проходит приёмку: факты и якоря на месте, ни один тезис не выброшен, усушка не больше 12 % слов. Подробности — в `skill/redaktura-ru/references/acceptance.md`.

## Скрипты

Запускать из корня репозитория.

```bash
python3 skill/redaktura-ru/scripts/diagnose.py текст.txt --json
python3 skill/redaktura-ru/scripts/check_facts.py до.txt после.txt --json
python3 skill/redaktura-ru/scripts/clean_paste.py файл.md --in-place
```

| Скрипт | Что делает |
|---|---|
| `diagnose.py` | снимает четыре числа: плотность маркеров на 1000 слов, разброс длин, долю абзацев с коротким финалом, усушку |
| `check_facts.py` | сверяет числа, даты, имена, ссылки, отрицания и модальность между исходником и правкой; код 0 — чисто, 1 — расхождения, 2 — не читается вход |
| `clean_paste.py` | снимает невидимые символы, `utm_source=chatgpt.com` и мусор вставки; с `--in-place` оставляет копию `<файл>.bak` |

У `diagnose.py` три профиля: `--profile text` (по умолчанию), `speech` и `transcript`. Числа — диагностика, а не приговор; подробнее — в `skill/redaktura-ru/references/measure.md`.

## Проверки

```bash
python3 -m py_compile skill/redaktura-ru/scripts/*.py
python3 skill/redaktura-ru/scripts/diagnose.py --selftest
```

`--selftest` проверяет, что профили идут по строгости `text → speech → transcript` и пороги не рассыпались.

## Происхождение

- База: `Vladimir-Human/humanizer-ru` v4.1.6, коммит `c3240ed1349c54465a69eed3ae84592e960779ea`.
- Дополнения: `artemiimillier/anti-ai-skill`, коммит `0e05482c2537d699685f5225557237679af07574`.

Построчные изменения и то, что сознательно не перенесено, — в [NOTICE.md](NOTICE.md). Порядок обновления — в [SOURCES.md](skill/redaktura-ru/SOURCES.md).

Навык улучшает текст для читателя. Он не обещает, что детекторы ИИ сочтут текст человеческим: они меряют другое.

## Лицензия

MIT для содержимого этого репозитория, включая унаследованные части. См. [LICENSE](LICENSE) и [NOTICE.md](NOTICE.md).
