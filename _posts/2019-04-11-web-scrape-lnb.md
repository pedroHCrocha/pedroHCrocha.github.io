---
title: "Web-Scrapping e análise dos jogadores da Liga Nacional de Basquete - temporada 2017/2018"
date: 2019-04-11
permalink: /posts/2019/04/blog-post-4/
tags:
  - web-scrapping
  - basquete
  - R
---

  O site da [Liga Nacional de Basquete](http://lnb.com.br/) apresenta uma seção onde se encontram as tabelas
com as estatisticas dos jogadores. São nessas tabelas que vamos fazer o web-scrapping. 
Aqui, vamos utilizar a linguagem de programação R. Primeiro, vamos utilizar os pacotes:

{% highlight r %}

library(dplyr)
library(rvest)

{% endhighlight %}

  Preste bem atenção na URL que estamos fazendo o web-scrapping. Queremos pegar as tabelas
com os dados agregados por atleta, para que depois seja possível modificar os dados da maneira que 
queremos. Selecionados nossas opções, coloque o código na IDE favorita:

{% highlight r %}

  ar <- read_html('http://lnb.com.br/nbb/estatisticas/arremessos/?aggr=sum&type=athletes&suffered_rule=0&season%5B%5D=47&wherePlaying=-1')
  ef <- read_html('http://lnb.com.br/nbb/estatisticas/eficiencia/?aggr=sum&type=athletes&suffered_rule=0&season%5B%5D=47&wherePlaying=-1')
  re <- read_html('http://lnb.com.br/nbb/estatisticas/rebotes/?aggr=sum&type=athletes&suffered_rule=0&season%5B%5D=47&wherePlaying=-1')
  
{% endhighlight %}

  Você deve ter três listas com os URL's. Depois, queremos captar apenas os "nodes" que representam
as tabelas com os dados. Para isso, rodamos:

{% highlight r %}
  tab_ar <- html_nodes(ar,"table")[[1]]
  tab_ef <- html_nodes(ef,"table")[[1]]
  tab_re <- html_nodes(re,"table")[[1]]
{% endhighlight %}

  Agora basta rodar o comando para montar os data frames e:
{% highlight r %}

  arr<-html_table(tab_ar)
  efi<-html_table(tab_ef)
  reb<-html_table(tab_re)

{% endhighlight %}

  Para facilitar a análise, organizamos alfabeticamente os dados:

{% highlight r %}

  arr <- arr[order(arr$Jogador),]
  efi <- efi[order(efi$Jogador),]
  reb <- reb[order(reb$Jogador),]
  
{% endhighlight %}

  Pronto! Bastar juntar os 3 data frames em um só:

{% highlight r %}

  arr$Pos. <- NULL
  efi$Pos. <-NULL
  reb$Pos. <- NULL
  temporada_2018 <- Reduce(merge, list(arr, efi, reb))
  
{% endhighlight %}

Análise dos jogadores da temporada 2018/2017 da Liga Nacional de Basquete
======
  O brilhantismo individual nunca foi tão evidente em um esporte em equipe quanto no basquete. Muitas vezes sobrevalorizado, outras não, a alta perfomance de um jogador durante uma temporada pode alavancar os resultados de uma equipe até o sucesso completo ou evitar o total fracasso. 

  Talentos desse patamar são a exceção, não a regra. E são dessas exceções que venho falar. Uma maneira rápida de conferir uma lista desses talentos é através do top-10 em pontos na temporada. Evidentemente, isso não implica que o maior pontuador seja o melhor jogador. 
  
  {% highlight r %}
  temporada_2018 %>%
    select(Jogador, Equipe, PTS) %>%
    arrange(desc(PTS)) %>%
    top_n(10)
  {% endhighlight r%}
  
| Nº | Jogador           | Equipe      | PTS |
|----|-------------------|-------------|-----|
| 1  | Fuller #2         | Corinthians | 464 |
| 2  | Zach Graham #1    | Brasília    | 387 |
| 3  | Shamell #8        | Mogi        | 380 |
| 4  | David Jackson #32 | Sesi Franca | 378 |
| 5  | JP Batista #13    | Mogi        | 378 |
| 6  | Lucas Dias #9     | Sesi Franca | 349 |
| 7  | D. Coleman #14    | Minas       | 336 |
| 8  | Léo Meindl #23    | Paulistano  | 322 |
| 9  | Marquinhos #11    | Flamengo    | 313 |
| 10 | Bennett #3        | Pinheiros   | 304 |
 
  No entanto, basquete não se resume em pontuação. Quais são as outras características de um jogador exímio em sua arte? Certamente, são muitas. Para isso, devemos fazer uso de alguns estatísticas não-usuais que compilam várias informações sobre os jogadores. Estas são comumente conhecidas como *analíticas avançadas*. Elas fornecem uma base estatística sobre a qual podemos rankear jogadores. 
  
  A primeira delas é a [**Eficiência**](https://en.wikipedia.org/wiki/Efficiency_(basketball)), talvez a mais conhecida. Ela junta todas as grandes estatísticas (pontos, rebotes, assistências, roubos, tocos e erros) junto com o total de arremessos convertidos e tentados. Resumidamente, a Eficiência é uma medida de referência generalizada sobre as contribuições ofensivas e defensivas do jogador em quadra. 
  {% highlight r%}
  
temporada_2018 %>% 
  mutate(EFF = ((PTS + RT + AS + BR + TO - (TCT - TCF) - (LLT - LLC) - ER)/JO)) %>% 
  select(Jogador, Equipe, EFF) %>% 
  arrange(desc(EFF)) %>% 
  top_n(10)
    
  {% highlight r%}
| Nº | Jogador           | Equipe      | EFF   |
|----|-------------------|-------------|-------|
| 1  | JP Batista #13    | Mogi        | 20.91 |
| 2  | David Jackson #32 | Sesi Franca | 18.86 |
| 3  | Jefferson #11     | Bauru       | 17.80 |
| 4  | Parodi #21        | Corinthians | 17.64 |
| 5  | Léo Meindl #23    | Paulistano  | 17.05 |
| 6  | Shamell #8        | Mogi        | 16.91 |
| 7  | D. Nunes #24      | São José    | 16.50 |
| 8  | Lucas Dias #9     | Sesi Franca | 16.45 |
| 9  | Fuller #2         | Corinthians | 16.00 |
| 10 | Leandrinho #91    | Minas       | 16.00 |

Em seguida, temos a queridinha dos analistas americanos, a [**True Shooting**](https://en.wikipedia.org/wiki/True_shooting_percentage) é uma medida de eficiência de arremesso que contabiliza os arremessos de 2 pontos, 3 pontos e lances livres. É de se esperar que bons arremessadores de lances livre se beneficiem dessa estatística pois é hipoteticamente mais fácil. Para evitar analisar que pouco tempo de quadra e porcetagens excessivas boas (pelo número de baixo de tentativas de arremesso), o parâmetro de seleção foi ter jogado, ao menos, 15 minutos por jogo.

{% highlight r%}
temporada_2018 %>% 
  mutate(MPJ = Min/JO) %>% 
  filter(MPJ >= 15) %>% 
  mutate(TS = 100* (PTS / ( 2 * (TCT + (0.44 * LLT) ) ) ) ) %>% 
  select(Jogador, Equipe, TS) %>% 
  arrange(desc(TS)) %>% 
  top_n(10) 
{% endhighlight r%}

| Nº | Jogador           | Equipe      | TS       |
|----|-------------------|-------------|----------|
| 1  | David Jackson #32 | Sesi Franca | 70.65949 |
| 2  | Teichmann #11     | Corinthians | 70.64948 |
| 3  | Cauê #1           | Bauru       | 67.21890 |
| 4  | Hettsheimeir #30  | Sesi Franca | 65.92827 |
| 5  | Alexey #4         | Sesi Franca | 64.60930 |
| 6  | Cipolini #15      | Sesi Franca | 64.25344 |
| 7  | Yago #2           | Paulistano  | 64.18156 |
| 8  | Pastor #9         | São José    | 63.94092 |
| 9  | Jamaal #5         | Botafogo    | 63.55932 |
| 10 | Shamell #8        | Mogi        | 63.51966 |

[**Effective Field Goal Percentage**](https://en.wikipedia.org/wiki/Effective_field_goal_percentage) é uma estatística fácil de calcular que ajusta a pontuação do arremesso de três pontos em relação ao arremesso de dois pontos,devido a bonificação da pontuação extra. 

Por exemplo: se um jogador X acerta 2/6 de 2 pontos e acerta 2/4 de 3 pontos, e um jogador Y arremessa 5/10 da área de 2 pontos com 0 dos três, cada jogador teria 10 pontos para 10 arremessos e, logo, a mesma porcentagem de pontos por arremesso, isto é, a mesma 
**effective field goal percentage**. Note que o parâmetro usado para filtrar os jogadores no cálculo do True Shooting (%), os minutos em quadra, foram aplicados aqui.

{% highlight r%}
temporada_2018 %>% 
  mutate(MPJ = Min/JO) %>% 
  filter(MPJ > 15) %>% 
  mutate(EFG = (((TCF + 0.5 * TresFeitas) / TCT) * 100) ) %>% 
  select(Jogador, Equipe, EFG) %>% 
  arrange(desc(EFG)) %>% 
  top_n(10) 
{% endhighlight r%}

| Nº | Jogador           | Equipe        | EFG      |
|----|-------------------|---------------|----------|
| 1  | Teichmann #11     | Corinthians   | 71.83099 |
| 2  | Cauê #1           | Bauru         | 66.90141 |
| 3  | David Jackson #32 | Sesi Franca   | 65.19824 |
| 4  | Bennett #3        | Pinheiros     | 60.76555 |
| 5  | Shamell #8        | Mogi          | 60.74219 |
| 6  | Alexey #4         | Sesi Franca   | 60.45455 |
| 7  | Pastor #9         | São José      | 60.21505 |
| 8  | Lucão #3          | Vasco da Gama | 59.72222 |
| 9  | Lucas Dias #9     | Sesi Franca   | 59.65251 |
| 10 | Cipolini #15      | Sesi Franca   | 59.49721 |

  Agora partirmos para as estatísticas complicadas. Apesar delas requererem uma quantia não-saudável de métricas que confusamente se relacionam, elas, na maioria dos casos, conseguem explicar bem a narrativa da história. 
  
  Um desses "horrores" analíticos é a [**Usage Rate**](https://www.nbastuffer.com/analytics101/usage-rate/). O caso mais célebre do uso dessa estatística foi o de [Russell Westbrook](https://www.statmuse.com/stories/0e15dfb5-4d87-4c2d-9ad3-994b554aa903), na temporada 2016/2017. Com Kevin Durant machudado, Russell teve de basicamente carregar o time nas costas, anotando vários triplos-duplos consecutivos e igualando o recorde de Oscar Robertson de ter, em média, um triplo-dublo na temporada. Evidentemente, para conseguir tal feito, foi preciso muito tempo com a bola na mão. E é basicamente isso o que esta estatística busca mensurar. Ela sumariza a porcentagem das jogadas de um time vinda de um jogador. 
  
  {%highlight r%} 

  temporada 2018 %>% 
  filter(MPJ > 15) %>% 
  group_by(Equipe) %>% 
  mutate(UR = 100 * ((TCT + 0.44 * LLT + ER) * (sum(Min)/ 5))
         /(Min * (sum(TCT) + 0.44 * sum(LLT) + sum(ER)))) %>% 
  ungroup()%>%
  select(Jogador, Equipe, UR) %>% 
  arrange(desc(UR)) %>% 
  top_n(10)

  {% endhighlight r%}
  
| Nº | Jogador          | Equipe           | UR       |
|----|------------------|------------------|----------|
| 1  | Hettsheimeir #30 | Sesi Franca      | 31.55825 |
| 2  | Leandrinho #91   | Minas            | 30.79446 |
| 3  | Starks #3        | Joinville / AABJ | 29.41618 |
| 4  | Jefferson #11    | Bauru            | 28.52987 |
| 5  | Fuller #2        | Corinthians      | 28.01836 |
| 6  | Betinho #26      | Pinheiros        | 26.95637 |
| 7  | Pedro #12        | São José         | 26.16757 |
| 8  | Léo Meindl #23   | Paulistano       | 25.94556 |
| 9  | Schneider #13    | São José         | 25.79185 |
| 10 | Alex #10         | Bauru            | 25.27061 |
  
  A estatística que melhor resume as qualidades individuais de um jogador, levando em consideração os minutos que ele passa em quadra e a performance geral da equipe, é o [**Player Efficience Rating**](https://en.wikipedia.org/wiki/Player_efficiency_rating), mais conhecido como **PER**. O PER é uma métrica que busca contemplar todas as contribuições do jogador, especialmente no quesito ataque. Ele é muito utilizado para comparar um jogador no tempo, por exemplo, se um jogador apresentou evolução durante várias temporadas, como também para comparar jogadores de diferentes temporadas. Retomando os exemplos da NBA, Michael Jordan, na temporada 1987-1988, teve um PER de 31.71 e ganhou o MVP, Lebron James, na temporada 2012-2013, teve um PER de 31.59 e ganhou MVP, MVP da final e o campeonato, e Stephen Curry, na temporada 2015-2016, teve um PER de 31.46 e ganhou o 1º MVP unânime em um time de 73 vitórias (mas perdeu o campeonato). 

  O **PER** é, de longe, a estatística mais difícil de ser calculada. O site do [basketball-reference](https://www.basketball-reference.com/about/per.html) dedica uma página inteira ao cálculo do PER. Um dica: Não se importe muito como o cálculo é realizado ou o 'porquê' das variáveis. A estatística PER, assim como a salsicha e as leis, fica melhor sem saber como é feita. Mas segue alguns passos a partir da base que coletamos. 
  
  *Observeração: As variáveis VOP, factor e rebratio foram calculadas a priori. Para ver como são calculadas, veja o site do 'Basketball Reference'*.

{% highlight r%}
temporada2018 <- temporada2018 %>% 
  group_by(Equipe) %>% 
  mutate(uPER = (1 / Min) *
  (TresFeitas + (2/3) * AS + (2 - factor * (sum(AS) / sum(TCF))) * TCF +
   (LLC *0.5 * (1 + (1 - (sum(AS) / sum(TCF))) + (2/3) * (sum(AS) / sum(TCF)))) -
    VOP * ER -
    VOP * rebratio * (TCT - TCF) -
    VOP * 0.44 * (0.44 + (0.56 * rebratio)) * (LLT - LLC) +
    VOP * (1 - rebratio) * (RT - RO) +
    VOP * rebratio * RO +
    VOP * BR +
    VOP * rebratio * TO -
    FC * ((lg_FT / lg_PF) - 0.44 * (lg_FTA / lg_PF) * VOP)))
  {% endhighlight r%}
  
  {% highlight r%}
  library(tidyr)
temporada2018 <- temporada2018 %>% 
  arrange(Equipe)
  
set.seed(1)
estimated_pace_adjustment <- runif(14, 0.96, 1.08)

t <- temporada2018 %>% 
  nest(-Equipe)
linhas <- sapply(t$data, nrow)

epa <- as.data.frame(unlist(mapply(rep,estimated_pace_adjustment,linhas)))
colnames(epa) <- 'EPA'

temporada2018 <- cbind(temporada2018, epa)

temporada2018 <- temporada2018 %>% 
  arrange(Jogador) %>%
  group_by(Equipe) %>% 
  mutate(aPER = uPER * EPA)
 {% endhighlight r%}

  {% highlight r%}
  lg_aPER <- temporada2018 %>% 
  summarise(team_aPER = weighted.mean(aPER, Min)) %>% 
  summarise(lg_aPER = sum(team_aPER)/14)

temporada2018 <- temporada2018 %>% 
  mutate(PER = aPER*(15/lg_aPER)) %>%
  filter(MPJ > 15) %>% 
  select(Jogador, Equipe, PER) %>% 
  arrange(desc(PER)) %>% 
  top_n(10)
  {% endhighlight r%}
  
 {% highlight r%}
| Nº | Jogador           | Equipe           | PER       |
|----|-------------------|------------------|-----------|
| 1  | Olivinha #16      | Flamengo         | 26.399667 |
| 2  | David Jackson #32 | Sesi Franca      | 26.397824 |
| 3  | Starks #3         | Joinville / AABJ | 24.642848 |
| 4  | JP Batista #13    | Mogi             | 24.172859 |
| 5  | Jefferson #11     | Bauru            | 23.519490 |
| 6  | Graterol #6       | Brasília         | 22.383982 |
| 7  | Marquinhos #11    | Flamengo         | 22.362188 |
| 8  | Léo Meindl #23    | Paulistano       | 22.335158 |
| 9  | Balbi #6          | Flamengo         | 22.106162 |
| 10 | Lucas Dias #9     | Sesi Franca      | 21.86010  |
 {% endhighlight r%}

   
 
