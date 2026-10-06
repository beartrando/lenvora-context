# Entry points

- gRPC `:50051`: `remy.Remy/Process`, `Health/check|status|livez|readyz`.
- `src/app.ts` → `composition/build-application.ts` → `Application.start()`.
- Вызывает `Process` пока никто; gateway знает remy только через
  `SERVICE_REMY_URL` для health-агрегата.
