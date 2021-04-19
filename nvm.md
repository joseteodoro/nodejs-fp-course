# NVM

NVM é o gerenciador de versões do node, tal como o SDK man para o java.

A instalação é bem tranquila: [site oficial](https://github.com/nvm-sh/nvm)

Depois de instaldo voce pode listar as versoes do node com o comando

```bash
    nvm list
```

Pode instalar versoes

```bash
    nvm install lts/fermium
```

Pode usar versoes ja instaladas

```bash
    nvm use lts/fermium
```

Ou ate mesmo pode criar um arquivo `.nvmsrc` no root do seu projeto e disparar o comando

```bash
    nvm use
```

que ele carrega o node especifico daquele arquivo.