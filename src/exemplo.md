Transformada de Hough
======

O objetivo deste handout é ajudá-los a entender o funcionamento e a utilidade do algoritmo da Transformada de Hough, possibilitando um uso mais consciente da implementação deste algoritmo.
____________

Para que usamos a Transformada de Hough?
---------

Pense que temos um jogo de sudoku do jornal em mãos e estamos com preguiça de resolvê-lo. Então, como bons desenvolvedores, pensamos em uma ideia de um programa que resolva o jogo para nós, a partir de uma foto que tiramos da câmera do celular, como esta, por exemplo:

![Sudoku Limpo](sudoku_limpo.jpg)

O primeiro passo é o processamento da imagem, sabendo o limite de cada célula, para que possamos criar uma lógica em cima. Para isso, temos que detectar as linhas da imagem e, posteriormente, identificar seus cruzamentos. Essa etapa da identificação de linhas é feita pela Transformada de Hough! Este algoritmo também é capaz de identificar circunferências e elipses, mas iremos focar na identificação de retas.

Um arquivo .png, por exemplo, não é uma entrada válida para o algoritmo, por isso ela passa por uma máscara, que a transforma em uma [imagem binária](https://embarcados.com.br/imagens-binarias/), ou seja, uma imagem em preto e branco. No caso da foto do sudoku, a imagem que representa esse conjunto seria esta:

![](sudoku_preto_e_branco.png)

Com essa entrada fornecida, entramos no funcionamento do algoritmo, que será explicado ao longo do handout, e esperamos uma saída como esta:

![](sudoku_saida.png)

A matemática por trás da Transformada de Hough
---------

Como dito anteriormente, o algoritmo recebe um conjunto de pontos brancos, que representam a imagem dada, só que em formato binário. Concorda que em uma imagem aleatória podemos representá-los em um plano cartesiano $xy$, como o abaixo:

![](pontos1.jpg)

Primeiramente, sabemos que a equação $y = mx + n$ é uma forma de representar uma reta. Assim, notamos que apenas dois parâmetros $(m, n)$ são suficientes para representar uma reta. Para entender melhor o algoritmo, vamos explorar um pouco melhor essa ideia, representar a reta apenas por $(m, n)$, ou seja, [transformar as coordenadas](https://www.ufrgs.br/lageo/calculos/coord_exp.html) de uma reta de $(x, y)$ para $(m, n)$.

Aqui temos um plano cartesiano com um ponto. Passando a animação, vemos várias retas que passam por esse ponto e suas respectivas equações.


:plano_xy

??? CHECKPOINT
Como ficaria a representação da {blue}(primeira reta) (verde claro) se, ao invés do plano $xy$, a representássemos apenas no plano $mn$?

**Dica:** Lembre-se $m$ e $n$ são os parâmetros angular e linear da reta.
::: Gabarito
Como a equação da primeira reta é $y=x$, concluímos que $m=1$ e $n=0$:

![](plano_mn1.png)
:::
???


??? CHECKPOINT
Agora, faça o mesmo para as outras retas. Qual ponto representaria cada uma delas no plano $mn$?

::: Gabarito
| Reta ($xy$) | Reta ($mn$) |
|----|-----|
| $y=x$ | $(1,0)$ |
| $y=0.9x+1$ | $(0.9,1)$ |
| $y=0.8x+2$ | $(0.8,2)$ |
| $y=0.7x+3$ | $(0.7,3)$ |
| $y=0.6x+4$ | $(0.6,4)$ |

![](plano_mn5.png)
:::
???

Notamos, pela imagem, que todas as retas que tínhamos representado no plano $xy$ ao serem passadas para $mn$, tornam-se pontos alinhados entre si. Isso não é coincidência; se representássemos as infinitas retas que passam pelo ponto preto em $mn$, elas formariam uma reta. Veja na animação.

:ponto_vira_reta

Note que, além de cada reta ter virado um ponto, todos os pontos estão alinhados. Isso não é coincidência; se plotássemos todas as retas ($mn$) que passam pelo ponto preto, em $xy$, elas formariam uma reta em $mn$. Por isso, o ponto de $xy$ pode ser associado a uma reta em $mn$, ou seja, transformamos as retas em pontos e os pontos em retas.

??? Desafio
Descubra a fórmula que transforma o ponto de $xy$ em sua reta de $mn$.

**Dica:** Descubra a equação da reta preta em função de $mn$ e compare com a posição do ponto preto $(x, y)$.

::: Gabarito
Dado um ponto $(x, y)$, sua reta será $n = -xm + y$.
:::
???

??? CHECKPOINT
Descubra enquação da reta equivalente de cada um dos pontos mostrados.

![](ex1.png)

::: Gabarito

{red}(Por gabarito)
:::
???

??? Desafio
Descubra o padrão entre as retas $(m,n)$ que representam pontos colineares $(x, y)$.

::: Gabarito
{red}(texto explicando o padrão e imagem mostrando)

