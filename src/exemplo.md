Transformada de Hough
======

O objetivo desse handout é ajudar vocês a entender o funcionamento e utilidade do algoritmo da Transformada de Hough. Assim possibilitando um uso mais consciente da implementação deste algoritmo.
____________

Para que usamos a Transformada de Hough?
---------

Pense que temos um jogo de sudoku do jornal em mãos e estamos com preguiça de resolve-lo. Então, como bons desenvolvedores pensamos em uma ideia de um programa que resolva o jogo para nós, a partir de uma foto que tiramos da câmera do celular, como essa por exemplo:

![](sudoku_limpo.jpg)

O primeiro passo é o processamento da imagem, sabendo o limite de cada célula, para que possamos criar uma lógica em cima. Para isso, temos que detectar as linhas da imagem e posteriormente identificar seus cruzamentos. Essa etapa da identificação de linhas é feita pela Transformada de Hough! Esse algoritmo também é capaz de identificar circunfêrencias e elipses, mas iremos focar na identificação de retas. Um arquivo .png, por exemplo, não é uma entrada válida para o algoritmo, por isso ela passa por uma máscara (para saber mais acesse ), que a transforma em uma imagem binária, ou seja um conjunto de pontos brancos. No caso da foto do sudoku, a imagem que representa esse conjunto seria essa aqui:

![](sudoku_preto_e_branco.png)

Com essa entrada fornecida, entramos no funcionamento algoritmo, que será explicado ao longo do handout, e esperamos uma saída como essa:

![](sudoku_saida.png)

A matemática por trás da Transformada de Hough
---------

Como dito anteriormente o algoritmo recebe um conjunto de pontos brancos, que representam a imagem dada, só que em formato binário. Concorda que em uma imagem aleatória podemos representá-los em um plano cartesiano $xy$, como o abaixo:

![](pontos1.jpg)


??? CHECKPOINT
Quantas retas podem passar por cada um dos pontos?
::: Gabarito
Se sua resposta foi $\infty$ retas, você acertou!

![](infinitasretas.png)
:::
???

Essas infinitas retas tem algo em comum? Como descobrimos isso?

Primeiramente concorda que podemos reprsentar as infintas retas coma equação $b = m*a + n$ cumpre?. Sendo que cada $m$ e cada $n$ representa uma das retas.
Generalizando, para qualquer ponto $(x,y)$, a equação que define uma reta que passa por esse ponto seria $y = m*x + n$.

Resumindo, na equação $y = m*x + n$, os elementos que definem a reta são:
* O coeficiente angular $m$;
* O coeficiente linear $n$;

Então, para o nosso objetivo de descobrir semelhanças entre as retas, podemos usar esse parâmetros para colocá-las em um plano. Como seria isso?

Mas nós não estamos em interessados em descobrir quantas retas passam por esse ponto, e sim **quantos pontos passam por uma determinada reta**. Isso porque quanto mais pontos, daqueles que recebemos como entrada do algoritmo, passarem por uma determinada reta, mais provável dela de fato existir na imagem. Talvez tenha ficado muito confuso, vamos tentar representar a situação visualmente para que fique mais claro!

*imagem aqui*

??? CHECKPOINT
Para tentarmos identificar quantos pontos passam por uma reta precisamos visualiza-la em outro plano. Sabendo que possuímos um ponto (a, b)presente no plano $xy$ e que uma de suas infinitas retas é representada pela equação $y = mx + n$, qual seria a representação desse ponto em um plano secundário com eixos $m$ e $n$?
*colocar fotinhoooooo*

**Dica:** O que todos os pontos de uma reta tem em comum?
::: Gabarito
Resposta *colocar fotinho do segundo plano *

:::
???

Sim, eu sei... Como é possível um ponto virar uma reta? Pode ser um pouco difícil visualizar esse conceito, mas talvez tentando com dois pontos pode ser que fique mais claro. 

??? CHECKPOINT
Sabendo que possuímos dois pontos (a, b) e (c, d) presentes no plano $xy$ e que uma das infinitas retas do primeiro ponto seja representada pela equação $y = mx + n$ e uma das retas infinitas do segundo ponto seja representada por $y = tx + s$, qual seria a representação desses pontos em um plano secundário com eixos $m$ e $n$?
*colocar fotinhoooooo*

::: Gabarito
Resposta *colocar fotinho do segundo plano *

:::
???

Essas retas presentes nesse plano secundário possuem uma intersecção, este ponto 

Para criar um parágrafo, basta escrever um texto contínuo, sem pular linhas.

Você também pode criar

1. listas;

2. ordenadas,

assim como

* listas;

* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta *img*.

![](logo.png)

Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| coluna a | coluna b |
|----------|----------|
| 1        | 2        |

Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md :` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

:bubble

Você também pode inserir código, inclusive especificando a linguagem.

``` py
def f():
    print('hello world')
```

``` c
void f() {
    printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```


!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!


??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???
