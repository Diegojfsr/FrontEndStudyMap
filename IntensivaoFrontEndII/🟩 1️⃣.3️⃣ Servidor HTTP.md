
Um servidor HTTP no Node.js é criado utilizando o módulo integrado chamado http. Este módulo permite que você construa servidores web capazes de processar requisições HTTP (como GET, POST, etc.) e enviar respostas.

# 1.1 Criação de um servidor HTTP básico

### Exemplo simples de servidor HTTP em Node.js:
Aqui está um exemplo de um servidor básico:
```
const http = require('http');

// Cria o servidor
const servidor = http.createServer((req, res) => {
  // Define o cabeçalho da resposta
  res.writeHead(200, { 'Content-Type': 'text/plain' });

  // Envia uma resposta
  res.end('Olá, mundo! Servidor HTTP no Node.js em execução.\n');
});

// Escolhe a porta onde o servidor estará disponível
const porta = 3000;
servidor.listen(porta, () => {
  console.log(`Servidor rodando em http://localhost:${porta}/`);
});
```

### Como funciona este código:

- http.createServer(): Cria o servidor. A função callback recebe dois parâmetros:
- req (request): Representa a requisição feita pelo cliente (navegador, por exemplo).
- res (response): Permite enviar uma resposta ao cliente.
- res.writeHead(): Define o cabeçalho da resposta (status HTTP e tipo de conteúdo).
- res.end(): Encerra a resposta e envia-a ao cliente.
- servidor.listen(): Faz o servidor "escutar" requisições na porta especificada (neste caso, 3000).
### Aplicações:
Servidores HTTP com Node.js são frequentemente usados para criar APIs RESTful, sites dinâmicos ou aplicações web em tempo real.
Integrando bibliotecas e frameworks como o Express.js, você pode criar estruturas mais robustas para manipulação de rotas, middleware e muito mais.

# 1.2 Definição de rotas para diferentes endpoints

Rotas são uma parte essencial ao configurar um servidor HTTP em Node.js, pois definem como o servidor deve responder a diferentes endpoints (URLs ou caminhos específicos). Vamos explorar como configurar rotas para diferentes endpoints!

Exemplo de Rotas:
Aqui está um exemplo básico de como configurar diferentes rotas em Node.js usando o módulo http:
```
const http = require('http');

const servidor = http.createServer((req, res) => {
  // Define o cabeçalho padrão da resposta
  res.writeHead(200, { 'Content-Type': 'text/plain' });

  // Rotas para diferentes endpoints
  if (req.url === '/') {
    res.end('Você está na página inicial!');
  } else if (req.url === '/sobre') {
    res.end('Bem-vindo à página Sobre!');
  } else if (req.url === '/contato') {
    res.end('Entre em contato conosco!');
  } else {
    // Rota padrão para quando a URL não é encontrada
    res.writeHead(404, { 'Content-Type': 'text/plain' });
    res.end('404 - Página não encontrada');
  }
});

// Define a porta e inicia o servidor
const porta = 3000;
servidor.listen(porta, () => {
  console.log(`Servidor rodando em http://localhost:${porta}/`);
});
```

### Como funciona:
- req.url: A propriedade url do objeto de requisição (req) contém o caminho solicitado pelo cliente (como /sobre ou /contato).
- Estrutura de Condição: Utilizamos if e else if para verificar qual endpoint foi solicitado e enviar a resposta correspondente.
- 404 - Página Não Encontrada: Caso o cliente tente acessar uma rota não definida, o servidor retorna um erro 404 com a mensagem personalizada.
### Usando Frameworks (como Express.js):
O exemplo acima funciona bem para servidores simples, mas frameworks como Express.js tornam a criação de rotas mais fácil e organizada:
```
const express = require('express');
const app = express();

// Define rotas
app.get('/', (req, res) => res.send('Você está na página inicial!'));
app.get('/sobre', (req, res) => res.send('Bem-vindo à página Sobre!'));
app.get('/contato', (req, res) => res.send('Entre em contato conosco!'));

// Inicia o servidor
const porta = 3000;
app.listen(porta, () => {
  console.log(`Servidor rodando em http://localhost:${porta}/`);
});
```

Com o **Express**, você pode gerenciar rotas de forma mais intuitiva e escalável, além de ter acesso a middleware e ferramentas adicionais.

# 1.3 Uso de middlewares para manipulação de requisições

Os middlewares são funções intermediárias que são executadas durante o ciclo de processamento de uma requisição em servidores HTTP. Eles são usados para manipular, modificar ou realizar alguma lógica específica em requisições ou respostas antes que elas cheguem ao destino final (seja uma rota ou outro middleware).

### Como funcionam os middlewares:
- Um middleware recebe três parâmetros principais:
- req (request): Representa a requisição do cliente.
- res (response): Representa a resposta que será enviada ao cliente.
- next: Uma função que chama o próximo middleware na cadeia (se existir).

### Exemplos de uso de middlewares:
Aqui está um exemplo básico usando o framework Express.js, que é amplamente utilizado para gerenciar middlewares no Node.js:
```
const express = require('express');
const app = express();

// Middleware global: executa para toda requisição
app.use((req, res, next) => {
  console.log(`Método: ${req.method}, Endpoint: ${req.url}`);
  next(); // Passa o controle para o próximo middleware ou rota
});

// Middleware específico de rota
app.get('/rota-exemplo', (req, res, next) => {
  console.log('Middleware para /rota-exemplo');
  res.send('Este é o endpoint /rota-exemplo.');
});

// Outro middleware para manipulação
app.use((req, res, next) => {
  res.status(404).send('Página não encontrada');
});

// Inicia o servidor
const porta = 3000;
app.listen(porta, () => {
  console.log(`Servidor rodando em http://localhost:${porta}/`);
});
```

### Explicação do exemplo:
- Middleware Global (app.use): Será executado para qualquer requisição. Neste caso, apenas registra no console o método HTTP e a URL.
- Middleware Específico (app.get): Executado apenas para o endpoint /rota-exemplo. Ele pode realizar alguma lógica antes de retornar a resposta.
- Middleware de Tratamento de Erros (404): Caso nenhuma rota tenha correspondido, este middleware é executado.

### Aplicações Comuns de Middlewares:
- Autenticação e Autorização: Validar se o usuário tem permissão para acessar um recurso.
- Manipulação de Requisições: Analisar, modificar ou validar os dados enviados pelo cliente.
- Registro de Logs: Gravar informações sobre as requisições no console ou em um arquivo.
- Manipulação de Erros: Lidar com erros e enviar mensagens apropriadas ao cliente.
- Resposta Estática: Servir arquivos estáticos, como HTML, CSS ou imagens.


