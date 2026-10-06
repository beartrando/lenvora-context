# Dependency direction

Как в родителе. Специфика: слоя `domain/` и `application/ports/` не существует —
use-cases (`health`, `status`, `livez`, `readyz`) импортируют конкретный
`infrastructure/grpc/clients/health.aggregator`.
