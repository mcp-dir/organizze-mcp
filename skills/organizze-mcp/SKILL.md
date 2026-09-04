---
name: organizze-mcp
description: Skill da REST API do Organizze na MCP.AI: 62 endpoints em /api/organizze. Suas finanças do Organizze por linguagem natural: saldos consolidados, visão do mês, busca de lançamentos, faturas de cartão, contas fixas, orçamentos, relatórios por categoria e tag, projeção de fluxo de caixa e status do Open Finance. Conecte sua conta em um clique, sem chave nem token. Na tela do Organizze, desmarque "Permitir alterações" se quiser acesso somente leitura. Autentique com workspace API key (sk_live) gerada em app.mcp.ai/settings/api-keys. Use quando o usuário pedir algo coberto pelos endpoints.
---

# Organizze — REST API skill

Você tem acesso à **Organizze** REST API na MCP.AI.

> Suas finanças do Organizze por linguagem natural: saldos consolidados, visão do mês, busca de lançamentos, faturas de cartão, contas fixas, orçamentos, relatórios por categoria e tag, projeção de fluxo de caixa e status do Open Finance. Conecte sua conta em um clique, sem chave nem token. Na tela do Organizze, desmarque "Permitir alterações" se quiser acesso somente leitura.

## Base URL

```
https://api.mcp.ai/api/organizze
```

Todo endpoint é um **POST** na Base URL + o path abaixo. Os parâmetros vão no corpo JSON.

## Autenticação

Inclua em toda request:

```
Authorization: Bearer sk_live_...
Content-Type: application/json
```

> Gere sua chave em **https://app.mcp.ai/settings/api-keys** (workspace API key `sk_live_…`, não expira, revogável). Uma única chave serve pra todos os seus MCPs.

## Formato de resposta

```json
{ "ok": true, "tool": "<tool_id>", "result": <payload> }
```

## Exemplo cURL

```bash
curl -X POST https://api.mcp.ai/api/organizze/add/transactions/tags \
  -H "Authorization: Bearer sk_live_..." \
  -H "Content-Type: application/json" \
  -d '{"items":"..."}'
```

## Reportar problemas

Se um endpoint retornar erro, vazio ou dado inesperado, reporte (não desista calado): **POST /api/organizze/report** com `{ "message": "...", "context"?: "...", "conversation"?: [...] }`. Isso notifica o time da MCP.AI.

## Endpoints (62)

#### `organizze_add_transactions_tags`

Adiciona tags a VÁRIAS transações em uma chamada (acumula sobre as existentes; máx. _(POST /api/organizze/add/transactions/tags)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `items` | object[] | Sim | Lista de objetos {transaction_id|transaction_uuid, tags} (máx. 200). |

#### `organizze_clone_budgets`

Clona os orçamentos do mês anterior para o mês de destino (não sobrescreve um mês que já tenha orçamentos). _(POST /api/organizze/clone/budgets)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `month` | integer | Não | Mês destino 1-12 (padrão: mês atual) |
| `year` | integer | Não | Ano destino (padrão: ano atual) |

#### `organizze_create_account`

Cria uma conta bancária manual. _(POST /api/organizze/create/account)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `name` | string | Sim | Nome da conta |
| `institution_id` | string | Não | ID da instituição (ex: nubank) |
| `initial_balance` | number | Não | Saldo inicial em reais |
| `description` | string | Não | Descrição da conta |

#### `organizze_create_budget`

Cria um limite de gastos (orçamento) para uma categoria. _(POST /api/organizze/create/budget)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `category` | string | Sim | Nome (preferido) ou id da categoria |
| `amount` | number | Sim | Valor do limite em reais (ex.: 1000.00) |
| `activity_type` | string | Não | 'expense' (padrão) ou 'earning' |
| `month` | integer | Não | Mês 1-12 (padrão: mês atual) |
| `year` | integer | Não | Ano (padrão: ano atual) |

#### `organizze_create_category`

Cria uma categoria de despesa ou receita (raiz ou subcategoria via parent_id). _(POST /api/organizze/create/category)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `name` | string | Sim | Nome da categoria |
| `kind` | string | Sim | 'expenses' (despesa) ou 'earnings' (receita) |
| `group_id` | string | Não | ID do grupo (ex.: food, health, salary). Padrão: 'other'/'other_earnings' |
| `color` | string | Não | Cor hexadecimal (ex.: #FF5733) |
| `parent_id` | integer | Não | ID da categoria pai (para subcategoria) |

#### `organizze_create_credit_card`

Cria um cartão de crédito MANUAL (automáticos vêm da conexão bancária). _(POST /api/organizze/create/credit/card)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `name` | string | Sim | Nome do cartão (ex.: Nubank, Itaú Platinum) |
| `billing_due_day` | integer | Sim | Dia de vencimento da fatura (1-31) |
| `billing_cycle_day` | integer | Sim | Dia de fechamento da fatura (1-31) |
| `flag` | string | Não | Bandeira: visa, master, elo, amex, hipercard, diners, nubank, etc |
| `limit` | number | Não | Limite do cartão em reais |
| `payment_account_id` | integer | Não | ID da conta padrão para pagamento das faturas |

#### `organizze_create_transaction`

Cria uma transação (receita ou despesa). _(POST /api/organizze/create/transaction)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `description` | string | Sim | Descrição da transação (obrigatório) |
| `amount` | number | Sim | Valor sempre positivo (obrigatório). O sinal vem de is_income. |
| `date` | string | Sim | Data YYYY-MM-DD (obrigatório) |
| `account` | string | Sim | NOME da conta ou cartão (preferido) ou UUID retornado por list_accounts/list_credit_cards. Nunca invente. |
| `category` | string | Não | NOME da categoria (preferido, como em list_categories) ou id retornado pela listagem. Opcional. |
| `is_income` | boolean | Sim | true = receita, false = despesa (obrigatório) |
| `done` | boolean | Não | true se já paga/recebida |
| `observation` | string | Não | Observações |
| `tags` | string | Não | Tags separadas por vírgula |
| `times` | integer | Não | Parcelas (ex: 12) |
| `recurring` | boolean | Não | Conta fixa recorrente |
| `periodicity` | string | Não | daily, weekly, monthly, yearly, etc. |
| `idempotency_key` | string | Não | Chave opcional para deduplicar retries (TTL 24h por entidade). |

#### `organizze_create_transfer`

Cria uma transferência entre duas contas bancárias (suporta recorrência). _(POST /api/organizze/create/transfer)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `amount` | number | Sim | Valor (sempre positivo) |
| `date` | string | Sim | Data (YYYY-MM-DD) |
| `credit_account` | string | Sim | Nome (ou UUID) da conta de ORIGEM |
| `debit_account` | string | Sim | Nome (ou UUID) da conta de DESTINO |
| `description` | string | Não | Descrição (padrão: 'Transferência') |
| `done` | boolean | Não | true se já efetuada |
| `observation` | string | Não | Observações |
| `tags` | string | Não | Tags separadas por vírgula |
| `recurring` | boolean | Não | true para transferência recorrente |
| `periodicity` | string | Não | daily, weekly, biweekly, monthly (padrão), bimonthly, trimonthly, sixmonthly, yearly |

#### `organizze_delete_account`

Exclui uma conta. ATENÇÃO: todas as transações da conta são excluídas em background. Contas automáticas não podem ser excluídas (desconecte pelo menu de conexões). _(POST /api/organizze/delete/account)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Nome (preferido) ou UUID da conta a excluir |

#### `organizze_delete_budget`

Remove um orçamento (limite de gastos) de uma categoria. _(POST /api/organizze/delete/budget)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `budget_id` | integer | Sim | ID do orçamento a remover |

#### `organizze_delete_category`

Exclui uma categoria; suas transações vão para outra categoria (substitute_category ou padrão). _(POST /api/organizze/delete/category)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `category` | string | Sim | Nome (preferido) ou id da categoria a excluir |
| `substitute_category` | string | Não | Nome ou id da categoria que recebe as transações (opcional) |

#### `organizze_delete_credit_card`

Exclui um cartão de crédito. ATENÇÃO: todas as transações e faturas do cartão são excluídas. Cartões automáticos não podem ser excluídos (desconecte pelo menu de conexões). _(POST /api/organizze/delete/credit/card)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `credit_card` | string | Sim | Nome (preferido) ou UUID do cartão a excluir |

#### `organizze_delete_transaction`

Exclui uma transação. Transações automáticas (Open Finance) não podem ser excluídas. Para recorrentes use delete_recurrence. O transaction_id/uuid DEVE ser copiado de list_transactions, search_transac _(POST /api/organizze/delete/transaction)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `transaction_id` | integer | Não | ID da transação (copiado de list/search/get nesta conexão) |
| `transaction_uuid` | string | Não | UUID da transação (copiado de list/search/get nesta conexão) |
| `delete_recurrence` | string | Não | Recorrentes: 'this_only', 'this_and_future' ou 'all' |

#### `organizze_find_duplicates`

Encontra transações possivelmente duplicadas (mesmo valor e descrição similar em datas próximas). _(POST /api/organizze/find/duplicates)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Início YYYY-MM-DD (padrão: início do mês atual) |
| `end_date` | string | Não | Fim YYYY-MM-DD (padrão: fim do mês atual) |
| `days_threshold` | integer | Não | Máximo de dias entre transações para considerar duplicata (padrão: 3) |
| `include_automatic` | boolean | Não | Incluir transações automáticas (Open Finance). Padrão: true |

#### `organizze_find_installments`

Encontra COMPRAS PARCELADAS no período (use isto, não busca por texto). _(POST /api/organizze/find/installments)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Início YYYY-MM-DD (padrão: 12 meses atrás) |
| `end_date` | string | Não | Fim YYYY-MM-DD (padrão: hoje) |
| `account` | string | Não | Nome ou id da conta/cartão (aceita cartões conectados) |
| `min_occurrences` | integer | Não | Mínimo de meses distintos para inferir parcelamento (padrão: 3, mín: 2) |

#### `organizze_find_subscriptions`

Detecta ASSINATURAS e serviços recorrentes (Netflix, Spotify, ChatGPT, aluguel...). _(POST /api/organizze/find/subscriptions)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Início YYYY-MM-DD (padrão: 6 meses atrás) |
| `end_date` | string | Não | Fim YYYY-MM-DD (padrão: hoje) |
| `account` | string | Não | Nome ou id da conta/cartão (aceita conectados) |
| `min_occurrences` | integer | Não | Mínimo de meses distintos para inferir assinatura mensal (padrão: 3, mín: 2) |
| `include_recurrences` | boolean | Não | Incluir contas fixas cadastradas (padrão: true) |

#### `organizze_get_account_context`

Orientação inicial em uma só chamada: entidade conectada, plano, data de hoje no fuso do usuário, e listas compactas (id ↔ nome) de contas, cartões e categorias. _(POST /api/organizze/get/account/context)_

#### `organizze_get_balances`

Saldo consolidado: saldo atual de cada conta, fatura atual em aberto de cada cartão e o patrimônio (contas − faturas atuais em aberto). _(POST /api/organizze/get/balances)_

#### `organizze_get_bank_connections`

Lista as conexões bancárias (Open Finance / Conectado) ATIVAS e o status de cada uma (estável/indisponível). _(POST /api/organizze/get/bank/connections)_

#### `organizze_get_budget_summary`

Resumo dos limites de gastos do mês com alertas de budgets excedidos ou próximos do limite ("estou no controle?"). _(POST /api/organizze/get/budget/summary)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `month` | integer | Não | Mês 1-12. Padrão: mês atual. |
| `year` | integer | Não | Ano. Padrão: ano atual. |

#### `organizze_get_cashflow_forecast`

Projeção de fluxo de caixa: parte do saldo atual e aplica os lançamentos não pagos de contas manuais na janela, retornando o saldo projetado ao fim e o menor ponto (risco de negativo). _(POST /api/organizze/get/cashflow/forecast)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `days` | integer | Não | Tamanho da janela em dias (padrão 30, máx 365) |
| `start_date` | string | Não | Início da janela (YYYY-MM-DD). Padrão: hoje |

#### `organizze_get_categories_evolution`

Evolução de gastos/receitas por categoria ao longo do tempo (diário/semanal/mensal). _(POST /api/organizze/get/categories/evolution)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Início YYYY-MM-DD (padrão: 3 meses atrás) |
| `end_date` | string | Não | Fim YYYY-MM-DD (padrão: fim do mês atual) |
| `periodicity` | string | Não | daily, weekly ou monthly (padrão: baseado no período) |
| `category_ids` | integer[] | Não | IDs de categorias para filtrar (via list_categories) |
| `only_parent_category` | boolean | Não | true = só a categoria pai, sem subcategorias |
| `lens` | string | Não | 'date' (padrão) ou 'bill_due_date' |

#### `organizze_get_categories_report`

Relatório de gastos e receitas agrupados por categoria (inclui compras de cartão; base POR LANÇAMENTO). _(POST /api/organizze/get/categories/report)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Início YYYY-MM-DD (padrão: início do mês atual) |
| `end_date` | string | Não | Fim YYYY-MM-DD (padrão: fim do mês atual) |
| `lens` | string | Não | 'date' (padrão, data do gasto) ou 'bill_due_date' (vencimento da fatura) |

#### `organizze_get_credit_card_invoice`

Detalhes de uma fatura específica de cartão: transações, pagamentos e gastos por categoria. _(POST /api/organizze/get/credit/card/invoice)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `credit_card` | string | Não | NOME (preferido) ou id do cartão. |
| `credit_card_id` | integer | Não | ID do cartão (alternativa ao nome). |
| `invoice_id` | integer | Não | ID da fatura específica. |
| `month` | integer | Não | Mês da fatura (1-12), se invoice_id não informado. |
| `year` | integer | Não | Ano da fatura, se invoice_id não informado. |

#### `organizze_get_credit_card_invoices`

Lista faturas de um cartão. Informe credit_card_id. _(POST /api/organizze/get/credit/card/invoices)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `credit_card_id` | integer | Sim | ID do cartão |
| `start_date` | string | Não | Data inicial (YYYY-MM-DD) para derivar o ano. |
| `end_date` | string | Não | Data final (YYYY-MM-DD). |

#### `organizze_get_financial_summary`

Resumo financeiro do período (receitas, despesas, comparação, maiores gastos). _(POST /api/organizze/get/financial/summary)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Data no período (YYYY-MM-DD). |
| `end_date` | string | Não | Data final (YYYY-MM-DD). |
| `year` | integer | Não | Ano |
| `month` | integer | Não | Mês 1-12 |

#### `organizze_get_income_vs_expenses`

Relatório de entradas vs saídas (receitas vs despesas) por período, em modo cashflow. _(POST /api/organizze/get/income/vs/expenses)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Data início (YYYY-MM-DD). Padrão: início do mês atual. |
| `end_date` | string | Não | Data fim (YYYY-MM-DD). Padrão: fim do mês atual. |
| `periodicity` | string | Não | daily, weekly ou monthly. |
| `account_id` | integer | Não | Filtra por uma conta específica. |

#### `organizze_get_invoices_matrix`

Matriz de faturas (mês × cartão) com totais por mês, por cartão e total geral, usando os mesmos valores de get_credit_card_invoices. _(POST /api/organizze/get/invoices/matrix)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Início YYYY-MM-DD (usa o mês inteiro; padrão: 01/01 do ano atual) |
| `end_date` | string | Não | Fim YYYY-MM-DD (usa o mês inteiro; padrão: 31/12 do ano atual) |
| `cards` | string[] | Não | Nomes (preferido) ou ids de cartões. Vazio = todos. |

#### `organizze_get_latest_imports`

Transações mais recentemente importadas via Open Finance (contas/cartões conectados). _(POST /api/organizze/get/latest/imports)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `limit` | integer | Não | Atalho legado: equivale a per_page (padrão 50, máx 100) |
| `page` | integer | Não | Página (base 1). Padrão: 1. |
| `per_page` | integer | Não | Itens por página (padrão 50, máx 100) |

#### `organizze_get_monthly_overview`

Visão geral do mês em uma só chamada: receitas, despesas, saldo, maiores categorias de despesa, status dos limites e contas a pagar dos próximos 7 dias. _(POST /api/organizze/get/monthly/overview)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `month` | integer | Não | Mês 1-12. Padrão: mês atual. |
| `year` | integer | Não | Ano. Padrão: ano atual. |

#### `organizze_get_open_finance_status`

Status AO VIVO do agregador Open Finance (Belvo), opcionalmente por banco. _(POST /api/organizze/get/open/finance/status)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `institution` | string | Não | Nome do banco (ex.: 'Itaú', 'Nubank'). Vazio = todos os bancos de varejo BR. |

#### `organizze_get_tags_report`

Relatório de transações agrupadas por tag (despesas e receitas, com percentuais). _(POST /api/organizze/get/tags/report)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Início YYYY-MM-DD (padrão: início do mês atual) |
| `end_date` | string | Não | Fim YYYY-MM-DD (padrão: fim do mês atual) |
| `tag_name` | string | Não | Filtrar por tag exata (não usar junto com tag_name_prefix) |
| `tag_name_prefix` | string | Não | Tags que começam com este texto (não usar junto com tag_name) |

#### `organizze_get_transaction`

Detalhes completos de uma transação pelo id ou uuid. _(POST /api/organizze/get/transaction)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `transaction_id` | integer | Não | ID da transação. |
| `transaction_uuid` | string | Não | UUID da transação. |

#### `organizze_get_upcoming_bills`

Contas a pagar dos próximos dias: lançamentos não pagos com vencimento na janela e faturas de cartão que vencem no período ("o que vence essa semana?"). _(POST /api/organizze/get/upcoming/bills)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `days` | integer | Não | Tamanho da janela em dias (padrão 7, máximo 90). |
| `start_date` | string | Não | Início da janela (YYYY-MM-DD). Padrão: hoje. |

#### `organizze_inform_invoice_payment`

Informa pagamento de fatura em cartão automático (Open Finance). _(POST /api/organizze/inform/invoice/payment)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `credit_card` | string | Sim | Nome ou UUID do cartão automático |
| `invoice_date_or_id` | string | Não | YYYY-MM-DD, YYYY-MM ou id da fatura |
| `date` | string | Não | Data do pagamento YYYY-MM-DD |
| `observation` | string | Não | Observações |

#### `organizze_list_accounts`

Lista as contas do usuário com ids. _(POST /api/organizze/list/accounts)_

#### `organizze_list_budgets`

Lista limites de gastos por categoria. _(POST /api/organizze/list/budgets)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `year` | integer | Não | Ano, ex: 2026 |
| `month` | integer | Não | Mês 1-12 |

#### `organizze_list_categories`

Lista as categorias usadas para classificar transações. _(POST /api/organizze/list/categories)_

#### `organizze_list_credit_cards`

Lista os cartões de crédito do usuário com ids e limites. _(POST /api/organizze/list/credit/cards)_

#### `organizze_list_institutions`

Catálogo de instituições financeiras (bancos/operadoras) com o id (slug) usado em institution_id ao criar/editar contas e cartões. _(POST /api/organizze/list/institutions)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `query` | string | Não | Filtro por nome/slug (ex.: 'nubank', 'itau') |
| `limit` | integer | Não | Máximo de itens (padrão 50, máx 200) |

#### `organizze_list_recurrences`

Lista as contas fixas CADASTRADAS (lançamentos recorrentes/infinitos): aluguel, assinaturas, salário, etc., com periodicidade, valor e a próxima ocorrência. _(POST /api/organizze/list/recurrences)_

#### `organizze_list_transactions`

Lista transações do usuário no período selecionado, com paginação (page/per_page; máximo 80 por página). _(POST /api/organizze/list/transactions)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Data inicial no formato YYYY-MM-DD (padrão: início do mês atual) |
| `end_date` | string | Não | Data final no formato YYYY-MM-DD (padrão: fim do mês atual) |
| `account_id` | integer | Não | ID da conta para filtrar |
| `credit_card_id` | integer | Não | ID do cartão de crédito para filtrar (apenas transações do cartão) |
| `category_id` | integer | Não | ID da categoria para filtrar. NOTA: Quando informado, o modo é automaticamente alterado para 'all_transactions' para incluir gastos do cartão de crédito |
| `order_by` | string | Não | Ordenação dos resultados (padrão: 'date_desc' - data mais recente primeiro). Opções: 'date_desc' (data mais recente primeiro), 'date_asc' (data mais antiga primeiro), 'created_at_desc' (última criada primeiro), 'created_at_asc' (primeira criada primeiro), 'amount_desc' (maior valor primeiro), 'amount_asc' (menor valor primeiro) |
| `list_mode` | string | Não | 'cashflow' (padrão): movimentos de CONTAS + faturas de cartão como valores únicos (SEM compras individuais de cartão). Use para 'contas a pagar', 'despesas não pagas', 'o que vence' e fluxo de caixa — nesse modo cada fatura aparece como UMA linha com seu status de pagamento. 'all_transactions': TODOS os itens, incluindo compras individuais de cartão com categorias. Use para análise de gastos por categoria ou relatórios de despesas. ATENÇÃO: em all_transactions, compras de cartão NUNCA são despesas não pagas individualmente — a compra já está efetivada; o que pode vencer é a FATURA (campos invoice_paid/payment_note). NOTA: category_id ou credit_card_id forçam automaticamente 'all_transactions'. |
| `page` | integer | Não | Página (padrão: 1). Use page=2, 3… quando truncated=true e precisar dos próximos itens. |
| `per_page` | integer | Não | Itens por página (padrão: 80, máximo: 80) |
| `installments_only` | boolean | Não | Se true, retorna SOMENTE transações PARCELADAS (repeat_total > 1) do período. Use para 'minhas compras parceladas'. ATENÇÃO: em cartões/contas conectados (Open Finance) as parcelas costumam chegar como lançamentos mensais SEPARADOS, sem repeat_total — nesses casos este filtro não as captura; use a ferramenta find_installments (que aplica heurística). |

#### `organizze_list_transferences`

Lista transferências entre contas com paginação (page/per_page). _(POST /api/organizze/list/transferences)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Data inicial (YYYY-MM-DD). |
| `end_date` | string | Não | Data final (YYYY-MM-DD). |
| `account_id` | integer | Não | Conta de origem ou destino. |
| `page` | integer | Não | Página (base 1). Padrão: 1. |
| `per_page` | integer | Não | Itens por página (máx 150). Padrão: 150. |

#### `organizze_load_skill`

Carrega um guia detalhado de uso (skill) sobre um tema do Organizze. _(POST /api/organizze/load/skill)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `skill_name` | string | Sim | Nome exato da skill (ver lista na descrição da ferramenta). (automatic_card_invoice_payments, bulk_transaction_operations, category_and_budget_management, credit_card_invoice_queries, credit_card_spending, financial_health, financial_summaries_and_bases, installments_search, invoice_payments, latest_open_finance_imports, open_finance_accounts, subscriptions_detection, support_and_connection_issues, sync_accounts_and_cards, transaction_listing_and_search) |

#### `organizze_mass_create_transactions`

Cria VÁRIAS transações em lote (máx. _(POST /api/organizze/mass/create/transactions)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `transactions` | object[] | Sim | Lista de objetos de transação (máx. 100). |
| `idempotency_key` | string | Não | Chave opcional para deduplicar retries (TTL 24h por entidade). |

#### `organizze_mass_delete_transactions`

Exclui VÁRIAS transações por lista de ids/uuids (máx. _(POST /api/organizze/mass/delete/transactions)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `transactions` | object[] | Sim | Lista de objetos identificando as transações (máx. 500). |

#### `organizze_mass_manage_categories`

Aplica VÁRIAS mudanças na árvore de categorias em uma chamada atômica (máx. _(POST /api/organizze/mass/manage/categories)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `operations` | object[] | Sim | Lista de operações (máx. 100). Cada objeto tem "op" e os campos da operação. |

#### `organizze_mass_update_transactions`

Atualiza VÁRIAS transações por FILTRO de busca (máx. _(POST /api/organizze/mass/update/transactions)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `query` | string | Não | Texto na descrição (LIKE) |
| `start_date` | string | Não | YYYY-MM-DD (padrão: início do mês) |
| `end_date` | string | Não | YYYY-MM-DD (padrão: fim do mês) |
| `category_id` | integer | Não | Filtrar pela categoria atual (id de list_categories) |
| `account_id` | integer | Não | Filtrar por conta |
| `credit_card_id` | integer | Não | Filtrar por cartão |
| `activity_type` | string | Não | 'income' ou 'expense' |
| `tag_filter` | string | Não | Filtrar por tag existente |
| `paid_filter` | boolean | Não | true (pagas) ou false (pendentes) |
| `new_category` | string | Não | Nome (preferido) ou id da nova categoria |
| `new_description` | string | Não | Substitui a descrição inteira |
| `description_find` | string | Não | Texto a localizar (com description_replace) |
| `description_replace` | string | Não | Texto de substituição |
| `new_observation` | string | Não |  |
| `new_tags` | string | Não | Tags separadas por vírgula (ação via tag_action) |
| `tag_action` | string | Não | 'add' (padrão), 'remove' ou 'replace' |
| `new_paid` | boolean | Não | Marcar todas como pagas/pendentes |

#### `organizze_register_invoice_payment`

Registra o pagamento de uma fatura de cartão MANUAL como lançamento, vinculado à fatura. _(POST /api/organizze/register/invoice/payment)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `credit_card` | string | Sim | Nome (preferido) ou UUID do cartão MANUAL pago |
| `source_account` | string | Não | Nome (ou UUID) da conta de onde saiu o pagamento |
| `amount` | number | Sim | Valor pago (positivo) |
| `date` | string | Sim | Data do pagamento (YYYY-MM-DD) |
| `invoice_date_or_id` | string | Não | ID numérico ou "YYYY-MM" de get_credit_card_invoices (recomendado). Sem isso: última fatura até a data. |
| `observation` | string | Não | Observações |

#### `organizze_resolve_entity`

Resolve um nome aproximado para o id/uuid real de conta, cartão ou categoria. _(POST /api/organizze/resolve/entity)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `name` | string | Sim | Nome (ou parte) a resolver. |
| `kind` | string | Não | 'account', 'credit_card', 'category' ou 'any' (padrão). |

#### `organizze_search_transactions`

Busca transações por texto na descrição, observação ou tags (ex.: "iFood", "Uber", "salário"). _(POST /api/organizze/search/transactions)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `query` | string | Sim | Termo de busca (descrição, observação e tags). |
| `start_date` | string | Não | Data inicial (YYYY-MM-DD). Padrão: 6 meses atrás. |
| `end_date` | string | Não | Data final (YYYY-MM-DD). Padrão: hoje. |
| `min_amount` | number | Não | Valor mínimo. |
| `max_amount` | number | Não | Valor máximo. |
| `activity_type` | string | Não | 'income' (receita) ou 'expense' (despesa). |
| `page` | integer | Não | Página (base 1). Use quando truncated=true. |
| `per_page` | integer | Não | Itens por página (máximo 100). |
| `tags_only` | boolean | Não | Se true, busca apenas no nome das tags. |

#### `organizze_set_transaction_paid`

Marca uma transação como paga/recebida (paid=true) ou não paga (paid=false) em conta MANUAL ("já paguei o aluguel"). _(POST /api/organizze/set/transaction/paid)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `paid` | boolean | Sim | true = paga/recebida; false = não paga |
| `transaction_id` | integer | Não | ID da transação |
| `transaction_uuid` | string | Não | UUID da transação |

#### `organizze_suggest_categories`

Sugere categorias para uma ou mais descrições de transação, usando as edições anteriores do próprio usuário, o histórico já categorizado e padrões globais. _(POST /api/organizze/suggest/categories)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `items` | object[] | Sim | Lista de itens a categorizar (máximo 100). |

#### `organizze_transactions_list_results`

Totais financeiros de um período (receitas, despesas, resultado, saldo, previstos) sem paginar transação por transação. _(POST /api/organizze/transactions/list/results)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start_date` | string | Não | Data início (YYYY-MM-DD). Padrão: início do mês atual. |
| `end_date` | string | Não | Data fim (YYYY-MM-DD). Padrão: fim do mês atual. |
| `account_id` | integer | Não | Filtra por uma conta. |
| `category_id` | integer | Não | Filtra por categoria (força modo all_transactions). |
| `list_mode` | string | Não | 'cashflow' (padrão) ou 'all_transactions'. |

#### `organizze_uninform_invoice_payment`

Desfaz um "Informar Pagamento" em cartão AUTOMÁTICO (marcador aguardando Open Finance). _(POST /api/organizze/uninform/invoice/payment)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `credit_card` | string | Sim | Nome (preferido) ou UUID do cartão AUTOMÁTICO |
| `invoice_date_or_id` | string | Sim | Obrigatório: "YYYY-MM-DD", "YYYY-MM" ou id numérico |

#### `organizze_update_account`

Atualiza uma conta (nome, descrição, instituição, arquivar). _(POST /api/organizze/update/account)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Nome (preferido) ou UUID da conta |
| `name` | string | Não | Novo nome |
| `description` | string | Não | Nova descrição |
| `institution_id` | string | Não | Nova instituição bancária |
| `archived` | boolean | Não | true = arquivar; false = desarquivar |
| `hide_balance` | boolean | Não | true = ocultar saldo na interface |

#### `organizze_update_budget`

Atualiza o valor de um orçamento existente. _(POST /api/organizze/update/budget)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `budget_id` | integer | Sim | ID do orçamento |
| `amount` | number | Sim | Novo valor em reais. Use 0 para remover. |

#### `organizze_update_category`

Atualiza uma categoria (nome, cor, pai, arquivar). _(POST /api/organizze/update/category)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `category` | string | Sim | Nome (preferido) ou id da categoria a atualizar |
| `name` | string | Não | Novo nome |
| `color` | string | Não | Nova cor hexadecimal |
| `parent` | string | Não | Nome ou id da nova categoria pai. '0' ou omitir = torná-la raiz |
| `archived` | boolean | Não | true = arquivar (ocultar); false = desarquivar |

#### `organizze_update_credit_card`

Atualiza um cartão de crédito (nome, bandeira, dias de fatura, limite, arquivar). _(POST /api/organizze/update/credit/card)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `credit_card` | string | Sim | Nome (preferido) ou UUID do cartão |
| `name` | string | Não | Novo nome |
| `flag` | string | Não | Nova bandeira |
| `billing_due_day` | integer | Não | Novo dia de vencimento (1-31) |
| `billing_cycle_day` | integer | Não | Novo dia de fechamento (1-31) |
| `limit` | number | Não | Novo limite em reais |
| `payment_account_id` | integer | Não | ID da conta de pagamento |
| `archived` | boolean | Não | true = arquivar; false = desarquivar |

#### `organizze_update_transaction`

Atualiza uma transação existente (informe só os campos a alterar). _(POST /api/organizze/update/transaction)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `transaction_id` | integer | Não | ID da transação (copiado de list/search/get nesta conexão) |
| `transaction_uuid` | string | Não | UUID da transação (copiado de list/search/get nesta conexão) |
| `description` | string | Não | Nova descrição |
| `amount` | number | Não | Novo valor (positivo = receita, negativo = despesa) |
| `date` | string | Não | Nova data (YYYY-MM-DD) |
| `category` | string | Não | Nome (preferido) ou id da nova categoria |
| `account` | string | Não | Nome (preferido) ou UUID da nova conta/cartão |
| `paid` | boolean | Não | Marcar como paga/recebida |
| `observation` | string | Não | Nova observação |
| `tags` | string | Não | Tags separadas por vírgula |
| `update_recurrence` | string | Não | Recorrentes: 'this_only', 'this_and_future' ou 'all' |

#### `organizze_update_transactions_categories`

Define a categoria de VÁRIAS transações em uma chamada, com destino por linha (máx. _(POST /api/organizze/update/transactions/categories)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `updates` | object[] | Sim | Lista {transaction_id|id|transaction_uuid|uuid, category (nome preferido)|category_id} (máx. 200). |

#### `organizze_update_transactions_descriptions`

Substitui a descrição de VÁRIAS transações em uma chamada (máx. _(POST /api/organizze/update/transactions/descriptions)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `updates` | object[] | Sim | Lista {transaction_id|id|transaction_uuid|uuid, description|new_description} (máx. 200). Aliases id/uuid vêm de list_transactions; camelCase também é aceito. |

---

Este MCP também funciona via **conexão MCP** (Claude / Cursor) em `https://api.mcp.ai/p_organizze` — veja o [README](../../README.md). A skill acima é pra consumir a **REST API** direto (agente próprio / código).
