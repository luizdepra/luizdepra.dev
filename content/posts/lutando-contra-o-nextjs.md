---
title: "Lutando contra o Next.js"
date: 2023-01-29
slug: "lutando-contra-on-extjs"
tags: ["next.js", "front-end"]
categories: ["front-end"]
draft: false
---

… e perdendo miseravelmente. Ou como eu tentei lidar com variáveis de ambiente em client e server-side no Next.js num ambiente dockerizado.

Estou saindo da minha zona de conforto no trabalho, e isso é bom, eu admito. Uma dessas oportunidades é poder contribuir com um projeto front-end feito com TypeScript e Next.js. Confesso que esse mundo é esquisito para alguém que a quase 10 anos praticamente só trabalhou com back-end. Fora toda a pluralidade de opções de frameworks e ferramentas, tudo parece exageradamente complicado.

Certo, vamos ao que interessa.

## O projeto

Esse projeto é uma interface "burra", mas isso não é nada ruim. :laugh: Explicando melhor, o projeto é uma UI sem lógicas de negócio ou outras complexidades que não sejam validações e transformações de dados. Ele tem um projeto irmão que chamamos de BFF (Back-end for Front-end), feito em Python com FastAPI, que abstrai as lógicas pesadas e faz acesso às APIs internas.

Temos 3 ambientes onde o projeto é executado: integração, pre-produção e produção. Variáveis de ambiente são interessantes para esse projeto, pois podemos configurá-lo conforme o local em que ele vai ser disponibilizado. Cada ambiente é um cluster Kubernetes separado, então criamos imagens Docker do projeto para cada versão do projeto.

Todas essas características me fizeram pensar que seria ideal:

- Usar e abusar de variáveis de ambiente para configurar o projeto.
- Ter apenas uma imagem Docker e usá-la em todos os ambientes.

## Como o Next.js lida com variáveis de ambiente?

O Next.js tem suporte a variáveis de ambiente, e inclusive sabe ler nativamente os arquivos `.env`[^1]. Mas tem um detalhe aqui. Existem variáveis que são expostas para o navegador e outras que não. A sinalização é simples, se a variável começa com `NEXT_PUBLIC_` ela será acessível no client-side, caso contrário, só no server-side.

O framework tem 2 formas de renderização de páginas: estática, que roda no browser, e as páginas são renderizadas no processo de geração (build-time) quando rodamos o `next build`; e sobre demanda, que roda no servidor, gerada no momento em que são feitas as requests (SSR, ou Server Side Rendered)[^2]. Então, as variáveis `NEXT_PUBLIC_` ficam acessíveis em ambos os cenários, e as normais apenas na parte SSR.

## Tudo certo! Ou será que não?

Temos um problema. As variáveis de ambiente acessíveis pelo browser são injetadas no momento da construção do projeto. Não é possível lê-las em tempo de execução. Mas, tudo bem. Basta configurar as variáveis no momento do build e tudo funcionará. Mas não é bem assim. Lembra da imagem Docker que deveria ser única? O processo de build é feito durante a construção das imagens, e eu precisaria configurar as variáveis nesse momento. Ok, para isso existem os `ARG`s[^3]. Porém, isso me forçaria a ter uma imagem por ambiente, e eu queria evitar isso.

Explorei outras opções. Uma delas seria usar Runtime Configuration, que caia exatamente no mesmo problema de usar env vars diretamente, e ainda é considerada uma funcionalidade legada[^4]. Outra opção é usar `getServerSideProps`[^5] para passar as variáveis de ambiente em tempo de execução para o browser. O ruim dessa estratégia é que toda página que precise usar os valores será obrigada a ser renderizada no servidor. Ou seja, perdemos as páginas estáticas[^6].

Essa solução funcionou bem até começarmos a configurar o Sentry no projeto[^7].

## O caso Sentry

Para que o Sentry opere corretamente, ele precisa ser configurado tanto no client-side quando no server-side. E para isso ele precisa encapsular todo o código do projeto no nível mais externo possível. Ou seja, para o código que roda no navegador, a configuração do Sentry precisa entrar no processo de build. Abusar de SSR não vai funcionar nesse caso.

Tentamos configurar o Sentry apenas nas páginas de erro, mas existem alguns erros de framework e outros gerados por requests que não passam por essas páginas. Isso limitaria muito a quantidade de erros que poderíamos capturar. No deal.

## Conclusão

Pesquisamos várias opções e nenhuma se encaixava com o nosso cenário. Pedi ajuda para alguns amigos mais experientes em front-end, mas ninguém tinha trabalhado com requisitos semelhantes aos que eu tinha. Então desistimos. **O framework venceu!** Optamos por manter uma imagem Docker por ambiente, e paciência. Infelizmente a necessidade de rodar código gerado e empacotado no browser não proporciona o dinamismo de configuração da mesma que executando no servidor.

Lição aprendida! ~~Nunca mais vou usar frameworks de front-end e vou renderizar tudo no back-end. Talvez com Django...~~

[^1]: [Variáveis de Ambiente no Next.js](https://nextjs.org/docs/basic-features/environment-variables).
[^2]: [Renderização de páginas no Next.js](https://nextjs.org/docs/basic-features/pages).
[^3]: [`ARG` no Dockerfile](https://docs.docker.com/engine/reference/builder/#arg).
[^4]: [Runtime Configuration no Next.js](https://nextjs.org/docs/api-reference/next.config.js/runtime-configuration).
[^5]: [Documentação do `getServerSideProps`](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props).
[^6]: Existem algumas páginas de erro que são obrigatoriamente estáticas e por isso não conseguimos usar o `getServerSideProps` nelas. Isso pode ser um problema.
[^7]: Sentry é uma ferramenta que captura erros e várias informações relacionadas que nos ajudam a investigar o porquê desses erros aconteceram.
