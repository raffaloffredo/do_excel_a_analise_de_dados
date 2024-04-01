# Do Excel à Análise de Dados de Ações de Empresas
</br>

<p align="center">
  <img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj4hPNKe6rHPzgsQ1Ocj0L1epCLRdz2z-Uz8oeVxoeyIxEVdqx8DVuTNpOxD-RO-d3DGXrJvVXHfQJrwdjGTh3GQ6hJiDHTMUo6qRFzAVyZo64QUJa2NvBPBcN1D2BI3TAQSm3oebJKALfVneW05F09vjfzv-25QV-HhtwooT2HzWN19Y0paASNXck3Ia8/w640-h366/DALL%C2%B7E-2024-03-17-13.30.png" width="60%">
</p>
<br/>

## Sobre o projeto
O objetivo desse projeto foi analisar ações de empresa da Bolsa do Brasil - B3 - e da bolsa dos Estados Unidos para mostrar minhas habilidades com o uso de planilhas, criação de fórmulas para se chegar no resultado obtido, desenvolvimento de análises de dados com criação de gráficos e previsão de séries temporais.
<br/>
<br/>

## Ferramentas utilizadas
- [Google Sheets](https://www.google.com/sheets/about/) - criação e manipulação de planilhas
- [Google Colab](https://colab.research.google.com/) - desenvolvimento e excução de código Python para análise de dados, criação de gráficos e do algoritmo para previsão de demanda

<br/>

## Materiais Extras
- [Google Sheets](https://docs.google.com/spreadsheets/d/1xjK3cCBwy22l8hRtdHqtwlkmtKMcYdc-PpjMIuxAN1c/edit#gid=0) - planilha utilizada com os dados originais obtido da B3 e o desevolvimento da análise de dados (explicação abaixo).
<br/>
<p align="center">
  <img src="">
</p>
<br/>

## Desevolvimento
Aqui axplico detalhadamente o que foi ao longo desse projeto, explicando o raciocínio e decisões tomadas, bem como as fórmulas utilizadas.

<br/>

### Calcular variação percentual diária
Precisamos converter o número decimal que representa a porcentagem para que possamos realizar cálculos utilizando-se desse valor, que é o valor dado na célula dividido por 100. Para isso foi criada uma nova coluna denominada `Var. Dia Percentual` com a fórmula `=D1/100` e replicado nas demais células da coluna E.

Da mesma forma foram criadas e calculadas as colunas: `Var. Sem. Percentual`, `Var. Mês Percentual`, `Var. Ano Percentual` e `Var. 12M Percentual`.

<br/>

### Calcular o valor inicial da ação
Com isso, agora podemos calcular o valor inicial da ação a partir do valor final. Para isso, criou-se uma nova coluna chamada de `Preço Inicial (R$)` que contém a fórmula `=D2/(1+F2)`, que foi replicadas para toda a coluna.

<br/>

### Calcular o volume financeiro
Também fiz o cálculo para encontrar a variação de volume financeiro movimentado no dia, a partir do valor de abertura e de fechamento, de acordo com o a quantidade teórica de ações. Para isso, primeiro criei uma coluna `Qtdade. Ações` para trazer essa informação da aba `Total_de_acoes` utilizando o VLOOKUP, também conhecido como PROCV no Excel.

<br/>

A fórmula ficou assim: `=VLOOKUP(A2; Total_de_acoes!A:B; 2; 0)`, onde:
- `A2` → é o valor chave que iremos usar para encontrar o valor que queremos na outra aba/planilha
- `Total_de_acoes!A:B` → identifica onde a pesquisa deve ser feita, ou seja, na aba Total_de_Acoes nas colunas A e B
- `2` → identifica que queremos que retorne o valor da coluna B, que corresponde ao número 2
- `0` → indica que queremos uma correspondência exata

<br/>

Com a quantidade de ações disponíveis agora é possível calcular o volume financeiro movimento no dia em uma nova coluna chamada `Vol. R$ Diário` **e** dado pela fórmula `= (Último (R$) - Preço Inicial (R$)) * Qtdade. Ações`.

Para ficar fácil de identificar se a variação foi positiva ou negativa, foi inserido uma formatação condicional em vermelho para negativo e verde para positivo.

Além disso, criei uma nova coluna que dará a indicação se o preço subiu, desceu, ou não se não houve variação, por meio de texto e utilizando a fórmula IFS que consegue avaliar diversas condições e retorna o valor correspondente à primeira condição verdadeira. Com isso, a fórmula ficou: `= IFS(Vol. R$ Diário > 0; "Subiu"; Vol. R$ Diário < 0; "Desceu"; Vol. R$ Diário = 0; "não houve variação")`. Também adicionei a formatação condicional nessa coluna para visualmente acompanhar a coluna anterior que indica se a variação foi positiva ou negativa.

<br/>

### Criar coluna com o nome da empresa, segmento e idade
Para essa planilha ficar ainda mais completa adicionei uma coluna com o nome da empresa (Nome)  a qual se refere determinada ação. Para isso, usei o comando VLOOKUP, para preenchimento automatizado a partir de outra tabela. A fórmula utilizada foi: `=VLOOKUP(A2; Ticker!A:B; 2; 0)`, onde:

- `A2` → é o valor chave que iremos usar para encontrar o valor que queremos na outra aba/planilha
- `Ticker!A:B` → identifica onde a pesquisa deve ser feita, ou seja, na aba Ticker nas colunas A e B
- `2` → identifica que queremos que retorne o valor da coluna B, que corresponde ao número 2
- `0` → indica que queremos uma correspondência exata

<br/>

Além disso, outras duas colunas foram criadas para informar o segmento da empresa (Segmento) e a idade em anos (Idade). Para isso, foi utilizado o ChatGPT, com o seguinte prompt, seguido da cópia da coluna B:

<br/>

```
Considere a tabela abaixo, que contém nome de empresas, e crie duas novas colunas,
uma com a informação de segmento da empresa a outra coluna com a idade, em anos, da empresa.
```

<br/>

A resposta foi copiada para uma nova aba na tabela, denominada ChatGPT. Novamente, o VLOOKUP entrou em ação na coluna Segmento com a fórmula: `=VLOOKUP(B2; ChatGPT!A:B; 2; 0)`, onde:
- `B2` → é o valor chave que iremos usar para encontrar o valor que queremos na outra aba/planilha
- `ChatGPT!A:B` → identifica onde a pesquisa deve ser feita, ou seja, na aba Ticker nas colunas A e B
- `2` → identifica que queremos que retorne o valor da coluna B, que corresponde ao número 2
- `0` → indica que queremos uma correspondência exata

<br/>

O mesmo foi feito para criar a coluna Idade, ou seja uma fórmula VLOOKUP que ficou: `=VLOOKUP(B2; ChatGPT!A:C; 3; 0`), onde:

- `B2` → é o valor chave que iremos usar para encontrar o valor que queremos na outra aba/planilha
- `ChatGPT!A:C` → identifica onde a pesquisa deve ser feita, ou seja, na aba Ticker nas colunas A e B
- `3` → identifica que queremos que retorne o valor da coluna B, que corresponde ao número 2
- `0` → indica que queremos uma correspondência exata

<br/>

Foi necessário editar a coluna para o formato de números e reduzir a quantidade de casas decimais para 0, ou seja, nenhuma.

<br/>

### Categoria de Idades
Para facilitar a análise dos dados, vamos criar uma coluna categórica com faixas de idade das empresas. Para isso, vou usar o IFS para criar diversas condicionais:
- se for maior que 100 anos
- se for entre 50 e 100 anos
- se for menor que 50

<br/>

Cuja fórmula ficou assim: `=IF(D2 > 100; "Mais de 100 anos"; if(D2 < 50; "Menos de 50 anos"; "Entre 50 e 100 anos"))`.

<br/>

## Outros projetos

* **[Detecção de Fake News com Redes Neurais](https://github.com/raffaloffredo/fake_news_detection_portuguese)**
* **[Análise de Risco de Crédito](https://github.com/raffaloffredo/credit_risk_analysis_portuguese)**
* **[Análise de séries temporais para previsão de demanda](https://github.com/raffaloffredo/demand_forecasting_with_time_series_portuguese)**
* **[Classificação para identificar a saúde de um feto](https://github.com/raffaloffredo/fetus_health_classification_portuguese)**
* **[Previsão de Custo de Seguro de Vida](https://github.com/raffaloffredo/life_insurance_price_prediction_portuguese)**
* **[Previsão de Churn](https://github.com/raffaloffredo/churn_prediction_portuguese)**
* **[Detecção de fraude em cartão de crédito](https://github.com/raffaloffredo/fraud_detection_portuguese)**
* **[Airbnb New York](https://github.com/raffaloffredo/airbnb_new_york_portuguese)**
* **[Estudo atualizado sobre COVID-19 no Brasil e no mundo](https://github.com/raffaloffredo/covid_2023_portuguese)**
* **[Modern Analytics Stack aplicado à Adventure Works](https://github.com/raffaloffredo/adventure_works_portuguese)**
<br/>

 ## Contatos
<div>
  <a href="https://www.linkedin.com/in/raffaela-loffredo/?locale=en_US" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a>
  <a href="https://sites.google.com/view/loffredo/" target="_blank"><img src="https://img.shields.io/badge/website-000000?style=for-the-badge&logo=About.me&logoColor=white"></a>
  <a href="https://medium.com/@loffredo.ds" target="_blank"><img src="https://img.shields.io/badge/Medium-12100E?style=for-the-badge&logo=medium&logoColor=white"></a>
</div>
