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

## Análise dos Dados
Vamos agora analisar os dados da nossa planilha.

<br/>

### Maior, menor e média de variação em volume financeiro R$
Vamos identificar de forma fácil qual foi a maior e a menor variação em volume monetário (R$) que ocorreu no dia (e quais empresas são essas), bem como o valor médio. Para isso vou usar a função MAX que ficou `=MAX(Principal!W:W)`, a função MIN que ficou `=MAX(Principal!W:W)` e a função AVERAGE para calcular a média que ficou assim `=AVERAGE(Principal!W:W)`.
    
<br/>

### Média segregada por empresas que subiram e das que desceram
O valor da média como calculado acima pode não trazer a melhor informação quando queremos saber qual a média da variação em volume financeiro das empresas que subiram e das que desceram. Por isso, criei separadamente a média para esses casos utilizando a função AVERAGEIF:
- Média de Variação R$ ↑: `=AVERAGEIF(Principal!X:X; "Subiu"; Principal!W:W)`
- Média de Variação R$ ↓: `=AVERAGEIF(Principal!X:X; "Desceu"; Principal!W:W)`

<br/>
  
### Identificar as empresas
De nada adianta termos os valores se não soubermos quem foi a empresa com a maior ou a menor variação. Vamos identificar esses valores com o uso das funções INDEX e MATCH. Não é possível utilizar o VLOOKUP nesse caso em específico porque essa função permite localizar valores que estão à direita da tabela onde o valor inicial é buscado e no nosso caso o valor a ser pesquisado está à esquerda.
    
Temos então: `=INDEX(Principal!B:B; MATCH(B1; Principal!W:W; 0))`, onde:
- `MATCH(B1; Principal!W:W; 0)` irá procurar o valor que está na célula B1 na coluna W da tabela Principal, sendo que o 0 indica que queremos apenas a correspondência exata.
- `INDEX(Principal!B:B`) serve para retornar o valor correspondente ao encontrado no MATCH na coluna B da tabela Principal.

<br/>

Para a menor variação a fórmula é semelhante, só mudando a célula com o valor a ser buscado que é a B2: `=INDEX(Principal!B:B; MATCH(B2; Principal!W:W; 0))`.

<br/>

### Análise de dados por Segmento
Vou reunir as informações por segmento para uma análise mais específica. Primeiro, crio a coluna segmento e puxo das informações da coluna de Segmento da planilha Principal. Para isso uso a função UNIQUE que não irá duplicar essa informação: `=UNIQUE(Principal!C:C)`.
    
Na coluna seguinte vou trazer a informação de variação de volume financeiro por segmento. Para isso preciso usar a função SUMIF pois irá somar somente se cumprir com uma condicional, ou seja, se for do mesmo segmento. A fórmula ficou assim: `=SUMIF(Principal!C:C; A11; Principal!W:W)`, onde:
- `Principal!C:C` indica a coluna que está o valor que queremos
- `A11` informa qual o valor que queremos encontrar
- `Principal!W:W`  é a coluna que tem o valor correspondente que será somado

<br/>

### Participação de cada segmento dentro do segmento 
Ainda na análise se segmento, gostaria de indicar qual foi a participação de cada segmento em relação ao seu segmento em si. Com isso temos dois critérios:
1. Tem que ser do mesmo segmento
2. A ação tem que ter subido

<br/>

Para isso vou usar a função SUMIFS que consegue atender mais de um critério para realizar uma soma. A fórmula ficou assim: `=SUMIFS(Principal!W:W; Principal!C:C; A11; Principal!X:X; "Subiu")`, onde:
- `Principal!W:W;` é a coluna que tem o valor que queremos somar
- `Principal!C:C;` corresponde ao primeiro critério, ou seja, do segmento
- `A11;`  indica qual o segmento que estamos buscando para essa soma
- `Principal!X:X;` inserimos a coluna que tem a informação do nosso segundo critério
- `"Subiu"` e esse nosso segundo critério irá somar apenas se a empresa teve uma variação positiva nas ações, ou seja, se o preço dela subiu

<br/>

### Qual a relação entre as ações que subiram e as que desceram?
Para responder a essa pergunta criei uma seção de Resultado, onde, primeiro identifiquei os tipos de variações possível com a função UNIQUE (`=UNIQUE(Principal!X:X)`). Em seguida, criei uma nova coluna Variação Volume R$ onde vou somar apenas se corresponder ao mesmo resultado.  Logo, a fórmula é `=SUMIF(Principal!X:X; A67; Principal!W:W)`, onde:
- `Principal!X:X;` a coluna onde devemos procurar nosso critério da soma
- `A67;` o valor do nosso critério da soma
- `Principal!W:W` o valor a ser somado

<br/>

### Saldo
Com as informações do quanto as ações subiram e o quanto elas desceram, é possível saber o valor de saldo somando-os, isto é: `=B67+B69`.

<br/>

### Análise por faixa etária da empresa
Primeiro criei a coluna Faixa Idade que puxa as informações da planilha Principal com o uso de UNIQUE (`=UNIQUE(Principal!E:E)`). Depois usei SUMIF para somar de forma condicional apenas o valor financeiro de acordo com a faixa etária de cada empresa (`=SUMIF(Principal!E:E; A75; Principal!W:W)`). Por fim, contei a quantidade de empresas em cada faixa usando o COUNTIF, que consegue contar de acordo com uma condição, no caso, a idade da empresa (`=COUNTIF(Principal!E:E; A75)`).
    
<br/>

## Análise Gráfica
Para tornar a análise mais fácil e rápida de ser compreendida foram construídos gráficos.

<br/>

### Variação das ações que subiram x Segmento
Para visualizar os segmentos que mais subiram no dia foi construído um gráfico de pizza com suas respectivas representações em porcentagens.

<br/>
<p align="center">
  <img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhJXy4ztRSziQwtt5o-M6udm2p3btku5DbDYj4bIJuJZGQrk_tZT4xYHH5wo8sYKPzqb-7UPjOOuQ9gl78YcIpiQZSO94Gl-UMv3rU7hyp1XT2H46gyFwqD02wa7oau_KNBVadXrvJv7myYr3sbnPxhR8FVMmeoEPsi5KPsE_19n9ekx_J5gO2oz9nv2zQ/s16000/1.png">
</p>
<br/>
    
### Variação de Volume por Resultado
Utilizei um gráfico de cascata, bem comum no ramo das finanças, para mostrar de quanto foi o volume financeiro das ações que subiram e desceram no dia.
    
<br/>
<p align="center">
  <img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgeHwLpw3dtah_D_WNzyjSXzvFMG1Mdfi8cjEYH5bdDO0aZGsuz6VGC4It9Pyg7lytP34cmennZLJS4ulefXBH1Kc3THgS0CLC8dK1eSMAaoyf1x5UyW0jHB_WcPfZlKpKUWNKne5xyZm62qdmX95-QNoUzTNtI7clSMe2bjTo590lTWh-2H2MDp_Wc7PM/s16000/2.png">
</p>
<br/>
    
### Variação de Volume x Faixa de Idade
Outro gráfico de barras foi feito para mostrar o volume financeiro movimentado no dia de acordo com a faixa de idade das empresas.

<br/>
<p align="center">
  <img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi0V7gpjGKD3iCHGv9WX56WqFd0j3FW6m6Cc6akIA_rIbwd0k_4JGxduHVNROjRqI2okoGLQQ-8nmsMGcRbXRPfOyR2bCVSNKukutUDpUplRqX8oHMak-fbpEcQzyhcxhst4DFARXb3nAKCjQkXy_6OvIBLKnfi8uwEjFDyNUt-eiVQG8r4Y3ySGwT3s6A/s16000/3.png">
</p>
<br/>
    
### Quantidade de Empresas x Faixa de Idade
Por fim, um gráfico de pizza nos ajuda a entender a quantidade de empresas por faixa etária.
    
<br/>
<p align="center">
  <img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEitOtQRGME1AQyYQW9_HOES73nf6erX8VSdQhWSQUPQ8UxAp-KVdEYXcpKJ2rullJd9FWZ0G08MZSJGODSle4NYX9THktc2cFT__0auIkftBiBumSL28nTIm9hK1Dlw5M2XWlXiK-v5cs9x-N6X0czFYIdz_IbaRl74bT-8fiFP3adNizpaxHsrhk4b1xk/s16000/4.png">
</p>
<br/>
    
## Análise de Dados com Python




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
