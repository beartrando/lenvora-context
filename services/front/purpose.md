# front — Flutter-клиент Lenvora (mobile, desktop, web)

Клиентское приложение: анонимная авторизация по устройству, профиль, настройки, локализация (en/ru/uk), Firebase Crashlytics/Analytics, in-app update на Android.

Общается с бэкендом через HTTP [gateway](../gateway/purpose.md) (`GATEWAY_HOST:GATEWAY_PORT`) и имеет инфраструктуру WebSocket к [streaming](../streaming/purpose.md) (`STREAMING_HOST:STREAMING_PORT`).
