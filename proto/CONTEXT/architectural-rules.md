# Architectural rules

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
