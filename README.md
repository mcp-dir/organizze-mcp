# Organizze

### Organizze para Claude, ChatGPT e agentes de IA

Suas finanças do Organizze por linguagem natural: saldos consolidados, visão do mês, busca de lançamentos, faturas de cartão, contas fixas, orçamentos, relatórios por categoria e tag, projeção de fluxo de caixa e status do Open Finance. Conecte sua conta em um clique, sem chave nem token. Na tela do Organizze, desmarque "Permitir alterações" se quiser acesso somente leitura.

- 📊 **62 ferramentas**
- ✏️ **Leitura e escrita**
- 💬 **Funciona com qualquer cliente MCP**: Claude Desktop, Cursor, VS Code, Cline, Continue
- 🔑 **Login via magic-link (sem senha)**

[English version](README.en.md) · [Documentação completa](docs/) · [Skill pra agentes](skills/)

---

## Instalar em 1 clique

### Claude (Web e Desktop)

A Anthropic unificou a instalação de MCPs em `claude.ai/customize/connectors`. **O mesmo link serve pra Claude Web e Claude Desktop** (basta estar logado):

[➕ Abrir no Claude e conectar](https://claude.ai/new?modal=add-custom-connector#settings/customize-connectors)

**Manual** (se o deeplink não abrir): [claude.ai/customize/connectors](https://claude.ai/customize/connectors?surface=cowork) → **+** → **Adicionar conector personalizado** → cole **Nome** `Organizze` e **URL** `https://api.mcp.ai/p_organizze`.

### Cursor

[➕ Instalar Organizze no Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=organizze&config=eyJ1cmwiOiJodHRwczovL2FwaS5tY3AuYWkvcF9vcmdhbml6emUifQ==)

### VS Code (Copilot Chat)

[➕ Instalar Organizze no VS Code](vscode:mcp/install?name=organizze&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.mcp.ai%2Fp_organizze%22%7D)

### ChatGPT, Manus, OpenClaw e mais 40+ clientes

Funciona em qualquer cliente MCP que suporte **MCP over HTTP**. A URL do servidor é sempre:

```
https://api.mcp.ai/p_organizze
```

Detalhes por cliente: [INSTALL.md](INSTALL.md).

---

## Exemplos de uso

```
Como foi meu mês? Receitas, despesas e saldo
Quais assinaturas recorrentes eu tenho?
Quanto ainda tenho pra pagar essa semana?
```

---

## 62 ferramentas disponíveis

| Tool | Descrição |
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

Detalhe de cada tool: [docs/ferramentas.md](docs/ferramentas.md)

---

## Preços

Grátis.

---

## Privacidade & LGPD

- **Sub-processadores**: Organizze, o LLM host que você escolher (Claude, ChatGPT, Cursor, agente próprio). Lista completa em [docs/privacidade-lgpd.md](docs/privacidade-lgpd.md).
- Os dados retornados pelas tools são enviados ao **LLM host que você escolher**, sub-processador fora do nosso controle. Recomendamos planos com opt-out de treinamento.

---

## Perguntas frequentes

**O servidor é open source?**
O servidor é proprietário (hosted). Este repositório é o wrapper público com manifestos, docs e skills — tudo MIT.

**Posso usar com agente próprio (não Claude/Cursor)?**
Sim — qualquer cliente que suporte MCP over HTTP. URL: `https://api.mcp.ai/p_organizze`.


---

## Suporte

- 📧 [organizze@mcp.ai](mailto:organizze@mcp.ai)
- 🐛 [GitHub Issues](https://github.com/mcp-dir/organizze-mcp/issues)
- 📄 [docs/](docs/)

---

## Licença

MIT — veja [LICENSE](LICENSE). O servidor MCP em `api.mcp.ai/p_organizze` é proprietário (hosted); este repositório (manifestos, docs, skills) é MIT.
