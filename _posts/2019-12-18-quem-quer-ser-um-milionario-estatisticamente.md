---
title: "Pedir ajuda à audiência ou pedir 50/50 primeiro? Quem quer ser um milionario... estatisticamente"
date: 2019-12-18
tags:
  - estatística
  - python
---
Recentemente, assisti um [vídeo](https://www.youtube.com/watch?v=B7l0eSQO9dM) no qual o participante da jogo "Quem quer ser um milionário?" estava na pergunta de 1 milhão. Ele ainda possuía duas ajudas: Pergunte a audiência e 50/50. Em algum momento, o participante até brinca com o apresentador sobre qual ajuda ele deveria pedir primeiro. O apresentador aconselha 50/50 e, depois, perguntar a audiência, pois discartaria as alternativas erradas e ajudaria a resposta correta ser a mais escolhida. O participante, porém, argumenta que é melhor pedir ajuda a audiência e, depois, solicitar o 50/50, pois distribuiria as respostas erradas por mais alternativas, sobrando a resposta correta como mais votos.

Ambas as escolhas fazem sentido, no entanto, qual delas é melhor estatisticamente? Para realizar uma análise desse tipo, é preciso 
supor algumas premissas, formular proposições e realizar milhares de simulações para tentar, no limite, retratar a um cenário no mundo real.

As premissas assumidas são:

1 - O jogador sabe ou não sabe a resposta;

2 - A respostade de um participante não tem influência sobre a resposta de outro. (Respostas independentes);

3 - Todos os participantes da audiência que sabem a resposta, respondem corretamente. (Respostas equiprováveis).

A analíse da estratégia 50/50 --> Perguntar à Audiência (ATA) - das duas estratégias, na verdade- será feita através de proposições acerca das suas características. A primeira proposição é:

**proposição 1:** Suponha que tenha *N* pessoas na audiência e $ X = {X_1, ... X_N} $ é o conjunto das respostas de cada um dos participantes. A distribuição de X segue uma distribuição de Bernoulli, onde:
\begin{equation}X_i \sim Ber(p, (1-p)p) \end{equation}
