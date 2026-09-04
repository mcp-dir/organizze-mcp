# Organizze

### Organizze for Claude, ChatGPT and AI agents

Your Organizze finances in natural language: consolidated balances, monthly overview, transaction search, credit card invoices, recurring bills, budgets, reports by category and tag, cash flow forecast and Open Finance status. Connect your account in one click, no key or token. On the Organizze screen, uncheck "Permitir alterações" if you want read-only access.

- 📊 **62 tools**
- ✏️ **Read and write**
- 💬 **Works with any MCP client**: Claude Desktop, Cursor, VS Code, Cline, Continue
- 🔑 **Magic-link login (no password)**

[Versão em português](README.md) · [Full docs (PT-BR)](docs/) · [Agent skill](skills/)

---

## One-click install

### Claude (Web and Desktop)

Anthropic unified MCP installation at `claude.ai/customize/connectors`. **The same link works for Claude Web and Claude Desktop** (just be logged in):

[➕ Open in Claude and connect](https://claude.ai/new?modal=add-custom-connector#settings/customize-connectors)

**Manual** (if the deeplink does not open): [claude.ai/customize/connectors](https://claude.ai/customize/connectors?surface=cowork) → **+** → **Add custom connector** → paste **Name** `Organizze` and **URL** `https://api.mcp.ai/p_organizze`.

### Cursor

[➕ Install Organizze in Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=organizze&config=eyJ1cmwiOiJodHRwczovL2FwaS5tY3AuYWkvcF9vcmdhbml6emUifQ==)

### VS Code (Copilot Chat)

[➕ Install Organizze in VS Code](vscode:mcp/install?name=organizze&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.mcp.ai%2Fp_organizze%22%7D)

### ChatGPT, Manus, OpenClaw and 40+ other clients

Works with any MCP client that speaks **MCP over HTTP**. The server URL is always:

```
https://api.mcp.ai/p_organizze
```

Per-client details: [INSTALL.md](INSTALL.md).

---

## Example prompts

```
How was my month? Income, expenses and balance
Which recurring subscriptions do I have?
How much do I still have to pay this week?
```

---

## 62 tools available

| Tool | Description |
|---|---|
| `organizze_list_accounts` | Lista as contas do usuário com ids. |
| `organizze_list_credit_cards` | Lista os cartões de crédito do usuário com ids e limites. |
| `organizze_list_categories` | Lista as categorias usadas para classificar transações. |
| `organizze_list_transactions` | Lista transações do usuário no período selecionado, com paginação (page/per_page; máximo 80 por página). |
| `organizze_list_transferences` | Lista transferências entre contas com paginação (page/per_page). |
| `organizze_list_budgets` | Lista limites de gastos por categoria. |
| `organizze_get_credit_card_invoices` | Lista faturas de um cartão. Informe credit_card_id. Bulk support: accepts credit_card_ids for batched execution. |
| `organizze_get_financial_summary` | Resumo financeiro do período (receitas, despesas, comparação, maiores gastos). |
| `organizze_search_transactions` | Busca transações por texto na descrição, observação ou tags (ex.: "iFood", "Uber", "salário"). |
| `organizze_get_transaction` | Detalhes completos de uma transação pelo id ou uuid. |
| `organizze_get_credit_card_invoice` | Detalhes de uma fatura específica de cartão: transações, pagamentos e gastos por categoria. |
| `organizze_get_budget_summary` | Resumo dos limites de gastos do mês com alertas de budgets excedidos ou próximos do limite ("estou no controle?"). |
| `organizze_get_income_vs_expenses` | Relatório de entradas vs saídas (receitas vs despesas) por período, em modo cashflow. |
| `organizze_transactions_list_results` | Totais financeiros de um período (receitas, despesas, resultado, saldo, previstos) sem paginar transação por transação. |
| `organizze_suggest_categories` | Sugere categorias para uma ou mais descrições de transação, usando as edições anteriores do próprio usuário, o histórico já categorizado e padrões globais. |
| `organizze_get_account_context` | Orientação inicial em uma só chamada: entidade conectada, plano, data de hoje no fuso do usuário, e listas compactas (id ↔ nome) de contas, cartões e categorias. |
| `organizze_resolve_entity` | Resolve um nome aproximado para o id/uuid real de conta, cartão ou categoria. |
| `organizze_get_upcoming_bills` | Contas a pagar dos próximos dias: lançamentos não pagos com vencimento na janela e faturas de cartão que vencem no período ("o que vence essa semana?"). |
| `organizze_get_monthly_overview` | Visão geral do mês em uma só chamada: receitas, despesas, saldo, maiores categorias de despesa, status dos limites e contas a pagar dos próximos 7 dias. |
| `organizze_get_categories_report` | Relatório de gastos e receitas agrupados por categoria (inclui compras de cartão; base POR LANÇAMENTO). |
| `organizze_get_categories_evolution` | Evolução de gastos/receitas por categoria ao longo do tempo (diário/semanal/mensal). |
| `organizze_get_tags_report` | Relatório de transações agrupadas por tag (despesas e receitas, com percentuais). |
| `organizze_get_invoices_matrix` | Matriz de faturas (mês × cartão) com totais por mês, por cartão e total geral, usando os mesmos valores de get_credit_card_invoices. |
| `organizze_find_duplicates` | Encontra transações possivelmente duplicadas (mesmo valor e descrição similar em datas próximas). |
| `organizze_find_installments` | Encontra COMPRAS PARCELADAS no período (use isto, não busca por texto). |
| `organizze_find_subscriptions` | Detecta ASSINATURAS e serviços recorrentes (Netflix, Spotify, ChatGPT, aluguel...). |
| `organizze_get_bank_connections` | Lista as conexões bancárias (Open Finance / Conectado) ATIVAS e o status de cada uma (estável/indisponível). |
| `organizze_get_open_finance_status` | Status AO VIVO do agregador Open Finance (Belvo), opcionalmente por banco. |
| `organizze_get_latest_imports` | Transações mais recentemente importadas via Open Finance (contas/cartões conectados). |
| `organizze_get_balances` | Saldo consolidado: saldo atual de cada conta, fatura atual em aberto de cada cartão e o patrimônio (contas − faturas atuais em aberto). |
| `organizze_get_cashflow_forecast` | Projeção de fluxo de caixa: parte do saldo atual e aplica os lançamentos não pagos de contas manuais na janela, retornando o saldo projetado ao fim e o menor ponto (risco de negativo). |
| `organizze_list_recurrences` | Lista as contas fixas CADASTRADAS (lançamentos recorrentes/infinitos): aluguel, assinaturas, salário, etc., com periodicidade, valor e a próxima ocorrência. |
| `organizze_list_institutions` | Catálogo de instituições financeiras (bancos/operadoras) com o id (slug) usado em institution_id ao criar/editar contas e cartões. |
| `organizze_load_skill` | Carrega um guia detalhado de uso (skill) sobre um tema do Organizze. |
| `organizze_create_transaction` | Cria uma transação (receita ou despesa). |
| `organizze_create_account` | Cria uma conta bancária manual. |
| `organizze_inform_invoice_payment` | Informa pagamento de fatura em cartão automático (Open Finance). |
| `organizze_update_transaction` | Atualiza uma transação existente (informe só os campos a alterar). |
| `organizze_delete_transaction` | Exclui uma transação. Transações automáticas (Open Finance) não podem ser excluídas. Para recorrentes use delete_recurrence. O transaction_id/uuid DEVE ser copiado de list_transactions, search_transactions ou get_tran… |
| `organizze_set_transaction_paid` | Marca uma transação como paga/recebida (paid=true) ou não paga (paid=false) em conta MANUAL ("já paguei o aluguel"). |
| `organizze_create_transfer` | Cria uma transferência entre duas contas bancárias (suporta recorrência). |
| `organizze_register_invoice_payment` | Registra o pagamento de uma fatura de cartão MANUAL como lançamento, vinculado à fatura. |
| `organizze_uninform_invoice_payment` | Desfaz um "Informar Pagamento" em cartão AUTOMÁTICO (marcador aguardando Open Finance). |
| `organizze_create_category` | Cria uma categoria de despesa ou receita (raiz ou subcategoria via parent_id). |
| `organizze_update_category` | Atualiza uma categoria (nome, cor, pai, arquivar). |
| `organizze_delete_category` | Exclui uma categoria; suas transações vão para outra categoria (substitute_category ou padrão). |
| `organizze_create_budget` | Cria um limite de gastos (orçamento) para uma categoria. |
| `organizze_update_budget` | Atualiza o valor de um orçamento existente. |
| `organizze_delete_budget` | Remove um orçamento (limite de gastos) de uma categoria. |
| `organizze_clone_budgets` | Clona os orçamentos do mês anterior para o mês de destino (não sobrescreve um mês que já tenha orçamentos). |
| `organizze_update_account` | Atualiza uma conta (nome, descrição, instituição, arquivar). |
| `organizze_delete_account` | Exclui uma conta. ATENÇÃO: todas as transações da conta são excluídas em background. Contas automáticas não podem ser excluídas (desconecte pelo menu de conexões). |
| `organizze_create_credit_card` | Cria um cartão de crédito MANUAL (automáticos vêm da conexão bancária). |
| `organizze_update_credit_card` | Atualiza um cartão de crédito (nome, bandeira, dias de fatura, limite, arquivar). |
| `organizze_delete_credit_card` | Exclui um cartão de crédito. ATENÇÃO: todas as transações e faturas do cartão são excluídas. Cartões automáticos não podem ser excluídos (desconecte pelo menu de conexões). |
| `organizze_mass_create_transactions` | Cria VÁRIAS transações em lote (máx. |
| `organizze_mass_update_transactions` | Atualiza VÁRIAS transações por FILTRO de busca (máx. |
| `organizze_mass_delete_transactions` | Exclui VÁRIAS transações por lista de ids/uuids (máx. |
| `organizze_add_transactions_tags` | Adiciona tags a VÁRIAS transações em uma chamada (acumula sobre as existentes; máx. |
| `organizze_update_transactions_categories` | Define a categoria de VÁRIAS transações em uma chamada, com destino por linha (máx. |
| `organizze_update_transactions_descriptions` | Substitui a descrição de VÁRIAS transações em uma chamada (máx. |
| `organizze_mass_manage_categories` | Aplica VÁRIAS mudanças na árvore de categorias em uma chamada atômica (máx. |

Details for each tool: [docs/ferramentas.md](docs/ferramentas.md) (PT-BR)

---

## Pricing

Free.

---

## Privacy & data protection

- **Sub-processors**: Organizze, the LLM host you choose (Claude, ChatGPT, Cursor, your own agent). Full list in [docs/privacidade-lgpd.md](docs/privacidade-lgpd.md).
- Data returned by the tools is sent to **the LLM host you choose**, a sub-processor outside our control. We recommend plans with training opt-out.

---

## FAQ

**Is the server open source?**
The server is proprietary (hosted). This repository is the public wrapper with manifests, docs and skills, all MIT.

**Can I use it with my own agent (not Claude/Cursor)?**
Yes, any client that speaks MCP over HTTP. URL: `https://api.mcp.ai/p_organizze`.


---

## Support

- 📧 [organizze@mcp.ai](mailto:organizze@mcp.ai)
- 🐛 [GitHub Issues](https://github.com/mcp-dir/organizze-mcp/issues)
- 📄 [docs/](docs/)

---

## License

MIT — see [LICENSE](LICENSE). The MCP server at `api.mcp.ai/p_organizze` is proprietary (hosted); this repo (manifests, docs, skills) is MIT.
