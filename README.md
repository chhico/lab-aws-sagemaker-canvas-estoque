# 📊 Previsão de Estoque Inteligente na AWS com [SageMaker Canvas](https://aws.amazon.com/pt/sagemaker/canvas/)

Bem-vindo ao desafio de projeto "Previsão de Estoque Inteligente na AWS com SageMaker Canvas. Neste Lab DIO, você aprenderá a usar o SageMaker Canvas para criar previsões de estoque baseadas em Machine Learning (ML). Siga os passos abaixo para completar o desafio!

## 🎯 Objetivos Deste Desafio de Projeto (Lab)

![image](https://github.com/digitalinnovationone/lab-aws-sagemaker-canvas-estoque/assets/730492/72f5c21f-5562-491e-aa42-2885a3184650)

## 🚀 Passo a Passo

### 1. Selecionar Dataset

-   Escolhi o arquivo "dataset-1000-com-preco-promocional-e-renovacao-estoque.csv" para utilizar como base para criar meu dataset, nele foi inserido mais uma coluna chamada de "DIA_SEMANA" para com base na coluna ja existente "DATA_EVENTO" saber qual foi o dia da semana afim de verificar se o dia da semana influenccia na quantidade do estoque.
  
-   Eu utilizei o chatGPT para fazer a criação e preenchimento da nova coluna, fiz o upload do arquivo escolhido no item anterior e adicionei o seguinte script: "Dada planilha onde existe a coluna chamada "DATA_EVENTO" crie uma nova coluna chamada "DIA_SEMANA" e preencha com o valor do dia da semana em português com base na data da coluna "DATA_EVENTO" e disponibilize a planilha com a nova coluna para que eu possa fazer o download". <img src="https://github.com/chhico/lab-aws-sagemaker-canvas-estoque/blob/main/img/ChatGPT%20incluindo%20nova%20coluna.png" alt="ChatGPT" height="330" width="600">

-   Fiz o download do arquivo e utilizei como meu Dataset final.
-   Dataset original <img src="https://github.com/chhico/lab-aws-sagemaker-canvas-estoque/blob/main/img/Dataset%20previo.png" alt="Dataset original" height="380" width="310">
-   Dataset final <img src="https://github.com/chhico/lab-aws-sagemaker-canvas-estoque/blob/main/img/Dataset%20final.png" alt="Dataset original" height="380" width="360">


### 2. Construir/Treinar

-   Criei o modelo do tipo "Preditivo"(Controle de Estoques). <img src="https://github.com/chhico/lab-aws-sagemaker-canvas-estoque/blob/main/img/Criar%20Novo%20Modelo.png" alt="Criação do Modelo" height="330" width="600">

-   Fiz o upload do Dataset "local"(dataset-1000-com-preco-promocional-diasemana-e-renovacao-estoque)
-   Configure as variáveis de entrada e saída de acordo com os dados.<img src="https://github.com/chhico/lab-aws-sagemaker-canvas-estoque/blob/main/img/Build.png" alt="Definindo o build" height="450" width="1000">
  
-   Inicie o treinamento do modelo. Isso pode levar algum tempo, dependendo do tamanho do dataset.

### 3. Analisar

-   Após o treinamento, examine as métricas de performance do modelo.<img src="https://github.com/chhico/lab-aws-sagemaker-canvas-estoque/blob/main/img/Analyze.png" alt="Analisando o modelo" height="780" width="450">
#### Verificando o Status do Modelo
-    1) Avg. wQL (Média da Perda Quantil Ponderada): Essa métrica mede o erro nas previsões, ponderado pela importância de cada item. Um valor menor indica maior precisão nas previsões. Em um cenário de estoque, isso ajuda a entender quais previsões estão mais próximas da realidade e onde podem estar os maiores riscos de erro, possibilitando ajustes mais precisos.
-    2) MAPE (Erro Percentual Médio Absoluto): Calcula a média da porcentagem de erro das previsões em relação aos valores reais. Um MAPE menor indica maior precisão nas previsões. Um MAPE de 0.149 significa que, em média, as previsões do modelo estão apenas 14.9% fora dos valores reais.
-    3) WAPE (Erro Percentual Absoluto Ponderado): Similar ao MAPE, mas leva em consideração a importância de cada item no estoque. Isso significa que itens de maior valor ou importância terão um impacto maior na métrica. Um WAPE menor é desejável, pois indica que o modelo está prevendo com mais precisão para os itens mais críticos, o que é essencial para uma gestão eficiente do estoque. Um WAPE de 0.103 indica um desempenho muito bom, com um erro absoluto ponderado médio de 10.3%.
-    4) RMSE (Raiz do Erro Quadrático Médio): Mede a diferença média entre os valores previstos e os valores reais, dando mais peso a grandes erros. Um RMSE menor é melhor, pois indica que as previsões estão, em média, próximas dos valores reais. No contexto de estoque, isso ajuda a minimizar grandes discrepâncias que podem levar a super ou subestimativas críticas.
-    5) MASE (Erro Escalado Médio Absoluto): Compara o erro da previsão com um modelo simples. Um valor de MASE menor que 1 indica que o modelo está fazendo previsões mais precisas do que simplesmente usar a média histórica. Para controle de estoque, um MASE menor que 1 é um bom sinal de que o modelo está efetivamente melhorando a previsão em comparação com métodos mais básicos.
- OBS.: Não existe indicadores ideais para todas as situações/projetos, deve ser analizada individualmente até atingir os indicadores aceitáveis para aquele projeto/situação.

#### Analizando as colunas que podem impactar
-    1) PREÇO: Podemos verificar que o preço impacta em 9,14% na análise
     2) Feriado: Impacta apenas 2,79%
     3) Dia da semana e produto promocional: Não impactam

### 4. Prever

-   Pegando um produto para analisar a previsão. <img src="https://github.com/chhico/lab-aws-sagemaker-canvas-estoque/blob/main/img/Predict.png" alt="Previsão" height="450" width="1000">

-    1) A demanda histórica foi de 70
-    2) P10 foi de ~55: Representa um valor abaixo do qual 10% das previsões estão. Em termos de estoque, este valor indica um cenário de baixa demanda. É útil para identificar períodos em que a demanda é mais baixa do que o esperado, ajudando a evitar excessos de estoque que podem resultar em custos adicionais de armazenamento ou perdas
     3) P50 foi de ~62: Este é o valor mediano nas previsões, mostrando a demanda média esperada. Para o controle de estoque, entender o P50 é crucial para planejamento médio de compras e reposição de produtos. Ele representa a situação mais comum de demanda que devemos considerar como base para as decisões.
     4) P90 foi de ~65: Indica um valor acima do qual 10% das previsões estão. Reflete um cenário de alta demanda, útil para planejar períodos de picos ou campanhas de vendas. Com essa informação, podemos assegurar que o estoque está preparado para atender à demanda máxima esperada sem faltar produtos.

