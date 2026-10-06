# Structure

```
shared/errors/src/
├── index.ts
├── errors/{domain,not-found,token-expired,unknown,invalid-authorization,
│           invalid-session,access-denied,invalid-response,
│           validation-failed,service-unavailable}.error.ts
└── contracts/proto/common/error.ts   # protoc-gen-ts_proto, DO NOT EDIT
```
