# Dependency direction

Как в родителе. Специфика: `domain/` — только value-objects; use-case
`authorize-profile` зависит от gRPC-клиента и `AuthHelper`, use-case
`websocket-disconnect` — от реестра сокетов.
