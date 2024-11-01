# Métricas de avaliação para comparação de modelos

### **Discussão:** 

O objetivo do presente estudo é elencar e adotar a melhor solução para identificação de outliers. Portanto, faz-se míster avaliar e comparar
o desempenho de diferentes modelos de identificação de outliers, especificamente para os dados hidrológicos das cotas da bacia amazônica, 
disponibilizados pela Agência Nacional de Águas (ANA). <br>

Para avaliar os diferentes modelos, faz-se necessário analisar a natureza dos dados e da tarefa de identificação dos outliers. <br> 

Os dados hidrológicos são dados reais, coletados manualmente em estações fluviométricas da bacia amazônica. Tal característica permite a presença de erros grosseiros,
decorrentes de diversos possíveis problemas na coleta. Para os dados em questão, **a presença de falsos negativos é a maior problemática**, visto que a não identificação de 
outliers pode trazer prejuízos semânticos aos usuários que utilizarão, no futuro, os dados corrigidos. 
A presença de falsos positivos não deixa de ser prejudicial, mas estes devem ser menos relevantes na avaliação dos diferentes modelos. <br> 
 
No que se refere à natureza da tarefa de identificação de outliers, é possível afirmar que, tipicamente, existe uma quantidade maior de dados "normais" em comparação com outliers
em uma distribuição genérica. Tal comportamento é chamado de **desbalanceamento**. Existem métricas que podem ser burladas quando existe grande desbalanceamento
no conjunto de dados analisado.

### **Métricas de avaliação em modelos de classificação:**

No presente estudo, as seguintes métricas de avaliação para modelos de classificação foram analisadas:

- Acurácia: Mede a proporção de previsões corretas entre todas as previsões feitas. É útil quando as classes estão balanceadas, mas pode ser enganosa em casos de dados desbalanceados.

- Precisão (Precision): Mede a proporção de verdadeiros positivos entre todas as previsões positivas. É importante quando se quer minimizar falsos positivos, ou seja, quando os casos positivos errados são custosos.

- Recall: Mede a proporção de verdadeiros positivos entre todos os exemplos que realmente são positivos. É útil quando se quer minimizar falsos negativos, principalmente em casos em que perder um positivo é crítico.

- F1 Score: É a média harmônica entre precisão e recall. Essa métrica equilibra ambos e é útil quando há um trade-off entre as duas e as classes estão desbalanceadas.

- AUC ROC: Representa a área sob a curva ROC, que é o gráfico da taxa de verdadeiros positivos (recall) contra a taxa de falsos positivos. Quanto mais próximo de 1, melhor a performance do modelo em distinguir as classes.

- AUC PR: Representa a área sob a curva de Precisão-Recall, útil em contextos de classes desbalanceadas, pois foca mais nos verdadeiros positivos. É especialmente útil para avaliar modelos onde o foco é nos casos positivos.

- Especificidade: representa a taxa de verdadeiros negativos entre todas as previsões negativas. É análoga à precisão, quando se quer minimizar falsos positivos.

A tabela abaixo identifica as fórmulas para algumas das métricas acima:

<br>

<div align="center">

|Método  | 	Fórmula|
|---   |   ---|
|Recall   |   VP / (VP+FN)|
|Especificidade   |   VN / (FP+VN)|
|Acurácia   |   (VP+VN) / N|
|Precisão   |   VP / (VP+FP)|
|F-score    |   2 x (Precisão x Recall) / (Precisão + Recall)|
  
</div>

<br>

Considerando a natureza dos dados hidrológicos fluviométricos e a tarefa de identificação de outliers, as métricas ideais para o problema devem ter como objetivo minimizar
a quantidade de falsos negativos (em maior peso) e falsos positivos (em menor peso), enquanto lida com dados desbalanceados. Portanto, **as métricas escolhidas, em ordem de relevância, foram: Recall, AUC PR e F1 Score.** <br>

O Recall é uma métrica que penaliza explicitamente a presença de falsos negativos e não é muito afetada por dados desbalanceados. Já as métricas AUC PR e F1 Score são bastante adequadas ao se considerar o equilíbrio entre a relevância dos falsos negativos (FN) e falsos positivos(FP). Dessa forma, o presente estudo propõe a utilização das três métricas combinadas, com pesos adequados à relevância de cada métrica.

Assim, a métrica de avaliação final do modelo, denotada por Avaliação Ponderada (AP), pode ser calculada segundo a seguinte formulação:

<div align="center">
 
$AP = \frac{5 x Recall + 2,5 * (AUC PR + F1 Score)}{10}$

</div>

### **Conclusão:**

Com base na análise das características dos dados hidrológicos e da tarefa de identificação de outliers, o presente estudo propõe uma abordagem de Avaliação Ponderada (AP) que prioriza a minimização de falsos negativos, dado o impacto negativo que eles podem gerar na qualidade dos dados corrigidos. A métrica de **Recall** foi escolhida como prioridade devido à sua sensibilidade em capturar verdadeiros positivos, essencial para evitar falsos negativos. As métricas **AUC PR** e **F1 Score** complementam essa avaliação ao equilibrar a importância de minimizar tanto falsos negativos quanto falsos positivos, sem que o desbalanceamento dos dados prejudique a análise. A fórmula de **Avaliação Ponderada (AP)** apresentada reflete a relevância de cada métrica e serve como uma medida abrangente para comparação dos modelos, de forma a identificar aquele com o melhor desempenho na detecção de outliers para dados hidrológicos.
