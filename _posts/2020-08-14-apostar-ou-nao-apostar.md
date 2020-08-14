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

**II)** A equipe B tem a probabilidade de vitória $p_b$.

Assuma que:
\begin{equation} 1 > p_a > p_b > 0 \end{equation}

Assuma também que a cotação da aposta (que é o múltiplo do ganho) é inversamente proporcional a probabilidade de vitória de uma das equipes:

\begin{equation} 0 < \sigma_a < \sigma_b \end{equation}

Desse modo, se o jogador apostar na equipe A e a equipe A ganhar, então o indivíduo sai com o valor que ele apostou vezes a cotação para a equipe A. Mas se a equipe A perde, o indivíduo sai com nada. Se assim, o valor esperado do jogo dado que o jogador apostou na equipe A é:

\begin{equation} 
E[Jogo|Escolha = A] = p_a(\sigma_a\theta) + p_b(-\sigma_b\theta); \quad \theta = valor apostado
\end{equation}

O valor de equilíbrio do jogo é dado quando $E[Jogo|Escolha = A] = 0$. Como esse resultado, é possível encontar que:

\begin{equation} 
p_a = \frac{\sigma_b}{\sigma_a + \sigma_b} \quad p_b = \frac{\sigma_a}{\sigma_a + \sigma_b}
\end{equation}



