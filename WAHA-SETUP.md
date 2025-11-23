# Configuração WAHA no Render

## Arquivos criados:
- `Dockerfile` - Imagem Docker do WAHA
- `render.yaml` - Configuração do Render (opcional)

## Passos no Render:

1. **Criar novo Web Service**
   - Vá em "New +" → "Web Service"
   - Conecte este repositório GitHub

2. **Configurações:**
   - **Name:** `waha-cozil`
   - **Region:** Escolha a mais próxima
   - **Branch:** `main` (ou sua branch principal)
   - **Root Directory:** (deixe vazio)
   - **Runtime:** `Docker`
   - **Build Command:** (deixe vazio)
   - **Start Command:** (deixe vazio)

3. **Variáveis de Ambiente** (já configuradas no render.yaml, mas você pode adicionar manualmente):
   ```
   WAHA_VERSION=latest
   WAHA_LOG_LEVEL=info
   WAHA_WEBHOOK_URL=
   WAHA_WEBHOOK_EVENTS=message
   ```

4. **Deploy:**
   - Clique em "Create Web Service"
   - Aguarde o build (pode levar alguns minutos)

## Após o Deploy:

### 1. Criar Sessão do WhatsApp:
```bash
POST https://sua-url.onrender.com/api/sessions
Content-Type: application/json

{
  "name": "cozil-bot",
  "config": {
    "proxy": null,
    "webhook": null
  }
}
```

### 2. Obter QR Code:
```
GET https://sua-url.onrender.com/api/sessions/cozil-bot/auth/qr
```

### 3. Escanear QR Code:
- Abra o WhatsApp no celular
- Vá em Configurações → Aparelhos conectados → Conectar um aparelho
- Escaneie o QR Code

### 4. Verificar Status:
```
GET https://sua-url.onrender.com/api/sessions/cozil-bot
```

## URLs Importantes:

- **API Base:** `https://sua-url.onrender.com/api`
- **Documentação:** `https://sua-url.onrender.com/docs`
- **Health Check:** `https://sua-url.onrender.com/health`

## Testar Envio de Mensagem:

```bash
POST https://sua-url.onrender.com/api/sessions/cozil-bot/messages/text
Content-Type: application/json
Authorization: Bearer SEU_TOKEN (se configurado)

{
  "to": "5511999999999",
  "body": "Teste de mensagem"
}
```

## Receber Mensagens (Webhook):

Configure o webhook no Render:
```
WAHA_WEBHOOK_URL=https://seu-backend.com/webhook
WAHA_WEBHOOK_EVENTS=message
```

Ou configure via API:
```bash
POST https://sua-url.onrender.com/api/sessions/cozil-bot/webhook
{
  "url": "https://seu-backend.com/webhook",
  "events": ["message"]
}
```

