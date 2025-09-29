

# Modelos de Variável Dependente Limitada

## Introdução

Muitas situações económicas envolvem variáveis dependentes que não são
contínuas e ilimitadas. Este capítulo apresenta modelos econométricos
especializados para lidar com diferentes tipos de limitações na variável
dependente: variáveis binárias, variáveis categóricas, dados censurados
e dados de contagem.

## Modelos para Variáveis Binárias

### Modelo de Probabilidade Linear (LPM)

O modelo mais simples para variáveis binárias é o Modelo de
Probabilidade Linear:

<span id="eq-modelo-probabilidade-linear">$$
P(Y_i = 1|X_i) = \beta_0 + \beta_1 X_{1i} + \cdots + \beta_k X_{ki}
 \qquad(1)$$</span>

### Modelo Logit

O modelo logit usa a função logística:

<span id="eq-modelo-logit">$$
P(Y_i = 1|X_i) = \frac{e^{\beta_0 + \beta_1 X_{1i} + \cdots + \beta_k X_{ki}}}{1 + e^{\beta_0 + \beta_1 X_{1i} + \cdots + \beta_k X_{ki}}}
 \qquad(2)$$</span>

### Modelo Probit

O modelo probit usa a função de distribuição normal:

<span id="eq-modelo-probit">$$
P(Y_i = 1|X_i) = \Phi(\beta_0 + \beta_1 X_{1i} + \cdots + \beta_k X_{ki})
 \qquad(3)$$</span>

### Comparação de Modelos

## Modelos para Variáveis Categóricas Ordenadas

### Modelo Logit Ordenado

### Teste de Hipótese de Linhas Paralelas

## Modelos para Variáveis Categóricas Não-Ordenadas

### Modelo Logit Multinomial

## Modelos para Dados Censurados

### Modelo Tobit

### Efeitos Marginais no Modelo Tobit

## Modelo de Seleção de Heckman

### Modelo em Duas Etapas

## Modelos para Dados de Contagem

### Modelo de Poisson

### Modelo Binomial Negativo

### Modelo Zero-Inflated

## Avaliação de Modelos para Variáveis Limitadas

### Tabelas de Classificação

### Curva ROC

## Exercícios

1.  Compare modelos LPM, Logit e Probit para um problema de escolha
    binária.

2.  Estime um modelo logit ordenado e teste a hipótese de linhas
    paralelas.

3.  Aplique o modelo Tobit a dados com censuramento e compare com OLS.

4.  Implemente um modelo de Heckman para corrigir viés de seleção.

5.  Compare modelos Poisson, Binomial Negativo e Zero-Inflated para
    dados de contagem.

6.  Avalie a qualidade preditiva dos modelos usando várias métricas.

## Resumo

Neste capítulo aprendemos:

- Modelos para variáveis binárias (LPM, Logit, Probit)
- Modelos para variáveis categóricas ordenadas e não-ordenadas
- Modelos para dados censurados (Tobit)
- Correção de viés de seleção (Heckman)
- Modelos para dados de contagem (Poisson, Binomial Negativo,
  Zero-Inflated)
- Métodos de avaliação específicos para variáveis limitadas

Estes modelos são essenciais para lidar com a natureza específica de
muitos dados económicos e fornecem ferramentas robustas para análise
econométrica aplicada.
