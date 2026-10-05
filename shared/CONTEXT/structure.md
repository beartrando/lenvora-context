# Structure

```
shared/
├── errors/               # src/errors/*.error.ts + src/contracts/proto/common/error.ts (codegen)
├── logger/               # src/index.ts (один файл; src/ также содержит устаревшие .js/.d.ts)
├── kafka-manager/        # src/{types,register,admin,producer,consumer,index}.ts
├── grpc-client-manager/  # src/index.ts (GrpcClientManager)
└── pg-boss-manager/      # src/{index,types,pg-boss-manager}.ts + src/internal/*
```
