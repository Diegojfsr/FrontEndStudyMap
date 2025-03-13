
Node.js é uma plataforma poderosa e flexível baseada no motor V8 do Google Chrome que permite executar JavaScript no lado do servidor. Aqui estão os fundamentos que você pode explorar.
### Modelo Assíncrono e Event-Driven:
Node.js usa um loop de eventos não bloqueante, permitindo a execução eficiente de operações assíncronas. Ideal para aplicativos que precisam lidar com muitas conexões ao mesmo tempo.
### Módulos:
Node.js tem um sistema modular nativo. Com o require(), você pode importar bibliotecas e pacotes. Exemplo: const fs = require('fs');.
### Gerenciador de Pacotes (npm):
O npm (Node Package Manager) é usado para gerenciar bibliotecas e dependências. Ele é uma parte essencial do ecossistema Node.js..
### Servidor HTTP Integrado:
É possível criar servidores HTTP sem a necessidade de bibliotecas adicionais. 

Exemplo básico:
```
const http = require('http');
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Olá, mundo!');
});
server.listen(3000, () => console.log('Servidor rodando na porta 3000'));
```

Streams:
São usadas para manipulação de dados de forma eficiente, como leitura/escrita em arquivos ou comunicação em rede.

Ferramentas de Debugging:
Node.js vem com ferramentas como o debugger integrado ou suporte a debugging em editores como VSCode.

# 1.1 O que é e por que usar

Node.js é uma plataforma de execução JavaScript no lado do servidor baseada no mecanismo V8 do Google Chrome. Ele permite que você construa aplicativos rápidos e escaláveis. Vamos entender o que é e por que vale a pena usar.

### O que é Node js
Plataforma Open Source: Código aberto e com uma comunidade vibrante.
JavaScript fora do navegador: Permite executar JavaScript diretamente no servidor.
Baseado em eventos: Usa um loop de eventos para gerenciar conexões simultâneas de forma eficiente.

### Por que usar Node js
Alta Performance: 1.Graças ao motor V8, o Node.js converte o JavaScript diretamente em código de máquina. 2.Processa solicitações de forma assíncrona, sem bloquear o fluxo de execução.
Aplicações em Tempo Real: Ideal para bate-papos online, jogos multiplayer e streaming.
Escalabilidade: Gerencia muitas conexões simultaneamente com consumo eficiente de recursos.
Ecossistema Amplo (npm): Acesse milhares de bibliotecas e pacotes reutilizáveis.
Desenvolvimento Unificado: Use JavaScript tanto no front-end quanto no back-end, facilitando a integração.
Fácil de Aprender: Se já conhece JavaScript, aprender Node.js é natural e rápido.

# 1.2 Instale o Node.jse configure seu ambiente de desenvolvimento

### Instalação do Node js
Acesse o site oficial do Node js [aqui](https://nodejs.org/).
Baixe a versão recomendada (LTS - Long Term Support) para maior estabilidade.
Siga as instruções do instalador para o seu sistema operacional (Windows, macOS ou Linux).
Após a instalação, verifique se o Node.js está funcionando:
```
node -v
```

Esse comando exibirá a versão instalada do Node.js..

O npm (Node Package Manager) também será instalado automaticamente. Verifique com:
```
npm -v
```

### Configuração de um Projeto Node js.
Crie uma pasta para o projeto:
```
mkdir meu-projeto
cd meu-projeto
```

Inicialize o projeto:
```
npm init
```

Este comando cria um arquivo `package.json`, que gerencia as dependências e metadados do seu projeto.

### Instalação de um Editor de Texto
Recomendo usar o Visual Studio Code (VSCode), que é gratuito e possui excelentes extensões para Node js.
Baixe-o [aqui](https://code.visualstudio.com/).
Instale extensões úteis, como:
ESLint (para análise de código).
Prettier (para formatação automática).
Node.js Extension Pack (para melhorar a experiência no Node.js).

### Dependências e Pacotes
Instale pacotes úteis como o express (framework para servidor):
```
npm install express
```

### Configuração Básica do Servidor
Crie um arquivo index.js e adicione o seguinte código para iniciar um servidor simples:
```
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Olá, mundo!');
});

app.listen(3000, () => {
  console.log('Servidor rodando na porta 3000');
});
```

Para executar, use:
```
node index.js
```


# 1.3 Crie um script simples para entender a execução no Node.js

Aqui está um script básico em Node.js que demonstra como ele funciona ao responder a uma solicitação HTTP:

Crie um arquivo chamado app.js em seu projeto.

Adicione este código ao arquivo:
```
// Importa o módulo HTTP nativo do Node.js
const http = require('http');

// Cria um servidor HTTP
const server = http.createServer((req, res) => {
  // Configura o cabeçalho de resposta
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  // Envia uma mensagem de resposta
  res.end('Olá, Node.js está funcionando!');
});

// Faz o servidor escutar na porta 3000
server.listen(3000, () => {
  console.log('Servidor rodando em http://localhost:3000');
});
```

Execute o servidor: No terminal, navegue até a pasta onde está o arquivo app.js e execute:
```
node app.js
```

Teste no navegador: Abra um navegador e vá até http://localhost:3000. Você verá a mensagem "Olá, Node.js está funcionando!".

Esse é um exemplo simples, mas eficaz para entender como o Node.js pode criar servidores e responder a solicitações.