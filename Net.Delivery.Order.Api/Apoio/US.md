# 🧾 US001 - Enviar e-mail de confirmação após pagamento

## 📘 Descrição funcional
O sistema deve enviar automaticamente um e-mail de confirmação ao cliente quando o pagamento for confirmado com sucesso.

---

## ⚙️ Descrição técnica
- Criar endpoint `POST /api/payment/confirm` que receba callbacks do provedor de pagamento.  
- Implementar serviço `IEmailService` para envio de e-mails com template HTML.  
- Registrar logs no banco de dados com informações do pagamento e status do e-mail.  
- Utilizar `ILogger` para registrar erros e mensagens informativas.  
- Garantir tratamento de exceções e retorno apropriado (HTTP 200 OK).

---

## ✅ Definition of Done (DoD)
- [ ] Código implementado e revisado (Code Review aprovado).  
- [ ] Testes unitários com cobertura mínima de 80%.  
- [ ] Testes manuais realizados no ambiente de homologação.  
- [ ] Documentação técnica atualizada.  
- [ ] Logs e métricas configurados.  
- [ ] Critérios de aceite (Acceptance Criteria) atendidos.  
- [ ] Build executa com sucesso no pipeline (CI/CD).  

---

## 🎯 Acceptance Criteria
- [ ] Quando o pagamento for aprovado, o cliente deve receber um e-mail de confirmação em até **5 minutos**.  
- [ ] O e-mail deve conter:
  - Nome do cliente  
  - Valor da transação  
  - Código da transação  
- [ ] O log da aplicação deve registrar o evento com `EmailSent = true`.  
- [ ] Caso o envio do e-mail falhe, o sistema deve registrar o erro e reprocessar automaticamente em até 3 tentativas.

---

## 🧪 Testes esperados
| Tipo de Teste | Descrição | Resultado Esperado |
|----------------|------------|--------------------|
| Unitário | Testar método de envio de e-mail | Retorna sucesso quando SMTP está disponível |
| Unitário | Testar callback de pagamento com status "PAID" | Dispara envio de e-mail e grava log |
| Integração | Simular chamada real do provedor de pagamento | Sistema responde 200 OK |
| Manual | Confirmar recebimento do e-mail pelo cliente | E-mail recebido em até 5 minutos |

---

## 🧱 Observações
- Utilizar `ILogger` e `IOptions` para configurar parâmetros de envio de e-mail.  
- Template HTML deve ser armazenado em `Resources/Templates/PaymentConfirmation.html`.  
- Validar autenticação do callback via header `X-Signature`.

---
