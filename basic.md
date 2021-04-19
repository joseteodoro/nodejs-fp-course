# JS básico

Vamos considerar 3 categorias no js: objetos, funções e módulos.

## Objetos

No js tudo é um objeto, alguns desses objetos sao executaveis (funcoes) outros possuem interpretacao e carga otimizadas para trabalhar com arquivos `*.js` (modulos).

Existem alguns tipos pré-definidos no js numeros inteiros, numeros reais, strings, booleanos, datas, arrays, buffers, objetos e funcoes. É dificil falar em primitivos, porque tudo no javascript sao objetos.

Segue a definição de alguns desses tipos.

```javascript
const tiposDeDados = () => { // isso é uma funcao sem parametro
    console.log(`Isso é um numero inteiro`, 1000)
    console.log(`Isso é um numero real`, 1000.1)
    console.log(`Isso é uma string`, 'banana')
    console.log(`Isso também é uma string`, "banana")
    console.log(`Isso é um boolean`, true)
    console.log(`Isso é um Date`, new Date())
    console.log(`Isso é um array`, [1, 2, 3, 4, 5])
    console.log(`Isso é um buffer`, Buffer.of('meu conteudo a ser bufferizado'))
    console.log(`Isso é um objeto com chaves e valores`, { id: 100, name: 'Joãozinho' })
    console.log(`Isso é uma função`, () => {})
}
```

Quando estamos fazendo o binding de objetos, podemos ter `let` e `const`. Antigamente existia o conceito de `var` que foi depreciado, mas na prática seria equivalente ao `let`. O `const` tem seu binding imutável (porém pode mudar seu conteúdo), já o `let` possibilita que voce refaça o binding mesmo que já tenha sido definido anteriormente.

```javascript
const variaveis = () => {
    let mutavel = 1 // isso pode mudar
    const imutavel = 10000 // isso nao pode mudar

    mutavel = 30 // isso funciona
    imutavel = 1111 // isso nao funciona e dá erro
}
```

O que acontece se eu não colocar nem `let` nem `var` nem `const` na frente do meu binding?

```javascript
    myVar = 1
    myVar = 1111
```

Isso é depreciado porque vai para o contexto global do js. Não cague no global porque isso pode causar memory leaks, conflitos de nomes, mantém os objetos desnecessáriamente durante toda a execução da aplicação e nem mesmo o GC consegue limpar esses caras porque eles possuem uma referencia no global.

### Todos contra o new

O js possibilita a criaçao de classes, construtores e afins. Porém, a maior fonte de erros em códigos concorrentes e paralelos é devida ao acesso compartilhado de memória. Se estamos falando de uma linguagem que é altamente assincrona, não bloqueante e concorrente, seria um tanto desaconselhado o uso de memória compartilhada. Por isso, em minha opinião, nao faz sentido usarmos essas estruturas de compartilhamento de memória num contexto que é altamente concorrente. 

Soma-se a isso o fato do js ser orientado a protótipos, não orientado a objetos. É parecido, mas é diferente. Você só vai perceber a diferença quando cair num gap que OO tem, mas que PO nao tem! :)

Por todos esses motivos, aqui não vamos aprender a fazer classes e construtores uma vez que vamos trabalhar voltados para free functions e um modelo funcional do js.

### Construindo objetos

No js construimos objetos declarando os nomes e valores dos atributos

```javascript
    const app = 'PotatoApp'
    const user0 = { name: 'banana', id: 'b1', app } // podemos atribuir valores explicitos ou objetos existentes
```

Podemos inclusive colocar funcoes como sendo um atributo

```javascript
    const print = function (nome) {
        console.log(nome)
    }

    const app = 'PotatoApp'
    const user0 = {
        name: 'banana',
        id: 'b1',
        app,
        print
        } // podemos atribuir valores explicitos ou objetos existentes, mesmo que sejam funcoes

    user0.print('rodando!') // invoca a funcao
```

O acesso aos valores podem ser feitos de dois modos

```javascript
    user0.print('rodando!') //acessamos diretamente pelo nome
    user0['print']('rodando!') //acessamos como sendo um dicionário / map
```

Ainda na definição de funções, podemos interpolar o conteúdo de uma variavel para que ela seja a chave do atributo que vamos construir

```javascript
    const role = 'admin'
    const app = 'PotatoApp'
    const user2 = {
        name: 'banana 2',
        id: 'b2',
        app,
        items: [1, 2, 3] //isso é um array
        [role]: true // isso nao é um array, é a interpolacao de um valor para a chave
        } // podemos atribuir valores explicitos ou objetos existentes, mesmo que sejam funcoes

    console.log(user2) // { name: 'banana 2', id: 'b2', app: 'PotatoApp', items: [1, 2, 3], admin: true }
```

## Funções

No js, funções não são apenas código executável, elas tambem são objetos que podemos executar, passar como parametro, inspecionar o conteudo, etc. Isso potencializa seu uso como uma linguagem funcional.

```javascript
const fn = function (arg1, arg2, arg3, arg4) { // funcao com binding usando const
    const allArgs = arg1 + arg2 + arg3 + arg4 // somando itens
    return `${allArgs}` //retornando valores da funcao
}

fn() // funcao sendo invocada
```

Existe como declarar uma função sem fazer o binding com `const`, isso é equivalente ao const, porém costuma-se colocar `const` na frente de tudo para deixar o código mais homogênio.

```javascript
function fn (arg1, arg2, arg3, arg4) { // funcao com binding sem const
    const allArgs = arg1 + arg2 + arg3 + arg4 // somando itens
    return `${allArgs}` //retornando valores da funcao
}

fn() // funcao sendo invocada
```

Veja que sempre damos um nome para as funções. Se você definir a função com a primeira abordagem, tenha certeza de dar um nome par aa função, caso contrário, quando acontecer uma exceção o js explode uma stacktrace com um monte de funções anônimas e vai ficar dificil fazer o trace de alguns problemas.

```javascript

const fnSemNome = function (arg1, arg2, arg3, arg4) { // ta sem nome a funcao!
    const allArgs = arg1 + arg2 + arg3 + arg4 
    return `${allArgs}`
}

const fnComNome = function nomeDaFnMostradoNaStack (arg1, arg2, arg3, arg4) { // agora eu dou um nome para a funcao!
    const allArgs = arg1 + arg2 + arg3 + arg4 
    return `${allArgs}`
}

function essaFuncaoTbTemNome (arg1, arg2, arg3, arg4) {
    const allArgs = arg1 + arg2 + arg3 + arg4 
    return `${allArgs}`
}

```

Quando usamos `const` ou `let` e arrow functions o js resolve isso para gente e não precisamos ficar dando os nomes de funções.

Ainda com relação a definição de funções, com o advento das arrow functions (definição encurtada das funções podemos simplificar a sua criação)

```javascript
const fn = () => { //
    // funcao sendo declarada com arrow function
    return { status: 200, response: 'banana' } // declarei o objeto na hora de retornar a funcao e ele que volta como resposta da invocacao da funcao
}

fn() // funcao sendo invocada
```

Podemos simplificar ainda mais a função acima porque ela não precisa de um corpo com chaves, uma vez que só tem um `return` dentro. Os parenteses são necessários por causa do uso de `{}` que definem um novo escopo.

```javascript
const fn = () => ({ status: 200, response: 'banana' }) // funciona do mesmo jeito que a definicao anterior

fn() // funcao sendo invocada
```

Segue um exemplo sem os parenteses (porque são desnecessários)

```javascript
const fn = () => fetch.get('https://google.com.br')

fn() // funcao sendo invocada fazendo um get para o google
```

As chaves são necessárias quando uma função tem um corpo com mais de uma linha. Caso contrário, podemos deixar inline e omitir as chaves.

Podemos ainda criar facilmente funções que retornam uma função

```javascript
const fn = (url) => {
    return () => {
        return fetch.get(url)
    }
}

const interna = fn('https://google.com.br') // extrai a funcao
fn('https://google.com.br')() // extrai e ja executa a funcao
```

Mais enxuto

```javascript
const fn = (url) => () => fetch.get(url)

const interna = fn('https://google.com.br') // extrai a funcao
fn('https://google.com.br')() // extrai e ja executa a funcao
```

Mas por que demonios eu ia fazer algo assim?

Suponha que temos uma lista de urls que vamos invocar uma funcao para modificar a url antes de fazer o get, mas essa função nao é definida por nós, ela vem como parametro pra gente. Nao sabemos o que vem dai, apenas que o restorno dessa funcao é uma string que faz algum saneamento nas nossas urls

```javascript

const getAll = (sanitizer, urls) => {
    return urls.map( url => sanitizer(url))
        .map(sane => fetch.get(sane))
}

// a invocacao da nossa funcao pode fazer

const urls = [
    'https://google.com.br',
    'https://facebook.com.br',
    'https://microsoft.com.br',
    'https://dasa.com.br',
]

const sanitize = (url) => `${url}/index.html`
getAll(sanitize, urls)
```

Podemos reescrever nossa funcao getAll para

```javascript
const getAll = (sanitizer) => (urls) => {
    return urls.map(url => fetch.get(sanitizer(sane)))
}

// a invocacao da nossa funcao pode fazer

const urls = [
    'https://google.com.br',
    'https://facebook.com.br',
    'https://microsoft.com.br',
    'https://dasa.com.br',
]

const sanitize = (url) => `${url}/index.html`
getAll(sanitize)(urls)
```

Porque se tivermos varios conjuntos de urls, com os mesmos sanitizers, podemos só fazer um map

```javascript
const urls = [
    [
        'https://facebook.com.br',
        'https://microsoft.com.br',
        'https://dasa.com.br',
    ],
    [
        'https://gmail.google.com.br',
        'https://calendar.google.com.br',
        'https://google.com.br',
    ],
]

const sanitize = (url) => `${url}/index.html`

urls.map(getAll(sanitize)) // faz o map do array de arrays empurrando cada item (subarray) para dentro de nossa funcao que roda os sanitizers pra cada subarray desses
```

Podemos usar essa estratégia para fazer o setup de dependencias que serao encapsuladas dentro de uma função sem que o caller da função saiba o que tem ali.

```javascript
// modulo de repository de user

const save = (connection) => (user) => connection.save(user)

const load = (connection) => (id) => connection.load(id)

// outro modulo faz o setup da connection dentro do nosso repository

module.exports = { // isso exporta o modulo ja embutindo a dependencia do banco
    save: save(sqlite),
    load: load(sqlite)
}

// quem usa chama sem saber que ta carregado o sqlite la dentro

const users = require('./repository/users')

users.save({id: 1, name: 'banana 1'})

users.load(1)
```

## Módulos

Modulos sao bibliotecas que voce adiciona pelo npm (e que vem do repositorio global [npmjs](https://www.npmjs.com/)).

Voce importa esses modulos dentro do seu arquivo js com o comando `import` ou `require` (seus arquivos locais tambem podem ser considerados módulos).

```javascript
const request = require('node-fetch') // importo uma biblioteca
const meuModulo = require('./meu-modulo') // importo um modulo meu
const modulos = require('./diretorio-de-modulos/modulos.json') // importo um arquivo / modulo de outro diretorio (seja js ou json)
const raizDoDiretorioModulos = require('./diretorio-de-modulos') // isso é equivalente a importar o arquivo /index dentro daquele diretorio
```

Uma vez importados, você trabalha como se esses modulos fossem objetos js que possuem as funções exportadas desses módulos.

```javascript
const invocandoFuncoesDoMeuModulo = () => {
    meuModulo.funcao('meu parametro')
}
```

Mas como é que eu exporto isso lá no meu arquivo js?

```javascript
// arquivo ./candango.js
const minhaFuncaoLinda = (nomeDoCandango) => {
    console.log('Olha o nome do candango:', nomeDoCandango)
}

module.exports = { minhaFuncaoLinda }

// arquivo app.js

const cdg = require('./candango.js')

cdg.minhaFuncaoLinda('Joselito')

// isso invoca a funcao do meu módulo e cospe no console a mensagem 'Olha o nome do candango: Joselito'

```

O comando `require` faz o cache dos modulos usando singletons e tem carregamento sincrono. Assim ele otimiza a carga e interpretacao do js dos modulos importados na primeira vez que forem importados e nas demais importacoes ele só carrega do cache.

## Atribuição da valores, binding, unboxing e spread

Quando falamos de atribuição de valres podemos fazer binding direto, unboxing ou mesmo spread. A atribuição de valores é equivalente ao que fazemos em java, python, etc.

```javascript
    const cliente = {id: 1, nome: 'Joselito'} // binding direto como fazemos em outras linguagens
    const app = 'Potato' // binding direto como fazemos em outras linguagens de novo
    const { nome } = cliente // eu estou fazendo binding com unboxing do atributo `nome de dentro do cliente`
    cliente.niver = new Date('1990-10-01') // aqui eu adiciono um novo campo no cliente
    const { niver, id } = cliente // aqui eu consigo fazer o unboxing de niver e id
    const { id } = cliente // isso aqui dá pau, porque já fizemos o binding do id
```

O unboxing explode os atributos de um objeto para dentro da definição. Conseguimos reduzir o número de linhas para atribuir valores para o contexto local

```javascript
    const { niver, id } = cliente // aqui eu consigo fazer o unboxing de niver e id
    const id = cliente.id // faz binding do id
    const niver = cliente.niver // faz binding do niver
```

O unboxing funciona inclusive na definição de funções

```javascript
const printId = (cliente) => {
    console.log(cliente.id)
}
```

É equivalente á

```javascript
const printId = ({ id }) => {
    console.log(id)
}
```

Melhor ainda

```javascript
const printId = ({ id }) => console.log(id)
```

No caso do `spread` nós literalmente explodimos os valores do objeto para dentro de outro (fazendo ou nao merge de dados)

```javascript
    const potato = 'PotatoApp'
    const cookie = 'CookieApp'
    const user0PotatoApp = { name: 'banana', id: 'b1', app: potato }
    const user0CookieApp = { name: 'banana', id: 'b1', app: cookie }

    // faz a mesma coisa que
    const potato = 'PotatoApp'
    const cookie = 'CookieApp'
    const user0 = { name: 'banana', id: 'b1' }
    const user0PotatoApp = { ...user0, app: potato }
    const user0CookieApp = { ...user0, app: cookie }

    // ou ainda
    const potato = 'PotatoApp'
    const cookie = 'CookieApp'
    const user0PotatoApp = { name: 'banana', id: 'b1', app: potato }
    const user0CookieApp = { ...user0PotatoApp, app: cookie } // os valores mais a direita sobreescrevem chaves que ja existam

    const { id, ...rest } = user0PotatoApp // id = b1 e rest = { name: 'banana', app: 'PotatoApp' }

    // tambem funcionam com arrays
    const src = [1, 3, 5, 7, 9]
    const [head, ...tail] = src // head = 1 e tail = [3, 5, 7, 9]
    const zeroBased = [0, ...src] // zeroBased = [ 0, 1, 3, 5, 7, 9 ]
```

## Built-in functions

Uma boa fonte para procurar sobre o que temos ou nao dentro no basico do js é o site do mdn da mozilla sobre javascript [link](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

Lá podemos encontrar as funcoes de praticamente todos os objetos que funcionam no javascript além de seus usos com exemplos.

Por exemplo, a descricao e exemplo do uso de find de um array [link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)

O que vamos usar na quase totalidade do nosso dia-a-dia? A documentação de Promise, Array, String, Object, JSON, RegExp e Math. O restante só se precisar, mas é raro.

Por ordem de uso, com os itens em negrito da lista voce ja entende / conhece quase 90% dos codigos em nodejs. Com todos os itens voce ja conhece mais de 90%.

**Promise.resolve**
**Promise.reject**
**Promise.all**
**Promise.then**
**Promise.catch**

**array.length**
**array.map**
**array.filter**
**array.find**
**array.reduce**
array.pop
array.push
array.concat
array.shift
array.reverse

**string.replace**
**string.toLowerCase**
**string.toUpperCase**
**string.match**
string.includes
string.startsWith
string.trim

**Object.keys**
**Object.assign**
**Object.values**

**JSON.parse**
**JSON.stringify**

**regex.compile**
**regex.test**
**regex.exec**