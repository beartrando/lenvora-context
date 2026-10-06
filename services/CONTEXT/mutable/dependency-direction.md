# Dependency direction

- **Сервисы не импортируют друг друга.** Связь между ними только через протоколы:
  gRPC (по `SERVICE_*_URL`), Kafka (топики), WebSocket.
- Сервисы → `@shared/*`. Обратного направления не существует: shared не знает о сервисах.
- Сервисы → `proto/` строго на этапе генерации кода, не рантайм-зависимость.

## Шаблон Node-сервиса (общий для всех восьми)

Точка входа и wiring — везде одинаковы:

```
src/app.ts → src/composition/build-application.ts → src/application/application.ts
```

`app.ts` импортирует **только** `composition/`. `Application` стартует сервисы в порядке:
message (Kafka) → job (pg-boss) → db (Prisma) → grpc, плюс websocket в streaming;
graceful shutdown по SIGINT/SIGTERM.

Слои:

| Слой | Содержимое |
|---|---|
| `contracts/` | только сгенерированный ts-proto код (`proto/*`, `common/*`) |
| `domain/` | сущности, value-objects, интерфейсы репозиториев; не импортирует ничего кроме domain и node |
| `application/` | `application.ts` (lifecycle), `use-cases/*`, `ports/*` |
| `infrastructure/` | адаптеры: `grpc/` (сервер, хендлеры, клиенты, мапперы), `http/`, `db/` (Prisma), `auth/`, `message/` (Kafka), `job/` (pg-boss), плюс уникальные подсистемы конкретного сервиса |
| `composition/` | единственная точка DI: собирает сервисы, репозитории, use-cases, хендлеры |
| `utils/` | мелкие помощники (`sleep.ts`) |

**Фактическое направление зависимостей** (проверено по импортам, не идеальное —
зафиксировано как есть):

- `domain` → только `domain` (+ node). Чистый.
- `application` → `domain`, `contracts`, **и конкретные классы `infrastructure`**
  (use-cases импортируют `DbService`, `JobService`, `MessageService`, Prisma-репозитории,
  gRPC-клиенты, proto-мапперы, `app/config`).
- `infrastructure` → `application` (гRPC/HTTP-хендлеры и Kafka-консьюмеры вызывают use-cases),
  `domain` (репозитории `implements` доменные интерфейсы), `contracts`.
- ⇒ связь `application ↔ infrastructure` **взаимная**. Это фактическое состояние.
- `composition` → `application` + `infrastructure`; импортируется только из `app.ts`.
- Dependency inversion реализован фрагментарно: порт `application/ports/token-service.port.ts`
  реализуется в `infrastructure/auth/service.ts`; интерфейсы репозиториев объявлены в `domain/`
  и реализуются Prisma-репозиториями. Остальные зависимости — конкретные классы.

## Межсервисная топология (фактическая)

- `front` → HTTP `gateway:9090` и WebSocket `streaming:8080` (туда же — аудио-чанки
  для распознавания речи).
- `gateway` → gRPC `orchestration.Proxy` (прокси всего HTTP-трафика); плюс health-фан-аут
  по всем `SERVICE_*_URL`.
- `orchestration` → gRPC клиенты `auth` и `profile` (через `@shared/grpc-client-manager`).
- `streaming` → gRPC `profile` (проверка профиля по токену).
- `streaming` → gRPC bidi-поток `speech-to-text` (`SERVICE_SPEECH_TO_TEXT_URL`):
  форвардинг аудио-чанков, первым сообщением идёт auth (`profile_id` + JWT), финальная
  транскрипция возвращается клиенту по WS. Клиент отдельный (не через
  `@shared/grpc-client-manager` — он unary-only).
- Kafka: `auth` produce `user.created` → `profile` consume; `profile` produce
  `profile.created`; `streaming` produce `bot.connecting-request`;
  `speech-to-text` produce `speech-to-text.transcription-ready`. Отправка в Kafka идёт
  через pg-boss → Kafka-мост (`@shared/pg-boss-manager`), а не напрямую.
- Каждый сервис регистрирует gRPC `Health` (`proto/common/health.proto`) на `:50051`.
