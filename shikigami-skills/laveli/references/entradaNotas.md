# Entrada de Notas

## Roteiro para Entrada/Confirmação de NFe

1. Conferir a NF-e importada, seu emissor, itens, parcelas e se já existe entrada/título para a mesma nota.
2. Quando o emissor não estiver cadastrado na Omie, cadastrá-lo automaticamente como fornecedor usando os dados fiscais da NF-e.
3. Validar os itens: descrição, quantidade, valor final, CFOP de Saída -> CFOP de entrada (tratamento fiscal).
	- A soma dos valores finais dos itens deve ser exatamente igual ao valor total da NF-e.
4. Definir uma única categoria financeira para a NF-e inteira, adequada à natureza da compra.
5. Definir um único projeto para a NF-e inteira.
6. Definir os departamentos por item. Cada item deve indicar seu departamento; se um item precisar de mais de um departamento, expor seu rateio individual. A soma dos rateios dos departamentos deve ser exatamente igual ao valor total da NF-e.

7. Antes de concluir, apresentar o formulário de validação com:
- dados gerais da NF-e;
- parcela(s);
- categoria;
- projeto;
- itens com descrição, valor final, CFOP de entrada — código e descrição — e departamento/rateio;
- resumo final dos departamentos.

8. Após aprovação, atualizar e concluir a entrada da NF-e, gerando o título de contas a pagar.

9. A quitação do título é uma etapa posterior: ela cria o lançamento financeiro vinculado na conta utilizada, na data efetiva identificada. Não criar lançamento financeiro avulso para NF-e.