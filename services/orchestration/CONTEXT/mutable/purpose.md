# Orchestration — внутренний маршрутизатор

Внутренний API-роутер: принимает `ProxyRequest` от gateway по gRPC, маршрутизирует по
путям к use-cases, выполняет транспортную авторизацию (JWT) и возвращает `ProxyResponse`.
