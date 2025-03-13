
Módulos no Node.js: No Node.js, os módulos são pedaços de código que podem ser reutilizados em diferentes partes de um projeto. É como dividir seu código em pequenas "caixinhas" especializadas em fazer algo específico. O Node.js vem com módulos embutidos (como o fs para manipular arquivos e o http para criar servidores web), mas você também pode criar seus próprios módulos ou usar módulos de terceiros.

NPM (Node Package Manager): É o gerenciador de pacotes do Node.js.. Ele facilita o download, instalação, gerenciamento e compartilhamento de bibliotecas e ferramentas feitas por outros desenvolvedores. Com o NPM, você pode adicionar funcionalidades ao seu projeto sem ter que codificar tudo do zero. Por exemplo, se você precisa de uma biblioteca para lidar com datas, pode usar o famoso pacote moment via NPM.

# 1.1 Entenda o sistema de módulos do Node js

O Node.js utiliza um sistema de módulos para organizar e reutilizar código. Isso significa que você pode dividir sua aplicação em pequenos pedaços, cada um com uma funcionalidade específica, em vez de colocar tudo em um único arquivo. Aqui estão os principais pontos sobre como ele funciona:

### Módulos Nativos
O Node.js já vem com vários módulos prontos que você pode usar imediatamente, como:
- fs (File System): Para manipular arquivos.
- http: Para criar servidores web.
- path: Para lidar com caminhos de arquivos e diretórios.
- Para usar um módulo nativo, você precisa "requisitá-lo" no seu código usando o método require. 
Exemplo:
```
const fs = require('fs');
```

### Módulos Criados por Você
Você pode criar seus próprios módulos para organizar melhor o código. Para isso:
- Escreva o código que quer modularizar em um arquivo separado.
- Exporte as funções ou variáveis que deseja compartilhar.
- Importe esse módulo onde quiser usá-lo.
Exemplo de módulo (arquivo meuModulo.js):
```
function saudacao(nome) {
    return `Olá, ${nome}!`;
}

module.exports = saudacao;
```

Importando o módulo:
```
const saudacao = require('./meuModulo');
console.log(saudacao('Diego')); // Saída: Olá, Diego!
```

### Módulos de Terceiros (via NPM)
Você também pode usar pacotes criados por outros desenvolvedores disponíveis no repositório NPM. Esses módulos ajudam a adicionar funcionalidades prontas ao seu projeto.
Exemplo:
```
npm install lodash
```

No seu código:
```
const _ = require('lodash');
console.log(_.capitalize('ola, diego!')); // Saída: Ola, diego!
```

### Sistema de Organização
O Node.js usa o padrão CommonJS, onde os módulos são gerenciados por meio do require e do module.exports.

Resumidamente, o sistema de módulos do Node.js oferece flexibilidade para organizar e compartilhar código, seja usando os módulos internos, criando os seus próprios ou integrando bibliotecas de terceiros.


# 1.2 Uso do Node Package Manager (NPM) para gerenciar dependências

O NPM é o gerenciador de pacotes padrão do Node.js.. Ele permite instalar, atualizar e remover bibliotecas e ferramentas (dependências) que você deseja usar no seu projeto. Essas dependências são armazenadas no repositório oficial do NPM, que conta com milhares de pacotes disponíveis.

### Principais funcionalidades do NPM
Instalação de Dependências: Com o comando npm install, você pode adicionar pacotes ao seu projeto. Eles serão armazenados na pasta node_modules e adicionados ao arquivo package.json (se você estiver usando um).

Exemplo:
```
npm install express
```
Isso instala o pacote `express` e o salva nas dependências.

### Gerenciar Dependências Locais e Globais:
Local: Os pacotes ficam disponíveis apenas dentro do projeto atual. É o modo padrão.
Global: Os pacotes ficam disponíveis para qualquer projeto no seu sistema. Use o comando -g para instalar globalmente.
Exemplo de instalação global:
```
npm install -g nodemon
```

### Atualização e Remoção de Pacotes: Você pode:
Atualizar pacotes com:
```
npm update nome-do-pacote
```

Remover pacotes com:
```
npm uninstall nome-do-pacote
```

### Arquivo package.json: 
Este é o coração do gerenciamento de dependências em um projeto. Ele armazena informações do seu projeto e as dependências necessárias. Para criá-lo, você pode usar:
```
npm init
```

Ou, para uma criação rápida:
```
npm init -y
```

### Scripts Personalizados: 
O NPM permite definir comandos personalizados no package.json. Por exemplo:
```
"scripts": {
  "start": "node app.js",
  "dev": "nodemon app.js"
}
```

Você pode rodar os scripts usando:
```
npm run start
```


### Exemplo prático
Suponha que você esteja criando uma aplicação web:
Instale pacotes necessários:
```
npm install express body-parser
```

No `package.json`, as dependências serão registradas assim:
```
"dependencies": {
  "express": "^4.18.2",
  "body-parser": "^1.20.1"
}
```

Quando alguém clonar seu projeto, basta rodar:
```
npm install
```
Isso instalará todas as dependências listadas.

Benefícios do NPM
- Economia de tempo ao usar ferramentas prontas.
- Facilita o compartilhamento de projetos com outros desenvolvedores.
- Mantém versões consistentes de pacotes para evitar conflitos.


# 1.3 Instale e use pacotes populares como express, lodash

### Instalando e Usando o Express
O Express é um framework leve e popular para criar servidores web com Node.js.

Passos para usar o Express:
Instale o Express no seu projeto: No terminal, dentro do diretório do seu projeto, execute:
```
npm install express
```

Crie um servidor básico: No seu arquivo JavaScript principal (por exemplo, app.js), use o seguinte código:
```
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Olá, Diego! Bem-vindo ao servidor Express!');
});

app.listen(3000, () => {
    console.log('Servidor rodando em http://localhost:3000');
});
```

Execute o servidor: No terminal, execute:
```
node app.js
```

Abra o navegador e acesse http://localhost:3000 para ver a mensagem.

### Instalando e Usando o Lodash
O Lodash é uma biblioteca utilitária que facilita a manipulação de arrays, objetos e strings.
Passos para usar o Lodash:
Instale o Lodash no seu projeto: No terminal, execute:
```
npm install lodash
```

**Use suas funções no código**: Aqui está um exemplo básico de uso do Lodash:
```
const _ = require('lodash');

const nomes = ['Diego', 'Ana', 'Carlos', 'Diego'];

// Remove duplicatas
const nomesUnicos = _.uniq(nomes);
console.log(nomesUnicos); // Saída: [ 'Diego', 'Ana', 'Carlos' ]
```

**Execute o código**: Assim como no caso do Express, use o Node.js para executar o arquivo onde está o código:
```
node seuArquivo.js
```


### Combinação de Ambos
Você também pode combinar pacotes como Express e Lodash no mesmo projeto. Por exemplo, em um servidor Express, pode usar o Lodash para formatar dados antes de enviá-los como resposta.

Benefícios desses Pacotes
Express: Facilita a criação de servidores e APIs de forma rápida.
Lodash: Simplifica tarefas comuns de manipulação de dados, economizando tempo e esforço.

