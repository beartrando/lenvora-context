# speech-to-text — bidi-gRPC-сервис потокового распознавания голоса

Контракт `SpeechToText.Transcribe` (первым сообщением `audio_start` с профилем и настройками аудио, далее `audio_chunk`, завершение `audio_stop`; ответ — `transcription | error`) описан в [proto](../../proto/purpose.md) и реализуется сервисом.

Вызывающая сторона — [streaming](../streaming/purpose.md). В текущей реализации сервис отвечает только health: bidi-обработчик — каркас, внешние AI-провайдеры не подключены.
