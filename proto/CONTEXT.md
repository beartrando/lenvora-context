# Proto — контракты сервисов (gRPC / protobuf)

## Purpose

Единственный источник определений RPC и сообщений для всего проекта: git-submodule
`lenvora-proto` с `.proto`-файлами. Отсюда генерируется и TypeScript-код для сервисов,
и Dart-код для клиента.

## Abstraction level

Уровень контрактов — сосед `services/` и `shared/` (все три — дети корня). Ниже него
находятся только сгенерированные файлы в `src/contracts/proto/*` каждого пакета;
собственных слоёв контракты не имеют.

## Parent

[../CONTEXT.md](../CONTEXT.md) — владеет сабмодулем (`git submodule update`),
оркестрацией codegen (`make proto-generate`) и тем, что контракты потребляются
всеми детьми корня.

## Children

Нет дочерних CONTEXT.md. Структура файлов — в разделе Structure; сгенерированный
результат живёт в каталогах `src/contracts/proto/` пакетов-потребителей, а не здесь.

## Responsibility

- Определения сервисов и сообщений: `auth.proto`, `profile.proto`, `orchestration.proto`,
  `gusteau.proto`, `streaming.proto` + общие `common/{error,empty,response,key_value,ping,health}.proto`.
- Версионирование контрактов (собственный репозиторий `lenvora-proto`, ветка `master`).

## Non-responsibility

- Генерируемый код (живёт в потребителях: `services/*/src/contracts/proto/`,
  `shared/errors/src/contracts/proto/`, `services/front/lib/contracts/proto/`).
- Реализация RPC, транспорт, адреса и настройки — это `services/`.
- Это **не pnpm-пакет**: в `pnpm-workspace.yaml` не объявлен, `package.json` нет,
  рантайм-зависимости от него ни у кого нет.

## Dependencies

Нет. Каталог — пассивный набор файлов, читаемых `protoc` по пути `--proto_path`.

## Dependency direction

Схема потребления:

- `services/*` → здесь **на этапе codegen**: `pnpm <svc> proto:generate`
  (protoc + ts-proto, `outputServices=grpc-js`) → `src/contracts/proto/`.
- `shared/errors` → здесь на codegen (только `common/error.proto`, `outputServices=none`).
- `services/front` → здесь на codegen (protoc `--dart_out=grpc`) в `lib/contracts/proto/`.
- Docker: каждый Dockerfile копирует `proto/` и запускает codegen на стадии `deps`.
- Обратных рантайм-зависимостей нет; правка `.proto` не ломает компиляцию, пока
  codegen не перезапущен.

## Entry points

- `make proto-generate` (корневой Makefile) — стирает и перегенерирует `src/contracts/proto`
  у всех `NODE_SERVICES`, `shared/errors` и делает Dart-codegen для `front`.
- `pnpm --filter <svc> proto:generate` — точечно для одного пакета.

## Structure

```
proto/
├── auth.proto           # service Auth: Logout, RefreshTokens, AnonymousSignIn, Get
├── profile.proto        # service Profile: GetByUser, Get, GetOrCreate (использует auth.UserIdRequest)
├── orchestration.proto  # service Orchestration: Proxy(ProxyRequest)→ProxyResponse
├── gusteau.proto        # service Gusteau: 6 RPC (products/nutrition/food-log)
│                        # + enum EventName, message EventObject (событийная модель)
├── streaming.proto      # service отсутствует: только EventRequest/EventResponse
│                        # (oneof: gusteau.EventObject | empty | error)
└── common/
    ├── health.proto     # service Health: Check, Status, Livez, Readyz
    ├── error.proto      # ErrorObject + ErrorCode (база для @shared/errors)
    ├── empty.proto, response.proto, key_value.proto, ping.proto
```

## Architectural rules

- **Добавление RPC**: править файл в этом сабмодуле → закоммитить в `lenvora-proto`
  → перегенерировать потребителей (`make proto-generate`) → сгенерированный код
  коммитится в репозиторий потребителя (он не в .gitignore).
- Сабмодули обновляются вручную/через Makefile и отдельным шагом CI
  (`build-and-push.yml`: «Update proto submodule»).
- Фактические несоответствия: `remy` и `speech-to-text` не имеют собственных `.proto`
  (поэтому их бизнес-RPC незарегистрированы); `streaming.proto` не определяет сервис;
  сгенерированный набор в каждом сервисе **полный** (все 5 сервисов + common), хотя
  используется подмножество.
- Мёртвые ссылки на несуществующие контракты: `gateway/src/healthcheck.js`
  (импортирует `contracts/proto/gateway`), `battle.test.ts` в remy/gusteau/speech-to-text
  (импортирует `contracts/proto/battle`).

## Git workflow

- Коммиты делаем в ветку `feature` (не в `master` напрямую).
- Пушим в `origin`: `git push origin feature`.
- Пул-реквест через `gh` из `feature` в `master`
  (default-ветка proto — `master`, CI шаг «Update proto submodule» тянет именно её):
  `gh pr create --base master --head feature --title "..." --body "..."`.

## Important files

- `common/error.proto` — коды ошибок, на которых построен `@shared/errors`.
- `common/health.proto` — контракт health-эндпоинтов всех сервисов и gateway-агрегатора.
- `gusteau.proto` (`EventObject`) — событийная модель для streaming.

## Navigation

- Кто реализует конкретный RPC → `services/<svc>/src/infrastructure/grpc/handlers/`.
- Где лежит сгенерированный Dart → `services/front/lib/contracts/proto/` (без отдельного
  контекста; см. [../services/front/](../services/front/CONTEXT.md)).
