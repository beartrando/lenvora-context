# Architectural rules

- Контракт — [proto](../../proto/purpose.md)/`speech-to-text.proto`; сервис не дублирует типы руками, код берётся из `src/contracts/proto` (`pnpm proto:generate`).
- Авторизации в контракте нет: JWT и принадлежность профиля проверяет вызывающая сторона; `profile_id` обязателен в `audio_start`.
- Ответ потока — только `oneof output { transcription | error }`, иных сообщений контракт не предполагает.
- Состояние потоков — только в памяти процесса; БД для транскрипций не используется (Prisma datasource без моделей).
- bidi-обработчик регистрируется в `GrpcService.registerSpeechToTextService()`; сейчас метод пуст — регистрация ограничена health.
- Внешний AI-провайдер подключается отдельным infrastructure-клиентом; в зависимостях и env провайдера сейчас нет (SDK распознавания не объявлен, ключей в `.env.dist` нет).
