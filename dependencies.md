# Dependencies

Внешние:

- Docker + docker compose — запуск и сборка стека;
- pnpm (`packageManager: pnpm@11.28.4`), turbo — workspace и задачи;
- Node.js, TypeScript (общий `tsconfig.base.json`);
- git + submodules — все подпроекты;
- protoc, ts-proto — генерация из proto;
- GitHub Actions — CI/CD.

Внутренние:

- подмодули как workspace-пакеты: [services/](services/purpose.md), [shared/](shared/purpose.md), [proto/](proto/purpose.md);
- Prisma, Jest, Dockerfile'ы — на стороне сервисов.

Корень не импортируется как модуль — собственного исполняемого кода нет.
