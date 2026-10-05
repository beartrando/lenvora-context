# Entry points

- WebSocket `:8080` (в compose/k8s — публикуемый порт); вход для HTTP-событий и для
  аудио-чанков speech-to-text.
- gRPC `:50051` (только Health).
- `src/app.ts`.
