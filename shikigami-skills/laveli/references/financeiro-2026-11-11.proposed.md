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

##### Roteiros para movimentações com erros

###### Movimentações sem par no sistema

1. Verificar se o extrato tem informações sobre nota, favorecido, mesagens com detalhes sobre a transação que possam ser utilizadas.
2. Em caso de Nota, procurar pela nota utilizando número, referência do favorecido ou mesmo valor que possam ser utilizadas.
	- Note que notas já com baixa de pagamento e com pagamentos liquidados marcados como conferidos podem ser avisados ao usuário mas não alteradas, pois conciliação indica que o usuário já conferiu com o lançamento de outra data.



## Regras de Campos para Lançamentos Financeiros

Antes de criar ou validar um lançamento financeiro, localize sua categoria nesta tabela e aplique as regras de Projeto, Contraparte e Departamento.

- \`O\` — obrigatório: o campo deve ter um valor válido antes do lançamento.
- \`P\` — proibido: não preencher nem enviar o campo.
- \`-\` — não validado: o campo pode ser enviado ou omitido; a categoria não impõe regra.
- O uso descrito delimita a escolha da categoria e não substitui a conferência dos dados do lançamento.
- O código Omie só pode ser preenchido após consulta à Omie. Enquanto estiver como \`Pendente de cadastro\`, não inferir ou reutilizar um código de outra categoria.

| Categoria | Quando utilizada | Código da categoria Omie | Projeto | Contraparte | Departamento |
| --- | --- | --- | :---: | :---: | :---: |
| Água e Esgoto | Despesas de fornecimento de água ou esgoto vinculadas a uma obra. | Pendente de cadastro | O | - | P |
| Aluguel de Máquinas e Ferramentas | Locação de máquinas ou ferramentas usadas em obra. | Pendente de cadastro | O | - | O |
| Aporte de Investidores nos Empreendimentos | Entrada de capital de investidor destinada a empreendimento específico. | Pendente de cadastro | O | O | P |
| Aporte em Empreendimentos Próprios | Aporte próprio da empresa em empreendimento. | Pendente de cadastro | P | P | P |
| Aquisição de Terreno | Compra de terreno para um empreendimento. | Pendente de cadastro | O | - | P |
| Aumento de Capital Social | Integralização ou aumento formal de capital social. | Pendente de cadastro | P | - | P |
| Cartórios | Serviços, taxas ou emolumentos cartorários. | Pendente de cadastro | - | - | P |
| Cashback de Cartões e Outros Programas | Crédito de cashback recebido de cartão ou programa de benefícios. | Pendente de cadastro | - | P | - |
| Certificado Digital | Aquisição, renovação ou manutenção de certificado digital. | Pendente de cadastro | P | - | P |
| Clientes - Administração de Obras | Receita ou cobrança por administração de obras. | Pendente de cadastro | P | - | P |
| Clientes - Serviços Prestados | Receita por serviços prestados a cliente. | Pendente de cadastro | P | O | P |
| Combustível | Despesas de combustível. | Pendente de cadastro | P | - | P |
| Comissões e Repasses de Faturamento | Pagamento de comissão ou repasse relacionado ao faturamento. | Pendente de cadastro | P | O | P |
| Compra de Máquinas e Equipamentos para Locação | Aquisição de ativo destinado à locação. | Pendente de cadastro | P | - | P |
| Compra de Veículos | Aquisição de veículos da empresa. | Pendente de cadastro | P | - | P |
| Compras de Materia Prima / Material de Construção | Compra de materiais para uso em obra. | Pendente de cadastro | O | - | O |
| Condomínio | Despesas condominiais de imóvel ou empreendimento. | Pendente de cadastro | O | O | P |
| Construtora/Engenheiro Responsável | Pagamento à construtora ou ao engenheiro responsável pela obra. | Pendente de cadastro | O | O | P |
| Contabilidade | Serviços contábeis. | Pendente de cadastro | P | O | P |
| Correções / Quebra de Caixa (Débito) | Ajuste de débito por correção ou quebra de caixa. | Pendente de cadastro | - | - | - |
| Correções / Quebra de Caixa (Crédito) | Ajuste de crédito por correção ou quebra de caixa. | Pendente de cadastro | - | - | - |
| Despachante de Veículos | Serviços de despachante relacionados a veículos. | Pendente de cadastro | P | - | P |
| CREA/SP | Taxas, anuidades ou serviços relacionados ao CREA/SP. | Pendente de cadastro | P | - | P |
| Custos de Escritório | Despesas operacionais do escritório. | Pendente de cadastro | O | - | P |
| DARF 1708 - Retenção IRRF Sobre Remuneração Serviços Pgto a PJ | Recolhimento de IRRF retido sobre remuneração de serviços pagos a pessoa jurídica. | Pendente de cadastro | O | P | O |
| DARF 5952 - Retenção Contribuições Pgto PJ | Recolhimento de contribuições retidas em pagamentos a pessoa jurídica. | Pendente de cadastro | O | P | O |
| Despesas com Cursos, Feiras, Workshops e Afins | Capacitação, feiras, workshops e despesas correlatas. | Pendente de cadastro | P | - | P |
| Empreiteiro | Pagamento de mão de obra ou serviços de empreiteiro em obra. | Pendente de cadastro | O | O | O |
| Empréstimos Particulares | Operações de empréstimo com pessoa física ou parte identificada. | Pendente de cadastro | P | O | P |
| Energia Elétrica da Obra | Fornecimento de energia elétrica vinculado a uma obra. | Pendente de cadastro | O | - | P |
| Entrada de Transferência | Entrada decorrente de transferência entre contas. | Pendente de cadastro | P | - | P |
| Equipamentos de Informática | Aquisição ou despesa com equipamentos de informática. | Pendente de cadastro | P | - | P |
| Equipamentos de Segurança e EPIs | Compra de equipamentos de segurança ou EPIs para obra. | Pendente de cadastro | O | - | P |
| INSS / eSocial | Recolhimentos e obrigações de INSS ou eSocial. | Pendente de cadastro | - | O | P |
| IPTU | Pagamento de IPTU de imóvel ou empreendimento. | Pendente de cadastro | O | - | P |
| IPVA | Pagamento de IPVA de veículo. | Pendente de cadastro | P | - | P |
| IRRF de Aplicações Financeiras | IRRF incidente sobre aplicações financeiras. | Pendente de cadastro | P | - | P |
| ISS | Recolhimento de ISS relacionado a obra ou serviço. | Pendente de cadastro | O | - | P |
| Manutenção de Equipamentos de Locação | Manutenção de equipamentos destinados à locação. | Pendente de cadastro | P | - | P |
| Manutenção de Veículos | Manutenção de veículos da empresa. | Pendente de cadastro | P | - | P |
| Máquinas e Equipamentos para Uso | Aquisição de máquinas ou equipamentos para uso interno. | Pendente de cadastro | P | - | P |
| Material de Escritório | Compra de material de escritório. | Pendente de cadastro | - | - | P |
| Multas | Pagamento de multas. | Pendente de cadastro | P | - | P |
| Papelaria | Despesas de papelaria. | Pendente de cadastro | P | - | P |
| Pendências (Crédito) | Ajuste de pendência financeira de crédito. | Pendente de cadastro | - | - | - |
| Pendências (Débito) | Ajuste de pendência financeira de débito. | Pendente de cadastro | - | - | - |
| Projetos e Consultorias | Serviços de projeto ou consultoria vinculados a obra. | Pendente de cadastro | O | O | O |
| Pró-labores | Pagamento de pró-labore. | Pendente de cadastro | P | O | P |
| Receita - Locação de Equipamentos | Receita de locação de equipamentos. | Pendente de cadastro | P | - | P |
| Reembolso de Despesas | Reembolso genérico de despesas. | Pendente de cadastro | - | - | P |
| Reembolso Despesas Sócios a Serviço da Empresa | Reembolso a sócio por despesa realizada em nome da empresa. | Pendente de cadastro | - | O | P |
| Registro Domínios de Internet (registro.br ou internacionais) | Registro ou renovação de domínios de internet. | Pendente de cadastro | P | - | P |
| Rendimentos de Aplicações | Rendimentos recebidos de aplicações financeiras. | Pendente de cadastro | P | - | P |
| Saída de Transferência | Saída decorrente de transferência entre contas. | Pendente de cadastro | P | - | P |
| Seguro de Veículos | Seguro relacionado a veículo da empresa. | Pendente de cadastro | P | - | P |
| Serviços Especializados na Obra | Serviço especializado executado para uma obra. | Pendente de cadastro | O | - | O |
| Simples Nacional (DAS) | Recolhimento do DAS do Simples Nacional. | Pendente de cadastro | P | - | P |
| Taxas da Prefeitura Relacionadas a Obra | Taxas municipais vinculadas a uma obra. | Pendente de cadastro | O | - | O |
| Transporte e Logística | Transporte ou logística vinculados a uma obra. | Pendente de cadastro | O | - | - |
| Devoluções de Compra de Material de Consumo | Devolução de compra de material de consumo. | Pendente de cadastro | - | - | - |
| Estorno de Valores | Estorno de lançamento ou valor financeiro. | Pendente de cadastro | - | - | - |
| Frete | Frete sem regra adicional de campos nesta categoria. | Pendente de cadastro | - | - | - |
| Transferências de Saldo Caixinha para Aporte | Transferência de saldo da Caixinha destinada a aporte. | Pendente de cadastro | P | - | P |
| Valores a Reembolsar | Valor registrado como pendente de reembolso. | Pendente de cadastro | - | - | - |
