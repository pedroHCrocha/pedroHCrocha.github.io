---
title: "Como saber se uma ação está cara ou não usando os preços mínimo, corrente e máximo?"
date: 2020-02-29
tags:
  - finanças
  - R
---
Após ler **"A Random Walk Down Wall Street"**, de Burton Malkiel e **"O Investidor Inteligente"**, de Benjamin Graham, decidi começar a operar na renda variável. 

Para analisar empresas de forma rápida e relativamente detalhada, eu utilizo frequentemente o site [Fundamentus](https://www.fundamentus.com.br/). É possível obter informações dos balanços das empresas, indicadores contábeis, como Liquidez Corrente,
ROE, Dividend Yield; e também indicadores de mercado, como P/L, EV/EBITDA, etc.

Os métodos ensinados pelos "Values Investors" e, em especial, Graham e Malkiel, enfatizam que constantes considerações
das flutuações de preços de curto prazo, atrapalha mais do que ajuda o investidor no processo de decisão de
compra ou venda de um ativo. 

Para eles, o mais importante é estabelcer um critério que possibilite a seleção de empresas com quadro financeira forte,
relativamente grandes, em termos de mercado de atuação, pagadoras de dividendos initerruptos e, sobretudo, com um preço razoável. 

Os indicadores contábeis e de mercado tradicionalmente utilizados para avaliar se uma  ação está barata ou cara são o Preço sobre o Lucro por Ação (P/L), 
o Preço sobre o Valor Patrimonial (P/VPA), o P/L com o lucro por ação médio dos últimos 3 anos contábeis,
o P/L médio do setor de atuação da empresa e o P/L médio do mercado. Vale ressaltar que não existe um indicador atemporal 
para todo tipo de negócio, bem como não existe um valor universal paraos indicadores que permita classificar um ação entre barata e cara. 

No entanto, fiquei interessado em saber se é possível determinar se uma ação está
cara ou barata através de um indicador não-contábil. Coincidentemente, esta ideia surgiu ao analisar os dados
do site Fundamentus. Resolvi utilizar os preços mínimo e máximo nas últimas 52 semanas e o seu preço corrente como *inputs* desse
indicador, ainda imaginário.

O desafio estava feito: Desenvolver um indicador que utilize apenas os preços mínimo, corrente e máximo para determinar se uma ação
está "barata" ou "cara". Este indicador deve ser:

**1** - *Insensível aos preços de diferentes ações*; 

**2** - *Comparável entre diferentes ações.*
