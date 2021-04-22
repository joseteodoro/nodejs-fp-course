# Biblitecas comuns

Existem dois modos de voce adicionar dependencias no seu projeto.

- Voce pode adicionar do site do npm [site oficial](https://www.npmjs.com/)
- Voce pode adicionar um repositorio git diretamente do projeto (indicando ou nao a branch)

## NPM

No site do npm voce pode usar a full text search para localizar modulos relacionados com o que voce precisa.

[express]: https://github.com/joseteodoro/nodejs-fp-course/raw/main/bin/search-for-express.gif "Buscando pelo express"

Na tela de descrição do modulo, no lado esquerda ele te mostra quantos downloads foram feitos daquele modulo na última semana. Isso serve de indicativo para saber quais sao modulos mais usados na comunidade e quais nao sao muito utilizados. Modulos mais usados tendem a ser mais maduros e ter mais suporte (respostas no stack overflow) caso voce encontre erros.

Depois de localizado o modulo que voce precisa, voce manda instalar ele e salvar a configuracao da dependencia dentro do seu projeto.

```shell
    npm install express --save
```

ou salvar dentro das dependencias de desenvolvimento (que rodam apenas para desenvolvimento e testes)

```
    npm install mocha --save-dev
```

## GIT

Voce pode adicionar um repositorio do git do mesmo modo como voce adiciona um modulo do npm.

```
    npm install git+ssh://git@github.com/visionmedia/express.git
 --save
```

No caso de uma branch especifica

```
    npm install git+ssh://git@github.com/visionmedia/express.git#main
 --save
```

Por que eu faria isso? Porque voce nao precisa publicar seu codigo no nexus para usar isso como um modulo de outros projetos, muito menos publicar no npm se seu codigo for proprietário e voce nao quiser entrar na burocracia de enviar coisas para o nexus.

## Biblitecas muito usadas

- mocha [link](https://www.npmjs.com/package/mocha): engine de testes;

- sinon [link](https://www.npmjs.com/package/sinon): mock e stubs para testes de integracao;

- chai [link](https://www.npmjs.com/package/chai): assertions dos seus testes;

- sequelize [link](https://www.npmjs.com/package/sequelize): ORM a lá hibernate para o nodejs;

- express [link](https://www.npmjs.com/package/express): servidor web para criaçao de APIs e templates HTML simples;
- koa [link](https://www.npmjs.com/package/koa): irmao e concorrente do express;

pino [link](https://www.npmjs.com/package/pino): logger para nodejs com integracao simples com express e koa;

morgan [link](https://www.npmjs.com/package/morgan): irmao e concorrente do pino, tem uma formacao melhor, apesar de ser um pouco mais chato de configurar;

moment-timezone [link](https://www.npmjs.com/package/moment-timezone): lib para trabalhar com datas e time-zones. O moment-js foi descontinuado porque a api de date do nodejs foi completamente reescrita, porém ainda nao confio na api de date do v8. Por isso continuo usando o moment;

eslint [link](https://www.npmjs.com/package/eslint): verificacao de estilo de código, bad smells e problemas de código. Integra com o vscode e vai apontando para voce onde temos problemas, desde possiveis excecoes por algo estar null até problemas de complexidade de funções;


nyc [link](https://www.npmjs.com/package/nyc): equivalente ao jacoco do java. Calcula coverage e pode ser integrado ao vscode para pintar as linhas que nao estao cobertas pelos testes;

sonar-scanner [link](https://www.npmjs.com/package/sonar-scanner): plugin do sonar que funciona com a simples adição de um arquivo de config na raiz do projeto;

supertest [link](https://www.npmjs.com/package/supertest): consegue subir sua aplicação em memoria para rodar os seus testes de integração. Não abre portas da maquina e afins, assim você pode rodar varios testes juntos sem se preocupar com os conflitos; Tambem deixa voce trabalhar com mocks nas dependencias;

ramda [link](https://www.npmjs.com/package/ramda): lib com utilidades de FP (functional programming) que ajudam e simplificam muito o código, apesar de necessitar de algum tempo para se acostumar com as funções da lib, a documentação é bem completa.

