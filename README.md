# Integração e Teste de API de Pagamentos

Este repositório documenta o processo de integração e teste de uma API de pagamentos, utilizando ferramentas como Postman e Azure.

## Visão Geral da API

A API de pagamentos está hospedada no Azure e é acessível através do endpoint `https://gerenciamentoapi.azure-api.net/pagamento/v1/`.

## Testes de Conectividade e Saúde da API

Verificamos inicialmente a saúde da API através de uma requisição GET.

![Realizando o teste de resposta da api no navegador](https://github.com/user-attachments/assets/d7066572-0ed4-496b-9d7a-dac76b9bae7a)

A resposta `{"status":"ok","message":"API de pagamentos está funcionando!"}` confirma que a API está operacional.

## Autenticação da API

A API utiliza autenticação baseada em chaves (`x-api-key`) e tokens de autorização (`Bearer`).

### Teste de Conectividade com Token Válido

Um teste bem-sucedido com um `x-api-key` e um token de autorização válido retorna uma mensagem de boas-vindas e a versão da API.

![Realizando teste de conectivade da API com o token correto](https://github.com/user-attachments/assets/426e7614-6b4f-4727-bf47-b53a51097d1b)

### Teste de Conectividade sem Token Válido

Ao tentar acessar a API sem um token válido (ou com um token expirado/incorreto), a API retorna um erro `401 Unauthorized` com a mensagem "Token não tem permissão de acesso".

![Realizando teste de conectividade sem o token valido](https://github.com/user-attachments/assets/79d3f828-2ae8-4123-90a4-0bfdaa417679)

## Processo de Pagamento (Requisições POST)

Realizamos testes de criação de pagamentos utilizando requisições `POST` para o endpoint `https://gerenciamentoapi.azure-api.net/pagamento/v1/payments`.

### Primeiro Teste de Pagamento

- **Amount:** 100.50
- **Currency:** BRL
- **Description:** Compra de livro
- **CustomerId:** user123

![Realizando o primeiro teste de pagamento](https://github.com/user-attachments/assets/4438f851-b783-4f1e-b11f-add9d1c5c13c)

A resposta indica `message: "Pagamento recebido com sucesso!"` e o status `pending`.

### Verificando o Status do Primeiro Teste de Pagamento

É possível verificar o status do pagamento enviando a mesma requisição `POST`. A API retorna os detalhes do pagamento, incluindo o ID e o status atual.

![Verificando o status de pagamento do primeiro teste](https://github.com/user-attachments/assets/bdb47bbd-9937-433f-a8ab-78fdfd72a9b1)

### Segundo Teste de Pagamento

- **Amount:** 200
- **Currency:** BRL
- **Description:** Compra de livro
- **CustomerId:** user123

![Realizando o segundo teste de pagamento](https://github.com/user-attachments/assets/ffc2698d-5e50-4ff8-a336-3c3e401ee8c2)

### Verificando o status do segundo pagamento

![Verificando o status de pagamento do segundo teste](https://github.com/user-attachments/assets/cfde5541-c241-434a-909f-5635a3d267aa)

### Terceiro Teste de Pagamento

- **Amount:** 140
- **Currency:** BRL
- **Description:** Compra de Material
- **CustomerId:** Anderson

![Realizando o terceiro teste de pagamento](https://github.com/user-attachments/assets/04bdd0c5-9c13-4dfe-aacf-fcc8c657d850)

### Verificando o status do segundo pagamento

![Verificando o status de pagamento do terceiro teste](https://github.com/user-attachments/assets/14551ec7-6f01-410d-af5b-138218ef1de2)

## Gerenciamento de Chaves de Assinatura

Durante o curso, aprendemos sobre a importância da segurança das chaves de acesso que é uma das boas práticas de segurança na API.

### Regeneração da Chave Primária

É possível regenerar a chave primária da assinatura para manter a segurança da API.

![Regeneração da chave primaria](https://github.com/user-attachments/assets/1137367b-31ec-47e1-80fc-50a85f3595c1)

### Regeneração da Chave Secundária

Da mesma forma, a chave secundária pode ser regenerada.

![Regeneração da chave secundaria](https://github.com/user-attachments/assets/72f17258-edc4-4f01-9b0a-08f632ed4fe0)

### Verificando a API após Regeneração das Chaves

Após a regeneração das chaves, é fundamental atualizar as credenciais em suas ferramentas de teste (como o Postman). A tentativa de acesso com as chaves antigas resultará em `401 Access Denied` devido a uma chave de assinatura inválida.

![Verificando a api após regeneração das chaves primarias e secundaria](https://github.com/user-attachments/assets/2d723ad4-4d77-48b6-a340-a4b378f15eff)

## Insights e Possibilidades

Durante o curso, obtivemos os seguintes insights e identificamos possibilidades:

* **Importância da Autenticação:** A autenticação robusta (chaves de API e tokens de autorização) é crucial para proteger os endpoints da API.
* **Testes Regressivos:** A regeneração de chaves demonstra a necessidade de testes regressivos para garantir que a aplicação continue funcionando corretamente com as novas credenciais.
* **Monitoramento de API:** A mensagem de saúde da API (`/health`) é fundamental para o monitoramento contínuo da disponibilidade e funcionalidade.
* **Gerenciamento de Erros:** As respostas de erro (e.g., `401 Unauthorized`, `401 Access Denied`) são claras e ajudam no diagnóstico de problemas de integração.
* **Status de Pagamento:** A API retorna um status `pending` para os pagamentos, indicando que há um processo assíncrono para a finalização da transação. Isso abre possibilidades para:
    * **Webhooks:** Implementação de webhooks para notificar sistemas externos sobre a mudança de status do pagamento (sucesso, falha, etc.).
    * **Consultas de Status:** Criação de um endpoint para consultar o status de um pagamento específico (e.g., `/payments/{id}`).
    * **Fluxos de Reconciliação:** Sistemas para conciliar pagamentos pendentes com as transações bancárias.
* **Extensibilidade da API:** A estrutura da API permite a adição de novos métodos de pagamento, moedas e informações adicionais de clientes conforme a necessidade.
* **Auditoria e Logs:** A capacidade de rastrear transações por `id` e `customerId` é importante para auditoria e resolução de problemas.
* **Ambientes de Desenvolvimento:** A prática de testar a API em um ambiente de desenvolvimento ou staging antes de ir para produção é essencial, especialmente após mudanças como a regeneração de chaves.

Este curso proporcionou uma compreensão prática de como interagir, testar e gerenciar uma API de pagamentos em um ambiente de teste
