# Entry points

- gRPC `:50051` — bidi-поток распознавания + `Health` (только внутренняя сеть).
- Единственный вызывающий — `streaming`: front шлёт чанки в WS `streaming:8080`,
  streaming форвардит их сюда по gRPC. HTTP-маршрутов через gateway нет
  (gateway/orchestration проксируют только unary-JSON).
- `src/app.ts`.
