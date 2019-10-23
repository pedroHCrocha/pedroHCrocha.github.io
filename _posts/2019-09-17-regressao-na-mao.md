---
title: "Fazendo uma regressão Linear na 'mão' usando R"
date: 2019-09-17
tags:
  - Estatística
  - R
---

# Etapas iniciais
O objetivo aqui é praticar a modelo clássico de Regressão Linear e, em especial, as derivações 
do modelo Mínimos Quadrados Ordinários (MQO) para a disciplina de Econometria.

Vamos fazer exemplos simples e explicar cada passo. Primeiro, temos que criar alguns dados aleatórios.
Eles estarão apresentados no formato de matriz, pois é assim que são comumente derivados os estimadores
MQO em regressão múltipla (Poderíamos resolver algebricamente, mas usaria muita notação científica).

Temos uma matriz **X**, com dimensões *10x5*, e uma vetor-coluna **y**, com dimensões *10x1*.
A matriz X representará o conjunto de variáveis independentes e o vetor y será a variável dependente.
Note que ambos são valores aleatórios gerados a partir da distribuição Exponencial.

```r
set.seed(492)
X <- matrix(rnorm(50,mean=0,sd=1), ncol=5, nrow = 10)
y <- matrix(rnorm(10,mean=0,sd=1), ncol=1, nrow = 10)
```

Caso o mundo fosse plenamente determinístico e tivéssemos pleno conhecimento das relações causais entre variáveis de qualquer natureza, poderiámos exprimir a relação entre X e y no formato do qual a variável y é uma função das variáveis do conjunto X:

\begin{equation}y = f(X)\end{equation}

No entanto, não temos essa capacidade e, assim, devemos nos pautar em estimativas para extrair significado dessas relações. Como estimativas não são exatas, por natureza, elas adicionam um termo de resíduo. Então, a estimativa **ŷ** de **y** considera o resíduo.

\begin{equation}e = y - \hat{y}\end{equation}

Note que, no contexto matricial, o resíduo **e** é um vetor-coluna com dimensões *10 x 1*. É importante distinguir resíduo $\epsilon$ e erro (e). O erro refere-se a diferença entre os valores populacionais, que não observamos, e os valores estimados. Resíduo, por sua vez, é a diferença entre o valor estimado e o valor observado. 

Se a relação entre X e y for linear, então o modelo MQO permite que se encontre uma combinação linear de estimadores que permite produzir valores de ŷ:

\begin{equation}y_i = \hat{\alpha} + \hat\beta_1 X_1 + \hat\beta_2 X_2 + \hat\beta_3 X_3 + \hat\beta_4 X_4 + \hat\beta_5 X_5 + e_i\end{equation}

Representando essa equação na forma matricial, temos:

\begin{equation}y = X\hat\beta + e_i\end{equation}

## Encontrandos os estimadores MQO
O objetivo do modelo de *Mínimos Quadrados* é encontrar os estimadores $\hat\beta$ que minimizem o valor da soma do resíduos.
**Opa!** O resultado teórico do modelo pressupõe que a soma dos resíduos seja igual à 0. Isso significa que:

\begin{equation} e = y - \hat{y} = y - X\hat\beta = 0 \end{equation}

 Desse modo, o modelo MQO atribui um maior peso às instâncias mais distantes do resultado verdadeiro. Ele faz isso considerando o **soma do quadrado dos resíduos (SQR)**. Então, reformulando: *O objetivo do modelo de Mínimos Quadrados é encontrar os estimadores beta que minimizem o valor da soma do quadrado resíduos.* Podemos encontrar a equação que descreve a SQR. 
O vetor de resíduos *e* é dado por:

\begin{equation}e = y - X\hat\beta\end{equation}

Tomando a matriz transposta do vetor-coluna de *resíduo*, obtemos um vetor com dimensão (1x10)

\begin{equation}e^T = (y - X\hat\beta)^T\end{equation}

O produto desses dois vetores resulta em:

\begin{equation}e^T * e = e_1 * e_1 + e_2 * e_2 + ... + e_{10} * e_{10} \end{equation}

Note que podemos reescrever a equação acima:

\begin{equation}e^T* e = (y - X\hat\beta)^T*(y-X\hat\beta)\end{equation}

Geralmente, é neste passo que os produtos vetoriais ficam mais complicados. Apesar de que o exemplo inicial é fácil de tratar numericamente (produto de matrizes, transposta, etc.), fazer a transformação acima não é tão trivial.Por isso, partiremos para a parte algébrica.

### Derivação álgebrica dos cálculos
O exemplo que será apresentado valerá apenas para a demonstração da equação acima. Depois, voltaremos às matrizes originais. Primeiro, resolve a parte que que corresponde sem a transposta.

\begin{equation}
(y - X\hat\beta) = [...] = [y_1 - \hat\beta_{1}x_{11} -\hat\beta_{2}x_{21}\ ,y_2-\hat\beta_{1}x_{11}-\hat\beta_{2}x_{22}] 
\end{equation}

Para obter a transposta, basta trocar a ordem da linhas pelas colunas:

\begin{equation}
(y - X\hat\beta)^T = [...] = [y_1 - \hat\beta_{1}x_{11} -\hat\beta_{2}x_{12}\ ,y_2-\hat\beta_{1}x_{21}-\hat\beta_{2}x_{22}] 
\end{equation}

Logo, o produto entre os dois é:

\begin{equation}
[y_1 - \hat\beta_{1}x_{11} -\hat\beta_{2}x_{12}\ ,y_2-\hat\beta_{1}x_{21} -\hat\beta_{2}x_{22}] * \left[\begin{array}{rr} y_1 - \hat\beta_{1}x_{11} -\hat\beta_{2}x_{12}\\ y_2-\hat\beta_{1}x_{21} -\hat\beta_{2}x_{22}\end{array}\right]
\end{equation}

Para evitar dois processos idênticos, faremos apenas uma distribuição do produto dos vetores. Também denotaremos os elementos que vieram da matriz transposta por aspas simples $`'`$. É aconselhável ao leitor (caso seja de interesse) que tente expandir essa expressão algébrica em todos os seu fatores. Para retornar a forma matricial, precisamos analisar elemento por elemento. Começemos pelo mais simples:

\begin{equation}y'_1 * y_1 + y'_2 * y_2 = y^T * y\end{equation}

Vale ressaltar que as matrizes já foram apresentadas, assim, para facilitar a exposição das fórmulas, foram utilizadas apenas as siglas. A próxima expressão é:

\begin{equation}
(\hat\beta^{'}_{1}x'_{11} +\hat\beta^{'}_{2}x'_{12}) * y_1 + (\hat\beta{'}_{1}x'_{21} +\hat\beta^{'}_{2}x'_{22}) * y_2 = \hat\beta^TX^Ty
\end{equation}

\begin{equation}
[y_1 - \hat\beta^{'}_{1}x_{11} -\hat\beta_{2}x_{12}\ ,y_2-\hat\beta_{1}x_{21} -\hat\beta_{2}x_{22}] * \left[\begin{array}{rr} y_1 - \hat\beta_{1}x_{11} -\hat\beta_{2}x_{12}\\ y_2-\hat\beta_{1}x_{21} -\hat\beta_{2}x_{22}\end{array}\right]
\end{equation}

Em seguida, temos:

\begin{equation}
y'_1 * (\hat\beta_{1}x_{11} +\hat\beta_{2}x_{12}) + y'_2 * (\hat\beta'_{1}x_{21} +\hat\beta'_{2}x_{22}) = y^T\hat\beta X\end{equation}

A última expressão é dado por:

\begin{equation}
\hat\beta'_{1}x'_{11}x_{11}\hat\beta_{1} + [...] + \hat\beta'_{2}x'_{22}x_{22}\hat\beta_{2} = \hat\beta'X'X\hat\beta\end{equation}

### Parâmetros na forma matricial
Agora, dado que o passo-a-passo foi feito, podemos juntar todas as expressões para encontrar, finalmente, o produto entre os termos de resíduos:

\begin{equation}e^Te = y^Ty - \hat\beta^TX^Ty -y^T\hat\beta X + \hat\beta^TX^TX\hat\beta\end{equation}

Se os passos foram realizados cuidadosamente, é possível notar que os dois termos do meio são idênticos. Assim, podemos reorganizar novamente a equação:

\begin{equation}e^Te = y^Ty - 2\hat\beta^TX^Ty + \hat\beta^TX^TX\hat\beta\end{equation}

Para encontrar os estimadores $\hat\beta$ que minimizam a SQR, devemos derivar a equação acima em relação à $\hat\beta$ e igualar a 0. As regras de derivação também se aplicam para matrizes (na verdade, é um somatório), no entanto, é entendível que a notação matricial dificulte a vizualização de como realizar essas derivadas.*

\begin{equation}\frac{\partial e^Te}{\partial \hat\beta} = -2X^Ty + 2X^TX\hat\beta = 0\end{equation}

O resultado dessa derivada nos retorna  as **equações normais**:

\begin{equation}X^TX\hat\beta = X^Ty\end{equation}

Note que o produto das matrizes $X^TX$ sempre retornará uma matriz quadrada. Relembrando nosso exemplo, a matriz $X$ tem dimensões *10 x 5* e a $X^T$ tem dimensões *5 x 10*, logo, o produto entre elas gerará uma matriz com dimensões *10 x 10*. Isso é importante pois teremos que encontrar a **matriz Inversa** dela, e só é possível encontrar a matriz inversa em matrizes quadradas (de mesma dimensão). $X^TX$ também é uma matriz simétrica, isto é, $X^T = X$.

Usando a já mencionada matriz inversa, podemos multiplicar \( (X^TX)^{-1}\) em ambos os lados:

\begin{equation}(X^TX)^{-1}(X^TX)\hat\beta = (X^TX)^{-1}X^Ty\end{equation}

Em Álgebra Linear, a multiplicação da matriz inversa pela matriz original retorna a **matriz Identidade (I)** que, por sua vez, é semelhante a multiplicar por 1. Desse modo, chegamos ao resultado final:

\begin{equation}\hat\beta = (X^TX)^{-1}X^Ty\end{equation}

## Usando dados 
Agora que o $\hat\beta$ está isolado em um dos membros da equação, pode usar nossos dados para encontrar os estimadores. Começando pelo produto dentro dos parênteses, $(X^TX)^{-1}$:

```r
# Para encontrar o valor de X transposto:
X_t <- t(X)

# A multiplicação de matrizes no R é dada pelo operador %*% :
X_tX <- X_t %*% X

# Para encontrar a matriz Inversa, usa-se o comando solve()
X_tX_inversa <- solve(X_tX)

# Multiplicação da matriz Transposta com o vetor y
X_ty <- X_t%*%y
```

Após todos esses cálculos, chegamos aos valores dos estimadores $\hat\beta$:

```r
beta <- X_tX_inversa%*%X_ty
```
# Resultados
Depois de todos os cálculos realizados, encontramos os valores do estimadores $\hat\beta$ que minimizam a soma do quadrados dos resíduos. Para verificar essa propriedades dos estimadores MQO, devemos primeiro obter os valores das estimativas de y, ou seja, os valores de **ŷ**.
Para encontrar esses valores, basta recuperar uma das equações já apresentadas:

\begin{equation}ŷ = X\hat\beta\end{equation}

Podemos transformar essa equação na forma algébrica. O exemplo abaixo mostra a equação usada para encontrar o valor de $ŷ_1$. Note que qualquer valor $y_i$* será encontrado usando a mesma equação, mudando apenas o índice dos valores de $X_{i,k}$.

\begin{equation}ŷ_1 = x_{11}\hat\beta_1 + x_{12}\hat\beta_2 + x_{13}\hat\beta_3 + x_{14}\hat\beta_4 + x_{15}\hat\beta_5\end{equation}

```r
# Valor de y estimado
ŷ <- X%*%beta
```
Os resíduos são obtidos por meio da subtração dos valores observados de *y* e os valores estimados *ŷ*. O valor teórico da soma desses resíduos é 0 e o valor da soma dos quadrados dos resíduos é o mínimo (possível).
``` 
# Resíduos das estimativas
residuos <- y - ŷ

# Soma dos resíduos
sum(residuos)

# Soma do quadrado dos resíduos
format((sum(y - ŷ)^2), scientific = F)
```

# Propriedades dos estimadores MQO
Até o momento, a única hipótese que fizemos foi a de que a soma dos resíduos é igual à 0. Dada essa hipótese, fomos capazes de checar a principal propriedade dos estimadores MQO: os estimadores $\hat\beta$ minimizam a SQR. 
No entanto, existem outras propriedades. Para demonstrá-las, começamos relembrando as *equações normais*:

\begin{equation}(X^TX)\hat\beta = X^Ty\end{equation}

Sabemos, previamente, que $y = X\hat\beta + e$. Substituindo nas equações normais, teremos que:
\begin{equation}(X^TX)\hat\beta = X^T(X\hat\beta + e)\end{equation}
\begin{equation}(X^TX)\hat\beta = (X^TX)\hat\beta + X^Te\end{equation}
\begin{equation}(X^TX)\hat\beta - (X^TX)\hat\beta  = X^Te\end{equation}
\begin{equation}X^Te = 0\end{equation}

Mas o que isso significa esse resultado? Significa que cada variável independente $x_{k}$ da matriz X não tem correlação amostral com os resíduos. Conferindo esse resultado com nossos dados:

```r
round(X_t%*%residuos, 4)
```

Outra propriedade do estimadores MQO é que **o hiperplano da regressão passa sobre a média dos valores observados ($\overline{X}$ e $\overline{y}$)**. Isso deriva do fato de que a média amostral dos resíduos é igual a 0, isto é, $\overline{e} = 0$. Uma implicação direta desse fato é que $\overline{y} = \overline{x}\hat\beta$. O leitor é convidado mostra isso.

Podemos verificar usando os dados:
```r
mean(y)             # Média de y
mean(X)             # Média de X
mean(residuos)      # Média dos resíduos
mean(ŷ)             # Média das estimativas
colMeans(X)%*%beta  # Produto das médias de cada uma das colunas de X pelos estimadores
```

Outra propriedade é de que a média dos valores previstos $\overline{ŷ}$ é igual aos valores observados de $\overline{y}$, ou seja, $\overline{ŷ} = \overline{y}$. Conferindo o resultado:
```r
mean(y) - mean(ŷ) 
```

