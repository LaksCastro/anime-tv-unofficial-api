<p align="center">
  <img src="/assets/animetv.png" width="70">
</p>
<h2 align="center">Anime TV API</h2>
<p align="center">
  Anime TV é um  <a href="https://play.google.com/store/apps/details?id=br.com.animetvon&hl=pt_BR">aplicativo da Play Store</a> utilizado para Stream de animes totalmente grátis mas com anúncios. E este repositório é a documentação não oficial da API utilizada para buscar os dados dos animes.
</p>

<br>

<blockquote>
  <h2>AVISO IMPORTANTE</h2>
  <h4>Este repositório tem como e SOMENTE o objetivo de ESTUDO. Eu não sou responsável por nada sobre este aplicativo ou pela sua API, apenas utilizei um simples processo de descompilação neste aplicativo para entender seu funcionamento com objetivo de ESTUDO, <a href="/MORE.md">para saber mais clique aqui</a></h4>
</blockquote>

<br>

## Considerações
As limitações foram concluídas somente olhando e testando os endpoints do código fonte do aplicativo **portanto é possível que essas conclusões estejam incorretas.** Então se quiser checar você mesmo, e tentar descobrir se a limitação está incorreta, sinta-se totalmente à vontade, todo o código que utilizei de base está na pasta `/app` deste repositório.

## Documentação

### Table of contents
1. [Iniciando](#1-iniciando)
2. [Buscar os dados dos últimos animes lançados](#2-buscar-os-dados-dos-últimos-animes-lançados)
3. [Buscar os dados de animes por categoria](#3-buscar-os-dados-de-animes-por-categoria)
4. [Buscar os dados de animes por letra](#4-buscar-os-dados-de-animes-por-letra)
5. [Buscar os dados dos episódios de um anime](#5-buscar-os-dados-dos-episódios-de-um-anime)
6. [Buscar os dados de Streaming de um episódio](#6-buscar-os-dados-de-streaming-de-um-episódio)
7. [Buscar os dados de um anime pelo nome](#7-buscar-os-dados-de-um-anime-pelo-nome)
8. [Buscar os detalhes de um anime](#8-buscar-os-detalhes-de-um-anime)
9. [Buscar os animes mais populares](#9-buscar-os-animes-mais-populares)
10. [Buscar o próximo episódio](#10-buscar-o-próximo-episódio)
11. [Buscar o episódio anterior](#11-buscar-o-episódio-anterior)

### 1. Iniciando
1. Tenha em mente que todas as requisições devem ser feitas usando query params, ou seja, usando o ponto de interrogação, exemplo: `?endpoint=abc`  
2. A URL de base é esta: `https://appanimeplus.tk/api-achance.php`  
3. A URL de base das imagens é esta: `https://cdn.appanimeplus.tk/img/`

> A URL de base é a URL que você utilizará para buscar os dados em JSON

> A URL de base das imagens é a URL das imagens completa, por exemplo, você verá que a API retorna o nome único da imagem, por exemplo: `5bd529d5b07b647a8863cf71e98d651a.jpg`,
> e então, para poder buscar a imagem, utilize a URL de base + este nome único da imagem, usando o mesmo exemplo, neste caso a URL ficará: `https://cdn.appanimeplus.tk/img/5bd529d5b07b647a8863cf71e98d651a.jpg`

<br>

### 2. Buscar os dados dos últimos animes lançados
Para buscar os últimos lançamentos, o endpoint é este:
```
https://appanimeplus.tk/api-achance.php?latest
```
Isto irá retornar, no momento em que estou buscando os últimos 30 lançamentos:
```js
[
  {
    "video_id": "433312",
    "category_id": "33193",
    "title": "Deca Dence Episodio 04",
    "category_image": "1a551b7323fefa14d9b4ac09bd73ee49.jpg"
  },
  {
    "video_id": "433311",
    "category_id": "33184",
    "title": "Re:Zero kara Hajimeru Isekai Seikatsu 2 Episodio 04",
    "category_image": "4a61f90089bdb5a4965c92b9b825afc5.jpg"
  },
  { .... }
]
```

Explicação da resposta:
```
video_id: ID do episódio do anime
category_id: este ID é usado para buscar a lista de todos os episódios do anime
category_image: nome único da imagem da capa do anime
```

Formato da resposta:
```ts
interface Episodio {
  video_id: string,
  category_id: string,
  title: string,
  category_image: string,  
}[]
```

**Limitações: o valor é fixo, você pode buscar apenas 30 e somente os últimos 30 animes lançados.**

<br>

### 3. Buscar os dados de animes por categoria
Para buscar os animes que se encaixam em uma categoria use o endpoint:
```
https://appanimeplus.tk/api-achance.php?categoria=NOME_DA_CATEGORIA
```

O retorno da requisição:
```js
[
  {
    "id": "33097",
    "category_name": "7 Seeds 2 Dublado (Seven Seeds 2 Dublado)",
    "category_image": "62c1507b8202cd94f44733de18ea736b.jpg"
  },
  {
    "id": "8461",
    "category_name": "7 Seeds Dublado (Seven Seeds Dublado)",
    "category_image": "c2839bed26321da8b466c80a032e4714.jpg"
  },
  { .... }
]
```

Explicação da resposta:
```
id: ID do anime
category_name: título/nome do anime
category_image: nome único da imagem da capa do anime (pode retornar 404, a API é inconsistente)
```

Formato da resposta:
```ts
interface Anime {
  id: string,
  category_name: string,
  category_image: string,
}[]
```

Todas as categorias disponíveis [podem ser vistas neste arquivo.](/CATEGORIES.md)    

**Limitações: não há paginação, assim como na primeira. Portanto a quantidade de animes da resposta não é fixo, por exemplo, no momento que estou testando, a categoria `dublado` retornou 340 animes; já a categoria `sci-fi` retornou 562.**

<br>

### 4. Buscar os dados de animes por letra
Para buscar os animes por sua letra inicial, utilize o endpoint: 
```
https://appanimeplus.tk/api-achance.php?letra=QUALQUER_LETRA_OU_#
```
Todas as letras disponíveis são \[A-Z] e também o jogo da velha "#" que simboliza todos os animes que possuem o nome que inicia com um caracter especial

O retorno da requisição:
```js
[
  {
    "id": "32995",
    "category_name": "Z/X: Code Reunion",
    "category_image": "bf66335975dc320ebc9692558107b536.jpg"
  },
  {
    "id": "846",
    "category_name": "Z/X: Ignition",
    "category_image": "84f7e69969dea92a925508f7c1f9579a.jpg"
  },
  { .... }
]
```

Explicação da resposta:
```
id: ID do anime
category_name: título/nome do anime
category_image: nome único da imagem da capa do anime (pode retornar 404, a API é inconsistente)
```

Formato da resposta:
```ts
interface Anime {
  id: string,
  category_name: string,
  category_image: string,
}[]
```

**Limitações: Não há paginação.**

<br>

### 5. Buscar os dados dos episódios de um anime
Para buscar os dados dos episódios de um anime, você precisa do ID deste anime, se não tem, obtenha ele utilizando algum dos endpoints anteriores. Com o ID do anime em mãos, use o endpoint: 
```
https://appanimeplus.tk/api-achance.php?cat_id=ID_DO_ANIME
```

O retorno da requisição:
```js
[
  {
    "video_id": "433311",
    "category_id": "33184",
    "title": "Re:Zero kara Hajimeru Isekai Seikatsu 2 Episodio 04"
  },
  {
    "video_id": "433272",
    "category_id": "33184",
    "title": "Re:Zero kara Hajimeru Isekai Seikatsu 2 Episodio 03"
  },
  { .... }
]
```

Explicação da resposta:
```
video_id: ID do episódio
category_id: ID do anime na qual o episódio pertence
title: título/Nome do episódio
```

Formato da resposta:
```ts
interface Anime {
  video_id: string,
  category_id: string,
  title: string,
}[]
```

**Limitações: Não há paginação.**

<br>

### 6. Buscar os dados de Streaming de um episódio
Os "dados de Streaming" contém os dados da URL do vídeo do episódio, em algum formato de vídeo. Para buscar estes dados, utilize o endpoint:
```
https://appanimeplus.tk/api-achance.php?episodios=ID_DO_EPISODIO
```

O retorno da requisição:
```js
[
  {
    "video_id": "433311",
    "category_id": "33184",
    "title": "Re:Zero kara Hajimeru Isekai Seikatsu 2 Episodio 04",
    "location": "https://redirector.googlevideo.com/videoplayback?expire=1596085231&ei=b-MhX876L-eD2LYPx--t4Ag&ip=149.56.143.221&id=59f5d41cf0c46131&itag=18&source=blogger&mh=Wo&mm=31&mn=sn-25glen7r&ms=au&mv=u&mvi=3&pl=27&susc=bl&mime=video/mp4&dur=1770.103&lmt=1596035453125206&mt=1596056012&sparams=expire,ei,ip,id,itag,source,susc,mime,dur,lmt&sig=AOq0QJ8wRQIhALRqheG3If-urnqIe-8v7cksKtYCep0TKO7n22nQiGthAiAPBO2vwigixDituK3Mbi8NL5hCPgzekweDehvNwUGmDQ%3D%3D&lsparams=mh,mm,mn,ms,mv,mvi,pl&lsig=AG3C_xAwRgIhAIqbApI4lnmFS_yH6OCK5cMfkaZRtxamvGZdqXHQSd7MAiEArSahkMovg0Slk-yZYAiA96jYF7JBdKo_Ge04f-Tn8y0%3D",
    "locationsd": "https://redirector.googlevideo.com/videoplayback?expire=1596085231&ei=b-MhX876L-eD2LYPx--t4Ag&ip=149.56.143.221&id=59f5d41cf0c46131&itag=22&source=blogger&mh=Wo&mm=31&mn=sn-25glen7r&ms=au&mv=u&mvi=3&pl=27&susc=bl&mime=video/mp4&dur=1770.103&lmt=1596035486386542&mt=1596056012&sparams=expire,ei,ip,id,itag,source,susc,mime,dur,lmt&sig=AOq0QJ8wRAIgHaZE30mFYOWl4rkH4TZC5hdxNLWkAB9Pb_lXmeaz544CICE7LE5mqENbeEjYrAWggBxAvPiaVpjxEQtyyZChiVcG&lsparams=mh,mm,mn,ms,mv,mvi,pl&lsig=AG3C_xAwRQIgZrr2C3tPHVdaH2pv50hUEpXYmWg4Uu_VHYe26-M7o2UCIQCdMYyvOkoKBWny9IEmh-QG22jJBKbJ5qMfzTBDzwca2g%3D%3D"
  }
]
```
O retorno deste endpoint sempre será apenas um objeto, mas mesmo assim ainda será dentro de um array. A razão disso eu não sei, só sei que é assim.

Explicação da resposta:
```
video_id: ID do episódio
category_id: ID do anime na qual o episódio pertence
location: URL do vídeo em qualidade normal
locationsd: URL do vídeo em qualidade HD (A maioria dos animes não possui esse campo, apenas os mais atuais entre +- de 2019 até agora)
```

Formato da resposta:
```ts
interface Stream {
  video_id: string,
  category_id: string,
  location: string,
  locationsd: string,
}[]
```

<br>

### 7. Buscar os dados de um anime pelo nome
Para buscar animes pelo nome, siga estas regras:
- Nomes em letra minúsculas
- Remova qualquer acento ou caracter especial e substitua por um espaço vazio
- E então agora remova todos os espaços vazios por underline  

E então utilize o endpoint:
```
https://appanimeplus.tk/api-achance.php?search=NOME_DO_ANIME
```

Por exemplo, o anime Steins;Gate ficará desta forma:
```
https://appanimeplus.tk/api-achance.php?search=steins_gate
```

O retorno da requisição:
```js
[
  {
    "id": "389",
    "category_name": "Steins;Gate",
    "category_image": "c86a7ee3d8ef0b551ed58e354a836f2b.jpg"
  },
  {
    "id": "8043",
    "category_name": "Steins;Gate 0",
    "category_image": "5bd529d5b07b647a8863cf71e98d651a.jpg"
  },
  { .... }
]
```

Explicação da resposta:
```
id: ID do anime
category_name: título/nome do anime
category_image: nome único da imagem da capa do anime (pode retornar 404, a API é inconsistente)
```

Formato da resposta:
```ts
interface Anime {
  id: string,
  category_name: string,
  category_image: string,
}[]
```

<br>

### 8. Buscar os detalhes de um anime
Para buscar os detalhes do anime basta fazer uma requisição para este endpoint usando o ID do anime:
```
https://appanimeplus.tk/api-achance.php?info=ID_DO_ANIME
```

O retorno da requisição:
```js
[
  {
    "id": "389",
    "category_name": "Steins;Gate",
    "category_image": "c86a7ee3d8ef0b551ed58e354a836f2b.jpg",
    "category_description": "Steins;Gate se passa no Verão de 2010, aproximadamente um ano após os acontecimentos que tiveram lugar em Chaos;Head, em Akihabara. Steins;Gate é sobre um grupo de amigos que personalizaram seus microondas num dispositivo que pode enviar mensagens de texto para o passado. Como eles realizam experiências diferentes, uma organização chamada Sern que vem a fazer sua própria pesquisa sobre viagem no tempo descobre sobre o grupo e agora os personagens têm de encontrar uma maneira de não serem capturados.",
    "category_genres": "Sci-Fi, Suspense",
    "ano": "2011",
    "count": "512",
    "off": "0"
  }
]
```

Explicação da resposta:
```
id: ID do anime
category_name: título/nome do anime
category_image: nome único da imagem da capa do anime (pode retornar 404, a API é inconsistente)
category_description: a sinopse do anime
category_genres: os gêneros do anime separado por vírgula dentro de uma string
ano: ano do anime
count: ???????
off: ???????
```

Formato da resposta:
```ts
interface Detalhes {
  id: string,
  category_name: string,
  category_image: string,
  category_description: string,
  category_genres: string,
  ano: string,
  count: string,
  off: string,
}[]
```

<br>

### 9. Buscar os animes mais populares
Para buscar uma lista com os animes mais populares utilize o endpoint:
```
https://appanimeplus.tk/api-achance.php?populares
```

O retorno da requisição:
```js
[
  {
    "id": "111",
    "category_name": "Naruto Shippuden (Naruto Shippuuden)",
    "category_image": "698d51a19d8a121ce581499d7b701668.jpg"
  },
  {
    "id": "2684",
    "category_name": "Boruto: Naruto Next Generations",
    "category_image": "7c4bf50b715509a963ce81b168ca674b.jpg"
  },
  { .... }
]
```

Nota: a quantidade de animes retornada pode mudar no futuro, mas neste momento o endpoint retorna 50 animes

Explicação da resposta:
```
id: ID do anime
category_name: título/nome do anime
category_image: nome único da imagem da capa do anime (pode retornar 404, a API é inconsistente)
```

Formato da resposta:
```ts
interface Detalhes {
  id: string,
  category_name: string,
  category_image: string,
}[]
```

**Limitações: A lista não é muito atualizada, não tem bastante precisão e não possui paginação**


<br>

### 10. Buscar o próximo episódio
Por exemplo, eu estou no episódio 12 de um Anime X, então para buscar os detalhes do próximo episódio chamo o endpoint abaixo enviando os dados: ID do episódio atual, ID do anime e a query param "next"
```
https://appanimeplus.tk/api-achance.php?episodios=ID_DO_EPISODIO_ATUAL&catid=ID_DO_ANIME&next
```

O retorno da requisição:
```js
[
  {
    "video_id": "8537",
    "category_id": "365",
    "title": "True Tears Episódio 13 Online",
    "location": "https://redirector.googlevideo.com/videoplayback?expire=1596694109&ei=3S0rX6XnGPqC2LYPrdGx4Ao&ip=149.56.143.221&id=63402cd052286e3e&itag=18&source=blogger&mh=5Y&mm=31&mn=sn-25ge7nse&ms=au&mv=u&mvi=1&pl=27&susc=bl&mime=video/mp4&dur=1455.426&lmt=1291592434035295&mt=1596665151&sparams=expire,ei,ip,id,itag,source,susc,mime,dur,lmt&sig=AOq0QJ8wRQIgQl6-CaRe9t0UlTH-4-pdzFfEi-1AzyjMrvC0fwbx4WwCIQCMBRYtxIdrGMNko7VCzU8MC3nFclAnTwbEoBDALCMPcA%3D%3D&lsparams=mh,mm,mn,ms,mv,mvi,pl&lsig=AG3C_xAwRQIgVoM2dM61uRyrfrssOY_TrHBD1BWWR0A-CeLIpivYfwgCIQCXBBQl0-BBR8FF7SKcGr6RKCtcfw8BSW0p16uHXeo3UQ%3D%3D",
    "locationsd": "https://redirector.googlevideo.com/videoplayback?expire=1586933343&ei=3z2WXtv3BJOqhwai2aHQBw&ip=149.56.143.221&id=433cf5d28647c5f2&itag=22&source=blogger&mh=Nw&mm=31&mn=sn-4g5e6nzl&ms=au&mv=u&mvi=1&pl=27&susc=bl&mime=video/mp4&dur=1455.194&lmt=1342396941052207&mt=1586904371&sparams=expire,ei,ip,id,itag,source,susc,mime,dur,lmt&sig=AJpPlLswRQIgPIjV32_P8chaUM7oM828NQcLWCdX-1wv7KeX-oRv25wCIQCGw031lOJ0s-FfnwMm2HHqhO-KelWjVmNnSg6P36dgcg%3D%3D&lsparams=mh,mm,mn,ms,mv,mvi,pl&lsig=ALrAebAwRgIhAMFXLvQChNCTcjQyoR4tg0cW-gi6s2siWaA83TAd3D0GAiEAqjmntzAmyuVy8uth9Ffg-Hp-Btk76sbBd-sWVbzGJkQ%3D"
  }
]
```

Explicação da resposta:
```
video_id: ID do episódio
category_id: ID do anime na qual o episódio pertence
location: URL do vídeo em qualidade normal
locationsd: URL do vídeo em qualidade HD (A maioria dos animes não possui esse campo, apenas os mais atuais entre +- de 2019 até agora)
```

Formato da resposta:
```ts
interface Stream {
  video_id: string,
  category_id: string,
  location: string,
  locationsd: string,
}[]
```


<br>

### 11. Buscar o episódio anterior
Por exemplo, eu estou no episódio 13 de um Anime X, então para buscar os detalhes do episódio anterior chamo o endpoint abaixo enviando os dados: ID do episódio atual, ID do anime e a query param "previous"
```
https://appanimeplus.tk/api-achance.php?episodios=ID_DO_EPISODIO_ATUAL&catid=ID_DO_ANIME&previous
```

O retorno da requisição:
```js
[
  {
    "video_id": "8537",
    "category_id": "365",
    "title": "True Tears Episódio 12 Online",
    "location": "https://redirector.googlevideo.com/videoplayback?expire=1596694109&ei=3S0rX6XnGPqC2LYPrdGx4Ao&ip=149.56.143.221&id=63402cd052286e3e&itag=18&source=blogger&mh=5Y&mm=31&mn=sn-25ge7nse&ms=au&mv=u&mvi=1&pl=27&susc=bl&mime=video/mp4&dur=1455.426&lmt=1291592434035295&mt=1596665151&sparams=expire,ei,ip,id,itag,source,susc,mime,dur,lmt&sig=AOq0QJ8wRQIgQl6-CaRe9t0UlTH-4-pdzFfEi-1AzyjMrvC0fwbx4WwCIQCMBRYtxIdrGMNko7VCzU8MC3nFclAnTwbEoBDALCMPcA%3D%3D&lsparams=mh,mm,mn,ms,mv,mvi,pl&lsig=AG3C_xAwRQIgVoM2dM61uRyrfrssOY_TrHBD1BWWR0A-CeLIpivYfwgCIQCXBBQl0-BBR8FF7SKcGr6RKCtcfw8BSW0p16uHXeo3UQ%3D%3D",
    "locationsd": "https://redirector.googlevideo.com/videoplayback?expire=1586933343&ei=3z2WXtv3BJOqhwai2aHQBw&ip=149.56.143.221&id=433cf5d28647c5f2&itag=22&source=blogger&mh=Nw&mm=31&mn=sn-4g5e6nzl&ms=au&mv=u&mvi=1&pl=27&susc=bl&mime=video/mp4&dur=1455.194&lmt=1342396941052207&mt=1586904371&sparams=expire,ei,ip,id,itag,source,susc,mime,dur,lmt&sig=AJpPlLswRQIgPIjV32_P8chaUM7oM828NQcLWCdX-1wv7KeX-oRv25wCIQCGw031lOJ0s-FfnwMm2HHqhO-KelWjVmNnSg6P36dgcg%3D%3D&lsparams=mh,mm,mn,ms,mv,mvi,pl&lsig=ALrAebAwRgIhAMFXLvQChNCTcjQyoR4tg0cW-gi6s2siWaA83TAd3D0GAiEAqjmntzAmyuVy8uth9Ffg-Hp-Btk76sbBd-sWVbzGJkQ%3D"
  }
]
```

Explicação da resposta:
```
video_id: ID do episódio
category_id: ID do anime na qual o episódio pertence
location: URL do vídeo em qualidade normal
locationsd: URL do vídeo em qualidade HD (A maioria dos animes não possui esse campo, apenas os mais atuais entre +- de 2019 até agora)
```

Formato da resposta:
```ts
interface Stream {
  video_id: string,
  category_id: string,
  location: string,
  locationsd: string,
}[]
```
