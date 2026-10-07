# proto/ — gRPC/protobuf-контракты сервисов Lenvora (отдельный репозиторий-подмодуль)

Исходники `.proto`: сервисы (auth, gusteau, orchestration, profile, remy, speech-to-text, streaming) и общие типы в `common/`.

Единый источник контрактов: код потребителей генерируется из него командой `make proto-generate` в [корне](../purpose.md).

Подключён как git-submodule (`.gitmodules` → `lenvora-proto.git`) и версионируется отдельно.
