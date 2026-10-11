# Dependency direction

Клиент зависит от публичных контрактов платформы (proto → Dart, HTTP/WS эндпоинты).
Обратного направления нет: бэкенд не знает о front. Кодовая база не входит в pnpm-workspace.
