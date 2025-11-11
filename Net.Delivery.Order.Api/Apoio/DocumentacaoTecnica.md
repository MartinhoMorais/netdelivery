# 📘 Documentação Técnica - US001

## 🧩 Contexto
Esta User Story implementa o envio automático de e-mail de confirmação para o cliente após a confirmação de pagamento.  
O provedor externo envia um **callback HTTP POST** com os dados do pagamento, e o sistema processa a requisição, atualiza o status do pedido e dispara o e-mail.

---

## ⚙️ Estrutura de Código

**Camadas impactadas:**
- `CallbackApiDemo.Controllers`
- `CallbackApiDemo.Services`
- `CallbackApiDemo.Models`
- `CallbackApiDemo.Infrastructure.Email`

---

### 📂 Classes criadas / alteradas

| Classe | Tipo | Descrição |
|---------|------|-----------|
| `PaymentCallbackController` | Controller | Recebe o callback de pagamento via endpoint `/api/payment/confirm`. |
| `PaymentCallbackModel` | Model | Representa o payload recebido do provedor (TransactionId, OrderId, Status, Amount, Timestamp). |
| `EmailService` | Service | Responsável por enviar e-mails transacionais usando SMTP. |
| `EmailTemplateHelper` | Helper | Carrega e substitui variáveis dentro dos templates HTML. |
| `PaymentProcessor` | Service | Processa callbacks e coordena o envio de e-mails e atualização de logs. |

---

### 📡 Endpoint exposto

| Método | Rota | Autenticação | Descrição |
|--------|------|---------------|------------|
| `POST` | `/api/payment/confirm` | Header `X-Signature` | Recebe callback do provedor com dados do pagamento. |

**Exemplo de requisição:**
```json
{
  "TransactionId": "abc123",
  "OrderId": "ORD-456",
  "Status": "PAID",
  "Amount": 150.75,
  "Timestamp": "2025-11-11T13:45:00Z"
}

** Resposta:
```json
{
  "message": "Callback recebido com sucesso"
}


##---
	🧠 Fluxo de execução

	Provedor envia POST /api/payment/confirm.

	PaymentCallbackController.Receive() valida assinatura.

	PaymentProcessor processa o callback.

	EmailService.SendPaymentConfirmation() é chamado.

	Log é gravado no banco (EmailSent = true).

	Retorna HTTP 200 OK.

	🧰 Dependências utilizadas

	Microsoft.Extensions.Logging

	System.Net.Mail (envio de e-mail SMTP)

	Microsoft.Extensions.Options (para configuração de SMTP)

	Newtonsoft.Json (serialização de payload, se necessário)

	🪵 Logs
	Nível	Mensagem	Contexto
	Information	"Callback recebido"	Controller
	Information	"E-mail de confirmação enviado"	EmailService
	Error	"Falha ao enviar e-mail"	EmailService
	Warning	"Assinatura inválida"	Controller
	⚠️ Exceções tratadas

	SmtpException → Tentativa de reenvio configurada até 3 vezes.

	UnauthorizedAccessException → Retorna 401 Unauthorized se X-Signature inválido.

	JsonSerializationException → Retorna 400 Bad Request se payload inválido.

	🧪 Testes relacionados

	PaymentCallbackControllerTests.Receive_ShouldReturnOk_WhenValidPayload()

	EmailServiceTests.SendPaymentConfirmation_ShouldSendEmailSuccessfully()

	PaymentProcessorTests.Should_Log_And_Send_Email_When_Payment_Is_Paid()

📚 Configurações adicionadas

appsettings.json:

"EmailSettings": {
  "SmtpServer": "smtp.mailtrap.io",
  "Port": 2525,
  "Username": "user",
  "Password": "pass",
  "From": "no-reply@empresa.com"
}