

# Breve Introdução ao R

## O que é o R?

O R é uma linguagem de programação e ambiente de software livre
especialmente desenvolvido para análise estatística e gráficos. É
amplamente utilizado por estatísticos, analistas de dados e
investigadores em diversas áreas, incluindo a econometria.

## Instalação do R e interface gráfica

É possível utilizar o R através de uma consola, mas é mais comum usar
uma interface gráfica como o RStudio, que facilita a escrita de código,
visualização de dados e criação de gráficos. Recentemente foi lançada
uma nova interface gráfica chamada **Positron**. É um *fork* do Visual
Studio Code, que oferece uma experiência moderna para trabalhar com R.
Pode ser mais familiar para quem já utilizou outras linguagens de
programação. Tem também a vantagem de ter um sistema de extensões que
permite adicionar funcionalidades.

### Instalação do R

1.  Aceder ao site oficial: <https://cloud.r-project.org/>
2.  Escolher o sistema operativo (Windows, macOS, Linux)
3.  Escolher a versão *base*
4.  Clicar em “Download R”
5.  Abrir o ficheiro descarregado e seguir as instruções de instalação
    (importante: saber qual o caminho de instalação, pode ser necessário
    dependendo da interface gráfica escolhida)

### Instalação do RStudio Desktop

O RStudio é um ambiente de desenvolvimento integrado (IDE) que facilita
o trabalho com R:

1.  Aceder a: <https://posit.co/download/rstudio-desktop/>
2.  Descarregue o RStudio Desktop gratuito atraves da opção “Download
    RStudio Desktop”
3.  Instalar seguindo as instruções

Uma visão geral do funcionamento e dos painéis do R-Studio pode ser
encontrada
[aqui](https://rstudio.github.io/cheatsheets/html/rstudio-ide.html?_gl=1*1jmp8e0*_ga*MzQ1Mjk4ODI4LjE3MTc1MTY2OTI.*_ga_2C0WZ1JHG0*MTcxNzUxNjY5Mi4xLjAuMTcxNzUxNjY5Mi4wLjAuMA..)
ou [aqui](https://rstudio.github.io/cheatsheets/rstudio-ide.pdf).

### Instalação do Positron

1.  Aceder a: <https://positron.posit.co/download>
2.  Aceitar os termos de utilização
3.  Descarregue o Positron gratuito atraves da opção “Windows 10, 11 x64
    (system level install)” ou da “Windows 10, 11 x64 (user level
    install)”
4.  Instalar seguindo as instruções

## Primeiros Passos no R

### Criar e executar código

O R base tem algumas funções incluidos que já permitem executar algumas
tarefas. Existem duas forma de introduzir código no R-studio. Uma é
diretamente na consola (painel inferior esquerdo no R-Studio) ou através
de um script (novo script: File -\> New File -\> R Script; ou
Ctrl+Shift+N).

Para executar um comando na consola/terminal apenas é necessário
pressionar `enter` e no script é necessário ter o cursor “`|`” na linha
a executar (em qualquer posição) e pressionar `ctrl` + `enter` . Para
executar mais do que uma linha é necessário selecionar as linhas a
executar e pressionar `ctrl` + `enter` .

### Algumas operações básicas em `R`

O `R` funciona como uma calculadora, um exemplo básico:

``` r
#ctrl + "enter" para executar no script ou enter na consola
1 + 1
```

    [1] 2

Mais exemplos:

``` r
2 * 2
```

    [1] 4

``` r
3+(1-2)
```

    [1] 2

### Atribuir valores a objetos `R`

É possível atribuir valores a letras (por exemplo), para isso utiliza-se
o símbolo `<-` da seguinte forma:

``` r
a <- 2
b <- 3
```

É possível inverter o símbolo `<-` para `->` e a ordem daatribuição para

``` r
4 -> c
```

**O `R` é Case sensitive** (**sensível a MAIÚSCULAS** e **minúsculas**)
O comando anterior vai gerar dois valores para os objetos `a` e `b` que
vão ficar visíveis no ambiente (painel superior direito do R-Studio). Se
já existir um objeto com o mesmo nome, o R apenas vai substituir o
anterior sem qualquer aviso. O comando `remove` ou `rm` elimina um
objeto do ambiente.

### Eliminar objetos do ambiente `R`

``` r
rm(a)
```

ou elimnar vários objetos, por exemplo `b` e `c`:

``` r
rm(b,c)
```

Para eliminar **todos** os objetos do ambiente R é necessário utilizar
uma lista dentro da função `rm`

``` r
rm(list=ls())
```

Sempre que que se inicia um novo projeto é recomendável a execução do
código anterior para não gerar conflitos na execução do novo código.

## Comentários - `#`

Os comentários são úteis e praticamente obrigatórios, pois permitem
documentar o código. É muito comum em `R` e noutras linguagens, fornecer
um comentário mais geral antes de qualquer linha ou bloco de código.
Para comentários mais específicos é comum escrever à frente da linha de
código. Em R para introduzir um comentário utiliza-se o `#`.

``` r
#atribuir valores a objetos
a <- 5
b <- 7

c <- 6 #atribuir o valor 6 ao objeto b
```

Quando se executa um `#comentário` nada acontece, nenhum erro é
apresentado, nem é efetuada qualquer execução ou alteração em
*background*.

## “*Packages*”

### Instalar `packages`

O `R` incluí algumas funções que permitem efetuar análises mais básicas,
contudo existe uma grande comunidade que desenvolve bibliotecas
adicionais que ajudam maximizar e a facilitar a utilização do `R`. Para
instalar um `package` utilizamos a função
`install.packages("nome do package")` como por exemplo:

``` r
install.packages("Gally")
```

A mesma função pode ser utilizada para atualzar os bibliotecas já
existentes. É possível instalar ou atualizar vários `packages` em
simultâneo com na mesma função utilizando o `c` (combinar vetores ou
listas):

``` r
install.packages(c("tidyverse","woolddridge","AER"))
```

### Carregar `Packages`

Para **carregar** uma biblioteca é necessário utilizar a função
`library` e o nome do `package` entre aspas. Por exemplo:

``` r
library(tidyverse) #manipular dados + gráficos
library(AER) #Applied Econometrics with R
library(MASS) 
library(scales) #gráficos
library(stargazer) #tabelas
library(lmtest) #test lm
library(sandwich) #robust standard errors
library(nlme) #modelos não lineares
library(skedastic) #heterocedasticidade
library(tseries) #séries temporais
library(wooldridge) #livro wooldridge
library(readxl) #importar ficheiro excel xlsx
library(performance) #avaliar modelos
library(psych) #avaliar modelos
```

É normal aparecerem avisos (`warnings`) após a carregar uma `library`,
nestes casos pode ser só um aviso de conflito de funções ou da
compatbilidade da versão do `R`. Nenhum destes avisos é impeditivo de
continuar (pelo menos nos packages utilizados em aula). Se a seguinte
mensagem de erro aparecer:

`Error in library(wooldridge) : there is no package called 'wooldridge'`

significa que o `package` wooldridge não está instalado e para isso é
necessário instalar de acordo com `install.packages("nome_do_package")`.

## Help

O `?` (help) pode e deve ser utilziado para mostrar a sintaxe das
funções e pode ser utilziado da seguinte forma:

``` r
?lm
```

Uma nova janela irá surgir no painel inferior diireito n separador
`Help` Por vezes existem funções com o mesmo nome em bibliotecas
diferentes, por exemplo a função `filter` existe na biblioteca do `R`
base e na blioteca `dplyr`, utilizar ou aceder ao ficheiro help dasta
função é necessária a utilização do nome da biblioteca +`::`+função

``` r
?dplyr::filter
```

## Pasta Diretório - “working directory”

A pasta de diretório é utilizada por defeito em qualquer função que
importe ou exporte objetos do `R`. Para ver a pasta diretório:

``` r
#get working directory
getwd()
```

Por defeito a pasta dos Documentos é utilizada como pasta diretório em
casa sessão do `R`. Uma boa prática é criar uma nova pasta para cada
projeto. Por exemplo criar uma pasta “Projeto1” dentro da pasta
documentos e depois atribuir essa pasta como diretório:

``` r
#set working directory
setwd("C:/Users/User/Documents/Projeto1")
```

O caminho da pasta é indicado por `/` ou por `\\` e nunca por `\`.
Também é possível alterar a pasta diretório através dos `menus` no
`R-Studio`, em `Session -> Set Working Directory -> Choose Directory` e
selecionar a pasta. Também é possível através do atalho `Ctrl+Shift+H`.

Se o ficheiro a importar para o R estiver dentro da pasta diretório, nas
funções (por exemplo `read.csv` ou `read.xlsx`) não é necessário
introduzir o caminho completo do ficheiro.

Na pasta diretório também serão armazenados todos os objetos do `R`,
desde que exportados, nomeadamente a exportação de `data frames` e de
gráficos. De forma a criar uma melhor organização dos conteúdos, é
recomendado utilizar um diretório de trabalho para cada projeto.

## Importar dados

Existem várias formas de importar dados para o `R`, desde a introdução
manual de dados, a importação de ficheiros `.xlsx`, `.txt`, `.csv` ou a
importação de dados de `packages` já existentes no `R`. Para importar
dados de um ficheiro `Excel` é necessário instalar o `package` `readxl`
e para importar dados de um ficheiro `txt` ou `csv` o `R` já tem funções
base que permitem importar esses ficheiros. Nas seguintes secções são
apresentados exemplos de importação de dados de diferentes fontes.

### Limpar ambiente

O primeiro passo para qualquer projeto do zero:

``` r
rm(list=ls())
```

É importante garantir que o ambiente está limpo antes de começar a
trabalhar em um novo projeto, para evitar conflitos com objetos
existentes.

### Introduzir dados

Introduzir dados através do script:

``` r
a <- c(1,2,3)
b <- c(4,5,6)
df <- data.frame(a,b)
```

Esta forma pode ser utilizada para introduzir dados pequenos, mas para
dados maiores é recomendável a importação de ficheiros.

### Importar dados a partir de um package

Para importar dados a partir de um `package`é necessário carregar o
`package` com a função `library` e depois utilizar a função `data` para
importar o dataset. Por exemplo, para importar o dataset `hprice1` do
`package` `wooldridge`:

``` r
library(wooldridge) #carregar biblioteca
data(hprice1) #escolher o dataset "hprice"
invisible(force(hprice1)) #importar o dataset
head(hprice1) #ver uma parte do dataset
```

        price assess bdrms lotsize sqrft colonial   lprice  lassess llotsize
    1 300.000  349.1     4    6126  2438        1 5.703783 5.855359 8.720297
    2 370.000  351.5     3    9903  2076        1 5.913503 5.862210 9.200593
    3 191.000  217.7     3    5200  1374        0 5.252274 5.383118 8.556414
    4 195.000  231.8     3    4600  1448        1 5.273000 5.445875 8.433811
    5 373.000  319.1     4    6095  2514        1 5.921578 5.765504 8.715224
    6 466.275  414.5     5    8566  2754        1 6.144775 6.027073 9.055556
        lsqrft
    1 7.798934
    2 7.638198
    3 7.225482
    4 7.277938
    5 7.829630
    6 7.920810

O dataset `hprice1` é um `data.frame` e é possível visualizar as
primeiras 6 linhas com a função `head` ou as últimas 6 linhas com a
função `tail`. Para ver o dataset completo é necessário utilizar a
função `view`.

Existem `packages` de várias bases de dados muito utilizadas em
econometria, como por exemplo o World Development Indicators (WDI) do
Banco Mundial. Para importar o dataset `wdi_data` do `package`
`wooldridge`:

``` r
library(WDI)
```

Por exemplo importar o GDP per capita (constant 2015 US\$) e a população
toal para Portugal, Espanha e França entre 1990 e 2023:

``` r
wdi_data <- WDI(country=c("PT","ES","FR"),
              indicator=c("NY.GDP.PCAP.KD","SP.POP.TOTL"),
              start=1990,end=2023)
head(wdi_data)
```

      country iso2c iso3c year NY.GDP.PCAP.KD SP.POP.TOTL
    1  France    FR   FRA 1990       27992.98    58261012
    2  France    FR   FRA 1991       28197.24    58554242
    3  France    FR   FRA 1992       28482.52    58846584
    4  France    FR   FRA 1993       28257.10    59103094
    5  France    FR   FRA 1994       28822.50    59324863
    6  France    FR   FRA 1995       29379.62    59541294

### Importar dados a partir de um ficheiro excel (.xlsx ou .xls)

Para importar qualuqer ficheiro externo (localizado na pasta diretório)
é necesário colocar **exatamente** o nome e a extensão do ficheiro. Pra
ver a extensão do ficheiro `alt` + `enter`

Importar um ficheiro `.xlsx`

``` r
library(readxl)
excel <- read_xlsx("EXCEL.xlsx")
```

Se o ficheiro tiver várias folhas, é necessário especificar qual folha
importar, exceto se for a primeira. O ficheiro `EXCEL.xlsx` tem duas
folhas, a `Data` e `Data2`. Para importar a folha `Data`não é necessária
qualquer informação para além do nome do ficheiro. Para importar a folha
`Data2` temos de utilizar o argumento `sheet` da função `read_xlsx`:

``` r
excel <- read_xlsx("EXCEL.xlsx", sheet = "Data2")
```

Importar um ficheiro `.xls`

``` r
#importar .xls
excel2 <- read_xls("EXCEL.xls")
```

Importar um ficheiro `.txt` com a função `read.csv` (o `R` tem funções
base para importar ficheiros `.txt` e `.csv`)

``` r
txt <- read.csv("TXT.txt",sep=",")
```

Importar um fcheiro `.csv`

``` r
csv <- read.csv("CSV.csv",sep=",")
```

Todos os ficheiros importados aparecem no ambiente do R-Studio como
`data.frame`.

### Importar dados a partir da web

Para importar dados a partir da web é necessário utilizar a função
`read.csv` ou `read.table` e o link do ficheiro. Por exemplo, para
importar o ficheiro `csv` do link
`https://github.com/tiagolafonso/Files_Intro_Applied_Econometrics/blob/main/CSV.csv`:

``` r
csv_web <- read.csv("https://raw.githubusercontent.com/tiagolafonso/Files_Intro_Applied_Econometrics/main/CSV.csv")
```

Importar um ficheiro `.xlsx` a partir da web. carregacr os packages
necessários:

``` r
library(readxl)
library(httr)
```

Download do ficheiro `.xlsx` a partir da web:

    Response [https://github.com/tiagolafonso/Files_Intro_Applied_Econometrics/blob/main/wdi_data.xlsx]
      Date: 2025-09-29 20:26
      Status: 200
      Content-Type: text/html; charset=utf-8
      Size: 188 kB
    <ON DISK>  C:\Users\User-Pc\AppData\Local\Temp\Rtmpqe5u93\file94bc1223189a.xlsx

Importar o ficheiro `.xlsx` para o ambiente do R:

``` r
wdi <- read_xlsx("EXCEL.xlsx")
head(wdi)
```

## Visão geral de dados

A visão geral dos dados é importante para perceber a estrutura dos
dados, o número de linhas e colunas, o nome das colunas e o tipo de
dados. Para isso é necessário utilizar algumas funções base do `R` e do
`package` `dplyr`.

Para mostrar primeiras 6 linhas do `data.frame` `csv` importado no
código anterior.

``` r
head(csv)
```

                 Y          X1        X2
    1 161589706000 34768754366 4.8983527
    2 167902333000 36393681329 0.5810776
    3 174309785000 38389407923 6.1486192
    4 177697796000 39318850931 5.0276144
    5 179067711000 41343708038 0.4361373
    6 177401448000 43508667153 6.2669726

Para mostrar últimas 6 linhas `data.frame`

``` r
tail(csv)
```

                  Y          X1       X2
    20 189771059000 83319584579 4.826503
    21 195178255000 86312561472 3.237995
    22 200414419000 90555041439 4.300252
    23 183778988000 74706623651 1.742925
    24 193891900000 83753092686 3.091140
    25 206855030000 94621684304 3.611657

Ver o `data.frame` completo:

``` r
View(csv)
```

Mostrar número de linhas de um objeto

``` r
nrow(csv)
```

    [1] 25

Mostar número de colunas de um objeto

``` r
ncol(csv)
```

    [1] 3

Mostrar o número de linhas e colunas de um `data.frame`

``` r
dim(csv)
```

    [1] 25  3

Mostar o nome das colunas

``` r
names(csv)
```

    [1] "Y"  "X1" "X2"

## Objetos no R

Quando introduzimos ou importamos dados para o R, eles são armazenados
em objetos. Os objetos no `R` são estruturas que armazenam dados. Os
tipos de objetos mais comuns incluem vetores, matrizes, listas e data
frames. Cada tipo de objeto tem características específicas e é
utilizado para diferentes propósitoss. Os objetos mais comuns sao:
vetores, matrizes, listas, fatores e data frames.

### Vectores

Um vetor é uma sequência de valores do mesmo tipo, por exemplo, um
vector numérico, um vector de caracteres ou um vector lógico. Os vetores
são a estrutura de dados mais simples no R e podem ser criados
utilizando a função `c()`. Exemplos de vetores:

Existem vários tipos de vetores:

- Vetores numéricos

``` r
# Vector numérico
numeros <- c(1, 2, 3, 4, 5)
print(numeros)
```

    [1] 1 2 3 4 5

- Vetores de caracteres:

``` r
# Vector de caracteres
nomes <- c("João", "Maria", "Pedro")
print(nomes)
```

    [1] "João"  "Maria" "Pedro"

- Vetores lógicos

``` r
# Vector lógico
logicos <- c(TRUE, FALSE, TRUE)
print(logicos)
```

    [1]  TRUE FALSE  TRUE

Para ver a estrutura de um objeto no R, podemos utilizar a função
`class()`.

``` r
# Ver a estrutura dos vetores
class(nomes)
```

    [1] "character"

``` r
class(logicos)
```

    [1] "logical"

``` r
class(numeros)
```

    [1] "numeric"

Ou a função `str()` para visualizar a estrutura interna e o tipo de um
objeto.

``` r
# Ver a estrutura dos vetores
str(nomes)
```

     chr [1:3] "João" "Maria" "Pedro"

``` r
str(logicos)
```

     logi [1:3] TRUE FALSE TRUE

``` r
str(numeros)
```

     num [1:5] 1 2 3 4 5

### Listas

As listas são estruturas de dados que podem conter elementos de
diferentes tipos, como vetores, matrizes, data frames e até outras
listas. As listas são criadas utilizando a função `list()`. Exemplo de
criação de uma lista:

``` r
# Criar uma lista
lista <- list(
  numeros = c(1, 2, 3),
  letras = c("A", "B", "C"),
  logicos = c(TRUE, FALSE, TRUE)
)
print(lista)
```

    $numeros
    [1] 1 2 3

    $letras
    [1] "A" "B" "C"

    $logicos
    [1]  TRUE FALSE  TRUE

### Fatores

Um fator é uma estrutura de dados utilizada para armazenar variáveis
categóricas. Os fatores são úteis para representar variáveis
qualitativas, como género, estado civil, etc. Os fatores são criados
utilizando a função `factor()`. Exemplo de criação de um fator:

``` r
# Criar um fator
genero <- factor(c("Masculino", "Feminino", "Masculino", "Feminino"))
print(genero)
```

    [1] Masculino Feminino  Masculino Feminino 
    Levels: Feminino Masculino

Um fator pode ter mais do que um nível. Por exemplo, um fator com cinco
níveis para cores de carros:

``` r
# Criar um fator
cores <- factor(c("Vermelho", "Azul", "Verde", "Amarelo", "Preto"))
print(cores)
```

    [1] Vermelho Azul     Verde    Amarelo  Preto   
    Levels: Amarelo Azul Preto Verde Vermelho

É possível converter um outro objeto num fator através da função
`factor()` ou `as.factor()`. A diferença está na forma como os níveis
são tratados. Na função `factor()`, os níveis são definidos
explicitamente, enquanto na função `as.factor()`, os níveis são
determinados automaticamente com base nos dados.

### Matrizes

As matrizes são estruturas de dados bidimensionais que armazenam
elementos do mesmo tipo. São criadas utilizando a função `matrix()`.
Exemplo para um matriz de 3x3:

``` r
# Criar uma matriz
matriz <- matrix(1:9, nrow = 3, ncol = 3)
print(matriz)
```

         [,1] [,2] [,3]
    [1,]    1    4    7
    [2,]    2    5    8
    [3,]    3    6    9

Também é possível criar uma matriz a partir de vetores, por exemplo:

``` r
# Criar uma matriz a partir de vetores
vetor1 <- c(1, 2, 3)
vetor2 <- c(4, 5, 6)
matriz2 <- rbind(vetor1, vetor2)
print(matriz2)
```

           [,1] [,2] [,3]
    vetor1    1    2    3
    vetor2    4    5    6

### Data Frames

Os data frames são estruturas fundamentais para análise de dados. Os
data frames permitem armazenar dados em formato tabular, onde cada
coluna pode ter um tipo de dado diferente, é esta a principal
característica que os distingue de matrizes. Os data frames são criados
utilizando a função `data.frame()`. Exemplo de criação de um data frame:

``` r
# Criar um data frame
dados <- data.frame(
  nome = c("Ana", "Bruno", "Carlos"),
  idade = c(25, 30, 35),
  salario = c(1200, 1500, 1800)
)
print(dados)
```

        nome idade salario
    1    Ana    25    1200
    2  Bruno    30    1500
    3 Carlos    35    1800

#### Função `data.frame()` vs `as.data.frame()`

A função `data.frame()` é utilizada para criar um novo data frame,
enquanto a função `as.data.frame()` é utilizada para converter outros
tipos de objetos (como matrizes ou listas) em data frames. Por exemplo:

``` r
# Converter uma matriz em data frame
matriz <- matrix(1:9, nrow = 3)
df_matriz <- as.data.frame(matriz)
df2_matriz <- data.frame(matriz)
print(df_matriz)
```

      V1 V2 V3
    1  1  4  7
    2  2  5  8
    3  3  6  9

``` r
print(df2_matriz)
```

      X1 X2 X3
    1  1  4  7
    2  2  5  8
    3  3  6  9

De notar que a função `as.data.frame()` mantém os nomes das colunas
originais, enquanto que a função `data.frame()` atribui nomes padrão
(V1, V2, …).

Existem outros formatos que para já não vão ser abordados como por
exemplo: tibbles, pdata.frames, entre outros.

## Manipulação de `data.frames` com R base

Para este exemplos vamos utilizar o ficheiro `CSV.csv` que contém as
variáveis `Y`, `X1` e `X2`.

``` r
# Carregar os dados
csv <- read.csv("CSV.csv")
colnames(csv)
```

    [1] "Y"  "X1" "X2"

Para adicionar colunas calculadas a um `data.frame`é necessário
identificar o data frame onde a variável está e a variávela que será
utilizada no cálculo exemplo: “nome_dataframe”+`$`+“nome_coluna”.
Exemplo para criar as variáveis `Z` e `W` que vão estar incluídas no
data frame `csv`.

$$
Z_i = X_{1i}+X_{2i}; W_i=Y_{i}+X_{1i}
$$

``` r
csv$Z <- csv$X1+csv$X2
csv$W <- csv$Y*csv$X1
```

Eliminar colunas dentro de um `data.frame` é possível utilizando o
símbolo `$` para identificar a coluna e atribuindo `NULL` a essa coluna.
Por exemplo, para eliminar as colunas `W` e `Z` do data frame `csv`:

``` r
csv$W <- NULL
csv$Z <- NULL
```

Não é possível eliminar colunas dentro de um data frame utilizando
apenas a função `remove()` ou `rm()`.

No próximo capítulo serão abordados os tipos de dados económicos,
transformação de dados e manipulação de dados com conjutos maiores.
