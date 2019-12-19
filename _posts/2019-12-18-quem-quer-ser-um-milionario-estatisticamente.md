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

**1** - *O jogador sabe ou não sabe a resposta;*

**2** - *A resposta de de um participante não tem influência sobre a resposta de outro. (Respostas independentes);*

**3** - *Todos os participantes da audiência que sabem a resposta, respondem corretamente. (Respostas equiprováveis).*

A analíse da estratégia 50/50 --> Perguntar à Audiência (ATA) - das duas estratégias, na verdade- será feita através de proposições acerca das suas características. A primeira proposição é:

**Proposição 1:** Suponha que tenha *N* pessoas na audiência e $ X = {X_1, ... X_N} $ é o conjunto das respostas de cada um dos participantes. A distribuição de X segue uma distribuição de Bernoulli, onde:
\begin{equation}X_i \sim Bernoulli(p, (1-p)p) \end{equation}

**Proposição 2:** Suponha que *n* pessoas na audiência sabem a resposta da pergunta de 1 milhão. Então, existem *N* pessoas na
audiência, *n* pessoas que sabem a resposta e *N-n* pessoas que não sabem a resposta.

**Proposição 3:** As respostas das *n* pessoas tem probabilidade **p = 1** de acerto da questão. As respostas das *N-n* pessoas tem probabilidade **p = 0.5** de acerto da questão.

Como as respostas são independentes, então podemos transformar a distribuição de Bernoulli em uma distribuição Binomial. Desse modo, formulamos mais duas proposições:

**Proposição 4:** As respostas das pessoas que sabem a resposta segue uma distribuição Binomial, tal que, p = 1:
\begin{equation}Y \sim Bin(np, n(1-p)p) \end{equation}

**Proposição 5:** As respostas das pessoas que não sabem a resposta segue uma distribuição Binomial, tal que, p = 0.5:
\begin{equation}Z \sim Bin((N-n)p, (N-n)(1-p)p) \end{equation}

Somando as proposições 4 e 5, podemos obter a média e a variância da distribuição das respostas de toda a audiência. Desse modo, temos:

**Proposição 6:** As respostas das N pessoas da audiência tem distribuição dada por:
\begin{equation} A = (Y + Z) \sim (E(Y + Z), Var(Y + Z)) \end{equation}

Onde: $ E(A) = E(Y + Z) = \frac{N+n}{2} $ e Var(A) = Var(Y + Z) = \frac{N-n}{4}
