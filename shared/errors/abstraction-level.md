# Abstraction level

Слой типов и контрактов: только классы и сгенерированный proto-код, без I/O, без side-effectов, без чтения окружения.

Ниже — `Error` и `@bufbuild/protobuf` (в сгенерированном контракте); источник истины контракта — `proto/common/error.proto` уровня [proto](../../proto/purpose.md). Выше — application-слой сервисов, который бросает эти ошибки.
