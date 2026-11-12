# Conciliação de Contas Financeiras

O processo de conciliação de contas financeiras consiste em equiparar movimentações de uma conta financeira do sistema com um extrato (ou relatório ou fatura) da instituição financeira.


# Regras Gerais

- Antes de começar é obrigatório identificar e relacionar corretamente a conta financeira do sistema com a qual o relatórioextrato se relaciona.
- Limitar a análise entre o extrato da instituição e aos **Lançamentos Financeiros** da mesma **Conta Financeira** no sistema.
- Comparar somente data, valor, natureza, descrição, beneficiário, identificadores e situação da conciliação.
- Não pesquisar internet, CNPJ, quadro societário, relação entre empresas, cadastros externos ou outras fontes. Somente ampliar a investigação quando o usuário solicitar explicitamente.
- Usar datas no formato `DD/MM/AAAA`.
- Exibir valores usando `+` para créditos e `-` para débitos.
- Não incluir o mesmo lançamento em mais de um grupo ou tabela.
- Jamais altere o status de conciliação de 'Não Conciliado' para 'Conciliado'. Essa flag indica que o usuário já validou a execução da sua operação no sistema.

# Glossário

- **Lançamento Financeiro:** registro individual de movimentação financeira.
- **Movimentação Financeira:** valor financeiro movimentado, sendo composto por um ou mais **Lançamentos Financeiros**.
- **Grupo de Lançamentos:** conjunto de **Lançamentos Financeiros** que compõe a **Movimentação Financeira**.

# Regras da Conciliação

- As **Movimentações Financeiras** tem somente uma única data. Ou seja, todos os lançamentos financeiros que compõe a **Movimentação Financeira** devem ter a mesma data.
- A data considera é sempre a data do **Lançamento Financeiro**, nunca outras datas que o objeto do sistema (ou objetos relacionados) possam ter, como data de vencimento, data de baixa/liquidação, data de conciliação, etc.
- A somatória dos lançamentos utilizados no **Grupo de Lançamentos** do sistema equivale a somatória do **Grupo de Lançamentos** relacionados no extrato.
- Cada **Lançamento Financeiro** deve ser utilizado uma única vez em um **Grupo de Lançamentos** para a conciliação. O uso de um lançamento mais de uma vez, ou de nenhuma vez, invalida a conciliação.
- **Lançamentos Financeiros** marcados como conciliado no sistema indicam lançamentos já validados pelo usuário. Isso quer dizer que sua data e valor já foram 'afirmados' que pertem a um **Grupo Financeiro** daquela data. Não pode ser mais considerado para uso em outra data.
	- Uma exceção de último caso, é que mesmo que o **Lançamento Financeiro** esteja conciliado e pertence a um período fora do que o extrato recebido represa - o que não permite garantir se ele foi mesmo utilizado em conciliação de outra movimentação - pode ser sugerido para o usuário, caso seu valor ou outros indícios de que a conciliação possa estar errada e ele fecha a **Movimentação Financeira** do período do extrato corrente.

# Workflow da Conciliação

Siga os passos a seguir até completar a conciliação.

## Triagem da Conciliação

- Leia todos os lançamentos do extrato recebido.
- Busque o sistema lançamentos financeiros na conta relacionada para estabelecer movimentações financeiras equivalentes entre o sistema e o extrato.
	* um grupo de lançamentos da conta financeira do sistema pode se relacionar com um grupo de lançamentos do extrado em cardinalidade M:N, sendo M e N maiores ou iguais a 1.
- O objetivo é garantir que as movimentações financeiras apresentadas no extrato existam no sistema na mesma proporção de valores (sem sobrar ou faltar lançamentos financeiros) para o período do extrato.

## Categorização das Movimentações Financeiras

As **Movimentações Financeiras** são categorizadas da seguinte forma para organizarmos a conciliação:
- **Prontos - Conciliados**: grupos de movimentações que atendem a todas as **Regas da Conciliação** e com os **Lançamentos Financeiros** já marcados como conciliados no sistema.
- **Prontos - Não Conciliados** - grupo de movimentações que atendem a todas as **Regras da Concialização** mas ainda não marcados como conciliados no sistema.
- **Com Erros**: grupos prováveis, mas com divergência de data, valor, natureza, descrição, beneficiário, duplicidade ou outro problema.
- **Sem Correspondência**: movimentações ou lançamentos que não pertencem a nenhum grupo.

## Workflow do Tratamento dos Lançamentos com o Usuário

1. Apresentar um totalizador dos lançamentos em cada categoria no formato abaixo. Jamais liste todos os lançamentos de uma única vez.
	```
	**Prontos e Conciliados:** [Total de Lançamentos Nessa Categoria]
	**Prontos, mas Não Conciliados:** [Total de Lançamentos Nessa Categoria]
	**Com Erros:** [Total de Lançamentos Nessa Categoria]
	**Sem Correspondência:** [Total de Lançamentos Nessa Categoria]
	```

2. Estabelece a ordem de tratamento das **Movimentações Financeiras** detectadas na ordem das categorias: `Prontos - Não Conciliados`, `Com Erros` e `Sem Correspondência`. Seguindo a ordem cronológica (da mais antiga para a mais nova).

3. Exibe as **Movimentações Financeiras** conciliadas na Triagem uma a uma no formato a seguir. Justificando as escolhas e motivos do agrupamento. 
	* Em caso de múltiplos **Lançamentos Financeiros** que possam satisfazer as mesmas **Movimentações Financeiras** exibir os detalhes.
	```
	**Data: ** [Data considerando os lançamentos do extrato]
	**Valor Total:** [Valor total da Movimentação Financeira quando a conciliação utilizar mais de 1 lançamento no extrato ou no sistema]
	**Extrato** [#A linhas a seguir se repete para cada lançamento financeiro do extrato utilizado para conciliar a movimentação financeira]
		- [Valor] - [Descrição no Extrato] 
	**Sistema**  [#A linhas a seguir se repete para cada lançamento financeiro do sistema utilizado ou sugerido para conciliar a movimentação financeira]
		- [Data] - [Valor] - [C|NC para indicar respectivamente se já conciliado ou não] - [Outros detalhes e observações, quando necessário, que justificam a escolha]	
	```	

4. Para os casos de Erros e Sem correspondência, deve ser realizada uma busca de referências no sistema para entender e sugerir a melhor opção de solução de conciliação. Os passos a serem seguidos são:
	1. Procurar nos últimos 90 dias para considerar lançamentos similares em categoria, contraparte, descrição que possam dar pistas de como criar esse lançamento;
	2. Busca de Notas Fiscais que possam estar relacioandas ao lançamento, considerando valores (exatos ou não), data próximas, contraparte, etc.;
	3. De acordo com as informações encontradas e analisadas, considere, ordene e exibas as opções encontradas para o usuário.
		1. Sugerir dar a entrada da Nota pendente, se encontrada;
		2. Dar baixa em nota ainda pendente de quitação de pagamento para conciliar com o lançamento do extrato;
		3. Corrigir o lançamento de quitação de uma nota, ainda que marcado como conciliado em período fora do analisado em que não se pôde confirmar a que a conciliação esteja correta. Neste caso destacar que o lançamento já está consolidado em que não está no período sendo fechado, a alteração da data pode 'desconciliar' outro período já fechado.
		4. Sugerir a criação de novo lançamento financeiro (ou transferência entre contas financeiras) seguindo as regras de **Lançamentos Financeiros** em `references/lançamentosFinanceiros.md`.
	
5. Somente após a consolidação da **Movimentação Financeira** avançar para a próxima. Não cite, mencione ou avance para a próxima conciliação enquanto a atual não for finalizada, ou o usuário solicitar para pular a atual. Uma vez que uma conciliação seja confirmada com sucesso, já avance para a próxima.
