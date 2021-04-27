# NPM

NPM é o gerenciador de pacote de projetos javascript. Existem algumas alternativas como yarn (originalmente do facebook), mas o npm é o mais usado atualmente e dele deriva o site que mantém os pacotes. Leia-se o npm como sendo o maven para o nodejs, o yarn seria o gradle.

O NPM ja vem quando voce instala o nodejs por padrao, ao contrario do NVM.

Na prática tudo vem do site npm. Quando voce da os comandos de instalacao de modulo, execucao de scripts, init da aplicacao etc , voce está mandando o npm rodar utilizando as configuracoes existentes no package.json. Praticamente todas as configs de libs, modulos e mesmo do seu projeto ficam ali no package.json. Leia-se o package.json como sendo seu pom.xml.

Para iniciar um projeto do zero, voce dispara o comando

```
npm init
```

Ele vai te perguntar coisas como nome do projeto, licensa, versao, etc. A partir disso ele cria um package.json na raiz do seu projeto e utiliza esse arquivo para adicionar tudo que for necessario para seu projeto buildar e rodar.

Os comandos mais utilizados do npm são:

## Criacao de projetos

Para criar um novo projeto:

```
npm init
```

## Instalar libs e sua aplicacao

Para instalar uma lib (sem adicionar ela no package.json, só instala mesmo):
```
npm install <nome-da-lib>

// ou

npm i <nome-da-lib>
```

Com opção para salvar a lib no seu package:

```
npm i <nome-da-lib> --save // para salvar

// ou

npm i <nome-da-lib> --save-dev // para salvar somente para desenvolvimento
```

Coisas como lib de teste, lint, sonar, etc podem ser adicionadas apenas para desenvolvimento. Assim nao suja o pacote final utilizado em producao.

Ainda na instalacao de modulos, voce pode instalar um modulo a partir de um repositorio `git` qualquer, como por exemplo:

```
npm install git+ssh://git@github.com/visionmedia/express.git#main
 --save
```

Esse comando instala o express na sua aplicacao, a partir da branch `main` do git existente no github.

Outro uso do comando install é quando voce acabou de clonar um projeto existente, ou mesmo se atualizou manualmente o package.json usamos a mesma opção de `install`, mas sem nenhum parametro na frente:

```
npm install

// ou

npm i
```

A versão de produçao pode ser instalada com:

```
npm install --only=prod //ou --only=production
```

Essa opção deve ser usada para buildar a versao que voce coloca no docker ou sobe no seu servidor. Uma vez que ela nao instala nada do que tenha sido colocado como lib de desenvolvimento.

Note que para que voce rode sua aplicacao, voce primeiro precisa executar o comando

```
npm i // ou npm install
```

para que voce tenha todas as dependencias dentro do seu projeto. Uma vez que voce instala essas dependencias elas vao parar num diretório chamado `node_modules` na raiz do seu projeto. Note que é possivel que uma lib especifica que voce instala (geralmente libs que compilam codigo C nativo como driver de banco, etc) podem ficar atreladas a uma versao especifica do nodejs que voce esta usando. Por isso, se voce roda `npm i` com o node 8.x por exemplo, na sequencia voce troca para o node 11.x e tenta executar o comando `npm start`, é natural que aconteçam erros por conta dessas libs que sao especificas de uma versao do node. Neste caso, voce pode apagar completamente o diretorio `node_modules`, re-executar o `npm i` e depois rodar o `npm start`.

Para voce rodar os testes, sonar, lint, etc voce tambem precisa ter rodado um `npm i` antes.

## Rodar scripts

O NPM roda os scripts existentes dentro do campo `scripts` do package.json com o seguinte comando:

```
npm run meuScript
```

Para que isso funcione, seu package.json precisa ter esse script definido no campo `scripts`, por exemplo no package.json abaixo:

```json
{
  "name": "handson",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "author": "",
  "license": "ISC",
  "scripts": {
    "main": "src/index.js",
    "test": "mocha --require \"tests/**/*@(.spec.js)\" --timeout 5000 --exit;",
    "lint": "eslint ./tests ./src --ext .js --fix",
    "setup-db": "node src/setup-db.js"
  },
  "dependencies": {
    "amqplib": "^0.7.0",
    "sqlite3": "^4.2.0",
    "uuid": "^3.3.2"
  },
  "devDependencies": {
    "chai": "^4.3.0",
    "eslint": "^4.19.1",
    "eslint-config-standard": "^11.0.0",
    "eslint-plugin-import": "^2.11.0",
    "eslint-plugin-json": "^1.2.0",
    "eslint-plugin-node": "^6.0.1",
    "eslint-plugin-promise": "^3.7.0",
    "eslint-plugin-standard": "^3.0.1",
    "mocha": "^8.2.1",
    "sinon": "^6.1.4",
    "sinon-chai": "^3.5.0"
  }
}
```

Para rodarmos o o script `lint`, invocamos o npm com

```
npm run lint
```

Mesmo se dá para o script de setup do banco de dados:

```
npm run setup-db
```

Existem atalhos para os scripts de `start` e `test`. Na pratica se voce chamar o comando de `start` ele invoca o script `start` ou o script `main`. Já no caso do script `test` voce pode abreviar sua invocacao usando

```
npm t // ou npm run test
```

Todo e qualquer script deve sair da raiz do projeto (mesmo path do package.json). Por exempo, no nosso json acima temos `"main": "src/index.js",`. Na nossa estrutura de diretorios temos a seguinte estrutura de diretorios:

```
.
├── package.json
├── src
│   ├── HealthCheck.js
│   ├── index.json
│   └── setup-db.js
└── tests
    ├── health-check.spec.js
    ├── unused.js
```

Mesmo se dá para o script `setup-db` e para os testes `"test": "mocha --require \"tests/**/*@(.spec.js)\" --timeout 5000 --exit;"`.

No caso dos testes, estamos usando um modulo `mocha` para rodar nossos testes e estamos dizendo para ele carregar todos os arquivos de dentro da pasta `./tests` que possuam a extensao `*.spec.js`. Veja que dentro da pasta teste tambem temos um arquivo de nome `unused.js`. Esse arquivo nao é lido pelo mocha porque não dá match com a extensão `*.spec.js`.

Modulos como `nyc` (coverage), utilizam o package.json para configurar seu comportamento. Por exemplo, no package.json abaixo:

```json
{
  "name": "handson",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "author": "",
  "license": "ISC",
  "scripts": {
    "main": "src/index.js",
    "test": "mocha --require \"tests/**/*@(.spec.js)\" --timeout 5000 --exit;",
    "test:cover": "nyc npm test"
  },
  "nyc": {
    "all": true,
    "instrument": true,
    "extension": [
      "js"
    ],
    "reporter": [
      "lcov",
      "text"
    ],
    "exclude": [
      "tests/**/*",
      "out/**/*",
      "lcov-report/**/*",
      ".scannerwork/**/*",
      "coverage"
    ]
  },
  "dependencies": {
    "sqlite3": "^4.2.0"
  },
  "devDependencies": {
    "chai": "^4.3.0",
    "mocha": "^8.2.1",
    "nyc": "^14.1.1",
    "sinon": "^6.1.4",
    "sinon-chai": "^3.5.0"
  }
}
```

Libs como o eslint, sonar, etc, utilizam arquivos na raiz do projeto num arquivo especifico para nao mexerem no package.json.
