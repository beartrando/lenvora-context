# Dependency direction

Как в родителе, специфика: use-case зависит от порта `TranscriberPort`, реализацию
которого собирает `composition/` из `infrastructure/stt/`. Внешние связи — только
входящий gRPC (из `streaming`) и исходящее Kafka-событие (через pg-boss-мост);
исходящих gRPC-клиентов у сервиса нет.
