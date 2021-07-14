<h1 align="center">Projeto de Ciência de Dados - Regressão</h1>

Esse projeto é baseado na competição do kaggle - Rossmann Store Sales, disponivel no link: https://www.kaggle.com/c/rossmann-store-sales

![rossmann_shop](https://user-images.githubusercontent.com/78423995/119698135-5391d400-be27-11eb-9682-f7ad963f03cf.jpg)

## Questão de Negócio

A Rossmann é uma grande rede de farmácias que possui mais de 3 mil unidades espalhadas em 7 paises europeus. Atualmente, foi solicitado aos gerentes da Rossmann entregar uma previsão do total de vendas diárias durante as 6 semanas seguintes para suas respectivas lojas. As vendas das lojas podem ser influenciadas por diversos fatores, incluindo promoções, competidores, feriados, temporadas e localização da loja. Com milhares de gerentes individuais prevendo vendas com base em suas circunstâncias únicas, a precisão dos resultados pode ser bastante variada, por isso foi solicitado que seja criado um modelo que faça a previsão baseado nas peculiaridades de cada loja.

## Entendimento do Negócio

### Qual a motivação?
 
A previsão de vendas foi requisitada aos gerentes pelo Chief Financial Officer (CFO) durante a última reunião de gerência da companhia.

### Qual a causa raiz do problema? 

O CFO realizará investimentos e reformas nas lojas e para calcular o orçamento desse investimento por loja, é de suma importância saber quanto cada loja venderá nas próximas 6 semanas.

### Quem é o Stakeholder?

O CFO da Rossmann.

### Qual será o formato da solução?

__Granularidade:__ previsão por loja do valor total de vendas diárias acumulada durante as próximas 6 semanas.

__Tipo de problema:__ previsão de vendas.

__Potencias métodos:__ Regressão e Séries Temporais com algumas modificações.

__Formato da entrega:__ 
* Entregar relatório com o valor total de vendas diárias acumulada durante as próximas 6 semanas.
* O CFO poderá fazer essa consulta através de seu celular, acessando um bot pelo app Telegram.

## Ferramentas Utilizadas

* __Linguagem:__ Python.
* __IDE:__ Jupyter notebook.
* __Libraries:__ encontradas no arquivo requirements.txt nesse repositório.
* __ML models:__ Linear Regression, Lasso Linear Regression, Random Forest Regressor e XGBoost Regressor.
* __API:__ Flask, Telegram.
* __Cloud:__ Heroku.

## Estratégia de Solução - 10 Etapas

### Etapa 01 - Descrição dos Dados

Após baixar os dados e carregá-los no jupyter notebook, foram feitos os seguintes processos:

* Verificação dos dados, conhecer a dimensão dos dados e tipo dos dados;
* Renomeação de colunas e correção dos tipos dos dados;
* Verificação dos dados faltantes e preenchimento dos mesmos com técnicas apropriadas para cada caso específico;
* Realização da análise estátistica descritiva dos dados;
* Verificação dos atributos numéricos e categóricos.

### Etapa 02 - Feature Engineering

* Criação de uma lista de hipóteses, onde foram definidas 12 hipóteses a serem testadas;
* Definição dos fenômenos, agentes e atributos;
* Definição de features adicionais a serem criadas;
* Derivação de features à partir das variáveis originais. 

### Etapa 03 - Filtragem de Variáveis

* Realização de análise e filtragem das variáveis de acordo com as restrições do negócio;
* Realização de filtragem de linhas e seleção de colunas de interesse.

### Etapa 04 - Análise Explorátoria dos Dados (AED)

* Realização das análises univariada, bivariada e multivariada;
* Realização do teste das hipóteses de acordo com as análises anteriores e correlações das variáveis;
* Conclusão do teste de hipóteses e definição de sua relevância. 

Hipoteses  |  Conclusao  |  Relevancia
----------- |  ----------- | ------------
H1     |      Falsa    |    Baixa
H2     |      Falsa    |    Media
H3     |      Falsa    |    Media
H4     |      Falsa    |    Baixa
H5     |      -        |    -
H6     |      False    |    Baixa
H7     |      Falsa    |    Media
H8     |      Falsa    |    Alta
H9     |      Falsa    |    Alta
H10    |     Verdadeira | Alta
H11    |      Verdadeira |  Alta
H12    |      Verdadeira |  Baixa

Hipóteses confirmadas que podem gerar __insights__ valiosos para o negócio: 
* __1.__ As lojas com maior sortimento não vendem mais que as lojas com menor sortimento.
* __2.__ As lojas com concorrentes mais próximos vendem mais.
* __3.__ As lojas vendem menos nas férias escolares, exceto nos meses de julho e agosto.

### Etapa 05 - Preparação dos Dados 

* Não foi necessário realizar a normalização dos dados; 
* Realização do rescaling dos dados numéricos;
* Transformação dos dados categóricos em numéricos;
* Transformação dos dados cíclicos como meses, semanas e dias utilizando funções trigonométricas matemáticas.

### Etapa 06 - Seleção de Variáveis

* Os recursos estatisticamente mais relevantes foram selecionados utilizando a biblioteca Boruta;
* Seleção manual das variáveis de interesse que foram adicionadas à seleção final de variáveis.

### Etapa 07 - Modelos de Machine Learning

* Separação entre dados de treino e dados de teste;
* Realização do cálculo do modelo de média para base comparativa dos outros modelos;
* Execução do modelo de Regressão Linear;
* Execução do modelo de Regressão Linear Regularizada;
* Execução do modelo de Random Forest Regressor;
* Execução do modelo de XGBoost Regressor;
* Medição da performance real dos modelos com a técnica de Cross-Validation.

__Resultados__

Modelo | MAE |	MAPE |	RMSE
-------- |  ------- | -------- | ------- 
__Random Forest Regressor__ |	836.61 +/- 217.1 |	0.12 +/- 0.02 |	1254.3 +/- 316.17
__XGBoost Regressor__ |	1030.28 +/- 167.19 |	0.14 +/- 0.02 |	1478.26 +/- 229.79
Linear Regression |	2081.73 +/- 295.63 |	0.3 +/- 0.02 |	2952.52 +/- 468.37
Linear Regression - Lasso |	2116.38 +/- 341.5 |	0.29 +/- 0.01 |	3057.75 +/- 504.26

Baseado nos resultados acima o modelo __XGBoost Regressor__ foi selecionado porque possui um modelo treinado de menor tamanho e utiliza menos poder de processamento, apesar de possuir o _MAE MAPE RMSE_ ligeiramente maiores que o modelo Random Forest.

### Etapa 08 - Hyperparameter Fine Tuning

* Utilização de técnica de random search para refinamento do modelo XGboost.

Resultado final do modelo __XGBoost__ com refinamento

Modelo | MAE |	MAPE |	RMSE
-------- |  ------- | -------- | ------- 
XGBoost Regressor |	664.974996 |	0.097529 |	957.774225
	
### Etapa 09 - Interpretação e Tradução do Erro

Geração de gráficos comparando a diferença entre a previsão e a venda para análise de erro do modelo.

![error_rate](https://user-images.githubusercontent.com/78423995/119714406-37972e00-be39-11eb-998a-77150baaf6c0.png)

A linha de previsão do modelo está próxima à linha de vendas, o que demonstra boa acurácia nas previsões ao se manter próximo à linha de vendas ao longo do tempo.

A distribuição do erro está próxima à curva gaussiana, demonstrando distribuição normal dos erros.

A análise de resíduo demonstra que o modelo possui boa acurácia na previsão para a grande maioria das lojas.

### Etapa 10 - Deploy do Modelo

* Criação da classe Rossmann com o modelo de machine learning XGBoost treinado;
* Criação da API Handler no Flask;
* Realização do deploy do modelo na nuvem, no servidor Heroku;
* Integração do bot do Telegram na nuvem com o modelo Rossmann no servidor Heroku;
* Realização de teste do modelo final pelo celular, via app Telegram.

_Para verificar o resultado final no projeto acesse o bot **@Alicia_helper_bot** no telegram._

![Telegram_bot](https://user-images.githubusercontent.com/78423995/119694990-22fc6b00-be24-11eb-9202-74ed997a7e13.gif)

## Melhorias a Serem Implementadas

* Análise da hipótese 05 no próximo ciclo do CRISP, visando aumentar acurácia do modelo;
* Análise do erro para as lojas específicas em que o modelo apresentou baixa acurácia e treinamento de um modelo específico para estas lojas;
* Implementar no bot do Telegram a entrega de gráficos de vendas previstas.




