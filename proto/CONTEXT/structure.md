# Structure

```
proto/
├── auth.proto           # service Auth: Logout, RefreshTokens, AnonymousSignIn, Get
├── profile.proto        # service Profile: GetByUser, Get, GetOrCreate (использует auth.UserIdRequest)
├── orchestration.proto  # service Orchestration: Proxy(ProxyRequest)→ProxyResponse
├── gusteau.proto        # service Gusteau: 6 RPC (products/nutrition/food-log)
│                        # + enum EventName, message EventObject (событийная модель)
├── streaming.proto      # service отсутствует: только EventRequest/EventResponse
│                        # (oneof: gusteau.EventObject | empty | error)
└── common/
    ├── health.proto     # service Health: Check, Status, Livez, Readyz
    ├── error.proto      # ErrorObject + ErrorCode (база для @shared/errors)
    ├── empty.proto, response.proto, key_value.proto, ping.proto
```
