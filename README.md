# cc-skills

Скиллы и правила для Claude Code, не привязанные к языку программирования и конкретному проекту.
Всё проектное (команды сборки и тестов, формат ID задач, доступ к БД, эталонные классы) скиллы
берут из `CLAUDE.md` проекта — шаблон в [`templates/CLAUDE.md`](templates/CLAUDE.md).

## Скиллы

| Скилл | Назначение |
|-------|------------|
| [`skill-orchestrator`](skills/skill-orchestrator/SKILL.md) | Обязательная проверка скиллов перед любым действием, порядок работы над фичей |
| [`write-spec`](skills/write-spec/SKILL.md) | Брейнсторминг и дизайн до реализации → `ai/<TASK-ID>/spec.md` |
| [`write-plan`](skills/write-plan/SKILL.md) | План реализации мелкими TDD-шагами → `ai/<TASK-ID>/plan.md` |
| [`execute-plan`](skills/execute-plan/SKILL.md) | Выполнение плана с контрольными точками |
| [`next-stage-prompt`](skills/next-stage-prompt/SKILL.md) | Промпт для следующего этапа в чистом контексте |
| [`write-test`](skills/write-test/SKILL.md) | Написание тестов для нового / изменённого кода |
| [`audit-tests`](skills/audit-tests/SKILL.md) | Аудит существующих тестов (только ручной вызов) |
| [`code-review`](skills/code-review/SKILL.md) | Ревью изменений: безопасность, качество, стиль, тесты |
| [`debugging`](skills/debugging/SKILL.md) | Систематическая отладка: корневая причина до фикса |
| [`verification-before-completion`](skills/verification-before-completion/SKILL.md) | Доказательства до заявлений о готовности |
| [`update-claude-md`](skills/update-claude-md/SKILL.md) | Актуализация `CLAUDE.md` под состояние кода |

Процесс работы над фичей:

```
write-spec → write-plan → execute-plan (write-test, code-review, verification-before-completion)
```

## Правила

| Правило | Когда загружается |
|---------|-------------------|
| [`code-style.md`](rules/code-style.md) | всегда |
| [`database.md`](rules/database.md) | всегда |
| [`api-endpoints.md`](rules/api-endpoints.md) | при работе с файлами эндпоинтов / контроллеров |
| [`repositories.md`](rules/repositories.md) | при работе с репозиториями / адаптерами |
| [`test-style.md`](rules/test-style.md) | при работе с тестами |

Условная загрузка задаётся `paths` во frontmatter правила. Глобы рассчитаны на типовые раскладки
(`controller/`, `*Repository*`, `*_test.*`, `*.spec.*` и т.п.) — поправь их под свой проект.

## Установка

Для всех проектов пользователя:

```bash
mkdir -p ~/.claude/skills ~/.claude/rules
ln -s "$PWD"/skills/* ~/.claude/skills/
ln -s "$PWD"/rules/*.md ~/.claude/rules/
```

Для одного проекта — то же самое в `<проект>/.claude/skills/` и `<проект>/.claude/rules/`.

Затем скопируй [`templates/CLAUDE.md`](templates/CLAUDE.md) в корень проекта и заполни: 
- формат ID задачи, 
- команды проверки («Проверка задачи перед завершением»),
- способ доступа к БД,
- защищённые тесты (которые нельзя менять агенту),
- эталонные классы (конкретные классы вашего проекта, на которые правило ссылается как на образец правильной реализации: «сделай так же, как здесь»).

Заполнение большей части CLAUDE.md можно поручить агенту.