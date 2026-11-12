# Overview

Playbook e orientações sobre a organização de lançamentos financeiros e contabeis da Laveli.

# Requisitos
- A empresa utiliza o ERP da Omie para lançamentos financeiros e contábeis.
	- Para integração com o Omie, use o perfil lógico `laveli` (ou o padrão se não houver opção de escolha) do MCP `Omie`. Esse perfil é um pré-requisito local: se não estiver configurado, oriente a configurá-lo na skill Omie e peça a intervenção necessária. Não suponha caminhos, crie credenciais, nem tente descobrir ou preencher configurações locais.
	- Sempre que uma chamada ao MCP da Omie falhar ou retornar erro que não seja esperado (diferente de chamadas de validação ou que você reconheça o erro de chamada) exiba para o usuário uma mensagem de relatório explicado exatamente a chamada e os parâmetros passados.

# Mandamentos

As instruções a seguir devem ser tratadas como mandamentos obrigatórios em toda a operação. Só podem ser sobrepostos com explicita autorização do usuário.
- A Omie é a fonte de verdade dos dados financeiros e contábeis.
- Memória deve ser utilizada para guardar instruções criadas a partir de definições e tomadas de decisões realizadas junto com o usuário no contexto atual, para serem utilizadas no futuro como 'memória' e melhor precisão nas tomadas de decisões autônomas futuras.
- Tabelas de dados, por meio do MCP `data_tables`, devem ser utilizadas durante a execução das tarefas do financeiro conforme suas instruções:
	- `laveli_financial_rules.department_rules` mantém os departamentos ativos da Omie. Em cada sincronização, atualize ou inclua os departamentos retornados como ativos e exclua os registros locais cujos códigos não estejam mais ativos na Omie. Durante a inclusão, questione o usuário para confirmar sua sugestão de instrução de uso do departamento.
	- A tabela `laveli_financial_rules.category_rules` do schema `laveli_financial_rules` contém informações relevantes de uso e relação de preenchimento entre os campos `categoria`, `projeto`, `contraparte` e `departamento`.

Omie:
- Lançamentos na Omie devem sempre ser criados com a flag de 'Conciliado' marcada como falso. Apenas o usuário pode alterar a conciliação para verdadeiro (conciliado).
- Ao alterar um lançamento já marcado como 'Conciliado' você voltar o lançamento para 'Não Conciliado'.
- Departamentos 'nós' (que tem departamentos filhos) jamais devem ser utilizados nos lançamentos.

# Organização do Financeiro da Laveli x Omie

## Glossário

- **Título a pagar / receber:** objeto da obrigação ou do direito financeiro. Consulte títulos pelos métodos `omie.financeiro.contas_pagar.listar_por_vencimento`, `omie.financeiro.contas_pagar.consultar`, `omie.financeiro.contas_receber.listar_por_vencimento` e `omie.financeiro.contas_receber.consultar`. Identifique o título por `codigo_lancamento_omie` ou `nCodTitulo` e use `valor_documento` como valor documental.

- **Lançamento financeiro:** objeto que registra a movimentação efetiva em uma conta financeira. Consulte-o pelos métodos `omie.financeiro.lancamentos_conta.listar_por_pagamento` e `omie.financeiro.lancamentos_conta.consultar`. Identifique-o por `nCodLanc`; use `cabecalho.nCodCC`, `cabecalho.nValorLanc`, `cabecalho.dDtLanc` e `diversos.cNatureza` para conta, valor, data e natureza.

- **Vínculo título–lançamento:** no lançamento financeiro, `diversos.nCodLancCP` referencia o título a pagar e `diversos.nCodLancCR` referencia o título a receber. Use esses campos para rastrear o lançamento financeiro equivalente a um título.

- **Baixa / pagamento / recebimento:** é o evento que liquida o título. Não trate campos vazios de pagamento no retorno do título como prova de ausência de baixa. Confirme sempre o lançamento financeiro vinculado em `omie.financeiro.lancamentos_conta.consultar`.

- **Lista de movimentos:** os métodos `omie.financeiro.movimentos.listar_por_inclusao`, `omie.financeiro.movimentos.listar_por_alteracao`, `omie.financeiro.movimentos.listar_por_pagamento`, `omie.financeiro.movimentos.listar_por_vencimento` e `omie.financeiro.movimentos.listar_por_previsao` são consultas auxiliares por data e podem retornar dados resumidos de títulos e baixas. Não os use isoladamente para identificar o lançamento financeiro da conta; confirme-o em `omie.financeiro.lancamentos_conta.consultar`.


## Contas Financeiras

A Laveli utiliza as contas listadas a seguir de acordo com as definições descritas:
- **BTG** - Conta Corrente — `nCodCC: 8382785861` — Conta corrente principal da empresa, utilizada para o pagamento de boletos, pix, recebimentos de clientes, etc.
	- Banco: BTG (208), Agência: 0050, Conta Corrente: 558726-9.
- **Caixinha** — `nCodCC: 8382785479` — Usada para lançamentos de contas cujo fornecedor/prestador não consegue apresentar um documento fiscal válido para a contabilidade (nota, cuopm, fatura, RPA, etc.). Essa conta é utilizada para lançamentos em espécie ou outras movimentações que não bancárias;
- **Caixinha - MP** — `nCodCC: 8983541466` — Mesma finalidade/caso da conta `Caixinha`, porém essas trasações ocorrem em uma conta bancária do MP (Mercado Pago).
- **NuBank - Caixa Reserva** — `nCodCC: 8697957173` — Representa uma aplicação realizada no banco NuBank. De regra geral só deve conter movimentações de aporte, resgate, rendimentos, e lançamentos de impostos relacionados ao investimento como IRRF e IOF.
- **NuBank - MasterCard** — `nCodCC: 8785885875` — Representa o cartão da empresa. Os lançamentos são realizados conforme a compra: Lançando o valor total da compra na data da transação. Não lançamos as parcelas separadamente com datas futuras. O pagamento da fatura do cartão é lançado como uma transferência entre esta e a conta em que a fatura foi paga (Geralmenta o BTG).

- **As contas a seguir devem ser ignoradas, ainda que não estejam inativas no sistema não estão em uso atualmente e não devem ser consideradas em buscas, nem criar movimentações de nenhum tipo!**


# Operações Financeiras e Contábeis

## Lançamentos Financeiros

Antes de criar ou alterar **Lançamentos Financeiros** leia as regras definidas `references/lançamentosFinanceiros.md`.


## Conciliação de Contas Financeiras

Para realizar conciliação de contas finaceiras siga as instruções de `references/conciliaçãoDeContasFinanceiras.md`.


## Entrada de Notas

Para realizar entrade de notas siga as instruções de `references/entradaNotas.md`.