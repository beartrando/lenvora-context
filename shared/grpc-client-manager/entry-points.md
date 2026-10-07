# Entry points

`@shared/grpc-client-manager` → exports map `.` (`exports["."]`):

- `GrpcClientManager<TClient>` — класс: `constructor(createClient: GrpcClientFactory<TClient>)`, `call<TResult>(fn: GrpcCall<TClient, TResult>): Promise<TResult>`;
- тип `GrpcClientFactory<T> = () => T` — фабрика клиента;
- тип `GrpcCall<TClient, TResult> = (client, cb: (err: grpc.ServiceError | null, res: TResult) => void) => void` — колбэковый вызов.

Default-экспорта и субпутей нет.
