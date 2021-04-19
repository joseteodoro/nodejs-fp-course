Modulos sao bibliotecas que voce adiciona pelo npm (e que vem do repositorio global [npmjs](https://www.npmjs.com/)).

Voce importa esses modulos dentro do seu arquivo js com o comando `import` ou `require` (seus arquivos locais tambem podem ser considerados módulos).

```javascript
const request = require('node-fetch') // importo uma biblioteca
const meuModulo = require('./meu-modulo') // importo um modulo meu
const modulos = require('./diretorio-de-modulos/modulos.json') // importo um arquivo / modulo de outro diretorio (seja js ou json)
const raizDoDiretorioModulos = require('./diretorio-de-modulos') // isso é equivalente a importar o arquivo /index dentro daquele diretorio
```

```javascript
const tiposDeDados = () => { // isso é uma funcao sem parametro
    console.log(`Isso é um numero`, 1000)
    console.log(`Isso é uma string`, 'banana')
    console.log(`Isso também é uma string`, "banana")
    console.log(`Isso é um boolean`, true)
    console.log(`Isso é um objeto`, { id: 100, name: 'Joãozinho' })
    console.log(`Isso é uma função`, () => {})
}
```

```javascript
const variaveis = () => {
    let mutavel = 1 // isso pode mudar
    const imutavel = 10000 // isso nao pode mudar

    mutavel = 30 // isso funciona
    imutavel = 1111 // isso nao funciona e dá erro
}
```

```javascript
const invocandoFuncoesDoMeuModulo = () => {
    meuModulo.funcao('meu parametro')
}
```

```javascript
const atribuindoValores = () => {
    const nome = modulos[0].name //acesso o nome dentro do primeiro item do array que tenho no JSON
    const { name, id } = modulos[0] // mesma coisa da linha de cima, mas aqui eu ja faço unbox na hora que bato no objeto do array
}
```

```javascript
const construindoObjetos = (userName, userId) => {
    const user1 = { name: userName, id: userId } // assim eu declaro as chaves
    const user2 = { userName, userId } // assim eu declaro o objeto como tendo o mesmo nome das variaveis que entraram
}
```

```javascript
const fn = () => {
    // funcao sendo declarada
    return { status: 200, response: 'banana' } // declarei o objeto na hora de retornar a funcao e ele que volta como resposta da invocacao da funcao
}

fn() // funcao sendo invocada
```