# Dependency direction

Внутреннее направление — как в родителе (`domain` чистый, `application ↔ infrastructure`
взаимно). Сервис зависит от shared и от proto-контрактов (codegen); сам от него никто
не зависит, кроме вызовов по gRPC.
