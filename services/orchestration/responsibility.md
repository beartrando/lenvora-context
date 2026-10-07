# Responsibility

- Роутинг HTTP-подобных запросов внутри gRPC `Proxy`: точное совпадение метода и пути, ответ 404 c `ErrorCode.NOT_FOUND` при отсутствии маршрута.
- Авторизация: извлечение `Authorization: Bearer …` из заголовков запроса и `verifyAccessToken` (`ACCESS_SECRET`); `TokenExpiredError` → 401 `ErrorCode.TOKEN_EXPIRED`.
- Оркестрация use-case'ов: вызовы `auth.AnonymousSignIn/RefreshTokens/Logout/Get` и `profile.GetOrCreate/Get/GetByUser` по gRPC.
- Сценарий регистрации: `anonymousSignIn` → `profile.GetOrCreate` с полученным access-токеном.
- Сборка ответа: `ResponseAdapter` → `ProxyResponse{statusCode: 200, content-type: JSON, body}`.
- Health `Check/Status/Livez/Readyz` (db, kafka, pg-boss) на своём gRPC-порту.
