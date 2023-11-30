Transformada de Hough
======

A matemática por trás da Transformada de Hough
---------

Como dito anteriormente, o algoritmo recebe uma imagem em preto e branco. Mas, para explicar a matemática por traz dos algoritomos vamos tomar a imagem como um plano e os pixels como pontos. Como na imagem abaixo.

![](pontos.png)

Vamos relembrar algumas propriedades basicas de retas e pontos no plano cartesiano, afinal a matemática desse algoritmo está diretamente ligada ao comportamento de retas e pontos.

??? CHECKPOINT
Quantas retas podem passar por **um** ponto?
::: Gabarito
Se sua resposta foi $\infty$ retas, você acertou!

![](inf_retas.png)
:::
???

Então, ao analisarmos a imagem inicialmente fornecida, se olharmos os pontos brancos separadamente, cada um desses pontos tem infinitas retas que passam por eles. Mas, nenhum pixel existe sozinho (em uma imagem normal, né...), por isso é sempre necessário analisar em um contexto maior.


??? CHECKPOINT
Quantas retas podem passar por **dois** pontos?
::: Gabarito
Se sua resposta foi uma reta, você acertou! 

![](dois_pontos.png)
:::
???


Sabemos que a equação $y = mx + n$ é uma forma de representar uma reta. Assim, notamos que apenas dois parâmetros $(m, n)$ são suficientes para representá-la, ou pelo menos para diferenciar uma de todas as outras. Para entender o algoritmo, vamos explorar um pouco melhor essa ideia, representar a reta apenas por $(m, n)$, ou seja, [transformar as coordenadas](https://www.ufrgs.br/lageo/calculos/coord_exp.html) de uma reta de $(x, y)$ para $(m, n)$.

Abaixo temos um plano cartesiano com um ponto. Passando a animação, vemos várias retas que passam por esse ponto e suas respectivas equações.


:plano_xy

??? CHECKPOINT
Como ficaria a representação da primeira reta (verde claro) se, ao invés do plano $xy$, a representássemos no plano $mn$?

**Dica:** Lembre-se $m$ e $n$ são os parâmetros angular e linear da reta.
::: Gabarito
Como a equação da primeira reta é $y=x$, concluímos que $m=1$ e $n=0$:

![](reta_xy_para_mn.png)
:::
???


??? CHECKPOINT
Agora, faça o mesmo para as outras retas. Qual ponto representaria cada uma delas no plano $mn$?

::: Gabarito
| Reta ($xy$) | Ponto ($mn$) |
|----|-----|
| $y=x$ | $(1,0)$ |
| $y=0.9x+1$ | $(0.9,1)$ |
| $y=0.8x+2$ | $(0.8,2)$ |
| $y=0.7x+3$ | $(0.7,3)$ |
| $y=0.6x+4$ | $(0.6,4)$ |

![](5_retas_xy_para_mn.png)
:::
???

Notamos, pela imagem, que todas as retas que tínhamos representado no plano $xy$, ao serem passadas para $mn$, tornam-se pontos alinhados entre si. Veja na animação.

:plano_mn

Isso não é coincidência, se todas as retas que passam pelo ponto preto fossem transformadas de $xy$ para $mn$, elas formariam uma reta "preta" em $mn$.


??? Desafio
Descubra a fórmula que transforma o ponto de $xy$ em sua reta de $mn$.

**Dica:** Descubra a equação da reta preta em função de $mn$ e compare com a posição do ponto preto $(x, y)$.

::: Gabarito
Dado um ponto $(x, y)$, sua reta será $n = -xm + y$.
:::
???

Até agora apenos vimos algumas propriedades dessa transformação, mas nesse checkpoint você entenderá o fundamento que precisavamos.

??? CHECKPOINT
Descubra a equação da reta equivalente de cada um dos pontos mostrados, e coloque as equações no [geogebra](https://www.geogebra.org/classic?lang=pt_PT). Que padrão você notou?

![](3_pontos_colineares.png)

::: Gabarito

Todas as retas se encontram no ponto $(1,0)$, que é o ponto que representa a única reta que passa por todos eles em $xy$

![](3_pontos_colineares_em_mn.png)
:::
???


Reforçando, em $xy$ todos os pontos (amarelo, vermelho, verde) passavam pela reta (roxa), em $mn$ todas as retas (amarela, vermelha, verde) passam pelo ponto (roxo). É esse o padrão que será utilizado pela Transformada de Hough.

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
Aparentemente, este algoritmo tem um funcionamento bom, mas temos um obstáculo.

??? CHECKPOINT
Observando a imagem abaixo, quais são os valores dos coeficientes (angular e linear) da reta apresentada? 

![retareta](retareta.png)

::: Gabarito

Como não tem inclinação, essa reta não possui coeficientes angular e linear.

:::
???

Ao analisar retas completamente verticais (com x constante), não existe coeficientes, fórmulas para prevermos determinadas coisas. Para isso, existem outros parâmetros para englobar todas possibilidades possíveis o $rho$ e o $theta$. Sua representação visual é:

![rhotheta](rhotheta.png)

A equação de retas utilizando esses parâmetros fica: $\rho = xcos(\theta) + ysen(\theta)$ e ao utiliza-los ao invés de m e n, o plano secundário no qual os pontos são transformados em retas passa a depender de $\rho$ e $\theta$. Quanto implementados a transformada de hough em código, os parâmetros utilizados são $\rho$ e $\theta$.

A representação visual desses pontos utilizando os parâmetros rho e theta é  a seguinte:

![rhothetaespaco](rhoethetaespaco.png)

Na primeira imagem é possível ver os pontos no plano $xy$, já na segunda imagem, esses pontos são convertidos em curvas, no qual o ponto que essas curvas se cruzam á a reta presente na imagem. A ideia geral desse formato é a mesma que foi passada nesse handout porém com outros parâmetros e, visto que, essa forma engloba até as retas verticais, é a utilizada no código.

Existe uma biblioteca pronta dentro do OpenCV para a realização da transformada de hough, chamada ```python HoughLines()```, que tem como parâmetros não só o $\rho$ e o $\theta$ mas também um número inteiro que representa o número mínimo de votos para que uma reta seja de fato considerada uma reta, tornando possível o processo ser mais maleável a depender da imagem.




