# Mastra + Copilotkit bundler issue

## Overview
The `@copilotkit/runtime` package must be bundled as an external dependency but this is not documented. When it is not excluded from the bundle, requests to the copilot endpoint return a 500 error with the message:
```
{"error":"Internal Server Error"}
```
and print an empty object in the server logs: 
```
ERROR [2026-01-08 11:22:02.484 -0500] (Mastra):
    msg: {}
```

I discovered the external dependency fix thanks to the UI dojo: https://github.com/mastra-ai/ui-dojo/blob/main/src/mastra/index.ts

## Steps to reproduce

1. Clone the repo
```
git clone git@github.com:julianmclain/mastra-copilotkit-bundler-issue.git
```

2. Install deps (note the project uses `@copilotkit/runtime@1.10.6` because the latest version is not compatible with `@ag-ui/mastra`)
```
npm install --legacy-peer-deps
```

3. Run the prod build
```
npm run build && npm run start
```

4. CURL the copilotkit endpoint
```
curl http://localhost:4111/chat
```

## Solution

Document that `@copilotkit/runtime` must be excluded from the bundle:

```typescript
// in index.ts
bundler: {
  externals: ["@copilotkit/runtime"],
},
```
