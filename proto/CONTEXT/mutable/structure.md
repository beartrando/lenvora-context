# Structure

```
proto/
├── auth.proto           # service Auth: Logout, RefreshTokens, AnonymousSignIn, Get
├── profile.proto        # service Profile: GetByUser, Get, GetOrCreate (использует auth.UserIdRequest)
├── orchestration.proto  # service Orchestration: Proxy(ProxyRequest)→ProxyResponse
├── gusteau.proto        # service Gusteau: 6 RPC (products/nutrition/food-log)
│                        # + enum EventName, message EventObject (событийная модель)
├── streaming.proto      # service отсутствует: только EventRequest/EventResponse
│                        # (oneof input: empty | audio_start | audio_chunk |
│                        # audio_stop | ping; oneof output: event | ping |
│                        # error | transcription)
├── remy.proto           # service Remy: Process(RemyRequest) → RemyResponse
├── speech-to-text.proto # service SpeechToText: bidi Transcribe
│                        # (audio_start с profile_id → chunk* → stop;
│                        # ответ — oneof: транскрипция | ошибка, без JWT)
└── common/
    ├── health.proto     # service Health: Check, Status, Livez, Readyz
    ├── error.proto      # ErrorObject + ErrorCode (база для @shared/errors)
    ├── empty.proto, response.proto, key_value.proto, ping.proto
```
