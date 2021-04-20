# Event-loop

Event loop é uma estratégia para usar melhor o processador. Voce cria uma lista de pequenos executaveis, coloca essa lista em um while true que vai consumindo esses eventos e realizando as operacoes desses executaveis.

O nodejs roda enquanto houverem entradas nessa lista de executaveis (leia-se executaveis como sendo suas funcoes)

Como consequencia, temos que o nodejs executa as coisas de modo concorrente. Isso nao significa que roda em paralelo (porque o node é monothread), significa que voce nao sabe exatamente QUANDO seu codigo será executado.

Contudo, podemos garantir a ordem em que as coisas executam. Fazemos isso usando promises com as funções `then` e `reject`. Desse jeito podemos definir, por exemplo, que uma busca no banco será realizada apenas depois de incluir alguns itens no banco do dados.

```javascript
const listAllItems = (userId) => () => datasource.listAllItems(userId)

const saveItem = (item) => () => datasource.save(item)

const formatItems = (items) => items.map(format)

const saveAndListAll = (userId, item) => Promise.resolve()
    .then(saveItem(item))
    .then(listAllItems(userId))
    .then(formatItems)
```

No exemplo acima, não sabemos exatamente quando `listAllItems`, `saveItem` e `formatItems`. Porém, estamos garantindo que `listAllItems` só será executada depois do retorno da função `saveItem` e que o output da função `listAllItems` será enviada como parâmetro de entrada para a função `formatItems`.

Esse uso do processador por pequenas tarefas possibilita que o nodejs performe melhor que outras linguagens usando um unico processador. Só precisamos lembrar de deixar essas funcoes como sendo pequenos trechos de codigo. Por exemplo, se fizermos um forEach numa lista muito grande vamos travar o processamento nessa lista ate que tudo esteja pronto, pra só então liberar o recurso pra que o event loop execute a proxima função. Por este motivo é desaconselhavel usar laços `for`/`while` em favor de `map`/`reduce`, uma vez que map e reduce ficam distribuidos entre varias invocacoes de funcoes menores e nao bloqueam o processador.
A palavra correta aqui é blocking e non-blocking. `For`/`while` sao blocking, `map`/`reduce` sao non-blocking.

[Como funciona o event loop](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/#what-is-the-event-loop)