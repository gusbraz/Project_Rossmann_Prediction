<h1 align="center">Projeto de Ciência de Dados - Regressão Linear</h1>

Esse projeto é baseado na competição do kaggle - Rossmann Store Sales, disponivel no link: https://www.kaggle.com/c/rossmann-store-sales

![rossmann_shop](https://user-images.githubusercontent.com/78423995/119698135-5391d400-be27-11eb-9682-f7ad963f03cf.jpg)

## Questão de Negócio

A Rossmann é uma grande rede de farmacias que possui mais de 3.000 unidades espalhadas em 7 paises europeus. Atualmente, os gerentes da Rossmann possuem a tarefa de realizar a previsão de vendas diárias de suas respectivas lojas nas proximas 6 semanas. As vendas das lojas são influenciadas por vários fatores, incluindo promoções, competidores, feriados, temporadas e localização da loja. Se cada gerente usar uma técnica para fazer a previsão de vendas da sua unidade os resultados vão variar demais, por isso foi pedido que seja criado um modelo que faça a previsão de cada loja baseado nas suas peculiaridades.

## Entendimento do Negócio

### Qual a motivação?
 
A previsão de vendas foi requisitada aos gerentes pelo Chief Financial Officer (CFO) em uma reunião mensal.

### Qual a causa raiz do problema? 

O CFO vai realizar investimentos e reformas nas lojas e para fazer o orçamento desse investimento ele precisa de saber quanto cada loja vai vender nas próximas 6 semanas.

### Quem é o Stakeholder?

o CFO da Rossmann.

### Qual será o formato da solução?

__Granularidade:__ previsão de vendas por dia, por loja, acumulada para as próximas 6 semanas.

__Tipo de problema:__ previsão de vendas.

__Potencias métodos:__ Regressão linear e séries temporais com algumas modificações.

__Formato da entrega:__ 
* O valor total de vendas, de cada loja no final da sexta semana.
* O CFO poderá fazer essa consulta pelo celular, acessando um bot pelo app Telegram. 

## Ferramentas utilizadas

* Linguagem: Python
* IDE: Jupyter notebook
* Libraries: encontradas no arquivo requirements.txt nesse repositório
* ML models: Linear Regression, Lasso Linear Regression, Random Forest Regressor e XGBoost Regressor.
* API: Flask, Telegram
* Cloud: Heroku

## Estratégia de solução - 10 Etapas

### Etapa 01 - Descrição dos Dados

Após baixar os dados e carrega-los no jupyter notebook, foram feitas os seguintes processos:
* Verificação dos dados;
* Renomear colunas;
* Dimensão dos dados;
* Tipo dos dados;
* Dados faltantes e preenchimento dos dados faltantes;
* Estátistica descritiva dos dados
* Atributos numéricos
* Atributos categóricos.


### Etapa 02 - Feature Engineering

* Criação de uma lista de hipóteses;
* Definição dos fenômenos, agentes e atributos;
* Definição das features que serão criadas;
* Derivação de features a partir das variáveis originais. 

### Etapa 03 - Filtragem de Variáveis

* Análise e filtragem das variáveis de acordo com as restrições do negócio;
* Filtragem de linhas e seleção de colunas de interesse.

### Etapa 04 - Análise Explorátoria dos Dados (AED)

* Análise Univariada;
* Análise Bivariada;
* Análise Multivariada;
* Teste das hipóteses de acordo com as análises anteriores e correlações;
* Conclusão do teste de hipóteses e definição da sua relevância. 

Hipoteses  |  Conclusao  |  Relevancia
----------- |  ----------- | ------------
H1     |      Falsa    |    Baixa
H2     |      Falsa    |    Media
H3     |      Falsa    |    Media
H4     |      Falsa    |    Baixa
H5     |      _        |    -
H6     |      False    |    Baixa
H7     |      Falsa    |    Media
H8     |      Falsa    |    Alta
H9     |      Falsa    |    Alta
H10    |     Verdadeira | Alta
H11    |      Verdadeira |  Alta
H12    |      Verdadeira |  Baixa

Hipóteses que são verdadeiras e são mais relevantes para o modelo. 
* __1.__ Lojas com maior sortimento não vendem mais.
* __2.__ As lojas com concorrentes mais próximos vendem mais.
* __3.__ As lojas vendem menos nas férias escolares (exceto durante o verão).

### Etapa 05 - Preparação dos Dados 

* Normalização dos dados. (Não foi necessária)
* Rescaling dos dados numéricos;
* Transformação dos dados categóricos em numéricos;
* Os dados cíclicos (como meses, semanas e dias) foram transformados usando funções trigonométricas matemáticas.

### Etapa 06 - Seleção de variáveis

* Os recursos estatisticamente mais relevantes foram selecionados usando o pacote Boruta;
* Manualmente as variáveis foram analisadas e as de interesse foram ao adicionadas a seleção final.

### Etapa 07 - Modelos de Machine Learning

* Separação entre dados de treino e dados de teste;
* Calculo do modelo de média para base comparativa dos outros modelos;
* Execução de modelo de Regressão Linear;
* Execução de modelo de Regressão Linear Regularizada;
* Execução de modelo de Random Forest Regressor;
* Execução de modelo de XGBoost Regressor;
* Medição da performance real dos modelos com a técnica de Cross-Validation.

__Resultados__

Modelo | MAE |	MAPE |	RMSE
-------- |  ------- | -------- | ------- 
Random Forest Regressor |	836.61 +/- 217.1 |	0.12 +/- 0.02 |	1254.3 +/- 316.17
XGBoost Regressor |	1030.28 +/- 167.19 |	0.14 +/- 0.02 |	1478.26 +/- 229.79
Linear Regression |	2081.73 +/- 295.63 |	0.3 +/- 0.02 |	2952.52 +/- 468.37
Linear Regression - Lasso |	2116.38 +/- 341.5 |	0.29 +/- 0.01 |	3057.75 +/- 504.26

* Com baso nos resultados acima, o __XGBoost__ foi selecionado por ser um modelo de tamanho menor que o Random Forest e ter uma acurácia próxima.

### Etapa 08 - Hyperparameter Fine Tunig

* Utilizando a estratégia de random search para refinar o modelo XGboost;

Resultado final do modelo __XGBoost__ com refinamento

Modelo | MAE |	MAPE |	RMSE
-------- |  ------- | -------- | ------- 
XGBoost Regressor |	664.974996 |	0.097529 |	957.774225
	
### Etapa 09 - Interpretação e tradução do erro

Gráficos para análise de erro do modelo em comparação as vendas reais.

![error_rate](https://user-images.githubusercontent.com/78423995/119714406-37972e00-be39-11eb-998a-77150baaf6c0.png)

A linha de previsão modelo esta sempre próximo da linha de vendas demonstra uma boa acurácia ao se manter próximo das vendas reais ao longo do tempo.

A distribuição do erro está próxima da gaussiana.

A análise de resíduo demonstra que o modelo tem uma acurácia muito boa para a grande maioria das lojas.

### Etapa 10 - Deploy do modelo

* Criar a classe Rossmann com o modelo de ML treinado;
* Criar a API Handler no Flask;
* Deploy do modelo na cloud (Heroku);
* Integrar o bot do Telegram na cloud com o modelo Rossmann;
* Teste do modelo pelo celular, via app Telegram.

_Para verificar o resultado final no projeto acesse o bot **@Alicia_helper_bot** no telegram._

![Telegram_bot](https://user-images.githubusercontent.com/78423995/119694990-22fc6b00-be24-11eb-9202-74ed997a7e13.gif)

## Próximos passos e melhorias futuras do modelo

* Analisar a hipótese 05 no próximo ciclo do CRISP.
* Analisar e erro para as lojas especificas em que o modelo apresentou baixa acurácia e treinar um modelo especifico para essas lojas.
* Implementar a entrega de gráficos das vendas previstas pelo bot do Telegram.




