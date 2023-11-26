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

??? CHECKPOINT
Quantas retas podem passar por **um** ponto?
::: Gabarito
Se sua resposta foi $\infty$ retas, você acertou!

![](infinitasretas.png)
:::
???

??? CHECKPOINT
Quantas retas podem passar por **dois** pontos?
::: Gabarito
Se sua resposta foi uma reta, você acertou!
*colocar imagem de uma reta entre dois pontos

![](infinitasretas.png)
:::
???

Associando isso a uma imagem binária com milhares de pixeis brancos, torna-se mais difícil garantir que todos esses espaços estão presentes a uma única reta. Em exemplos da vida real, muitas vezes esses pixeis não possuem uma distribuição uniforme a ponto de vincular cada pixel a uma reta específica, para isso seria necessário que o mundo fosse perfeitamente reto.

??? CHECKPOINT
Ao observar uma imagem como de uma rua:

![rua](rua.png)

O que faz uma faixa branca segmentada pertencer a uma reta de outra faixa branca segmentada? 

**Dica:** Vide a linha vermelha direita na imagem, o que torna as faixas brancas semelhantes?

::: Gabarito
Uma reta possui em comum duas coisas: o coeficiente angular e o coeficiente linear!
:::
???

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

Notamos, pela imagem, que todas as retas que tínhamos representado no plano $xy$, ao serem passadas para $mn$, tornam-se pontos alinhados entre si. Veja na animação.

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
Descubra a equação da reta equivalente de cada um dos pontos mostrados.

![](ex1.png)

::: Gabarito

![](ex2.png)
:::
???


Aqui entendemos porque realizamos a transformada, podemos definir que um ponto de $mn$ representa uma reta em $xy$ quando há muitas retas em $mn$ que passam por eles. No checkpoint acima, no plano $mn$, todas as 3 retas passam pelo ponto $(1,0)$.

Como aplicar à matemática ao algoritmo
---------

Porém, ao associarmos essas ideias a exemplos da vida real é complexo garantir que um ponto pertença uma reta específica ou a uma reta diferente. Para isso, é necessário que exista uma espécie de "votação". Levando o termo "votação" ao pé da letra, isso é um processo que as pessoas (no nosso caso, o código) escolhem determinada coisa seguindo uma regra particular.


??? CHECKPOINT
Suponha que  você estivesse elaborando um esquema de detcção de linhas e que o que te foi fornecido fossem vários pontos, como na imagem

![votacao](votacao/votacao222.png)

Nesse momento, o que foi pedido é que seja descoberto a qual linha o ponto roxo (0.8, 2.0) possa pertencer. Sabendo que infinitas retas passam por esse ponto, qual seria o **seu** critério para definir qual reta de fato é uma reta presente na imagem?

**Dica:** Lembre-se de olhar todo o contexto da imagem (pontos) e aquilo que torna uma reta "única"

::: Gabarito
Levando em consideração que todos os pontos estão presentes em um contexto conjunto, o ideal seria analisar quantos pontos passam por uma mesma reta, assim, quanto mais pontos passam, mais provável que aquele ponto pertença a uma reta
:::
???


??? CHECKPOINT
Levando em consideração a mesma imagem

![votacao](votacao/votacao222.png)

Se você pudesse traçar algumas retas ligando pelo menos dois pontos, qual você acreditaria ser "a reta correta", isto é, a reta que de fato existe?

::: Gabarito

A reta preta!

![votacao2](votacao/votacao333.png)

*Nesse caso foram traçadas outras retas para exemplificação, mas a reta correta seria a laranja*
:::
???

Nesse caso, para considerarmos que esse conjunto de pontos está presente em uma reta, foi necessário 5 pontos, porém isso pode ser maleável. Ai que entra a ideia de regra particular da votação (que foi comentado acima). Cabe a você decidir quantos pontos são suficientes para formar uma reta. Tudo isso vai depender da imagem de interesse, mas tendo uma regra de corte, torna-se possível a decisão. 

Ao ter uma regra estabelecida (quantidade pontos, por exemplo), a transformada de hough faz uso de uma matriz de acumulação para manter registrado os votos para cada linha possível. Para cada ponto na imagem, um processo de votação é realizado, assim o algoritmo calcula todas as possíveis linhas que podem passar por esse ponto e adiciona um voto para cada uma dessas linhas na matriz de acumulação.

Visto que um mesmo ponto tem infinitas linhas que o atravessam, torna-se ineficiente visualizar cada uma dessas linhas, por isso todo esse processo ocorre no plano $mn$. Além disso, as imagens "do mundo real" apresentam um ruído, afinal não necessariamente todas as retas são perfeitamente retas.

Logo, uma representação mais real daquilo que é observado é:

:votacao2

??? CHECKPOINT
Utilizando os pontos mostrados acima detectados como presentes em uma reta. Ao transpassar esses pontos para o plano $mn$ encontramos o seguinte resultado:

:votacao3

É perceptível que todos se cruzam em um ponto

![votacao2](votacao/votacao6.png)

O que esse ponto representa?

::: Gabarito

Uma reta que passa por todos os pontos
:::
???

Porém, ao aproximarmos é possível perceber que essas retas não cruzam perfeitamente no mesmo lugar.
Então consideremos essa região: 

![votacao7](votacao/votacao7.png)

Para isso, considera-se que os pontos do plano $mn$ (retas do plano $xy$), podem ser maiores que pontos. Assim, cada quadrado presente na imagem diz respeito a uma reta no plano $xy$.

![votacao8](votacao/votacao8.png)

Dessa forma, cada quadrado recebe uma votação de acordo com a quantidade de retas que passam por ele.

??? CHECKPOINT
Sabendo disso, qual seria a quantidade de votos dos quadrados 1, 2 e 3?

![votacao9](votacao/votacao9.png)

::: Gabarito

1- 5 votos

2- 2 votos

3- 1 voto
:::
???

??? CHECKPOINT
Utilizando a mesma imagem da questão anterior e os conhecimentos adquiridos ao longo deste handout, qual quadrado você diria que representa uma reta no plano $xy$

::: Gabarito

Quadrado 1

:::
???

Um ultimo detalhe matemático
---------

Mesmo assim, temos um obstáculo, ao analisar retas completamente verticais (com x constante), não existe coeficientes, fórmulas para prevermos determinadas coisas. Para isso, existem outros parâmetros para englobar todas possibilidades possíveis o $rho$ e o $theta$. Sua representação visual é:

![rhotheta](rhotheta.png)

A equação de retas utilizando esses parâmetros fica: $\rho = xcos(\theta) + ysen(\theta)$ e ao utiliza-los ao invés de m e n, o plano secundário no qual os pontos são transformados em retas passa a depender de $\rho$ e $\theta$. Quanto implementados a transformada de hough em código, os parâmetros utilizados são $\rho$ e $\theta$.

Existe uma biblioteca pronta dentro do OpenCV para a realização da transformada de hough, chamada ```python HoughLines()```, que tem como parâmetros não só o $\rho$ e o $\theta$ mas também um número inteiro que representa o número mínimo de votos para que uma reta seja de fato considerada uma reta, tornando possível o processo ser mais maleável a depender da imagem.




