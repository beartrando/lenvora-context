# Structure

```
services/
├── <svc>/                 # git-submodule отдельного сервиса
│   ├── src/               # слои по шаблону выше
│   ├── __tests__/         # тесты (в части сервисов; см. jest testMatch)
│   ├── package.json       # deps: @shared/* workspace:*, prisma, kafkajs, pg-boss …
│   ├── Dockerfile         # multi-stage: base → deps → dev | build → prod + predeploy
│   ├── .env.dist          # шаблон env (compose копирует в .env)
│   └── jest.config.js, tsconfig*.json
├── front/                 # Flutter: lib/, web/, android/, ios/ … (см. front**/CONTEXT)
├── postgres/, pgbouncer/  # конфиги инфраструктуры, не сервисы
```
