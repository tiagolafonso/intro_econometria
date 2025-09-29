

<link href="capitulo-06-extensoes-regressao_files/libs/htmltools-fill-0.5.8.1/fill.css" rel="stylesheet" />
<script src="capitulo-06-extensoes-regressao_files/libs/htmlwidgets-1.6.4/htmlwidgets.js"></script>
<script src="capitulo-06-extensoes-regressao_files/libs/plotly-binding-4.11.0/plotly.js"></script>
<script src="capitulo-06-extensoes-regressao_files/libs/setprototypeof-0.1/setprototypeof.js"></script>
<script src="capitulo-06-extensoes-regressao_files/libs/typedarray-0.1/typedarray.min.js"></script>
<script src="capitulo-06-extensoes-regressao_files/libs/jquery-3.5.1/jquery.min.js"></script>
<link href="capitulo-06-extensoes-regressao_files/libs/crosstalk-1.2.2/css/crosstalk.min.css" rel="stylesheet" />
<script src="capitulo-06-extensoes-regressao_files/libs/crosstalk-1.2.2/js/crosstalk.min.js"></script>
<link href="capitulo-06-extensoes-regressao_files/libs/plotly-htmlwidgets-css-2.11.1/plotly-htmlwidgets.css" rel="stylesheet" />
<script src="capitulo-06-extensoes-regressao_files/libs/plotly-main-2.11.1/plotly-latest.min.js"></script>

# Extensões dos Modelos de Regressão

Este capítulo apresenta várias extensões dos modelos de regressão linear
básicos que são frequentemente necessárias na estimação de modelos
econométricos. Estas extensões permitem lidar com situações mais
complexas e realistas que os modelos lineares simples não conseguem
capturar adequadamente.

## Modelos com Variáveis Dummy

Uma variável dummy é uma variável que assume valores binários (0 ou 1)
que indica apresença ou não de uma característica específica. Por
exemplo, uma variável dummy pode ser usada para indicar a presença de
uma determinada condição (sim ou não) ou para categorias. Por exemplo,
uma variável dummy pode ser usada para indicar se uma empresa está
localizada numa determinada região (1) ou não (0), ou em várias regiões
(mais de duas categorias).

### com variáveis Dummy Simples

As variáveis dummy são variáveis binárias que assumem valores 0 ou 1.

Uma dummmy, é geralmente representada por:

$$
D_i = \begin{cases}
1, & \text{se a condição é satisfeita} \\
0, & \text{a condição não é satisfeita}
\end{cases}
$$

Num modelo de regressão, uma variável dummy pode ser incluída como uma
variável explicativa para capturar o efeito de uma característica
qualitativa na variável dependente:

<span id="eq-reg-dummy">$$
y_i = \beta_0 + \beta_1 x_i + \beta_2 D_i + u_i
 \qquad(1)$$</span>

É necessário definir muito bem o que é o 0 ou o que e o 1., pois vai
mudar a interpretação dos coeficientes no modelo. Para este exemplo
vamos utilizar os dados `wage2` da biblioteca `wooldridge`, que contém
informações sobre o salário, bem como outras características como anos
de experiência, anos na empresa e outras características. O conjunto de
dados contém variáveis dummy em que uma delas (`married`), que indica se
o funcionário é casado (1) ou não (0). No `R` uma variável não tem de
ser necessariamente binária para ser tratada como dummy, o `R` cria
automaticamente as dummies para variáveis categóricas que são fatores
(para o `R`). Vamos estimar a regressão do salário em função dos anos de
educação, anos de experiência e se é casado ou não:

<span id="eq-reg-dummy-simples">$$
wage_i = \beta_0 + \beta_1 educ_i + \beta_2 exper_i + \beta_3 married_i + u_i
 \qquad(2)$$</span>

A interpretação do coeficientes da dummy ($\beta_3$) é a diferença média
no salário entre indivíduos casados e não casados ($married = 1$ e
$married = 0$), mantendo a educação e a experiência constantes. Ou seja,
depende das unidades da variável dependente (neste caso, salário em
dólares americanos).

``` r
library(tidyverse)
library(wooldridge)
data("wage2")

# Estimar modelo de regressão
modelo_dummy <- lm(wage ~ educ + exper + married, data = wage2)
summary(modelo_dummy)
```


    Call:
    lm(formula = wage ~ educ + exper + married, data = wage2)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -864.16 -241.25  -39.12  194.22 2145.23 

    Coefficients:
                Estimate Std. Error t value Pr(>|t|)    
    (Intercept) -427.775    111.109  -3.850 0.000126 ***
    educ          76.550      6.227  12.293  < 2e-16 ***
    exper         16.317      3.139   5.198 2.48e-07 ***
    married      185.907     39.604   4.694 3.08e-06 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 372.1 on 931 degrees of freedom
    Multiple R-squared:  0.1558,    Adjusted R-squared:  0.1531 
    F-statistic: 57.29 on 3 and 931 DF,  p-value: < 2.2e-16

Interpretação dos coeficientes:

- $\beta_0$ (constante): O salário médio para alguém com 0 anos de
  educação, 0 anos de experiência e que não é casado é de -427.76 US\$.
- $\beta_1$ (`educ`): Cada ano adicional de educação está associado a um
  aumento médio de 76.55 US\$ no salário, *ceteris paribus*.
- $\beta_2$ (`exper`): Cada ano adicional de experiência está associado
  a um aumento médio de 16.32 US\$ no salário, *ceteris paribus*.
- $\beta_3$ (`married`): um indivíduo casado ganha, em média, 185.91
  US\$ a mais do que um indivíduo não casado, *ceteris paribus*.

Para um regressão simples, apenas com a variável dummy como
independente, o valor da constante é o valor médio da variável
dependente quando a dummy é 0. Considerando o modelo:

``` r
# Estimar modelo de regressão com apenas a dummy
modelo_dummy_s <- lm(wage ~ married, data = wage2)
summary(modelo_dummy_s)
```


    Call:
    lm(formula = wage ~ married, data = wage2)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -862.05 -284.05  -51.05  210.95 2100.95 

    Coefficients:
                Estimate Std. Error t value Pr(>|t|)    
    (Intercept)   798.44      40.08  19.922  < 2e-16 ***
    married       178.61      42.41   4.211 2.78e-05 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 400.8 on 933 degrees of freedom
    Multiple R-squared:  0.01865,   Adjusted R-squared:  0.0176 
    F-statistic: 17.74 on 1 and 933 DF,  p-value: 2.784e-05

O salário médio para indivíduos não casados é de 798.44 US\$. E qual o
salário médio para indicíduos casados? Podemos calculamos o valor do
salário para quando $married = 1$ considerando os coeficientes do
modelo:

``` r
# Calcular salário médio para indivíduos casados
salario_casado <- predict(modelo_dummy_s, newdata = data.frame(married = 1))
salario_casado
```

           1 
    977.0479 

O salário médio para indivíduos casados é em média de 977.05 US\$.
Apenas para confirmar, podemos calcular a média diretamente com uma
subamostra para cada grupo:

``` r
# Calcular salário médio para indivíduos casados

# Para indivíduos não casados (married = 0)
salario_nao_casado <- wage2 %>% 
    filter(married == 0) %>% 
    summarise(salario_medio = mean(wage))
salario_nao_casado
```

      salario_medio
    1        798.44

``` r
# Para indivíduos casados (married = 1)
salario_casado <- wage2 %>% 
    filter(married == 1) %>% 
    summarise(salario_medio = mean(wage))
salario_casado
```

      salario_medio
    1      977.0479

Atravé do gráfico podemos visualizar a diferença nos salários entre
indivíduos casados e não casados:

<div class="plotly html-widget html-fill-item" id="htmlwidget-2bd846de37bb89d157c0" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-2bd846de37bb89d157c0">{"x":{"visdat":{"3c843d3f36d8":["function () ","plotlyVisDat"],"3c84207043f1":["function () ","data"]},"cur_data":"3c84207043f1","attrs":{"3c843d3f36d8":{"x":{},"y":{},"color":{},"colors":["steelblue","orange"],"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"box"},"3c843d3f36d8.1":{"x":{},"y":{},"color":{},"colors":["steelblue","orange"],"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter","mode":"markers","marker":{"size":3,"opacity":0.29999999999999999},"showlegend":false,"inherit":true},"3c84207043f1":{"x":{},"y":{},"color":{},"colors":["steelblue","orange"],"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter","mode":"markers","marker":{"size":10,"color":"red","symbol":"circle"},"name":"Média","showlegend":false,"inherit":true}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"title":"Distribuição do Salário por Estado Civil","xaxis":{"domain":[0,1],"automargin":true,"title":"Estado Civil","type":"category","categoryorder":"array","categoryarray":["Não Casado","Casado"]},"yaxis":{"domain":[0,1],"automargin":true,"title":"Salário (US$)"},"showlegend":true,"hovermode":"closest"},"source":"A","config":{"modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"data":[{"fillcolor":"rgba(70,130,180,0.5)","x":["Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado"],"y":[600,900,635,981,1749,769,875,963,619,1694,1076,1299,1282,433,865,1346,480,513,1105,450,1899,578,1250,652,727,1014,449,1031,537,930,769,417,490,898,571,445,1050,812,841,475,721,745,879,1212,562,375,854,888,758,1000,310,1000,556,606,812,549,1417,417,1039,685,577,1250,600,480,961,900,409,769,1098,1500,377,722,350,577,1105,600,950,533,1354,550,360,1212,1699,900,1154,369,800,875,680,875,640,425,510,681,661,503,400,485,618,481],"type":"box","name":"Não Casado","marker":{"color":"rgba(70,130,180,1)","line":{"color":"rgba(70,130,180,1)"}},"line":{"color":"rgba(70,130,180,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(255,165,0,0.5)","x":["Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado"],"y":[769,808,825,650,562,1400,1081,1154,1000,930,921,1318,1792,958,1360,850,830,471,1275,1615,873,2137,1053,1602,1188,800,1417,1000,1424,2668,666,1779,782,1572,1274,714,1081,692,1318,1239,1027,1748,770,1154,1155,808,1100,1154,1000,462,1375,1452,800,1748,1151,840,978,442,600,1366,1643,1455,2310,1682,1235,855,1072,1040,1000,675,1100,996,732,1200,686,754,857,832,579,672,2500,750,1186,833,650,1250,1122,865,808,903,900,625,1586,962,1539,1110,770,1000,895,1205,750,654,601,600,1188,635,1225,1151,1031,1049,1000,1105,1924,1346,809,1495,1200,500,1325,900,800,800,1034,980,884,923,1193,2771,779,950,1394,1495,650,670,1126,1028,2404,757,1250,1162,1025,1100,714,1318,1411,2162,1273,1140,942,1058,750,1000,951,635,1250,675,400,577,590,923,1100,1130,618,962,529,817,962,840,866,2404,1126,1160,723,1778,1903,1010,971,525,525,670,500,1058,550,500,865,1081,1304,575,623,515,1273,990,600,1160,500,795,500,740,1250,1250,913,1346,445,265,1250,1607,1452,1391,821,794,500,520,1730,1924,1155,2162,923,1115,1500,826,937,978,1272,1136,800,1339,1063,935,808,375,1082,1155,548,622,841,587,1924,1058,1202,1154,1070,1202,711,1202,850,1000,1000,865,1375,1586,1602,3078,906,952,289,1444,962,1075,909,1250,620,1016,800,1079,654,781,1038,1924,1202,666,905,890,817,577,756,1011,1155,1025,1350,1001,796,1230,754,714,1000,2067,912,600,951,711,1151,1000,841,400,1175,1202,1442,538,781,750,700,1346,800,1250,1105,762,962,800,658,1270,1313,824,1442,1400,1038,668,1100,1000,523,1111,962,729,690,1010,600,596,850,670,793,1442,670,876,841,975,1223,910,533,750,1206,900,1170,540,550,615,909,769,984,833,1027,1000,465,1100,641,1035,950,938,1250,586,693,673,654,692,1111,1368,1282,1250,1346,1424,1161,583,1260,947,1850,1575,1442,489,1126,500,1200,565,1920,684,774,233,975,1366,2137,700,1200,1161,729,750,1026,1111,625,1200,1541,1154,610,1749,350,765,790,818,477,938,2099,350,940,1202,450,1058,1000,318,958,995,1600,511,1411,1346,1522,1075,1200,1377,874,625,1250,1082,693,727,615,913,884,698,800,1000,1300,923,1539,721,815,600,1100,1058,962,433,940,1000,500,800,766,750,550,795,723,1039,200,400,575,850,508,1162,1199,1270,350,963,1463,802,642,751,488,1400,940,866,675,375,325,346,1442,560,550,950,684,1322,1634,472,666,1679,1250,760,1154,1602,1442,865,705,1000,852,770,1000,1329,1682,577,1040,1000,988,478,440,485,705,1550,960,1000,625,1346,479,1201,1346,1122,543,450,507,547,586,462,705,1566,1634,1282,556,1200,855,1053,1000,1602,508,1500,1682,750,843,1004,650,808,2137,761,345,495,987,2500,1212,577,390,1500,583,460,945,1442,1333,1333,700,973,2162,797,400,1015,1744,630,445,660,779,560,1122,453,1386,1539,1154,962,480,808,1442,1091,500,1026,1333,915,910,1000,1160,1001,713,929,400,1241,1065,403,940,812,700,575,450,621,441,625,726,500,1000,393,962,962,865,1154,1386,732,865,700,975,1300,900,829,1000,827,500,1155,700,1710,2004,890,3078,1539,508,1143,962,1250,990,905,926,1559,1312,923,879,800,1049,1190,583,1200,797,1371,1270,1832,909,1746,520,808,2137,692,931,812,866,1442,500,1384,528,881,1026,1620,1843,1602,1000,737,1699,1025,1107,625,720,855,1250,1130,1924,962,1874,1573,940,900,882,1710,1260,751,1097,1001,1000,1250,857,1211,937,904,872,1200,1261,950,115,673,1084,1058,800,1924,855,864,1092,1025,850,673,800,1049,884,1200,664,1111,850,1000,1202,666,923,788,950,1442,866,625,693,2308,1196,700,721,1500,1050,801,1198,390,889,1076,1127,750,855,525,1104,553,800,1384,370,596,402,418,1133,950,600,925,550,606,425,340,692,575,571,817,987,616,1026,808,808,788,850,900,1418,260,662,562,562,357,1009,1442,651,750,754,700,937,624,750,900,540,642,900,513,894,1282,325,769,1040,751,380,300,753,1065,1070,1573,650,700,494,890,520,891,570,1444,500,1473,803,962,1000,600,450,629,492,1562,357,960,566,481,1442,645,788,644,477,664,520,1202,538,873,1000],"type":"box","name":"Casado","marker":{"color":"rgba(255,165,0,1)","line":{"color":"rgba(255,165,0,1)"}},"line":{"color":"rgba(255,165,0,1)"},"xaxis":"x","yaxis":"y","frame":null},{"x":["Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado","Não Casado"],"y":[600,900,635,981,1749,769,875,963,619,1694,1076,1299,1282,433,865,1346,480,513,1105,450,1899,578,1250,652,727,1014,449,1031,537,930,769,417,490,898,571,445,1050,812,841,475,721,745,879,1212,562,375,854,888,758,1000,310,1000,556,606,812,549,1417,417,1039,685,577,1250,600,480,961,900,409,769,1098,1500,377,722,350,577,1105,600,950,533,1354,550,360,1212,1699,900,1154,369,800,875,680,875,640,425,510,681,661,503,400,485,618,481],"type":"scatter","mode":"markers","marker":{"color":"rgba(70,130,180,1)","size":3,"opacity":0.29999999999999999,"line":{"color":"rgba(70,130,180,1)"}},"showlegend":false,"name":"Não Casado","textfont":{"color":"rgba(70,130,180,1)"},"error_y":{"color":"rgba(70,130,180,1)"},"error_x":{"color":"rgba(70,130,180,1)"},"line":{"color":"rgba(70,130,180,1)"},"xaxis":"x","yaxis":"y","frame":null},{"x":["Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado"],"y":[769,808,825,650,562,1400,1081,1154,1000,930,921,1318,1792,958,1360,850,830,471,1275,1615,873,2137,1053,1602,1188,800,1417,1000,1424,2668,666,1779,782,1572,1274,714,1081,692,1318,1239,1027,1748,770,1154,1155,808,1100,1154,1000,462,1375,1452,800,1748,1151,840,978,442,600,1366,1643,1455,2310,1682,1235,855,1072,1040,1000,675,1100,996,732,1200,686,754,857,832,579,672,2500,750,1186,833,650,1250,1122,865,808,903,900,625,1586,962,1539,1110,770,1000,895,1205,750,654,601,600,1188,635,1225,1151,1031,1049,1000,1105,1924,1346,809,1495,1200,500,1325,900,800,800,1034,980,884,923,1193,2771,779,950,1394,1495,650,670,1126,1028,2404,757,1250,1162,1025,1100,714,1318,1411,2162,1273,1140,942,1058,750,1000,951,635,1250,675,400,577,590,923,1100,1130,618,962,529,817,962,840,866,2404,1126,1160,723,1778,1903,1010,971,525,525,670,500,1058,550,500,865,1081,1304,575,623,515,1273,990,600,1160,500,795,500,740,1250,1250,913,1346,445,265,1250,1607,1452,1391,821,794,500,520,1730,1924,1155,2162,923,1115,1500,826,937,978,1272,1136,800,1339,1063,935,808,375,1082,1155,548,622,841,587,1924,1058,1202,1154,1070,1202,711,1202,850,1000,1000,865,1375,1586,1602,3078,906,952,289,1444,962,1075,909,1250,620,1016,800,1079,654,781,1038,1924,1202,666,905,890,817,577,756,1011,1155,1025,1350,1001,796,1230,754,714,1000,2067,912,600,951,711,1151,1000,841,400,1175,1202,1442,538,781,750,700,1346,800,1250,1105,762,962,800,658,1270,1313,824,1442,1400,1038,668,1100,1000,523,1111,962,729,690,1010,600,596,850,670,793,1442,670,876,841,975,1223,910,533,750,1206,900,1170,540,550,615,909,769,984,833,1027,1000,465,1100,641,1035,950,938,1250,586,693,673,654,692,1111,1368,1282,1250,1346,1424,1161,583,1260,947,1850,1575,1442,489,1126,500,1200,565,1920,684,774,233,975,1366,2137,700,1200,1161,729,750,1026,1111,625,1200,1541,1154,610,1749,350,765,790,818,477,938,2099,350,940,1202,450,1058,1000,318,958,995,1600,511,1411,1346,1522,1075,1200,1377,874,625,1250,1082,693,727,615,913,884,698,800,1000,1300,923,1539,721,815,600,1100,1058,962,433,940,1000,500,800,766,750,550,795,723,1039,200,400,575,850,508,1162,1199,1270,350,963,1463,802,642,751,488,1400,940,866,675,375,325,346,1442,560,550,950,684,1322,1634,472,666,1679,1250,760,1154,1602,1442,865,705,1000,852,770,1000,1329,1682,577,1040,1000,988,478,440,485,705,1550,960,1000,625,1346,479,1201,1346,1122,543,450,507,547,586,462,705,1566,1634,1282,556,1200,855,1053,1000,1602,508,1500,1682,750,843,1004,650,808,2137,761,345,495,987,2500,1212,577,390,1500,583,460,945,1442,1333,1333,700,973,2162,797,400,1015,1744,630,445,660,779,560,1122,453,1386,1539,1154,962,480,808,1442,1091,500,1026,1333,915,910,1000,1160,1001,713,929,400,1241,1065,403,940,812,700,575,450,621,441,625,726,500,1000,393,962,962,865,1154,1386,732,865,700,975,1300,900,829,1000,827,500,1155,700,1710,2004,890,3078,1539,508,1143,962,1250,990,905,926,1559,1312,923,879,800,1049,1190,583,1200,797,1371,1270,1832,909,1746,520,808,2137,692,931,812,866,1442,500,1384,528,881,1026,1620,1843,1602,1000,737,1699,1025,1107,625,720,855,1250,1130,1924,962,1874,1573,940,900,882,1710,1260,751,1097,1001,1000,1250,857,1211,937,904,872,1200,1261,950,115,673,1084,1058,800,1924,855,864,1092,1025,850,673,800,1049,884,1200,664,1111,850,1000,1202,666,923,788,950,1442,866,625,693,2308,1196,700,721,1500,1050,801,1198,390,889,1076,1127,750,855,525,1104,553,800,1384,370,596,402,418,1133,950,600,925,550,606,425,340,692,575,571,817,987,616,1026,808,808,788,850,900,1418,260,662,562,562,357,1009,1442,651,750,754,700,937,624,750,900,540,642,900,513,894,1282,325,769,1040,751,380,300,753,1065,1070,1573,650,700,494,890,520,891,570,1444,500,1473,803,962,1000,600,450,629,492,1562,357,960,566,481,1442,645,788,644,477,664,520,1202,538,873,1000],"type":"scatter","mode":"markers","marker":{"color":"rgba(255,165,0,1)","size":3,"opacity":0.29999999999999999,"line":{"color":"rgba(255,165,0,1)"}},"showlegend":false,"name":"Casado","textfont":{"color":"rgba(255,165,0,1)"},"error_y":{"color":"rgba(255,165,0,1)"},"error_x":{"color":"rgba(255,165,0,1)"},"line":{"color":"rgba(255,165,0,1)"},"xaxis":"x","yaxis":"y","frame":null},{"x":["Casado"],"y":[977.04790419161679],"type":"scatter","mode":"markers","marker":{"color":"red","size":10,"symbol":"circle","line":{"color":"rgba(255,165,0,1)"}},"name":"Média","showlegend":false,"textfont":{"color":"rgba(255,165,0,1)"},"error_y":{"color":"rgba(255,165,0,1)"},"error_x":{"color":"rgba(255,165,0,1)"},"line":{"color":"rgba(255,165,0,1)"},"xaxis":"x","yaxis":"y","frame":null},{"x":["Não Casado"],"y":[798.44000000000005],"type":"scatter","mode":"markers","marker":{"color":"red","size":10,"symbol":"circle","line":{"color":"rgba(70,130,180,1)"}},"name":"Média","showlegend":false,"textfont":{"color":"rgba(70,130,180,1)"},"error_y":{"color":"rgba(70,130,180,1)"},"error_x":{"color":"rgba(70,130,180,1)"},"line":{"color":"rgba(70,130,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

O ponto vermelho indica a média do salário para cada grupo.

Podemos converter a variável `married` para `non-married` (0 = casado, 1
= não casado) e re-estimar o modelo:

``` r
# Converter variável married para non-married
wage2$non_married <- ifelse(wage2$married == 1, 0, 1)

# Estimar modelo de regressão com a nova variável
modelo_dummy_non_married <- lm(wage ~ non_married,
                                data = wage2)

#Comparar modelos
library(stargazer)
stargazer(modelo_dummy_s,
        modelo_dummy_non_married,
        type = "text")
```


    ===========================================================
                                       Dependent variable:     
                                   ----------------------------
                                               wage            
                                        (1)            (2)     
    -----------------------------------------------------------
    married                          178.608***                
                                      (42.411)                 
                                                               
    non_married                                    -178.608*** 
                                                    (42.411)   
                                                               
    Constant                         798.440***    977.048***  
                                      (40.079)      (13.870)   
                                                               
    -----------------------------------------------------------
    Observations                        935            935     
    R2                                 0.019          0.019    
    Adjusted R2                        0.018          0.018    
    Residual Std. Error (df = 933)    400.786        400.786   
    F Statistic (df = 1; 933)        17.736***      17.736***  
    ===========================================================
    Note:                           *p<0.1; **p<0.05; ***p<0.01

Podemos ver que o coeficiente da dummy mudou de sinal, mas a
interpretação alterou de a diferença média no salário entre indivíduos
casados e não casados (`modelo_dummy_s`) para a diferença média no
salário entre indivíduos não casados e casados
(`modelo_dummy_non_married`). A constante do segundo modelo é o salário
médio para indivíduos casados (quando `non_married = 0`).

Quando a variável dependete está em logaritmos, o coeficiente da dummy
pode ser interpretado como uma diferença percentual aproximada. Por
exemplo para o modelo:

<span id="eq-reg-dummy-log">$$
\log(wage_i) = \beta_0 + \beta_1 educ_i + \beta_2 exper_i + \beta_3 married_i + u_i
 \qquad(3)$$</span>

No `R`:

``` r
# Estimar modelo de regressão com log do salário
modelo_dummy_log <- lm(log(wage) ~ educ + exper + married,
                        data = wage2)
summary(modelo_dummy_log)
```


    Call:
    lm(formula = log(wage) ~ educ + exper + married, data = wage2)

    Residuals:
         Min       1Q   Median       3Q      Max 
    -1.89479 -0.23544  0.02585  0.25920  1.27757 

    Coefficients:
                Estimate Std. Error t value Pr(>|t|)    
    (Intercept) 5.327960   0.115832  45.997  < 2e-16 ***
    educ        0.078158   0.006492  12.039  < 2e-16 ***
    exper       0.018290   0.003273   5.588 3.01e-08 ***
    married     0.209262   0.041288   5.068 4.84e-07 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.3879 on 931 degrees of freedom
    Multiple R-squared:  0.1542,    Adjusted R-squared:  0.1515 
    F-statistic: 56.58 on 3 and 931 DF,  p-value: < 2.2e-16

Portanto a variação percentual aproximada é de 20.9%
$100 \times \beta_{married}$. Neste caso, indivíduos casados ganham, em
média, cerca de 20.9% a mais do que indivíduos não casados, *ceteris
paribus*. Para uma interpretação mais precisa, podemos usar a fórmula de
Halvorsen & Palmquist (1980):

$$
\text{Variação Percentual} = (e^{\beta_3} - 1) \times 100
$$

no `R` é necessário extrair o coefiente do modelo e calcular o valor
exponencial:

``` r
# Extrair coeficiente da dummy
beta_married <- coef(modelo_dummy_log)["married"]
# Calcular Variação percentual
variacao_percentual <- (exp(beta_married) - 1) * 100
variacao_percentual
```

     married 
    23.27677 

Em média, indivíduos casados ganham cerca de 23.28% a mais do que
indivíduos não casados, *ceteris paribus*.

ou então pelo método alternativo sugerido por Kennedy (1981):

$$
\text{Variação Percentual} \approx 100 \times \left(\frac{e^{\beta_3} - 1}{1}\right)
$$

Este método é especialmente útil quando o coeficiente é grande
(geralmente maior que 0.1 ou 10%) e para amostras pequenas, pois leva em
consideração a variância do estimador:

$$
\Delta Y \approx \left(e^{\beta - \frac{1}{2} \cdot \text{Var}(\beta)} - 1\right) \times 100
$$

No `R`, a variância do coeficiente pode ser obtida a partir da matriz de
variância-covariância do modelo:

``` r
# Obter variância do coeficiente
var_beta_married <- vcov(modelo_dummy_log)["married", "married"]
# Calcular Variação percentual ajustada
delta_y <- (exp(beta_married - 0.5 * var_beta_married) - 1) * 100
delta_y
```

     married 
    23.17174 

Neste caso, a variação percentual ajustada é de 23.17%, que é
ligeiramente diferente da estimativa anterior.

### Múltiplas Categorias

As variáveis dummy também podem ser usadas para representar variáveis
categóricas. Por exemplo, se tivermos uma variável multinominal que
indica a cor dos carros vendidos (vermelho, azul, verde), podemos criar
dummies para cada categoria:

<span id="eq-dummy-vermelho">$$
D_{vermelho} = \begin{cases}  
1, & \text{se o carro é vermelho} \\
0, & \text{não é vermelho}
\end{cases}
 \qquad(4)$$</span>

<span id="eq-dummy-azul">$$
D_{azul} = \begin{cases}
1, & \text{se o carro é azul} \\
0, & \text{não é azul}
\end{cases}
 \qquad(5)$$</span>

<span id="eq-dummy-verde">$$
D_{verde} = \begin{cases}
1, & \text{se o carro é verde} \\
0, & \text{não é verde}
\end{cases}
 \qquad(6)$$</span>

Exemplo no `R`:

``` r
#introduzir dados
cores <- c("vermelho", "azul", "verde", "vermelho",
            "verde", "azul", "vermelho", "verde", 
            "azul", "vermelho")
preço <- c(50000, 52000, 48000, 51000, 49000, 53000, 
            50000, 52000, 48000, 51000)

dados <- data.frame(cores, preço)

#para tabela resumo
library(gtsummary)

# Criar dummies e taela
dados |>
    mutate(
        vermelho = ifelse(cores == "vermelho", 1, 0),
        azul = ifelse(cores == "azul", 1, 0),
        verde = ifelse(cores == "verde", 1, 0)
    ) |>
    select(cores, vermelho, azul, verde) |>
    tbl_summary(
        by = cores)
```

<div id="wfvotmhpbc" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#wfvotmhpbc table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
&#10;#wfvotmhpbc thead, #wfvotmhpbc tbody, #wfvotmhpbc tfoot, #wfvotmhpbc tr, #wfvotmhpbc td, #wfvotmhpbc th {
  border-style: none;
}
&#10;#wfvotmhpbc p {
  margin: 0;
  padding: 0;
}
&#10;#wfvotmhpbc .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}
&#10;#wfvotmhpbc .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}
&#10;#wfvotmhpbc .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}
&#10;#wfvotmhpbc .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}
&#10;#wfvotmhpbc .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}
&#10;#wfvotmhpbc .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#wfvotmhpbc .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}
&#10;#wfvotmhpbc .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}
&#10;#wfvotmhpbc .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}
&#10;#wfvotmhpbc .gt_column_spanner_outer:first-child {
  padding-left: 0;
}
&#10;#wfvotmhpbc .gt_column_spanner_outer:last-child {
  padding-right: 0;
}
&#10;#wfvotmhpbc .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}
&#10;#wfvotmhpbc .gt_spanner_row {
  border-bottom-style: hidden;
}
&#10;#wfvotmhpbc .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}
&#10;#wfvotmhpbc .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}
&#10;#wfvotmhpbc .gt_from_md > :first-child {
  margin-top: 0;
}
&#10;#wfvotmhpbc .gt_from_md > :last-child {
  margin-bottom: 0;
}
&#10;#wfvotmhpbc .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}
&#10;#wfvotmhpbc .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#wfvotmhpbc .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}
&#10;#wfvotmhpbc .gt_row_group_first td {
  border-top-width: 2px;
}
&#10;#wfvotmhpbc .gt_row_group_first th {
  border-top-width: 2px;
}
&#10;#wfvotmhpbc .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#wfvotmhpbc .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}
&#10;#wfvotmhpbc .gt_first_summary_row.thick {
  border-top-width: 2px;
}
&#10;#wfvotmhpbc .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#wfvotmhpbc .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#wfvotmhpbc .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}
&#10;#wfvotmhpbc .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}
&#10;#wfvotmhpbc .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}
&#10;#wfvotmhpbc .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}
&#10;#wfvotmhpbc .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}
&#10;#wfvotmhpbc .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#wfvotmhpbc .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}
&#10;#wfvotmhpbc .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}
&#10;#wfvotmhpbc .gt_left {
  text-align: left;
}
&#10;#wfvotmhpbc .gt_center {
  text-align: center;
}
&#10;#wfvotmhpbc .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}
&#10;#wfvotmhpbc .gt_font_normal {
  font-weight: normal;
}
&#10;#wfvotmhpbc .gt_font_bold {
  font-weight: bold;
}
&#10;#wfvotmhpbc .gt_font_italic {
  font-style: italic;
}
&#10;#wfvotmhpbc .gt_super {
  font-size: 65%;
}
&#10;#wfvotmhpbc .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}
&#10;#wfvotmhpbc .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}
&#10;#wfvotmhpbc .gt_indent_1 {
  text-indent: 5px;
}
&#10;#wfvotmhpbc .gt_indent_2 {
  text-indent: 10px;
}
&#10;#wfvotmhpbc .gt_indent_3 {
  text-indent: 15px;
}
&#10;#wfvotmhpbc .gt_indent_4 {
  text-indent: 20px;
}
&#10;#wfvotmhpbc .gt_indent_5 {
  text-indent: 25px;
}
&#10;#wfvotmhpbc .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}
&#10;#wfvotmhpbc div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>

<table class="gt_table" data-quarto-postprocess="true"
data-quarto-disable-processing="false" data-quarto-bootstrap="false">
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="gt_col_headings">
<th id="label" class="gt_col_heading gt_columns_bottom_border gt_left"
data-quarto-table-cell-role="th"
scope="col"><strong>Characteristic</strong></th>
<th id="stat_1"
class="gt_col_heading gt_columns_bottom_border gt_center"
data-quarto-table-cell-role="th" scope="col"><strong>azul</strong><br />
N = 3<span class="gt_footnote_marks"
style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span></th>
<th id="stat_2"
class="gt_col_heading gt_columns_bottom_border gt_center"
data-quarto-table-cell-role="th"
scope="col"><strong>verde</strong><br />
N = 3<span class="gt_footnote_marks"
style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span></th>
<th id="stat_3"
class="gt_col_heading gt_columns_bottom_border gt_center"
data-quarto-table-cell-role="th"
scope="col"><strong>vermelho</strong><br />
N = 4<span class="gt_footnote_marks"
style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span></th>
</tr>
</thead>
<tbody class="gt_table_body">
<tr>
<td class="gt_row gt_left" headers="label">vermelho</td>
<td class="gt_row gt_center" headers="stat_1">0 (0%)</td>
<td class="gt_row gt_center" headers="stat_2">0 (0%)</td>
<td class="gt_row gt_center" headers="stat_3">4 (100%)</td>
</tr>
<tr>
<td class="gt_row gt_left" headers="label">azul</td>
<td class="gt_row gt_center" headers="stat_1">3 (100%)</td>
<td class="gt_row gt_center" headers="stat_2">0 (0%)</td>
<td class="gt_row gt_center" headers="stat_3">0 (0%)</td>
</tr>
<tr>
<td class="gt_row gt_left" headers="label">verde</td>
<td class="gt_row gt_center" headers="stat_1">0 (0%)</td>
<td class="gt_row gt_center" headers="stat_2">3 (100%)</td>
<td class="gt_row gt_center" headers="stat_3">0 (0%)</td>
</tr>
</tbody><tfoot>
<tr class="gt_footnotes">
<td colspan="4" class="gt_footnote"><span class="gt_footnote_marks"
style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span>
n (%)</td>
</tr>
</tfoot>
&#10;</table>

</div>

A função `ifelse()` é usada para criar as variáveis dummy. A sintaxe é
`ifelse(condição, valor_se_condicao_verificada, valor_se_condicao_nao_verificada)`,
ver `?ifelse` para mais detalhes.

Para um exemplo real vamos usar o conjunto de dados `ceosal1` da
biblioteca `wooldridge`, que contém informações sobre o salário dos CEOs
de várias empresas, bem como outras características como anos de
experiência, anos na empresa dummies que ondicam o setor de atividade da
empresa:

- `finance` - empresa do setor financeiro (1) ou não (0)
- `indus` - empresa do setor industrial (1) ou não (0)
- `consprod` - empresa do setor de bens de consumo (1) ou não (0)
- `utility` - empresa do setor de serviços essenciais (1) ou não (0)

Neste conjunto não é necessário calcular as dummies, pois já estão
incluídas (o que nem sempre acontece). Vamos estimar um modelo de
regressão do logaritmo do salário dos CEOs (`lsalary`) em função do
logaritmo vendas
(`lsales), da rendibilidade dos capitais próprios (`roe\`) e o setor de
atividade da empresa:

<span id="eq-reg-dummy-multiplas-categorias">$$
\begin{split}
\log(salary_i) = \beta_0 &+ \beta_1 \log(sales_i) + \beta_2 roe_i + \beta_3 D_{finance} + \\
&\beta_4 D_{indus} + \beta_5 D_{consprod} + \beta_6 D_{utility} + u_i
\end{split}
 \qquad(7)$$</span>

No `R`:

``` r
library(wooldridge)
data("ceosal1")
# Estimar modelo de regressão
modelo_dummy_mult <- lm(lsalary ~ lsales + roe + finance + indus + consprod + utility,
                        data = ceosal1)
summary(modelo_dummy_mult)
```


    Call:
    lm(formula = lsalary ~ lsales + roe + finance + indus + consprod + 
        utility, data = ceosal1)

    Residuals:
         Min       1Q   Median       3Q      Max 
    -1.09465 -0.22173 -0.01973  0.17141  2.64394 

    Coefficients: (1 not defined because of singularities)
                Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  4.30510    0.28156  15.290  < 2e-16 ***
    lsales       0.25719    0.03204   8.029 7.85e-14 ***
    roe          0.01115    0.00430   2.594   0.0102 *  
    finance      0.44096    0.10365   4.254 3.20e-05 ***
    indus        0.28300    0.09923   2.852   0.0048 ** 
    consprod     0.46389    0.10887   4.261 3.12e-05 ***
    utility           NA         NA      NA       NA    
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.4598 on 203 degrees of freedom
    Multiple R-squared:  0.3569,    Adjusted R-squared:  0.341 
    F-statistic: 22.53 on 5 and 203 DF,  p-value: < 2.2e-16

Na regressão anterior o coeficiente da dummy `utility` aparece com valor
NA, porque existe multicolinearidade perfeita entre as dummies (todas
somadas é obtida uma coluna de 1’s, que é uma constante).

``` r
# Verificar multicolinearidade perfeita
ceosal1 %>%
    mutate(soma_dummies = finance + indus + consprod + utility) %>%
    summarise(min_soma = min(soma_dummies),
              max_soma = max(soma_dummies),
              unique_somas = n_distinct(soma_dummies))
```

      min_soma max_soma unique_somas
    1        1        1            1

Este fenómeno é conhecido como a armadilha das dummies. Em alguns
softwares econométrico pode aparecer uma mensagem de erro a indicar que
existe multicolinearidade perfeita. Para evitar este problema, uma das
categorias deve ser excluída do modelo, ou seja, o modelo deve incluir
apenas $k-1$ dummies (com $k$ o número de categorias). A categoria
excluída é a categoria de referência (base) e as outras dummies medem o
efeito relativo em comparação com essa mesma categoria. De referir que
nesta amostra cada empresa pertence a apenas um setor. Neste caso,
podemos excluir a dummy `utility` e re-estimar o modelo:

<span id="eq-reg-dummy-multiplas-categorias-2">$$
\begin{split}
\log(salary_i) = \beta_0 &+ \beta_1 \log(sales_i) + \beta_2 roe_i + \beta_3 D_{finance} + \\
& \beta_4 D_{indus} + \beta_5 D_{consprod} + u_i
\end{split}
 \qquad(8)$$</span>

No `R`:

``` r
# Estimar modelo de regressão sem a dummy utility
modelo_dummy_mult2 <- lm(lsalary ~ lsales + roe + finance + indus + consprod,
                         data = ceosal1)
summary(modelo_dummy_mult2)
```


    Call:
    lm(formula = lsalary ~ lsales + roe + finance + indus + consprod, 
        data = ceosal1)

    Residuals:
         Min       1Q   Median       3Q      Max 
    -1.09465 -0.22173 -0.01973  0.17141  2.64394 

    Coefficients:
                Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  4.30510    0.28156  15.290  < 2e-16 ***
    lsales       0.25719    0.03204   8.029 7.85e-14 ***
    roe          0.01115    0.00430   2.594   0.0102 *  
    finance      0.44096    0.10365   4.254 3.20e-05 ***
    indus        0.28300    0.09923   2.852   0.0048 ** 
    consprod     0.46389    0.10887   4.261 3.12e-05 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.4598 on 203 degrees of freedom
    Multiple R-squared:  0.3569,    Adjusted R-squared:  0.341 
    F-statistic: 22.53 on 5 and 203 DF,  p-value: < 2.2e-16

Interpretação dos coeficientes :

- $\beta_0$ (constante): O salário médio para uma empresa do setor de
  serviços essenciais (categoria de referência) com vendas iguais a 1
  (log(1) = 0) e rendibilidade dos capitais próprios igual a 0 é de 257
  US\$.
- $\beta_1$ (lsales): Um aumento de 1% nas vendas está associado a um
  aumento médio de 25.7% no salário do CEO, *ceteris paribus*.
- $\beta_2$ (roe): Um aumento de 1 ponto percentual na rendibilidade dos
  capitais próprios está associado a um aumento médio de aproximadamente
  1.1% no salário do CEO, *ceteris paribus* (a variável `roe` está
  expressa em percentagem, 0-100).
- $\beta_3$ (finance): CEOs de empresas do setor financeiro ganham,
  aproximadamente em média, cerca de 44.1% a mais do que CEOs de
  empresas do setor de serviços essenciais (categoria de referência),
  *ceteris paribus*.
- $\beta_4$ (indus): CEOs de empresas do setor industrial ganham,
  aproximadamente em média, cerca de 28.3% a mais do que CEOs de
  empresas do setor de serviços essenciais, *ceteris paribus*.
- $\beta_5$ (consprod): CEOs de empresas do setor de bens de consumo
  ganham, aproximadamente em média, cerca de 3.3% a mais do que CEOs de
  empresas do setor de serviços essenciais, *ceteris paribus*.

podemos considerar a dummy `indus` como a categoria de referência,
excluindo-a do modelo:

``` r
# Estimar modelo de regressão sem a dummy indus
modelo_dummy_mult3 <- lm(lsalary ~ lsales + roe + finance + consprod + utility,
                         data = ceosal1)

library(stargazer)
stargazer(modelo_dummy_mult2,
        modelo_dummy_mult3,
        type = "text")
```


    ===========================================================
                                       Dependent variable:     
                                   ----------------------------
                                             lsalary           
                                        (1)            (2)     
    -----------------------------------------------------------
    lsales                            0.257***      0.257***   
                                      (0.032)        (0.032)   
                                                               
    roe                               0.011**        0.011**   
                                      (0.004)        (0.004)   
                                                               
    finance                           0.441***       0.158*    
                                      (0.104)        (0.089)   
                                                               
    indus                             0.283***                 
                                      (0.099)                  
                                                               
    consprod                          0.464***       0.181**   
                                      (0.109)        (0.085)   
                                                               
    utility                                         -0.283***  
                                                     (0.099)   
                                                               
    Constant                          4.305***      4.588***   
                                      (0.282)        (0.295)   
                                                               
    -----------------------------------------------------------
    Observations                        209            209     
    R2                                 0.357          0.357    
    Adjusted R2                        0.341          0.341    
    Residual Std. Error (df = 203)     0.460          0.460    
    F Statistic (df = 5; 203)        22.529***      22.529***  
    ===========================================================
    Note:                           *p<0.1; **p<0.05; ***p<0.01

Podemos observar que do modelo `modelo_dummy_mult2` e
`modelo_dummy_mult3` que os coeficientes das dummies dos setores
alteraram, pois a categoria de referência mudou. O coeficiente da dummy
`indus` no primeiro modelo é o inverso do coeficiente da dummy `utility`
no segundo modelo. Tal como na regressão
<a href="#eq-reg-dummy-simples" class="quarto-xref">Equation 2</a>,
também é possível calcular a variação de salário em percentagem para as
dummies do modelo <a href="#eq-reg-dummy-multiplas-categorias-2"
class="quarto-xref">Equation 8</a>, usando a fórmula de Halvorsen &
Palmquist (1980) ou o método alternativo sugerido por Kennedy (1981).

``` r
# Extrair coeficientes das dummies
beta_finance <- coef(modelo_dummy_mult2)["finance"]
beta_indus <- coef(modelo_dummy_mult2)["indus"]
beta_consprod <- coef(modelo_dummy_mult2)["consprod"]
# Calcular variação percentual usando a fórmula de Halvorsen e Palmquist (1980)
variacao_percentual_finance <- (exp(beta_finance) - 1) * 100
variacao_percentual_indus <- (exp(beta_indus) - 1) * 100
variacao_percentual_consprod <- (exp(beta_consprod) - 1) * 100

#calcular variação percentual ajustada usando o método de Kennedy (1981)
var_beta_finance <- vcov(modelo_dummy_mult2)["finance", "finance"]
var_beta_indus <- vcov(modelo_dummy_mult2)["indus", "indus"]
var_beta_consprod <- vcov(modelo_dummy_mult2)["consprod", "consprod"]
delta_y_finance <- (exp(beta_finance - 0.5 * var_beta_finance) - 1) * 100
delta_y_indus <- (exp(beta_indus - 0.5 * var_beta_indus) - 1) * 100
delta_y_consprod <- (exp(beta_consprod - 0.5 * var_beta_consprod) - 1) * 100

#organizar em tabela:
tabela_variacao <- data.frame(
    Categoria = c("finance", "indus", "consprod"),
    Variacao_Percentual = c(variacao_percentual_finance,
                            variacao_percentual_indus,
                            variacao_percentual_consprod),
    Variacao_Percentual_Ajustada = c(delta_y_finance,
                                    delta_y_indus,
                                    delta_y_consprod)
)
tabela_variacao
```

             Categoria Variacao_Percentual Variacao_Percentual_Ajustada
    finance    finance            55.41952                     54.58691
    indus        indus            32.71071                     32.05889
    consprod  consprod            59.02531                     58.08560

A interpretação é semelhantes ao exemplo anterior. Também é possível
utilizar variáveis dummy como variável independente. Esta abordagem será
discutida na **?@sec-variavel-dependente-limitada**.

## Modelos com Termos de Interação

Um termo de interação é utilizado para capturar o efeito conjunto de
duas ou mais variáveis independentes na variável dependente. Portanto
num exemplo simples, um termo de interação entre duas variáveis $X_1$ e
$X_2$ é representado como $X_1 \times X_2$. O termo de interação permite
que o efeito de uma variável independente dependa do valor de outra
variável independente.

<span id="eq-reg-interacao">$$
y_i = \beta_0 + \beta_1 X_{1i} + \beta_2 X_{2i} + \beta_3 (X_{1i} \times X_{2i}) + u_i
 \qquad(9)$$</span>

Podemos criar termos de interaão entre variáveis contínuas, entre
variáveis dummy e entre variáveis dummy e contínuas.

### entre variáveis contínuas

Para esta apicação vamos utilizar o conjunto de dados `wage2` da
biblioteca `wooldridge`. Mais informação sobre o conjunto de dados pode
ser obtida com `?wage2`.

Vamos estimar a regressão do salário em função dos anos de educação,
anos de experiência e a interacção entre educação e experiência:

<span id="eq-reg-interacao-continua">$$
wage_i = \beta_0 + \beta_1 educ_i + \beta_2 exper_i + \beta_3 (educ_i \times exper_i) + u_i
 \qquad(10)$$</span>

O termo de interação pode ser crido no `data.frame`
(`mutate(educ_exper = educ* exper)`) ou diretamente na fórmula do
modelo. No `R`:

``` r
library(tidyverse)
library(wooldridge)

# carregar dados
data("wage2")

#Estimar modelo de regressão com interacção
modelo_interacoes <- lm(wage ~ educ + exper + I(educ * exper), data = wage2)
summary(modelo_interacoes)
```


    Call:
    lm(formula = wage ~ educ + exper + I(educ * exper), data = wage2)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -948.46 -254.53  -29.57  192.59 2150.77 

    Coefficients:
                    Estimate Std. Error t value Pr(>|t|)   
    (Intercept)      271.934    230.227   1.181  0.23784   
    educ              35.106     16.626   2.112  0.03499 * 
    exper            -32.663     19.099  -1.710  0.08757 . 
    I(educ * exper)    3.904      1.462   2.670  0.00771 **
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 375.1 on 931 degrees of freedom
    Multiple R-squared:  0.1424,    Adjusted R-squared:  0.1397 
    F-statistic: 51.54 on 3 and 931 DF,  p-value: < 2.2e-16

O `I` é necessário para indicar ao `R` que a operação deve ser
interpretada literalmente, ou seja, como uma multiplicação entre as
variáveis `educ` e `exper`. Sem o `I`, o `R` pode interpretar o símbolo
`*` como um operador especial na fórmula do modelo, o que pode levar a
resultados inesperados.

Na interpretação dos coeficientes é necessário ter alguma atenção pois o
coeficiente de cada variável que também faz parte do termo de interação
depende do valor da outra variável. A interpretação dos coeficientes é a
seguinte:

- $\beta_0$ (constante): O salário médio para alguém com 0 anos de
  educação e 0 anos de experiência é de 271.934 US\$. Pode não ter uma
  interpretação prática, pois é improvável que alguém tenha 0 anos de
  educação e experiência.
- $\beta_1$ (`educ`): Cada ano adicional de educação está associado a um
  aumento médio de 35.106 US\$ no salário, *ceteris paribus*, **para
  alguém sem experiência** (exper = 0).
- $\beta_2$ (`exper`): Cada ano adicional de experiência está associado
  a uma diminuição média de 32.663 US\$ no salário, *ceteris paribus*,
  **para alguém sem educação** (educ = 0).
- $\beta_3$ (`I(educ * exper)`): O coeficiente de interação indica que o
  efeito combinado de educação e experiência no salário é positivo.
  Especificamente, para cada ano adicional de educação, o efeito da
  experiência no salário aumenta em 3.904 US\$, e vice-versa.

Num caso em que o termo de interação é negativo, por exemplo de -3.904,
indicaria que o efeito combinado de educação e experiência no salário é
negativo, ou seja, para cada ano adicional de educação, o efeito da
experiência no salário diminui em 3.904 US\$, e vice-versa.

Para calcular o efeito marginal da educação no salário para diferentes
níveis de experiência. O efeito marginal da educação é dado por:

$$
\frac{\partial wage}{\partial educ} = \beta_1 + \beta_3 \times exper
$$ Portanto, o efeito marginal da educação varia com o nível de
experiência. Podemos calcular o efeito marginal da educação para
diferentes níveis de experiência (0, 5, 10, 20 anos):

``` r
# Níveis de experiência para calcular o efeito marginal
niv_experiencia <- c(0, 5, 10, 20)
# Coeficientes do modelo
coeficientes <- coef(modelo_interacoes)
# Calcular efeito marginal da educação para diferentes níveis de experiência
efeito_marginal_educ <- coeficientes["educ"] +
                coeficientes["I(educ * exper)"] * niv_experiencia
# Criar data frame com os resultados obtidos
data.frame(Nivel_Experiencia = niv_experiencia,
            Efeito_Marginal_Educacao = efeito_marginal_educ)
```

      Nivel_Experiencia Efeito_Marginal_Educacao
    1                 0                 35.10600
    2                 5                 54.62379
    3                10                 74.14158
    4                20                113.17716

Para um modelo onde a variável dependente está em logaritmo natural, o
coeficiente da variável contínua no termo de interação pode ser
interpretado como uma variação percentual aproximada. Por exemplo, para
o modelo:

``` r
modelo_interacoes_log <- lm(log(wage) ~ educ + exper + I(educ * exper), data = wage2)
summary(modelo_interacoes_log)
```


    Call:
    lm(formula = log(wage) ~ educ + exper + I(educ * exper), data = wage2)

    Residuals:
         Min       1Q   Median       3Q      Max 
    -1.88558 -0.24553  0.03558  0.26171  1.28836 

    Coefficients:
                     Estimate Std. Error t value Pr(>|t|)    
    (Intercept)      5.949455   0.240826  24.704   <2e-16 ***
    educ             0.044050   0.017391   2.533   0.0115 *  
    exper           -0.021496   0.019978  -1.076   0.2822    
    I(educ * exper)  0.003203   0.001529   2.095   0.0365 *  
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.3923 on 931 degrees of freedom
    Multiple R-squared:  0.1349,    Adjusted R-squared:  0.1321 
    F-statistic: 48.41 on 3 and 931 DF,  p-value: < 2.2e-16

- `constante`: O salário médio para alguém com 0 anos de educação e 0
  anos de experiência é de 6.579 US\$ (log(719.82)).
- `educ`: Cada ano adicional de educação está associado a um aumento
  médio de aproximadamente 4.4% no salário, *ceteris paribus*, **para
  alguém sem experiência** (exper = 0).
- `exper`: Cada ano adicional de experiência está associado a uma
  diminuição média de aproximadamente 2.15% no salário, *ceteris
  paribus*, **para alguém sem educação** (educ = 0).
- `I(educ * exper)`: O coeficiente de interação indica que o efeito
  combinado de educação e experiência no salário é positivo.
  Especificamente, para cada ano adicional de educação, o efeito da
  experiência no salário aumenta em aproximadamente 0.32%, e vice-versa.

De notar que o coeficiente da variável `exper` não é estatisticamente
diferente de zero para nenhum nível de significância estatística.

### entre variáveis dummy

Recorrendo ao conjunto de dados anterior (`wage2`), onde a variável
`married` indica se o indivíduo é casado (1) ou não (0) e a variável
dummy `urban` indica se o indivíduo vive numa área urbana (1) ou não
(0), vamos estimar o modelo de regressão com termos de interação entre
variáveis dummy:

<span id="eq-reg-interacao-dummy">$$
\log(wage_i) = \beta_0 + \beta_1 educ_i + \beta_2 exper_i + \beta_3 married_i + \beta_4 urban_i + \beta_5 (married_i \times urban_i) + u_i
 \qquad(11)$$</span>

no `R`:

``` r
#estimar modelo de regressão com interacção entre dummies
modelo_interacoes_dummy <- lm(log(wage) ~ educ + exper + married + 
                            urban + I(married * urban), data = wage2)
summary(modelo_interacoes_dummy)
```


    Call:
    lm(formula = log(wage) ~ educ + exper + married + urban + I(married * 
        urban), data = wage2)

    Residuals:
         Min       1Q   Median       3Q      Max 
    -1.94789 -0.22230  0.02629  0.24607  1.22611 

    Coefficients:
                        Estimate Std. Error t value Pr(>|t|)    
    (Intercept)         5.198499   0.134840  38.553  < 2e-16 ***
    educ                0.075924   0.006374  11.911  < 2e-16 ***
    exper               0.018587   0.003207   5.796 9.29e-09 ***
    married             0.240197   0.083079   2.891  0.00393 ** 
    urban               0.204342   0.090328   2.262  0.02391 *  
    I(married * urban) -0.028581   0.094926  -0.301  0.76341    
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.3799 on 929 degrees of freedom
    Multiple R-squared:  0.1905,    Adjusted R-squared:  0.1861 
    F-statistic: 43.71 on 5 and 929 DF,  p-value: < 2.2e-16

interpretação dos coeficientes: - $\beta_0$ (constante): O salário médio
para alguém com 0 anos de educação, 0 anos de experiência, que não é
casado e que não vive numa área urbana é de 180.91 US\$ (`exp(5.198)`).

- $\beta_1$ (educ): Cada ano adicional de educação está associado a um
  aumento médio de aproximadamente 7.5 % no salário, *ceteris paribus*.

- $\beta_2$ (exper): Cada ano adicional de experiência está associado a
  um aumento médio de aproximadamente 1.8% no salário, *ceteris
  paribus*.

- $\beta_3$ (married): Ser casado está associado a um aumento médio de
  aproximadamente 24.20% no salário, *ceteris paribus*, **para alguém
  sem experiência e sem educação** (exper = 0, educ = 0).

- $\beta_4$ (urban): Viver numa área urbana está associado a um aumento
  médio de aproximadamente 20.4.% no salário, *ceteris paribus*, **para
  alguém sem experiência e sem educação** (exper = 0, educ = 0).

- $\beta_5$ (married \* urban): O coeficiente de interação indica que o
  efeito combinado de ser casado e viver em uma área urbana no salário é
  negativo. Especificamente, para indivíduos casados e viver numa área
  urbana, o efeito combinado no salário é de aproximadamente -2.86% em
  relação ao efeito individual de ser casado e viver numa área urbana.

No termo de interação entre variáveis dummy, neste caso `married` e
`urban`, o 1 (da multiplicaão) corresponde a indivíduos que são casados
e vivem numa área urbana (1*1), o 0 (da multiplicação) corresponde a
indivíduos que não são casados e não vivem numa área urbana (0*0), e os
outros dois casos (1*0 e 0*1) correspondem a indivíduos que são casados
mas não vivem numa área urbana ou que não são casados mas vivem numa
área urbana. Portanto, o coeficiente do termo de interação mede o efeito
adicional de ser casado e viver numa área urbana em comparação com os
efeitos individuais de ser casado e viver numa área urbana. Se criarmos
uma tabela com os diferentes grupos, podemos ver melhor a interpretação
de cada caso: ou seja:

| married | urban | Interação (= married × urban) | log(wage) previsto | wage previsto (e^{}) | Interpretação |
|----|----|----|----|----|----|
| 0 | 0 | 0 | $\beta_0$ | $e^{\beta_0}$ | Não casado, não vive numa área urbana |
| 1 | 0 | 0 | $\beta_0 + \beta_3$ | $e^{\beta_0 + \beta_3}$ | Casado, não vive numa área urbana |
| 0 | 1 | 0 | $\beta_0 + \beta_4$ | $e^{\beta_0 + \beta_4}$ | Não casado, vive numa área urbana |
| 1 | 1 | 1 | $\beta_0 + \beta_3 + \beta_4 + \beta_5$ | $e^{\beta_0 + \beta_3 + \beta_4 + \beta_5}$ | Casado, vive numa área urbana |

De notar que no modelo
<a href="#eq-reg-interacao-dummy" class="quarto-xref">Equation 11</a> o
coeficiente do termo de interação não é estatisticamente diferente de
zero para nenhum nível de significância estatística. Ou seja, não existe
evidência estatística de que o efeito combinado de ser casado e viver
numa área urbana no salário seja diferente do efeito individual de ser
casado e viver numa área urbana, o que também é um resultado
interessante.

### entre variáveis dummy e contínuas

Para esta aplicação vamos considerar o mesmo conjunto de dados `wage2` e
vamos estimar a regressão com o termo de interação entre a variável
dummy `south` e a variável contínua `educ`:

<span id="eq-reg-interacao-dummy-cont">$$
\log(wage_i) = \beta_0 + \beta_1 educ_i + \beta_2 exper_i + \beta_3 south_i + \beta_4 (south_i \times educ_i) + u_i
 \qquad(12)$$</span>

no `R`:

``` r
#carregar dados
data("wage2")

# Estimar modelo
modelo_interacoes_dummy_cont <- lm(log(wage) ~ educ + exper + south +
                                     I(south * educ), data = wage2)

summary(modelo_interacoes_dummy_cont)
```


    Call:
    lm(formula = log(wage) ~ educ + exper + south + I(south * educ), 
        data = wage2)

    Residuals:
         Min       1Q   Median       3Q      Max 
    -1.93539 -0.23116  0.02579  0.25730  1.28557 

    Coefficients:
                     Estimate Std. Error t value Pr(>|t|)    
    (Intercept)      5.699845   0.122511  46.525  < 2e-16 ***
    educ             0.066953   0.007543   8.876  < 2e-16 ***
    exper            0.019671   0.003256   6.041 2.21e-09 ***
    south           -0.464313   0.167538  -2.771  0.00569 ** 
    I(south * educ)  0.024110   0.012422   1.941  0.05257 .  
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.3868 on 930 degrees of freedom
    Multiple R-squared:  0.1601,    Adjusted R-squared:  0.1564 
    F-statistic: 44.31 on 4 and 930 DF,  p-value: < 2.2e-16

Em que a interpretação dos coeficientes é a seguinte:

- $\beta_0$ (constante): O salário médio para alguém com 0 anos de
  educação, 0 anos de experiência e que não vive no sul é de 298.87 US\$
  (`exp(5.70)`).

- $\beta_1$ (educ): Cada ano adicional de educação está associado a um
  aumento médio de aproximadamente 6.7% no salário, *ceteris paribus*,
  **para alguém que não vive no sul** (south = 0).

- $\beta_2$ (exper): Cada ano adicional de experiência está associado a
  um aumento médio de aproximadamente 2% no salário, *ceteris paribus*.

- $\beta_3$ (south): Viver no sul está associado a uma diminuição média
  de aproximadamente 46.4% no salário, *ceteris paribus*, **para alguém
  sem anos de educação** (educ = 0).

- $\beta_4$ (south \* educ): O coeficiente de interação indica que o
  efeito combinado de viver no sul e anos de educação no salário é
  positivo. Especificamente, para cada ano adicional de educação, o
  efeito de viver no sul no salário aumenta em aproximadamente 2.41%,
  *ceteris paribus*.

O efeito marginal da educação no salário para diferentes níveis de
experiência é dado por: <span id="eq-efeito-marginal-educacao-dummy">$$
\frac{\partial \log(wage)}{\partial educ} = \beta_1 + \beta_4 \times south
 \qquad(13)$$</span>

Portanto, o efeito marginal da educação varia com o valor da variável
dummy `south`. Podemos calcular o efeito marginal da educação para os
dois grupos (south = 0 e south = 1):

``` r
# Coeficientes do modelo
coeficientes <- coef(modelo_interacoes_dummy_cont)
# Calcular efeito marginal da educação para os dois grupos
efeito_marginal_educacao <- data.frame(
  south = c(0, 1),
  efeito_marginal = c(coeficientes["educ"] + 0 * coeficientes["I(south * educ)"],
                      coeficientes["educ"] + 1 * coeficientes["I(south * educ)"])
)
efeito_marginal_educacao
```

      south efeito_marginal
    1     0      0.06695309
    2     1      0.09106290

Para indivíduos que não vivem no sul (south = 0), cada ano adicional de
educação está associado a um aumento médio de aproximadamente 6.7% no
salário, *ceteris paribus*. Para indivíduos que vivem no sul (south =
1), cada ano adicional de educação está associado a um aumento médio de
aproximadamente 9.11% no salário, *ceteris paribus*.

## Modelos Não Lineares

Modelos não lineares são modelos em que a relação entre a variável
dependente e uma ou mais variáveis independentes não é linear. A equação
do modelo não linear para a variável dependente `y` e para as variáveis
independentes `X` é dada por:

<span id="eq-reg-nao-linear">$$
y_i = \beta_0 + \beta_1 x_{1i} + \beta_2 x_{1i}^2 + u_i
 \qquad(14)$$</span>

em que $\beta_0$ é a constante, $\beta_1$ é o coeficiente da variável
independente `x1`, $\beta_2$ é o coeficiente do termo quadrático da
variável independente `x2` e $u_i$ é o termo de erro. O modelo é
estimado pelo método dos mínimos quadrados ordinários. Quando devemos
estimar um modelo não linear? Uma abordagem comum é a análise gráfica.
Se a relação entre a variável dependente e a variável independente não
for aproximadamente linear, pode ser apropriado considerar um modelo não
linear. Outra abordagem é incluir termos polinomiais (como o termo
quadrático) na regressão e verificar se esses termos são
estatisticamente diferentes de 0. Existem alguma forma de lidar com a
não linearidade sem recorrer a modelos não lineares, como por exemplo,
transformar as variáveis (logaritmos).

Um exemplo de um modelo que poderá ser não linear é a relação entre o
preço das casas e a distância à autoestrada mais próxima. Vamos
considerar o conjunto de dados `hprice2` da biblioteca `wooldridge`, que
contém informações sobre o preço das casas (`price`), a distância à
autoestrada mais próxima (`dist`) e outras características das casas.
Mais informação sobre o conjunto de dados pode ser obtida com
`?hprice2`.

Para esta aplicação vamos estimar o modelo:

<span id="eq-reg-nao-linear-hprice">$$
\log(price_i) = \beta_0 + \beta_1 dist_i + \beta_2 dist_i^2 + u_i
 \qquad(15)$$</span>

no `R`:

``` r
library(wooldridge)
data("hprice3")

# Estimar modelo de regressão não linear
modelo_nao_linear <- lm(price ~ inst + I(inst^2), data = hprice3)
summary(modelo_nao_linear)
```


    Call:
    lm(formula = price ~ inst + I(inst^2), data = hprice3)

    Residuals:
       Min     1Q Median     3Q    Max 
    -82012 -25286  -8394  20140 194917 

    Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  3.443e+04  7.340e+03   4.691 4.04e-06 ***
    inst         8.618e+00  1.013e+00   8.510 6.95e-16 ***
    I(inst^2)   -2.276e-04  2.953e-05  -7.708 1.64e-13 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 38860 on 318 degrees of freedom
    Multiple R-squared:  0.1969,    Adjusted R-squared:  0.1918 
    F-statistic: 38.98 on 2 and 318 DF,  p-value: 7.228e-16

A interpretação do termo de interação é a seguinte:

- $\beta_2$ (I(inst^2)): O coeficiente do termo quadrático indica que o
  efeito da distância no preço da casa não é constante. Especificamente,
  o efeito da distância no preço da casa diminui à medida que a
  distância aumenta. Isto sugere que o impacto inicial de uma casa estar
  afastada da autoestrada é maior do que o impacto adicional de uma casa
  ainda mais afastada. O coeficiente do termo quadrático é -0,0002276, o
  que indica que para cada unidade de aumento na distância, o efeito da
  distância no preço da casa diminui em aproximadamente 0,0002276
  dólares, *ceteris paribus*. O efeito é muito pequeno o que pode estar
  relacionado com a escala da variável `inst` (distância em pés), 1 pé
  são aproximadamente 0.3048 metros.

Vamos converter a variável `inst` de pés para quilómetros (1 pé =
0.0003048 km) e re-estimar o modelo:

``` r
# Converter inst de pés para quilómetros
hprice3 <- hprice3 %>%
    mutate(inst_km = inst * 0.0003048)
# Estimar modelo de regressão não linear com inst em km
modelo_nao_linear_km <- lm(price ~ inst_km + I(inst_km^2), data = hprice3)
summary(modelo_nao_linear_km)
```


    Call:
    lm(formula = price ~ inst_km + I(inst_km^2), data = hprice3)

    Residuals:
       Min     1Q Median     3Q    Max 
    -82012 -25286  -8394  20140 194917 

    Coefficients:
                 Estimate Std. Error t value Pr(>|t|)    
    (Intercept)   34434.5     7339.9   4.691 4.04e-06 ***
    inst_km       28275.8     3322.5   8.510 6.95e-16 ***
    I(inst_km^2)  -2449.7      317.8  -7.708 1.64e-13 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 38860 on 318 degrees of freedom
    Multiple R-squared:  0.1969,    Adjusted R-squared:  0.1918 
    F-statistic: 38.98 on 2 and 318 DF,  p-value: 7.228e-16

A interpretação dos coeficientes é a seguinte:

- $\beta_0$ (constante): O preço médio de uma casa localizada exatamente
  na autoestrada (distância = 0 km) é de aproximadamente 34434.5
  dólares.

- $\beta_1$ (inst_km): Cada quilómetro adicional de distância à
  autoestrada está associado a uma diminuição média de aproximadamente
  28278.80 dólares no preço da casa, *ceteris paribus*.

- $\beta_2$ (I(inst_km^2)): O coeficiente do termo quadrático indica que
  o efeito da distância no preço da casa não é constante.
  Especificamente, o efeito da distância no preço da casa diminui à
  medida que a distância aumenta. Isto sugere que o impacto inicial de
  uma casa estar afastada da autoestrada é maior do que o impacto
  adicional de uma casa ainda mais afastada. O coeficiente do termo
  quadrático é -2449.70, o que indica que para cada quilómetro de
  aumento na distância, o efeito da distância no preço da casa diminui
  em aproximadamente 2449.70 dólares, *ceteris paribus*.

Se o coeficiente do termo quadrático fosse positivo, indicava que o
efeito da distância no preço da casa aumentaria à medida que a distância
aumenta, o que poderia sugerir que casas mais afastadas da autoestrada
são mais valorizadas.

Também é possível calcular o efeito marginal da distância no preço da
casa. O efeito marginal da distância é dado por:

<span id="eq-efeito-marginal-distancia">$$
\frac{\partial y}{\partial x} = \beta_1 + 2 \beta_2 x
 \qquad(16)$$</span>

Portanto, o efeito marginal da distância varia com o nível de distância.
Podemos calcular o efeito marginal da distância para diferentes níveis
de distância (0, 1, 2, 5, 10 unidades):

``` r
# Níveis de distância para calcular o efeito marginal
niv_distancia <- c(0, 1, 2, 5, 10)

# Coeficientes do modelo
coeficientes <- coef(modelo_nao_linear_km)

# Calcular efeito marginal para diferentes níveis de distância
efeito_marginal <- coeficientes["inst_km"] + 2 * coeficientes["I(inst_km^2)"] * niv_distancia
efeito_marginal
```

    [1]  28275.79  23376.30  18476.81   3778.34 -20719.11

``` r
# Criar data frame com os resultados obtidos
data.frame(Nivel_Distancia = niv_distancia,
            Efeito_Marginal_Distancia = efeito_marginal)
```

      Nivel_Distancia Efeito_Marginal_Distancia
    1               0                  28275.79
    2               1                  23376.30
    3               2                  18476.81
    4               5                   3778.34
    5              10                 -20719.11

Com isto podemos ver que o efeito marginal da distância no preço da casa
diminui à medida que a distância aumenta (relação de U invertido). Por
exemplo, para uma casa localizada exatamente na autoestrada
(`inst_km = 0`), o efeito marginal da distância é de aproximadamente
28275.79 dólares. Isto significa que se a casa estiver afastada 1
unidade da autoestrada, o preço da casa aumentará em aproximadamente
23376.30 doláres, *ceteris paribus*. No entanto, para uma casa
localizada a 10 unidades da autoestrada (`inst_km = 10`), o efeito
marginal da distância é de aproximadamente -20719.11 dólares. Portanto
existe um ponto em que o efeito marginal da distância se torna negativo
(*turning point*), ou seja, a partir desse ponto, aumentar a distância à
autoestrada está associado a uma diminuição no preço da casa. Podemos
calcular o ponto de inversão (máximo da função) do efeito marginal da
distância resolvendo a equação:

<span id="eq-ponto-inversao">$$
\frac{\partial price}{\partial inst} = 0  \implies \beta_1 + 2 \beta_2 inst_i = 0 \implies inst_i = -\frac{\beta_1}{2 \beta_2}
 \qquad(17)$$</span>

no `R`:

``` r
# Calcular ponto de inversão
ponto_inversao <- -coef(modelo_nao_linear_km)["inst_km"] / (2 * coef(modelo_nao_linear_km)["I(inst_km^2)"])
ponto_inversao
```

    inst_km 
    5.77117 

A partir da distância de aproximadamente 5.77 km, o efeito marginal da
distância no preço da casa torna-se negativo, o que indica que aumentar
a distância à autoestrada está associado a uma diminuição no preço da
casa. Isto faz todo o sentido, pois se a casa está demasiado perto da
autoestrada, o ruído, a poluição e o trãnsito podem diminuir o valor da
casa. No entanto, se a casa está demasiado longe da autoestrada, a
acessibilidade pode ser um fator negativo para o valor da casa.

Podemos calcular o valor máximo do preço da casa substituindo o ponto de
inversão na equação do modelo:

``` r
# Calcular valor máximo do preço da casa
valor_maximo_preco <- predict(modelo_nao_linear_km, newdata = data.frame(inst_km = ponto_inversao))
valor_maximo_preco
```

     inst_km 
    116026.7 

A mesma abordagem pode ser seguida para modelos com mais variáveis, por
exemplo, incluindo a variável `area` (área da casa) e a variável `rooms`
(número de quartos):

<span id="eq-reg-nao-linear-hprice-2">$$
\log(price_i) = \beta_0 + \beta_1 inst_i + \beta_2 inst_i^2 + \beta_3 area_i + \beta_4 rooms_i + u_i
 \qquad(18)$$</span>

no `R`:

``` r
# Estimar modelo de regressão não linear com mais variáveis
modelo_nao_linear_2 <- lm(log(price) ~ inst_km + I(inst_km^2)
                            + area + rooms, data = hprice3)
summary(modelo_nao_linear_2)
```


    Call:
    lm(formula = log(price) ~ inst_km + I(inst_km^2) + area + rooms, 
        data = hprice3)

    Residuals:
         Min       1Q   Median       3Q      Max 
    -1.27199 -0.17547 -0.01315  0.22890  0.81846 

    Coefficients:
                   Estimate Std. Error t value Pr(>|t|)    
    (Intercept)   9.947e+00  1.299e-01  76.583  < 2e-16 ***
    inst_km       1.668e-01  2.890e-02   5.773 1.86e-08 ***
    I(inst_km^2) -1.366e-02  2.706e-03  -5.046 7.61e-07 ***
    area          2.989e-04  3.018e-05   9.906  < 2e-16 ***
    rooms         6.256e-02  2.365e-02   2.645  0.00858 ** 
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.3093 on 316 degrees of freedom
    Multiple R-squared:  0.508, Adjusted R-squared:  0.5017 
    F-statistic: 81.56 on 4 and 316 DF,  p-value: < 2.2e-16

``` r
# Calcular ponto de inversão
ponto_inversao_2 <- -coef(modelo_nao_linear_2)["inst_km"] / (2 * coef(modelo_nao_linear_2)["I(inst_km^2)"])

ponto_inversao_2
```

     inst_km 
    6.107991 

A interpretação dos coeficientes é a seguinte:

- $\beta_0$ (constante): O preço médio de uma casa localizada exatamente
  na autoestrada (distância = 0 km), com área igual a 0 e 0 quartos é de
  aproximadamente 9947 dólares. Neste caso a constante não tem uma
  interpretação prática, pois é praticamente uma casa estar na
  autoestrada, com área igual a 0 e 0 quartos.

- $\beta_1$ (inst_km): Cada quilómetro adicional de distância à
  autoestrada está associado a um aumento médio de aproximadamente 0.17
  dólares no preço da casa, *ceteris paribus*.

- $\beta_2$ (I(inst_km^2)): O coeficiente do termo quadrático indica que
  o efeito da distância no preço da casa não é constante.
  Especificamente, o efeito da distância no preço da casa diminui à
  medida que a distância aumenta. Isto sugere que o impacto inicial de
  uma casa estar afastada da autoestrada é maior do que o impacto
  adicional de uma casa ainda mais afastada. O coeficiente do termo
  quadrático é -0.01, o que indica que para cada quilómetro de aumento
  na distância, o efeito da distância no preço da casa diminui em
  aproximadamente 0.01 dólares, *ceteris paribus*.

- $\beta_3$ (area): Cada unidade adicional de área da casa está
  associada a um aumento médio de aproximadamente 0.002 dólares no preço
  da casa, *ceteris paribus*.

- $\beta_4$ (rooms): Cada quarto adicional na casa está associado a um
  aumento médio de aproximadamente 0.06 dólares no preço da casa,
  *ceteris paribus*.

O *ceteris paribus* é interpretado como “para uma casa com as mesmas
características, com exceção da variável em questão”.

O ponto de inversão (máximo da função) do efeito marginal da distância é
de aproximadamente 6.11 km. É a partir desta distância que o efeito
marginal da distância no preço da casa se torna negativo.

Podemos ter também o termo ao cubo. Ao aumentar o grau do polinómio, o
modelo torna-se mais flexível, podendo capturar relações mais complexas
entre as variáveis. No entanto, também pode levar a problemas de
*overfitting*, onde o modelo se ajusta demasiado aos dados e não pode
ser generalizado para novos dados. Por isso, temos de mantar sempre um
equilíbrio entre a complexidade e a interpretação do modelo.

## Modelo com o desvio padrão robusto

Para calcular o desvio padrão robusto para a heterocedasticidade no `R`,
podemos utilizar a função `vcovHC()` do pacote `sandwich`. A função
`vcovHAC()` do mesmo pacote permite calcular o desvio padrão robusto a
autocorrelação e heterocedasticidade. A função `coeftest()` do pacote
`lmtest` pode ser utilizada para apresentar os resultados da regressão
com o desvio padrão robusto.

Os argumentos da função `vcovHC()` permitem especificar o tipo de matriz
de covariância robusta a ser utilizada. O argumento `type` pode assumir
os seguintes valores:

- `"HC0"`: Matriz de covariância robusta de White (1980).
- `"HC1"`: Matriz de covariância robusta de MacKinnon e White (1985),
  que ajusta a matriz de covariância de White para amostras pequenas.
- `"HC2"`: Matriz de covariância robusta de Long e Ervin (1983), que
  ajusta a matriz de covariância de White para amostras pequenas,
  considerando o número de regressoras.
- `"HC3"`: Matriz de covariância robusta de Davidson e MacKinnon (1993),
  que ajusta a matriz de covariância de White para amostras pequenas,
  considerando o número de regressoras e o número de observações.
- `"HC4"`: Matriz de covariância robusta de Cribari-Neto (2004), que
  ajusta a matriz de covariância de White para amostras pequenas,
  considerando o número de regressoras e o número de observações, com um
  ajuste adicional para observações influentes.
- `"HC4m"`: Matriz de covariância robusta de Cribari-Neto (2004), que é
  uma versão modificada da HC4, com um ajuste adicional para observações
  influentes.
- `"HC5"`: Matriz de covariância robusta de Pustejovsky e Tipton (2018),
  que é uma versão modificada da HC4, com um ajuste adicional para
  observações influentes, considerando o número de regressoras e o
  número de observações.

A função `vcovHAC()` permite especificar o número de desfasamentos a
considerar na matriz de covariância robusta para autocorrelação e
heterocedasticidade, através do argumento `lag`.

## Modelo dos Mínimos Quadrados Ponderados (WLS)

## Modelo Generalizado de Mínimos Quadrados (GLS)

função `gls()` do pacote `nlme` permite estimar modelos de regressão
linear com erros que podem ter diferentes estruturas de correlação e
variância. A função `gls()` é particularmente útil quando os
pressupostos clássicos da regressão linear (como homocedasticidade e
independência dos erros) não são verificados.

<div id="refs" class="references csl-bib-body hanging-indent"
entry-spacing="0" line-spacing="2">

<div id="ref-halvorsen_interpretation_1980" class="csl-entry">

Halvorsen, R., & Palmquist, R. (1980). The Interpretation of Dummy
Variables in Semilogarithmic Equations. *The American Economic Review*,
*70*(3), 474–475. <http://www.jstor.org/stable/1805237>

</div>

<div id="ref-kennedy_estimation_1981" class="csl-entry">

Kennedy, P. E. (1981). Estimation with Correctly Interpreted Dummy
Variables in Semilogarithmic Equations. *The American Economic Review*,
*71*(4), 801–801. <http://www.jstor.org/stable/1806207>

</div>

</div>
