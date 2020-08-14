---
title: "Apostar ou não apostar? Eis a questão"
date: 2020-08-14
tags:
  - estatística
---
Para quem gosta de esporte, existe um submundo que se extende à torcida tradicional. As apostas esportivas ocupam um espaço interesse entre o "nossa, com certeza 
que aquele time vai ganhar" e "quanto você estaria disposto a arriscar nesse resultado?". Mas onde existem o risco e o retorno, moram as probabilidades. E esse é motivo desse
post. Iremos criar um modelo simples que busca respondem se vale ou não apostar. 

Tenha um jogo com as seguintes probabilidades:

**I)** A equipe A tem a probabilidade de vitória $p_a$;

**II)** A equipe B tem a probabilidade de vitória $p_b$;

**III)** Não há empate.

Assuma que:
\begin{equation} 1 > p_a > p_b > 0 \end{equation}

Assuma também que a cotação da aposta (que é o múltiplo do ganho) é inversamente proporcional a probabilidade de vitória de uma das equipes:

\begin{equation} 0 < \sigma_a < \sigma_b \end{equation}

Desse modo, se o jogador apostar na equipe A e a equipe A ganhar, então o indivíduo sai com o valor que ele apostou vezes a cotação para a equipe A. Mas se a equipe A perde, o indivíduo sai com nada. Se assim, o valor esperado do jogo dado que o jogador apostou na equipe A é:

\begin{equation} 
E[Jogo|Escolha = A] = p_a(\sigma_a\theta) + p_b(-\sigma_b\theta); \quad \theta = \text{valor apostado}
\end{equation}

O valor de equilíbrio do jogo é dado quando o valor esperado do jogo é igual a 0. Como esse resultado, é possível encontrar que:

\begin{equation} 
p_a = \frac{\sigma_b}{\sigma_a + \sigma_b}; \quad p_b = \frac{\sigma_a}{\sigma_a + \sigma_b}
\end{equation}

Ou seja, as probabilidades que o mercado (que nesse caso, é a agência de aposta com base na oferta e demanda) estabelece da vitória de cada equipe é uma razão entre as cotações das equipes. Um exemplo númerico é: A equipe *A* tem uma cotação de 1.5 e a equipe *B* tem uma cotação de 3.5. Desse modo, o mercado prevê que a probabilidade de vitória da equipe A é 70% e da equipe B é 30%.

Mas não paramos aí. Suponha que queremos reduzir o risco da nossa aposta. Em finanças, nós queremos fazer um *hedge*. Para isso, iremos investir em apostar nas duas equipes como uma maneira de praticar diversificação. Desse modo, podemos assumir que:

**I)** Dado a equipe A tem maior probabilidade de vitória ($p_a > p_b$), então: $\theta_a > \theta_b$

Assim, para igualar o valor das apostas para ambas equipes, devemos ter que:

\begin{equation} 
\sigma_a\theta_a = \sigma_b\theta_b
\end{equation}

Se subtituirmos os valores das cotações pelas equações da probabilidade de vitória, chegaremos ao seguinte resultado:

\begin{equation} 
\theta_a = \frac{p_a}{p_b}\theta_b
\end{equation}

O mercado atribui um valor para a razão das probabilidades de vitória das equipes. Se o indivíduo acha que o valor da razão das probabilidades é maior que a do mercado (ou seja, o indivíduo acha que a equipe A tem maior chance de vitória), então o mercado está subestimando as probabilidades relativas, de apostar na equipe A em uma proporção maior do que a definida pela razão.

Para avaliar os lucros dessa estratégia de diversificação, tenha que:

**I) Lucro líquido, se equipe A ganhar**
\begin{equation} 
\pi_a = \sigma_a\theta_a - \theta; \quad \text{onde:} \theta = \theta_a + \theta_b
\end{equation}

**II) Lucro líquido, se equipe B ganhar**
\begin{equation} 
\pi_b = \sigma_b\theta_b - \theta
\end{equation}

O lucro esperado da estratégia de aposta mista é:
\begin{equation} 
E[\pi] = p_a\pi_a + p_b\pi_b 
\end{equation}

Se abrirmos um pouco a equação, teremos que:
\begin{equation} 
E[\pi] = (\frac{\sigma_b}{\sigma_a + \sigma_b})(\sigma_a\theta_a - \theta) + (\frac{\sigma_a}{\sigma_a + \sigma_b})(\sigma_b\theta_b - \theta)
\end{equation}

Depois de muitas transformações, chegaremos ao seguinte resultado:
\begin{equation} 
E[\pi] = \frac{\theta(\gamma -\beta)}{\beta} \quad \gamma = \sigma_a\sigma_b; \beta = \sigma_a + \sigma_b
\end{equation}




