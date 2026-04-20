# Shopify — instrukcja podpięcia do sklepu

Instrukcja dla Claude. W każdym nowym czacie podmień domenę sklepu przed `.myshopify.com` na tę, którą podam.

## Dane wejściowe
- **Domena sklepu:** `<SHOP>.myshopify.com` (np. `89baa4-ec.myshopify.com`)

## Kroki

### 1. Zainstaluj Shopify CLI
```
npm install -g @shopify/cli@latest
```

### 2. Zaloguj się do sklepu
```
shopify auth login --store <SHOP>.myshopify.com
```

### 3. Install with a plugin
https://shopify.dev/docs/apps/build/ai-toolkit#install-with-a-plugin

Claude Code (wykonuje użytkownik w terminalu Claude Code):
```
/plugin marketplace add Shopify/shopify-ai-toolkit
/plugin install shopify-plugin@shopify-plugin
```

### 4. Install with agent skills
https://shopify.dev/docs/apps/build/ai-toolkit#install-with-agent-skills
```
npx skills add Shopify/shopify-ai-toolkit
```

### 5. Install with the dev MCP server
https://shopify.dev/docs/apps/build/ai-toolkit#install-with-the-dev-mcp-server
```
claude mcp add --transport stdio shopify-dev-mcp -- npx -y @shopify/dev-mcp@latest
```
Po instalacji zrestartuj Claude Code.

## Weryfikacja
- `shopify version`
- `claude mcp list` — powinien pokazać `shopify-dev-mcp`
- W Claude Code widoczne skille `shopify-plugin:*` i narzędzia `mcp__shopify-dev-mcp__*`
