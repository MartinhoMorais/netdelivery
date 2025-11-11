# 🧾 [USXXX] - [Título da User Story]

---

## 📘 Descrição funcional
Descreva o **objetivo da funcionalidade** do ponto de vista do usuário ou do negócio.  
Use a estrutura clássica:  
> **Como [tipo de usuário]**, quero **[ação/necessidade]**, para que **[benefício/resultado]**.

**Exemplo:**
> Como cliente, quero receber um e-mail de confirmação após efetuar um pagamento, para ter certeza de que a transação foi concluída com sucesso.

---

## ⚙️ Descrição técnica
Explique **como será implementada** a funcionalidade.  
Liste os componentes que serão criados ou modificados e as tecnologias usadas.

**Exemplo:**
- Criar endpoint `POST /api/payment/confirm` para receber callbacks de provedores externos.  
- Implementar `PaymentProcessorService` responsável por processar o payload e enviar o e-mail de confirmação.  
- Criar template HTML para o corpo do e-mail.  
- Registrar logs e erros usando `ILogger<PaymentCallbackController>`.  
- Adicionar configuração SMTP em `appsettings.json`.

---

## ✅ Definition of Done (DoD)
Critérios gerais que definem quando esta tarefa está **completamente pronta**.

- [ ] Código implementado e revisado (Code Review aprovado).  
- [ ] Testes unitários criados com cobertura mínima de 80%.  
- [ ] Testes manuais realizados e validados em ambiente de homologação.  
- [ ] Documentação técnica atualizada.  
- [ ] Logs e tratamento de exceções implementados.  
- [ ] Pipeline (build e deploy) executa com sucesso.  
- [ ] Todos os **Acceptance Criteria** foram atendidos.  
- [ ] Nenhum bug pendente relacionado à task.

---

## 🎯 Acceptance Criteria
Critérios **específicos de aceitação**, verificáveis e testáveis, que garantem que a história atende ao comportamento esperado.

- [ ] Quando o pagamento for aprovado, o cliente deve receber um e-mail em até **5 minutos**.  
- [ ] O e-mail deve conter o nome do cliente, valor da transação e código da operação.  
- [ ] O sistema deve registrar log com `EmailSent = true`.  
- [ ] Se o envio falhar, o sistema deve tentar novamente até 3 vezes.  
- [ ] O endpoint deve retornar `HTTP 200 OK` para callbacks válidos.

---

## 🧪 Testes esperados
| Tipo de Teste | Descrição | Resultado Esperado |
|----------------|------------|--------------------|
| Unitário | Testar método de envio de e-mail | Retorna sucesso quando SMTP está disponível |
| Unitário | Testar callback com status "PAID" | Dispara e-mail e grava log |
| Integração | Simular callback real do provedor | Sistema responde HTTP 200 OK |
| Manual | Confirmar recebimento do e-mail | E-mail recebido em até 5 minutos |

---

## 🧱 Observações
- Garantir que o endpoint seja protegido por assinatura (`X-Signature`).  
- Adicionar logs no banco em caso de falha de envio.  
- Template de e-mail armazenado em `Resources/Templates/PaymentConfirmation.html`.

---

📅 **Criado em:** [dd/mm/yyyy]  
👨‍💻 **Responsável:** [Nome do desenvolvedor]  
🧩 **Relacionado à documentação técnica:** [Link para o arquivo ou PR]
