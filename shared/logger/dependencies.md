# Dependencies

- Runtime: нет (ни одной зависимости в `dependencies`).
- Dev: `typescript ^5.9.3`.
- Собственных `pnpm-lock.yaml` и `pnpm-workspace.yaml` нет — пакет входит в workspace монорепы, потребители получают его через `workspace:*` (корневой `overrides`).
