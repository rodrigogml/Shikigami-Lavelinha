# Notas Mercado Livre

Explicação e operação para tratar notas de compras realizada no mercado livre.

# Introdução (Overview)

As compras do mercado livre costumam ser pagas das contas **Caixinha** ou **Caixinha - MP**, porque nunca sabemos se o vendedor enviará nota fiscal, ou mesmo se ela terá o valor inteiro do pagamento.
Como já é determinado pela organização da empresa, **lançamentos financeiros** que tenham documento contábil válido devem ser pagos da conta financeira **BTG** para que fique um registro contábil e saída equivalente na conta da empresa.
Somente depois que a compra é recebida, fazemos a análise se a nota fiscal foi emitida e seu valor para realizar o 'balanço dos valores' no sistema.

Geralmente as operações envolvendo compra do Mercado Livre ocorrem da seguinte forma:

1. As compras são realizadas com pagamentos no **Caixinha** ou **Caixinha - MP**. O **Lançamento Financeiro** é criado correto e completamente na conta em que foi realizado.
2. Se a compra não tiver documento contábil emitido, nada mais é feito e o lançamento já está pronto.
3. Se recebermos qualquer documento contábil válido (como nota, fatura, cupom, etc.) começamos o processo de 'balanço contábil' independente se a NFe veio no valor total e correto ou não. Utilizamos o valor da NFe e não da compra inicial:
	1. Se for uma NFe realizamos a entrada da NFe no sistema seguindo as instruções de entrada de nota normalmente, gerando o **Título a Pagar** da NFe.
	2. Liquidamos o **Título a Pagar** na conta do **BTG** na data em que o usuário solicitar.
	3. Criamos um lançamento de contra-partida com o mesmo valor da NFe e mesmos rateio nos departamentos utilizando a categoria de receita `Estorno de Compras do Caixinha Com Nota Fiscal (1.04.93)` na conta **Caixinha - MP**.
	4. O usuário efetivará um pagamento (transferência) entre a conta **BTG** e a **Caixinha - MP** para que ambos os extrados e saldos continuem corretos.
* No final da operação o esperado é que:
	* O lançamento inicial realizado na compra, com o valor real da compra, não sofre nenhuma alteração e continua declarando o valor real na conta e nos departamentos;
	* A entrada da NFe registra na contabilidade o documento contábil, no valor do documento, independente se veio igual ou diferente;
	* A saída equivalente ao valor do documento é movimentado da conta de lançamentos contábeis (**BTG**);
	* O valor declarado no ratio dos departamentos do documento contábil é anulado pelos mesmos valores lançados nos créditos da tranferência na conta não contábil (**Caixinhas**);
