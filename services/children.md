# Children

- [auth](auth/purpose.md) — gRPC-сервис аутентификации: анонимный вход, выдача/ротация JWT, users/sessions/devices в PostgreSQL.
- [profile](profile/purpose.md) — gRPC-сервис профилей: профиль игрока и достижения, создаётся по Kafka-событию `user.created`.
- [gateway](gateway/purpose.md) — единственная HTTP-точка входа (:9090): health-агрегация по всем сервисам и проксирование запросов в orchestration по gRPC.
- [orchestration](orchestration/purpose.md) — gRPC-роутер `Proxy`: HTTP-поверхность API (auth/profile), JWT-авторизация, вызовы auth/profile (клиент gusteau объявлен, но не используется).
- [streaming](streaming/purpose.md) — WebSocket-сервис реального времени (:8080) + gRPC-health: голосовые сессии и WS-события, gRPC-клиенты profile и speech-to-text.
- [gusteau](gusteau/purpose.md) — gRPC-сервис нутрициологии: продукты, продуктовые логи, нутрициональные коэффициенты.
- [remy](remy/purpose.md) — gRPC-сервис AI-ассистента (OpenRouter через SDK `openai`): `Remy.Process(text) → text`; сейчас use-case возвращает эхо, AI-узел в wiring не подключён.
- [speech-to-text](speech-to-text/purpose.md) — bidi-gRPC-сервис распознавания речи: `SpeechToText.Transcribe(stream)`; фактически зарегистрирован только health, bidi-обработчик — каркас.
- [front](front/purpose.md) — Flutter-приложение (mobile/web): features/domain/infrastructure, ходит в API через gateway.

`postgres/` и `pgbouncer/` — не сервисы: только конфиги и данные для docker-compose (тома, userlist, pgbouncer.ini). Их контекстов нет.
