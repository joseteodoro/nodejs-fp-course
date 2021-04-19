# Event-loop

Event loop é uma estratégia para usar melhor o processador. Voce cria uma lista de pequenos executaveis, coloca essa lista em um while true que vai consumindo esses eventos e realizando as operacoes desses executaveis.

O nodejs roda enquanto houverem entradas nessa lista de executaveis (leia-se executaveis como sendo suas funcoes)

Como consequencia, temos que o nodejs executa as coisas de modo concorrente. Isso nao significa que roda em paralelo (porque o node é monothread), mas sim que voce nao sabe exatamente QUANDO seu codigo será executado.

Contudo, podemos garantir a ordem em que as coisas executam. Fazemos isso usando promises com as funções `then` e `reject`.

[Como funciona o event loop](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/#what-is-the-event-loop)