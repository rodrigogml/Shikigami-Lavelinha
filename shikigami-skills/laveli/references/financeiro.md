# Overview

Playbook e orientações sobre a organização de lançamentos financeiros e contabeis da Laveli.

## Requisitos
- A empresa utiliza o ERP da Omie para lançamentos financeiros e contábeis.
	- Para integração com o Omie, use o perfil lógico `laveli` (ou o padrão se não houver opção de escolha) do MCP `Omie`. Esse perfil é um pré-requisito local: se não estiver configurado, oriente a configurá-lo na skill Omie e peça a intervenção necessária. Não suponha caminhos, crie credenciais, nem tente descobrir ou preencher configurações locais.

## Mandamentos

As instruções a seguir devem ser tratadas como mandamentos obrigatórios em toda a operação. Só podem ser sobrepostos com explicita autorização do usuário.
- A Omie é a fonte de verdade dos dados financeiros e contábeis.
- Memória deve ser utilizada para guardar instruções criadas a partir de definições e tomadas de decisões realizadas junto com o usuário no contexto atual, para serem utilizadas no futuro como 'memória' e melhor precisão nas tomadas de decisões autônomas futuras.
- Tabelas de dados devem ser utilizadas conforme as instruções de cada tabela para armazenar dados relevantes e temporários.

Omie:
- Lançamentos na Omie devem sempre ser criados com a flag de 'Conciliado' marcada como falso. Apenas o usuário pode alterar a conciliação para verdadeiro (conciliado).
- Ao alterar um lançamento já marcado como 'Conciliado' você voltar o lançamento para 'Não Conciliado'.

# Organização do Financeiro da Laveli

## Contas Financeiras

A Laveli utiliza as contas listadas a seguir de acordo com as definições descritas:
- **BTG** - Conta Corrente — `nCodCC: 8382785861` — Conta corrente principal da empresa, utilizada para o pagamento de boletos, pix, recebimentos de clientes, etc.
	- Banco: BTG (208), Agência: 0050, Conta Corrente: 558726-9.
- **Caixinha** — `nCodCC: 8382785479` — Usada para lançamentos de contas cujo fornecedor/prestador não consegue apresentar um documento fiscal válido para a contabilidade (nota, cuopm, fatura, RPA, etc.). Essa conta é utilizada para lançamentos em espécie ou outras movimentações que não bancárias;
- **Caixinha - MP** — `nCodCC: 8983541466` — Mesma finalidade/caso da conta `Caixinha`, porém essas trasações ocorrem em uma conta bancária do MP (Mercado Pago).
- **NuBank - Caixa Reserva** — `nCodCC: 8697957173` — Representa uma aplicação realizada no banco NuBank. De regra geral só deve conter movimentações de aporte, resgate, rendimentos, e lançamentos de impostos relacionados ao investimento como IRRF e IOF.
- **NuBank - MasterCard** — `nCodCC: 8785885875` — Representa o cartão da empresa. Os lançamentos são realizados conforme a compra: Lançando o valor total da compra na data da transação. Não lançamos as parcelas separadamente com datas futuras. O pagamento da fatura do cartão é lançado como uma transferência entre esta e a conta em que a fatura foi paga (Geralmenta o BTG).

- **As contas a seguir devem ser ignoradas, ainda que não estejam inativas no sistema não estão em uso atualmente e não devem ser consideradas em buscas, nem criar movimentações de nenhum tipo!**

# Operações Financeiras

## Conciliação de Contas

O processo de conciliação de contas consiste em conciliar lançamentos e saldo das contas financeiras do sistema e seus extratos.

### Regras Gerais

- Identificar corretamente a conta financeira do sistema a qual o relatório/extrato se relaciona.
- Limitar a análise ao extrato da instituição e aos lançamentos da mesma conta financeira no sistema.
- Comparar somente data, valor, natureza, descrição, beneficiário, identificadores e situação da conciliação.
- Não pesquisar internet, CNPJ, quadro societário, relação entre empresas, cadastros externos ou outras fontes.
- Quando os dados da conta e do extrato não forem suficientes, manter o grupo em “Com Erros” e solicitar a confirmação do usuário.
- Somente ampliar a investigação quando o usuário solicitar explicitamente.

### Triagem da Conciliação

Após comparar as movimentações da instituição com os lançamentos do sistema, exibir quatro tabelas:
- **Prontos**: grupos de conciliação confirmados, sem divergências.
- **Prontos - Sem Conciliação** - grupo de movimentações sem erros que faltam apenas a confirmação de conciliação do usuário no sistema.
- **Com Erros**: grupos prováveis, mas com divergência de data, valor, natureza, descrição, beneficiário, duplicidade ou outro problema.
- **Sem Correspondência**: movimentações ou lançamentos que não pertencem a nenhum grupo.

Um grupo pode relacionar uma ou mais movimentações da instituição com um ou mais lançamentos do sistema, em cardinalidade M:N, sendo M e N maiores ou iguais a 1.

Colunas das tabelas:
- Grupo
- Movimentações [Instituição]
- Lançamentos [Sistema]
- Total [Instituição]
- Total [Sistema]
- Diferença
- Situação ou erro encontrado

Em cada grupo, exibir individualmente:
- Instituição: data da movimentação, valor e descrição.
- Sistema: data do lançamento, valor, descrição ou beneficiário e ID.

Regras de exibição
- Usar datas no formato `DD/MM/AAAA`.
- Exibir valores em reais, usando `+` para créditos e `-` para débitos.
- Usar `—` quando não existir registro de um dos lados.
- Não incluir o mesmo registro em mais de um grupo ou tabela.
- Ordenar pela data mais antiga de cada grupo.
- Exibir os totais de cada lado e sua diferença.
- Grupois de Lançamentos detectados como 'Prontos' e que já estão conciliados não precisam e não devem ser explicitados na mensagem (salvo se perguntado). O foco é sempre no que ainda precisa ser conciliado e nos problemas. Não gaste tempo no que já está certo.

### Passos Sugeridos para Conciliação

Passos a serem sugeridos para concluir a conciliação.
De regra geral deve-se começar dos que parecem mais fáceis e óbvios, para eliminar os lançamentos e no fim trabalhar com os remanescentes.

#### Prontos - Sem Conciliação

Primeiro lote a ser exibido, os lançamentos que estão prontos e verificados, faltando apenas a marcação de conciliação do usuário no sistema.

#### Com Erros — Divergência Cadastral

- Analisar a divergência somente com os dados do extrato e dos lançamentos da conta no sistema.
- Quando data, natureza e valor coincidirem, apresentar o grupo como provável correspondência e destacar a diferença de descrição ou beneficiário.
- Não realizar pesquisa cadastral para tentar comprovar a relação.
- Mover o grupo para “Prontos — Sem Conciliação” somente após confirmação do usuário. Sem confirmação, manter o grupo em “Com Erros”.
- Liste todos os registros da seção, mas foque em resolver um por vez.
