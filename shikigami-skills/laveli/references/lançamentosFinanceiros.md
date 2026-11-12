# Lançamentos Financeiros

Regras dos lançamentos financeiros.

# Regras do Lançamentos Financeiros

- Todo lançamento financeiro precisa da Categoria definida.
- A categoria determina as regras de preenchimento dos campos `projeto`, `counterparty` e `department`.
- Antes de criar ou validar o lançamento, o agente deve consultar o registro da categoria na tabela `category_rules` do schema `laveli_financial_rules`, por meio do MCP `data_tables`.
- O agente deve usar `usage_scope` para confirmar se a categoria é adequada ao lançamento e aplicar `project_rule`, `counterparty_rule` e `department_rule`:
  - `required`: o campo deve ser preenchido com um valor válido;
  - `prohibited`: o campo não pode ser preenchido;
  - `not_validated`: a tabela não impõe validação ao campo.
- Se não houver regra cadastrada para a categoria, ou se `usage_scope` não for suficiente para confirmar a categoria, o agente deve solicitar orientação ao usuário antes de criar o lançamento.
- **Lançamentos Financeiros** vinculados a **Títulos de Pagamento/Recebimento** devem ter seus valores coerentes com os pagamentos/liquidações desses títulos.
- **Lançamentos Financeiros** relacionados a Notas fiscais não devem ser criados 'soltos'. Devem sempre seguir o relacionamento: Entrada da Nota -> Título de Contas a Pagar -> Quitação da Conta com Lançamento Financeiro.
- O campo número de documento só pode ser utilizado para número de NFe, Fatura, e outros números até 20 caracteres. Códigos de barra e representação numéricas de Boleto e outras guias devem ser salvas no campo de observações.

# Criação de Lançamentos Financeiros

Para criar um **Lançamento Financeiro** siga os seguintes passos:
1. Levante todos os dados necessários;
2. Avalie a categoria em que o **Laçamento Financeiro** se encaixa, considerando sua natureza de Crédito/Débito;
3. Valide o preenchimento dos campos `projeto`, `counterparty` e `department` seguindo as regras de validação definidos na tabela `category_rules` citadas acima.
4. Exiba o lançamento a ser criado para aprovação do usuário sempre no formato do formulário a seguira. Mesmo que alguns campos ainda não estejam prontos ou sejam alterados, sempre exiba o formulário completo para que se for aprovado não seja necessário exibir outra mensagem com o formulário para confirmação.
   ```
   **Data:** [formato dd/mm/aaaa]
   **Valor:** [utilizando + ou - para indicar crédito ou débito]
   **Categoria:** [nome da categoria] (ID da categoria no Omie)
   **Contra Parte:** [nome do favorecido/cliente] (ID da contraparte no Omie]
   **Projeto:** [nome do projeto] (ID do projeto no Omie]
   **Departamento:** [nome do departamento] (ID do departamento no Omie)
   **Conta:** [nome da conta financeira] (ID da conta financeira no Omie)
   **Tipo do Documento:** [Nome do tipo do documento]
   **Número do Documento:** [Número do documento se existir]
   **Observações:** [Inclua as observações com as informações pertinentes que deram origem ao lançamento, e no final a frase 'Criado por Lavelinha em dd/mm/aaaa']
   ```
   * Quando os campos do formulário modelo não tiverem valor, mantenha-os no formulário e complete com o texto "--" para indicar 'sem preenchimento'.
5. Após confirmação crie o lançamento com os dados aprovados, sempre deixando o novo lançamento no estado sem conciliação.


# Criação de Lançamentos Financeiros Pendentes

- A ausência de alguma nota fiscal (para as contas que existem), alguma pendência ou informação que impeça o lançamento completo, realizamos os lançamentos da melhor forma que podemos mas deixamos a categoria **Pendências (Débito)** ou **Pendências (Crédito)** de acordo com a movimentação.
- No campo de observações marcamos todas as pendências a serem resolvidas para finalizar aquele lançamento.
- Assim, qualquer lançamento com essas categorias indicam que ainda têm alguma pendência.