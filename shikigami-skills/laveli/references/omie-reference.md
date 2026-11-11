# Manual Operacional da Omie

Referência de procedimentos validados para operações na Omie que exigem mais de uma chamada ou possuem diferenças entre o objeto financeiro e sua movimentação na conta.

## Regras Gerais

- Identifique primeiro o objeto que precisa ser alterado: título, baixa, recebimento ou movimento da conta corrente.
- Leia o registro atual antes de alterá-lo e preserve os campos que não fazem parte da correção.
- Use o identificador interno retornado pela leitura; não reconstrua nem suponha identificadores.
- Envie `confirm: true` somente após conferir o objeto, os valores e os campos obrigatórios.
- Não considere a mensagem de sucesso suficiente: releia o registro após a escrita e confirme o campo alterado.
- Se a escrita retornar sucesso, mas a leitura continuar igual, não repita a escrita. Verifique se o objeto correto foi escolhido.
- Se uma leitura retornar HTTP 500, tente novamente uma única vez. Se o erro persistir, interrompa a operação e informe a falha.
- Nunca repita uma mutação quando o resultado estiver incerto.
- Apenas o usuário pode marcar um lançamento como conciliado.
- Alterar um lançamento conciliado pode devolvê-lo ao estado não conciliado; verifique a situação antes da escrita.

## Objetos Financeiros Relacionados

- **Título a pagar:** representa o documento ou obrigação financeira. É tratado pelas operações `omie_payables_*`.
- **Movimento da conta corrente:** representa o valor efetivamente movimentado em uma conta financeira. É tratado pelas operações `omie_account_transactions_*`.
- Um título pago pode possuir um movimento de conta com valor diferente do valor documental, por exemplo quando o pagamento inclui juros, multa ou outro ajuste.
- Alterar o título não garante que o movimento da conta seja atualizado.
- Quando o objetivo for corrigir o valor usado na conciliação bancária, altere o movimento da conta corrente.

## How-To: Corrigir o Valor de um Movimento da Conta

Use este procedimento quando o extrato da instituição estiver correto e o valor do movimento da mesma conta estiver incorreto na Omie.

### 1. Localizar o movimento

Consulte os movimentos financeiros da conta e do período com `omie_financial_movements_list`.

Considere como movimento de conta o registro que possui `nCodMovCC`. Registre:

- `nCodMovCC`
- Conta financeira
- Data
- Valor
- Categoria
- Tipo de documento
- Situação da conciliação

### 2. Consultar o movimento diretamente

Use `omie_account_transactions_get` com o identificador no campo `nCodLanc`:

```text
params:
  nCodLanc: <nCodMovCC>
```

Na resposta, confira:

- `cabecalho.nCodCC`
- `cabecalho.dDtLanc`
- `cabecalho.nValorLanc`
- `detalhes.cCodCateg`
- `detalhes.cTipo`
- `diversos.dDtConc`
- `diversos.nCodLancCP` ou `diversos.nCodLancCR`, quando presentes

Se `diversos.dDtConc` estiver preenchido, não altere sem autorização explícita, pois a alteração pode remover a conciliação.

### 3. Alterar o movimento

Use `omie_account_transactions_update` com o corpo simplificado do MCP:

```text
body:
  id: <nCodLanc>
  account_id: <nCodCC>
  date: <DD/MM/AAAA>
  amount: <novo valor positivo>
  category_code: <cCodCateg>
  document_type: <cTipo>
confirm: true
```

Para movimentos de natureza `P`, informe `amount` como valor positivo. A natureza do movimento preserva o sentido de débito.

Inclua somente os demais campos aceitos pela ferramenta quando eles também precisarem ser alterados.

### 4. Confirmar a alteração

Consulte novamente com `omie_account_transactions_get` e confirme:

- `cabecalho.nValorLanc` contém o novo valor.
- Conta, data, categoria e tipo de documento foram preservados.
- `diversos.dDtConc` continua vazio quando o movimento não estava conciliado.
- `info.dAlt` e `info.hAlt` refletem a alteração.

Considere a operação concluída somente após essa leitura.

## How-To: Distinguir Título e Movimento Antes de Corrigir

1. Consulte o título com a operação `get` do módulo correspondente.
2. Consulte o movimento da conta com `omie_account_transactions_get`.
3. Compare o objetivo da correção:
   - Corrigir o valor documental da obrigação: atuar no título.
   - Corrigir o valor efetivamente movimentado e conciliado: atuar no movimento da conta.
4. Não altere os dois objetos automaticamente. Cada alteração deve responder a uma inconsistência comprovada.
5. Após a alteração, releia o mesmo objeto que foi modificado.

## Falhas Conhecidas e Recuperação

### Resposta de sucesso sem persistência

Uma operação pode responder com sucesso e a leitura posterior continuar mostrando o valor anterior.

- Não considere a alteração concluída.
- Não repita imediatamente a mesma mutação.
- Confirme se foi alterado o título quando o objetivo era alterar o movimento, ou o inverso.
- Consulte o objeto correto e refaça a operação somente com dados atuais.

### HTTP 500 em consultas

- Trate como falha de consulta, não como prova de que o registro não existe.
- Repita a leitura uma única vez.
- Não repita uma escrita por causa do erro de leitura.
- Se a falha persistir, interrompa o procedimento e informe que o resultado não pôde ser confirmado.

### Parâmetros do movimento

- `omie_account_transactions_get` consulta pelo campo `nCodLanc`.
- O valor retornado como `nCodMovCC` em movimentos financeiros corresponde ao identificador usado como `nCodLanc` na consulta direta.
- `omie_account_transactions_update` usa os nomes simplificados `id`, `account_id`, `date`, `amount`, `category_code` e `document_type`, não a estrutura bruta `cabecalho` e `detalhes` da API oficial.

## Modelo para Novos How-Tos

Registre somente procedimentos executados e confirmados.

Cada novo How-To deve informar:

- Situação em que se aplica
- Objeto correto da Omie
- Leitura inicial necessária
- Identificador utilizado
- Campos obrigatórios da escrita
- Necessidade de `confirm: true`
- Leitura de confirmação
- Condição de parada em caso de erro
- Efeitos sobre a conciliação
