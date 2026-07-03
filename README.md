# entrar.api.br — a camada de identidade brasileira 🇧🇷

**Autenticação e verificação de identidade real para desenvolvedores brasileiros.** CPF direto da Receita Federal, OTP por WhatsApp oficial e verificação de idade — tudo em uma API HTTP simples, sem SDK, compatível com a LGPD.

> Pense num **"Login com Google", só que com CPF real** — mais OTP e idade quando você precisar.

🔗 **Site:** https://entrar.api.br · 📡 **Status:** https://entrar.api.br/status

---

## O que dá pra fazer

### 🪪 Login / validação de CPF (OAuth com CPF)
Confirme a identidade real do usuário na Receita Federal, com consentimento dele. Funciona como um fluxo OAuth:

1. Você redireciona o usuário para `https://cpf.entrar.api.br/?appId=SEU_TOKEN`
2. Ele informa CPF, data de nascimento e resolve o captcha
3. A validação acontece **em tempo real na Receita Federal**
4. O usuário volta pra sua `callbackUrl` com uma `session`
5. Você consulta a session e recebe **nome, CPF, data de nascimento e situação cadastral** confirmados

```bash
# Após o redirect, você recebe ?session=... na sua callbackUrl:
curl https://cpf.entrar.api.br/api/session/SESSION_ID
# → { "status": true, "nome": "...", "cpf": "...", "dataNascimento": "...", "situacao": "REGULAR" }
```

📖 https://entrar.api.br/validar-cpf-api

### 💬 OTP por WhatsApp oficial
Confirme que o usuário controla o telefone com um código de uso único pelo WhatsApp oficial. Dois endpoints:

```bash
# 1. Enviar
curl -X POST https://cpf.entrar.api.br/api/otp/send \
  -H "Authorization: Bearer SEU_API_SECRET" -H "Content-Type: application/json" \
  -d '{"telefone":"+5511999999999"}'
# → { "ok": true, "otpId": "...", "status": "pending" }

# 2. Verificar
curl -X POST https://cpf.entrar.api.br/api/otp/verify \
  -H "Authorization: Bearer SEU_API_SECRET" -H "Content-Type: application/json" \
  -d '{"otpId":"...","codigo":"123456"}'
# → { "ok": true, "verified": true }
```

O código expira em 10 minutos. Você não precisa de webhook: a verificação já é a prova de entrega.

📖 https://entrar.api.br/otp-whatsapp

### 🔞 Verificação de idade
Confirme a maioridade pela data de nascimento real (da Receita), sem autodeclaração — útil para apostas, +18 e conformidade com o ECA Digital.

📖 https://entrar.api.br/verificacao-de-idade

---

## Preços

| Produto | Preço |
|---|---|
| Validação de CPF | A partir de **R$ 0,02**/consulta (1.000 por R$ 20) · 10 grátis/mês |
| OTP por WhatsApp | **R$ 0,03**/envio · verificação grátis |

Créditos por PIX, sem mensalidade, sem expiração.

---

## Por que usar

- ✅ **Feito para o Brasil** — Receita Federal, WhatsApp e LGPD são o contexto nativo
- ✅ **Sem SDK** — API HTTP REST + JSON, funciona em qualquer stack (Node, PHP, Python, Java, .NET, Go…)
- ✅ **Consentimento do titular** — o próprio usuário valida CPF e telefone
- ✅ **Dados em tempo real** — direto da fonte oficial, não banco estático
- ✅ **99,9% de disponibilidade-alvo** · infra AWS + Google Cloud no Brasil · suporte 24/7

## Quem já usa

[Unifama](https://unifama.edu.br) (cadastro de vestibular e alunos) · [123 Bolão](https://123bolao.com) (alto volume diário) · [SIACE](https://siace.com.br) · [Cache Sistemas](https://www.cachesistemas.com.br) · [Wame](https://wame.api.br)

---

## Comece agora

1. Crie sua conta em **https://entrar.api.br/registro**
2. Gere seu `appId` (login com CPF) e/ou `API_SECRET` (OTP) no painel
3. Integre com os exemplos acima

📚 **Guias:** [Camada de identidade](https://entrar.api.br/verificacao-identidade) · [Validar CPF](https://entrar.api.br/validar-cpf-api) · [OTP WhatsApp](https://entrar.api.br/otp-whatsapp) · [Status](https://entrar.api.br/status)

💬 Suporte: WhatsApp (66) 99685-2025 · contato@entrar.api.br

<sub>Um produto da CACHE SISTEMAS — CNPJ 23.711.695/0001-15. Serviço sem vínculo oficial com o Governo Federal ou a Receita Federal; dados consultados na fonte pública oficial com consentimento do titular.</sub>
