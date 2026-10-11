# Structure

```
ai/
├── service.ts                        # AiService: фабрика клиента + process(text)
├── service.spec.ts                   # юнит: конструктор через фабрику, делегирование chat
├── clients/
│   ├── create-ai-client.ts           # createAiClient(config) → OpenRouterClient
│   └── openrouter.ts                 # OpenRouterClient: SDK openai, chat.completions.create
└── interfaces/
    ├── ai-client.interface.ts        # AiClient.chat(messages) → Promise<string>
    └── ai-config.interface.ts        # AiConfig { apiKey, baseUrl, model }
```

`CONTEXT/` — этот каталог, git-игнорируется (`.gitignore` сервиса: `**/CONTEXT`).
