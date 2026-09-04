# Ferramentas

Organizze expõe 62 ferramentas.

### 1. `organizze_list_accounts`
**Input**: nenhum input

Lista as contas do usuário com ids.

### 2. `organizze_list_credit_cards`
**Input**: nenhum input

Lista os cartões de crédito do usuário com ids e limites.

### 3. `organizze_list_categories`
**Input**: nenhum input

Lista as categorias usadas para classificar transações.

### 4. `organizze_list_transactions`
**Input**: `start_date` (opcional), `end_date` (opcional), `account_id` (opcional), `credit_card_id` (opcional), `category_id` (opcional), `order_by` (opcional), `list_mode` (opcional), `page` (opcional), `per_page` (opcional), `installments_only` (opcional), `account_ids` (opcional), `credit_card_ids` (opcional), `category_ids` (opcional)

Lista transações do usuário no período selecionado, com paginação (page/per_page; máximo 80 por página).

### 5. `organizze_list_transferences`
**Input**: `start_date` (opcional), `end_date` (opcional), `account_id` (opcional), `page` (opcional), `per_page` (opcional), `account_ids` (opcional)

Lista transferências entre contas com paginação (page/per_page).

### 6. `organizze_list_budgets`
**Input**: `year` (opcional), `month` (opcional)

Lista limites de gastos por categoria.

### 7. `organizze_get_credit_card_invoices`
**Input**: `credit_card_id`, `start_date` (opcional), `end_date` (opcional), `credit_card_ids` (opcional)

Lista faturas de um cartão. Informe credit_card_id. Bulk support: accepts credit_card_ids for batched execution.

### 8. `organizze_get_financial_summary`
**Input**: `start_date` (opcional), `end_date` (opcional), `year` (opcional), `month` (opcional)

Resumo financeiro do período (receitas, despesas, comparação, maiores gastos).

### 9. `organizze_search_transactions`
**Input**: `query`, `start_date` (opcional), `end_date` (opcional), `min_amount` (opcional), `max_amount` (opcional), `activity_type` (opcional), `page` (opcional), `per_page` (opcional), `tags_only` (opcional)

Busca transações por texto na descrição, observação ou tags (ex.: "iFood", "Uber", "salário").

### 10. `organizze_get_transaction`
**Input**: `transaction_id` (opcional), `transaction_uuid` (opcional), `transaction_ids` (opcional)

Detalhes completos de uma transação pelo id ou uuid.

### 11. `organizze_get_credit_card_invoice`
**Input**: `credit_card` (opcional), `credit_card_id` (opcional), `invoice_id` (opcional), `month` (opcional), `year` (opcional), `credit_card_ids` (opcional), `invoice_ids` (opcional)

Detalhes de uma fatura específica de cartão: transações, pagamentos e gastos por categoria.

### 12. `organizze_get_budget_summary`
**Input**: `month` (opcional), `year` (opcional)

Resumo dos limites de gastos do mês com alertas de budgets excedidos ou próximos do limite ("estou no controle?").

### 13. `organizze_get_income_vs_expenses`
**Input**: `start_date` (opcional), `end_date` (opcional), `periodicity` (opcional), `account_id` (opcional), `account_ids` (opcional)

Relatório de entradas vs saídas (receitas vs despesas) por período, em modo cashflow.

### 14. `organizze_transactions_list_results`
**Input**: `start_date` (opcional), `end_date` (opcional), `account_id` (opcional), `category_id` (opcional), `list_mode` (opcional), `account_ids` (opcional), `category_ids` (opcional)

Totais financeiros de um período (receitas, despesas, resultado, saldo, previstos) sem paginar transação por transação.

### 15. `organizze_suggest_categories`
**Input**: `items`

Sugere categorias para uma ou mais descrições de transação, usando as edições anteriores do próprio usuário, o histórico já categorizado e padrões globais.

### 16. `organizze_get_account_context`
**Input**: nenhum input

Orientação inicial em uma só chamada: entidade conectada, plano, data de hoje no fuso do usuário, e listas compactas (id ↔ nome) de contas, cartões e categorias.

### 17. `organizze_resolve_entity`
**Input**: `name`, `kind` (opcional)

Resolve um nome aproximado para o id/uuid real de conta, cartão ou categoria.

### 18. `organizze_get_upcoming_bills`
**Input**: `days` (opcional), `start_date` (opcional)

Contas a pagar dos próximos dias: lançamentos não pagos com vencimento na janela e faturas de cartão que vencem no período ("o que vence essa semana?").

### 19. `organizze_get_monthly_overview`
**Input**: `month` (opcional), `year` (opcional)

Visão geral do mês em uma só chamada: receitas, despesas, saldo, maiores categorias de despesa, status dos limites e contas a pagar dos próximos 7 dias.

### 20. `organizze_get_categories_report`
**Input**: `start_date` (opcional), `end_date` (opcional), `lens` (opcional)

Relatório de gastos e receitas agrupados por categoria (inclui compras de cartão; base POR LANÇAMENTO).

### 21. `organizze_get_categories_evolution`
**Input**: `start_date` (opcional), `end_date` (opcional), `periodicity` (opcional), `category_ids` (opcional), `only_parent_category` (opcional), `lens` (opcional)

Evolução de gastos/receitas por categoria ao longo do tempo (diário/semanal/mensal).

### 22. `organizze_get_tags_report`
**Input**: `start_date` (opcional), `end_date` (opcional), `tag_name` (opcional), `tag_name_prefix` (opcional)

Relatório de transações agrupadas por tag (despesas e receitas, com percentuais).

### 23. `organizze_get_invoices_matrix`
**Input**: `start_date` (opcional), `end_date` (opcional), `cards` (opcional)

Matriz de faturas (mês × cartão) com totais por mês, por cartão e total geral, usando os mesmos valores de get_credit_card_invoices.

### 24. `organizze_find_duplicates`
**Input**: `start_date` (opcional), `end_date` (opcional), `days_threshold` (opcional), `include_automatic` (opcional)

Encontra transações possivelmente duplicadas (mesmo valor e descrição similar em datas próximas).

### 25. `organizze_find_installments`
**Input**: `start_date` (opcional), `end_date` (opcional), `account` (opcional), `min_occurrences` (opcional)

Encontra COMPRAS PARCELADAS no período (use isto, não busca por texto).

### 26. `organizze_find_subscriptions`
**Input**: `start_date` (opcional), `end_date` (opcional), `account` (opcional), `min_occurrences` (opcional), `include_recurrences` (opcional)

Detecta ASSINATURAS e serviços recorrentes (Netflix, Spotify, ChatGPT, aluguel...).

### 27. `organizze_get_bank_connections`
**Input**: nenhum input

Lista as conexões bancárias (Open Finance / Conectado) ATIVAS e o status de cada uma (estável/indisponível).

### 28. `organizze_get_open_finance_status`
**Input**: `institution` (opcional)

Status AO VIVO do agregador Open Finance (Belvo), opcionalmente por banco.

### 29. `organizze_get_latest_imports`
**Input**: `limit` (opcional), `page` (opcional), `per_page` (opcional)

Transações mais recentemente importadas via Open Finance (contas/cartões conectados).

### 30. `organizze_get_balances`
**Input**: nenhum input

Saldo consolidado: saldo atual de cada conta, fatura atual em aberto de cada cartão e o patrimônio (contas − faturas atuais em aberto).

### 31. `organizze_get_cashflow_forecast`
**Input**: `days` (opcional), `start_date` (opcional)

Projeção de fluxo de caixa: parte do saldo atual e aplica os lançamentos não pagos de contas manuais na janela, retornando o saldo projetado ao fim e o menor ponto (risco de negativo).

### 32. `organizze_list_recurrences`
**Input**: nenhum input

Lista as contas fixas CADASTRADAS (lançamentos recorrentes/infinitos): aluguel, assinaturas, salário, etc., com periodicidade, valor e a próxima ocorrência.

### 33. `organizze_list_institutions`
**Input**: `query` (opcional), `limit` (opcional)

Catálogo de instituições financeiras (bancos/operadoras) com o id (slug) usado em institution_id ao criar/editar contas e cartões.

### 34. `organizze_load_skill`
**Input**: `skill_name`

Carrega um guia detalhado de uso (skill) sobre um tema do Organizze.

### 35. `organizze_create_transaction`
**Input**: `description`, `amount`, `date`, `account`, `category` (opcional), `is_income`, `done` (opcional), `observation` (opcional), `tags` (opcional), `times` (opcional), `recurring` (opcional), `periodicity` (opcional), `idempotency_key` (opcional)

Cria uma transação (receita ou despesa).

### 36. `organizze_create_account`
**Input**: `name`, `institution_id` (opcional), `initial_balance` (opcional), `description` (opcional), `institution_ids` (opcional)

Cria uma conta bancária manual.

### 37. `organizze_inform_invoice_payment`
**Input**: `credit_card`, `invoice_date_or_id` (opcional), `date` (opcional), `observation` (opcional), `invoice_date_or_ids` (opcional)

Informa pagamento de fatura em cartão automático (Open Finance).

### 38. `organizze_update_transaction`
**Input**: `transaction_id` (opcional), `transaction_uuid` (opcional), `description` (opcional), `amount` (opcional), `date` (opcional), `category` (opcional), `account` (opcional), `paid` (opcional), `observation` (opcional), `tags` (opcional), `update_recurrence` (opcional), `transaction_ids` (opcional)

Atualiza uma transação existente (informe só os campos a alterar).

### 39. `organizze_delete_transaction`
**Input**: `transaction_id` (opcional), `transaction_uuid` (opcional), `delete_recurrence` (opcional), `transaction_ids` (opcional)

Exclui uma transação. Transações automáticas (Open Finance) não podem ser excluídas. Para recorrentes use delete_recurrence. O transaction_id/uuid DEVE ser copiado de list_transactions, search_transactions ou get_tran…

### 40. `organizze_set_transaction_paid`
**Input**: `paid`, `transaction_id` (opcional), `transaction_uuid` (opcional), `transaction_ids` (opcional)

Marca uma transação como paga/recebida (paid=true) ou não paga (paid=false) em conta MANUAL ("já paguei o aluguel").

### 41. `organizze_create_transfer`
**Input**: `amount`, `date`, `credit_account`, `debit_account`, `description` (opcional), `done` (opcional), `observation` (opcional), `tags` (opcional), `recurring` (opcional), `periodicity` (opcional)

Cria uma transferência entre duas contas bancárias (suporta recorrência).

### 42. `organizze_register_invoice_payment`
**Input**: `credit_card`, `source_account` (opcional), `amount`, `date`, `invoice_date_or_id` (opcional), `observation` (opcional), `invoice_date_or_ids` (opcional)

Registra o pagamento de uma fatura de cartão MANUAL como lançamento, vinculado à fatura.

### 43. `organizze_uninform_invoice_payment`
**Input**: `credit_card`, `invoice_date_or_id`, `invoice_date_or_ids` (opcional)

Desfaz um "Informar Pagamento" em cartão AUTOMÁTICO (marcador aguardando Open Finance).

### 44. `organizze_create_category`
**Input**: `name`, `kind`, `group_id` (opcional), `color` (opcional), `parent_id` (opcional), `group_ids` (opcional), `parent_ids` (opcional)

Cria uma categoria de despesa ou receita (raiz ou subcategoria via parent_id).

### 45. `organizze_update_category`
**Input**: `category`, `name` (opcional), `color` (opcional), `parent` (opcional), `archived` (opcional)

Atualiza uma categoria (nome, cor, pai, arquivar).

### 46. `organizze_delete_category`
**Input**: `category`, `substitute_category` (opcional)

Exclui uma categoria; suas transações vão para outra categoria (substitute_category ou padrão).

### 47. `organizze_create_budget`
**Input**: `category`, `amount`, `activity_type` (opcional), `month` (opcional), `year` (opcional)

Cria um limite de gastos (orçamento) para uma categoria.

### 48. `organizze_update_budget`
**Input**: `budget_id`, `amount`, `budget_ids` (opcional)

Atualiza o valor de um orçamento existente.

### 49. `organizze_delete_budget`
**Input**: `budget_id`, `budget_ids` (opcional)

Remove um orçamento (limite de gastos) de uma categoria.

### 50. `organizze_clone_budgets`
**Input**: `month` (opcional), `year` (opcional)

Clona os orçamentos do mês anterior para o mês de destino (não sobrescreve um mês que já tenha orçamentos).

### 51. `organizze_update_account`
**Input**: `account`, `name` (opcional), `description` (opcional), `institution_id` (opcional), `archived` (opcional), `hide_balance` (opcional), `institution_ids` (opcional)

Atualiza uma conta (nome, descrição, instituição, arquivar).

### 52. `organizze_delete_account`
**Input**: `account`

Exclui uma conta. ATENÇÃO: todas as transações da conta são excluídas em background. Contas automáticas não podem ser excluídas (desconecte pelo menu de conexões).

### 53. `organizze_create_credit_card`
**Input**: `name`, `billing_due_day`, `billing_cycle_day`, `flag` (opcional), `limit` (opcional), `payment_account_id` (opcional), `payment_account_ids` (opcional)

Cria um cartão de crédito MANUAL (automáticos vêm da conexão bancária).

### 54. `organizze_update_credit_card`
**Input**: `credit_card`, `name` (opcional), `flag` (opcional), `billing_due_day` (opcional), `billing_cycle_day` (opcional), `limit` (opcional), `payment_account_id` (opcional), `archived` (opcional), `payment_account_ids` (opcional)

Atualiza um cartão de crédito (nome, bandeira, dias de fatura, limite, arquivar).

### 55. `organizze_delete_credit_card`
**Input**: `credit_card`

Exclui um cartão de crédito. ATENÇÃO: todas as transações e faturas do cartão são excluídas. Cartões automáticos não podem ser excluídos (desconecte pelo menu de conexões).

### 56. `organizze_mass_create_transactions`
**Input**: `transactions`, `idempotency_key` (opcional)

Cria VÁRIAS transações em lote (máx.

### 57. `organizze_mass_update_transactions`
**Input**: `query` (opcional), `start_date` (opcional), `end_date` (opcional), `category_id` (opcional), `account_id` (opcional), `credit_card_id` (opcional), `activity_type` (opcional), `tag_filter` (opcional), `paid_filter` (opcional), `new_category` (opcional), `new_description` (opcional), `description_find` (opcional), `description_replace` (opcional), `new_observation` (opcional), `new_tags` (opcional), `tag_action` (opcional), `new_paid` (opcional), `category_ids` (opcional), `account_ids` (opcional), `credit_card_ids` (opcional)

Atualiza VÁRIAS transações por FILTRO de busca (máx.

### 58. `organizze_mass_delete_transactions`
**Input**: `transactions`

Exclui VÁRIAS transações por lista de ids/uuids (máx.

### 59. `organizze_add_transactions_tags`
**Input**: `items`

Adiciona tags a VÁRIAS transações em uma chamada (acumula sobre as existentes; máx.

### 60. `organizze_update_transactions_categories`
**Input**: `updates`

Define a categoria de VÁRIAS transações em uma chamada, com destino por linha (máx.

### 61. `organizze_update_transactions_descriptions`
**Input**: `updates`

Substitui a descrição de VÁRIAS transações em uma chamada (máx.

### 62. `organizze_mass_manage_categories`
**Input**: `operations`

Aplica VÁRIAS mudanças na árvore de categorias em uma chamada atômica (máx.

## Prompts de exemplo

```
Como foi meu mês? Receitas, despesas e saldo
Quais assinaturas recorrentes eu tenho?
Quanto ainda tenho pra pagar essa semana?
```
