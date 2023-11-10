Transformada de Hough
======

O objetivo desse handout é ajudar vocês a entender o funcionamento e utilidade do algoritmo da Transformada de Hough. Assim possibilitando um uso mais consciente da implementação deste algoritmo.
____________

Para que usamos a Transformada de Hough?
---------

Pense que temos um jogo de sudoku do jornal em mãos e estamos com preguiça de resolve-lo. Então, como bons desenvolvedores pensamos em uma ideia de um programa que resolva o jogo para nós, a partir de uma foto que tiramos da câmera do celular, como essa por exemplo:

![](sudoku_limpo.jpg)

O primeiro passo é o processamento da imagem, sabendo o limite de cada célula, para que possamos criar uma lógica em cima. Para isso, temos que detectar as linhas da imagem e posteriormente identificar seus cruzamentos. Essa etapa da identificação de linhas é feita pela Transformada de Hough! Esse algoritmo também é capaz de identificar circunfêrencias e elipses, mas iremos focar na identificação de retas.

Um arquivo .png, por exemplo, não é uma entrada válida para o algoritmo, por isso ela passa por uma máscara, que a transforma em uma [imagem binária](https://embarcados.com.br/imagens-binarias/), ou seja, uma imagem em preto e branco. No caso da foto do sudoku, a imagem que representa esse conjunto seria essa aqui:

![](sudoku_preto_e_branco.png)

Com essa entrada fornecida, entramos no funcionamento algoritmo, que será explicado ao longo do handout, e esperamos uma saída como essa:

![](sudoku_saida.png)

A matemática por trás da Transformada de Hough
---------

Como dito anteriormente o algoritmo recebe um conjunto de pontos brancos, que representam a imagem dada, só que em formato binário. Concorda que em uma imagem aleatória podemos representá-los em um plano cartesiano $xy$, como o abaixo:

![](pontos1.jpg)


??? CHECKPOINT
Quantas retas podem passar por um ponto?
::: Gabarito
Se sua resposta foi $\infty$ retas, você acertou!

![](infinitasretas.png)
:::
???

Primeiramente concorda que podemos reprsentar as infintas retas com a equação $y = m*x + n$? Sendo que teremos ifinitos pares $m$ e $n$, 1 para cada reta.

Assim, notamos que com apenas dois parametros (m, n) podemos representar uma reta. Para entender melhor o algoritimo vamos comparar a representação de uma reta no modo tradicional vs no 'modo novo'.

Aqui temos um plano cartesiano com um ponto, passando a animação vamos várias retas que passam por esse ponto e suas respectivas equações.

:ani

Agora, vamos comparar o plano cartesiano a esquerda vs o plano da transformada de hough a direita.

:ani2

note que cada reta vira um ponto na transformada, além disso, todas as retas que passam por um mesmo ponto estão alinhados e se conectados formariam uma reta. Por isso o ponto pode ser associado à essa nova reta formada, o que transforma as retas em pontos e pontos em retas.

*imagem aqui*

??? CHECKPOINT
Desenhe em uma folha o que acontecerá com a representação de varios pontos que passam por uma mesma reta

**Dica:** Lembres-se que todos os pontos viram retas e todas as retas viram pontos.
::: Gabarito
Resposta *colocar fotinho do segundo plano *

:::
imagem que mostra varias retas (de pontos diferentes) passando pelo ponto de uma reta.