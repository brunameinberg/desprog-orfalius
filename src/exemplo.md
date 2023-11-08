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

Como dito anteriormente o algoritmo recebe um conjunto de pontos brancos, que representam a imagem dada, só que em formato binário. Concorda que podemos representá-los em um plano cartesiano $xy$, como o abaixo:

![](logo.png)


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
