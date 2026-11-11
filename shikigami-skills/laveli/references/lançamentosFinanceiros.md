# Como Criar Lançamentos Financeiros

Regras para criação de lançamentos financeiros.

# Lançamentos Financeiros

- Todo lançamento financeiro precisa da Categoria definida.
- A categoria determina as regras de preenchimento dos campos `projeto`, `counterparty` e `department`.
- Antes de criar ou validar o lançamento, o agente deve consultar o registro da categoria na tabela `category_rules` do schema `laveli_financial_rules`, por meio do MCP `data_tables`.
- O agente deve usar `usage_scope` para confirmar se a categoria é adequada ao lançamento e aplicar `project_rule`, `counterparty_rule` e `department_rule`:
  - `required`: o campo deve ser preenchido com um valor válido;
  - `prohibited`: o campo não pode ser preenchido;
  - `not_validated`: a tabela não impõe validação ao campo.
- Se não houver regra cadastrada para a categoria, ou se `usage_scope` não for suficiente para confirmar a categoria, o agente deve solicitar orientação ao usuário antes de criar o lançamento.
