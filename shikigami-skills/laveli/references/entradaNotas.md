# Entrada de Notas

## Roteiro para Entrada/Confirmação de NFe

1. Conferir a NF-e importada, seu emissor, itens, parcelas e se já existe entrada/título para a mesma nota.
2. Quando o emissor não estiver cadastrado na Omie, cadastrá-lo automaticamente como fornecedor usando os dados fiscais da NF-e.
3. Validar os itens: descrição, quantidade, valor final, CFOP de Saída -> CFOP de entrada (tratamento fiscal).
	- A soma dos valores finais dos itens deve ser exatamente igual ao valor total da NF-e.
4. Definir uma única categoria financeira para a NF-e inteira, adequada à natureza da compra.
5. Definir um único projeto para a NF-e inteira.
6. Definir os departamentos por item. Cada item deve indicar seu departamento; se um item precisar de mais de um departamento, expor seu rateio individual. A soma dos rateios dos departamentos deve ser exatamente igual ao valor total da NF-e.
7. Antes de concluir, apresentar o formulário de validação no formato a seguir:
	```
	**NFe:** [Número da NFe] | **Valor:** [Valor da NFe]
	**Emissor:** [Nome do Emissor (CPF/CNPJ)]
	**Projeto:** [Nome do Projeto]
	**Conta:** [Conta Financeira]
	**Item:** [Nome do item - Esse Bloco se repete para cada item da nota]
		**Valor:** [Valor total do Item - que somados representam o valor total da nota]
		**Departamento:** [Nome do departamento considerado para esse item]
		**CFOP Saída:** (CFOP e nome na nota)
		**CFOP Entrada:** (CFOP de entrada considerado para o produto com o tratamento fiscal)
	**Ratio Departamentos:**
		**[Nome do Departamento]:** [valor / percentual ratiado nesse departamento - uma linha para cada departamento considerado]
	```

8. Após aprovação, atualizar e concluir a entrada da NF-e, gerando o título de contas a pagar.

9. A quitação do título é uma etapa posterior que não faz parte da entrada/confirmação da nota. Pode ser considerada junto das ações de conciliação, pagamento e outras. Não realizar a quitação sem autorização ou instrução como parte da entrada da nota. Título “atrasado” pode ser esperado quando a nota possui vencimento anterior à data atual; isso não é motivo, por si só, para quitá-lo.


## Etapas da NFe

A Omie controla a 'Etapa da NFe' por números do atributo `etapa`, sendo:
 - Faturada pelo Fornecedor;
60 - Recebido;
80 - Conferido;

