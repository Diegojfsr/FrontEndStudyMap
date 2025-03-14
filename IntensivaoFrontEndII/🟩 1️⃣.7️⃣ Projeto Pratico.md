

## 1.1 Projeto utilizando Node.js e Express

### Passo 1: Configurar o projeto
Crie uma pasta para o projeto.
No terminal, dentro dessa pasta, rode o comando:
```
npm init -y
```
Isso criará o arquivo `package.json`

Instale o Express:
```
npm install express
```

### Passo 2: Criar um servidor básico
Crie um arquivo chamado index.js e adicione o seguinte código:
```
const express = require('express');
const app = express();
const port = 3000; // Porta onde o servidor vai rodar

// Rota principal
app.get('/', (req, res) => {
    res.send('Olá, mundo! Este é meu primeiro projeto com Node.js e Express!');
});

// Rota adicional
app.get('/sobre', (req, res) => {
    res.send('Esta é a rota "Sobre" do meu projeto simples!');
});

// Inicia o servidor
app.listen(port, () => {
    console.log(`Servidor rodando em http://localhost:${port}`);
});
```

### Passo 3: Executar o servidor
No terminal, rode o comando:
```
node index.js
```
Acesse http://localhost:3000 no navegador para ver a aplicação rodando.

Pronto! Esse é um exemplo simples, mas você pode expandir para adicionar mais funcionalidades, como usar um banco de dados ou configurar rotas dinâmicas.


## 1.2 Projeto usando Node.js com um servidor HTTP básico e rotas para diferentes endpoints

#### Passo 1: Criar o projeto
Crie uma pasta para o projeto e inicialize com npm:
```
npm init -y
```
Isso criará um arquivo package.json

Não precisa instalar dependências adicionais, pois usaremos apenas o módulo http nativo do Node.js..

### Passo 2: Criar o arquivo principal
Crie um arquivo chamado server.js e adicione o seguinte código:
```
const http = require('http');

// Criando o servidor HTTP
const server = http.createServer((req, res) => {
    // Definindo cabeçalhos de resposta
    res.setHeader('Content-Type', 'text/plain');

    // Configurando as rotas
    if (req.url === '/' && req.method === 'GET') {
        res.statusCode = 200;
        res.end('Bem-vindo à página inicial!');
    } else if (req.url === '/sobre' && req.method === 'GET') {
        res.statusCode = 200;
        res.end('Esta é a página Sobre.');
    } else if (req.url === '/contato' && req.method === 'GET') {
        res.statusCode = 200;
        res.end('Esta é a página de Contato.');
    } else {
        res.statusCode = 404;
        res.end('Página não encontrada.');
    }
});

// Escolhendo a porta para o servidor
const PORT = 3000;

// Iniciando o servidor
server.listen(PORT, () => {
    console.log(`Servidor rodando em http://localhost:${PORT}`);
});
```

### Passo 3: Executar o servidor
No terminal, execute o comando:
```
node server.js
```

### Passo 4: Teste as rotas:
- http://localhost:3000/: Página inicial com a mensagem "Bem-vindo à página inicial!"
- http://localhost:3000/sobre: Exibe "Esta é a página Sobre."
- http://localhost:3000/contato: Exibe "Esta é a página de Contato."
- Rota inválida: Retorna "Página não encontrada."

E está pronto! Esse é um servidor HTTP básico em Node.js com rotas simples.



## 1.3 Projeto Node usando Mongoose para implementar operações CRUD com MongoDB

### Passo 1: Configurar o projeto
Crie uma pasta para o projeto e inicialize com o comando:
```
npm init -y
```
Isso criará o arquivo package.json.

Instale as dependências necessárias:
```
npm install express mongoose body-parser
```

### Passo 2: Criar o arquivo principal
Crie um arquivo chamado server.js com o seguinte código:
```
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

const app = express();
const PORT = 3000;

// Configurar body-parser para lidar com JSON
app.use(bodyParser.json());

// Conectar ao MongoDB
mongoose.connect('mongodb://127.0.0.1:27017/crud_example', {
    useNewUrlParser: true,
    useUnifiedTopology: true,
})
    .then(() => console.log('Conectado ao MongoDB!'))
    .catch((err) => console.error('Erro ao conectar ao MongoDB:', err));

// Definir um esquema e modelo Mongoose
const ItemSchema = new mongoose.Schema({
    name: { type: String, required: true },
    description: { type: String, required: true },
});

const Item = mongoose.model('Item', ItemSchema);

// Rotas CRUD

// CREATE - Adicionar um novo item
app.post('/items', async (req, res) => {
    try {
        const newItem = new Item(req.body);
        const savedItem = await newItem.save();
        res.status(201).json(savedItem);
    } catch (err) {
        res.status(400).json({ message: 'Erro ao criar item.', error: err.message });
    }
});

// READ - Buscar todos os itens
app.get('/items', async (req, res) => {
    try {
        const items = await Item.find();
        res.json(items);
    } catch (err) {
        res.status(500).json({ message: 'Erro ao buscar itens.', error: err.message });
    }
});

// READ - Buscar um item por ID
app.get('/items/:id', async (req, res) => {
    try {
        const item = await Item.findById(req.params.id);
        if (!item) {
            return res.status(404).json({ message: 'Item não encontrado.' });
        }
        res.json(item);
    } catch (err) {
        res.status(500).json({ message: 'Erro ao buscar item.', error: err.message });
    }
});

// UPDATE - Atualizar um item por ID
app.put('/items/:id', async (req, res) => {
    try {
        const updatedItem = await Item.findByIdAndUpdate(req.params.id, req.body, { new: true });
        if (!updatedItem) {
            return res.status(404).json({ message: 'Item não encontrado.' });
        }
        res.json(updatedItem);
    } catch (err) {
        res.status(400).json({ message: 'Erro ao atualizar item.', error: err.message });
    }
});

// DELETE - Deletar um item por ID
app.delete('/items/:id', async (req, res) => {
    try {
        const deletedItem = await Item.findByIdAndDelete(req.params.id);
        if (!deletedItem) {
            return res.status(404).json({ message: 'Item não encontrado.' });
        }
        res.json({ message: 'Item deletado com sucesso!' });
    } catch (err) {
        res.status(500).json({ message: 'Erro ao deletar item.', error: err.message });
    }
});

// Iniciar o servidor
app.listen(PORT, () => {
    console.log(`Servidor rodando em http://localhost:${PORT}`);
});
```

### Passo 3: Configurar o MongoDB
Certifique-se de que o MongoDB esteja instalado e rodando na sua máquina.
A conexão no código aponta para mongodb://127.0.0.1:27017/crud_example. Esse será o banco de dados usado no projeto.

### Passo 4: Testar o servidor
Inicie o servidor:
```
node server.js
```

Use ferramentas como Postman ou Insomnia para testar as operações CRUD:
- POST /items: Cria um novo item (enviar um JSON no corpo).
- GET /items: Retorna todos os itens.
- GET /items/:id: Retorna um item específico por ID.
- PUT /items/:id: Atualiza um item por ID.
- DELETE /items/:id: Deleta um item por ID.

E está pronto! Este é um exemplo básico de um sistema CRUD com Node.js e Mongoose.


## 1.4 Projeto Node.js que utiliza JSON Web Tokens (JWT) para autenticação e controle de acesso às rotas

### Passo 1: Configuração do projeto
Crie uma pasta para o projeto e inicialize-o:
```
npm init -y
```

Instale os pacotes necessários:
```
npm install express jsonwebtoken body-parser bcryptjs
```

### Passo 2: Criar o arquivo principal (server.js)
Crie um arquivo chamado server.js e insira o seguinte código:
```
const express = require('express');
const jwt = require('jsonwebtoken');
const bcrypt = require('bcryptjs');
const bodyParser = require('body-parser');

const app = express();
const PORT = 3000;
const SECRET_KEY = 'minhaChaveSecreta'; // Não use isso em produção! Use variáveis de ambiente.

// Simulação de banco de dados (apenas para testes)
const users = [];

// Middleware para analisar JSON
app.use(bodyParser.json());

// Rota: Registrar um novo usuário
app.post('/register', async (req, res) => {
    try {
        const { username, password } = req.body;

        // Criptografar a senha
        const hashedPassword = await bcrypt.hash(password, 10);

        // Salvar o usuário
        users.push({ username, password: hashedPassword });
        res.status(201).json({ message: 'Usuário registrado com sucesso!' });
    } catch (error) {
        res.status(500).json({ message: 'Erro ao registrar usuário.' });
    }
});

// Rota: Login
app.post('/login', async (req, res) => {
    try {
        const { username, password } = req.body;

        // Encontrar o usuário
        const user = users.find(u => u.username === username);
        if (!user) return res.status(404).json({ message: 'Usuário não encontrado.' });

        // Verificar a senha
        const isPasswordValid = await bcrypt.compare(password, user.password);
        if (!isPasswordValid) return res.status(401).json({ message: 'Senha inválida.' });

        // Gerar o token JWT
        const token = jwt.sign({ username }, SECRET_KEY, { expiresIn: '1h' });
        res.json({ message: 'Login bem-sucedido!', token });
    } catch (error) {
        res.status(500).json({ message: 'Erro no login.' });
    }
});

// Middleware: Verificar autenticação
function authenticateToken(req, res, next) {
    const token = req.headers.authorization && req.headers.authorization.split(' ')[1];
    if (!token) return res.status(401).json({ message: 'Acesso negado. Token não fornecido.' });

    jwt.verify(token, SECRET_KEY, (err, user) => {
        if (err) return res.status(403).json({ message: 'Token inválido ou expirado.' });
        req.user = user;
        next();
    });
}

// Rota protegida: Acessível apenas para usuários autenticados
app.get('/protected', authenticateToken, (req, res) => {
    res.json({ message: `Bem-vindo, ${req.user.username}! Você acessou uma rota protegida.` });
});

// Iniciar o servidor
app.listen(PORT, () => {
    console.log(`Servidor rodando em http://localhost:${PORT}`);
});
```


### Passo 3: Testar o projeto

Registrar usuário:
- Método: POST
- Endpoint: http://localhost:3000/register
- Corpo JSON:
```
{
    "username": "usuario1",
    "password": "senha123"
}
```

Fazer login:
- Método: POST
- Endpoint: http://localhost:3000/login
- Corpo JSON:
```
{
    "username": "usuario1",
    "password": "senha123"
}
```
Resposta: Um token JWT será retornado.

Acessar rota protegida:
- Método: GET
- Endpoint: http://localhost:3000/protected
- Header:
```
Authorization: Bearer <TOKEN>
```

Este é um exemplo básico, mas funcional, de um sistema de autenticação com JWT. Em um projeto real, você deve usar um banco de dados para armazenar usuários e gerenciar variáveis sensíveis, como a SECRET_KEY, utilizando variáveis de ambiente (como com dotenv).


## 1.5 Projeto Node.js com APIs RESTful usando Express e bibliotecas para validação de dados, como express-validator

### Passo 1: Configurar o projeto
Crie uma pasta para o projeto e inicialize com o comando:
```
npm init -y
```

Instale as dependências necessárias:
```
npm install express express-validator body-parser
```

### Passo 2: Criar o arquivo principal (server.js)
Crie um arquivo chamado server.js e insira o seguinte código:
```
const express = require('express');
const { body, validationResult } = require('express-validator');
const bodyParser = require('body-parser');

const app = express();
const PORT = 3000;

// Middleware para analisar JSON
app.use(bodyParser.json());

// Simulação de banco de dados na memória
let items = [];

// Rotas RESTful com validação

// CREATE - Adicionar um item com validação
app.post(
    '/items',
    [
        body('name').isString().notEmpty().withMessage('O nome é obrigatório e deve ser uma string.'),
        body('price').isFloat({ gt: 0 }).withMessage('O preço deve ser um número positivo.'),
    ],
    (req, res) => {
        const errors = validationResult(req);
        if (!errors.isEmpty()) {
            return res.status(400).json({ errors: errors.array() });
        }

        const { name, price } = req.body;
        const newItem = { id: items.length + 1, name, price };
        items.push(newItem);

        res.status(201).json(newItem);
    }
);

// READ - Buscar todos os itens
app.get('/items', (req, res) => {
    res.json(items);
});

// READ - Buscar um item por ID
app.get('/items/:id', (req, res) => {
    const item = items.find((i) => i.id === parseInt(req.params.id));
    if (!item) {
        return res.status(404).json({ message: 'Item não encontrado.' });
    }

    res.json(item);
});

// UPDATE - Atualizar um item por ID com validação
app.put(
    '/items/:id',
    [
        body('name').optional().isString().notEmpty().withMessage('O nome deve ser uma string.'),
        body('price').optional().isFloat({ gt: 0 }).withMessage('O preço deve ser um número positivo.'),
    ],
    (req, res) => {
        const errors = validationResult(req);
        if (!errors.isEmpty()) {
            return res.status(400).json({ errors: errors.array() });
        }

        const item = items.find((i) => i.id === parseInt(req.params.id));
        if (!item) {
            return res.status(404).json({ message: 'Item não encontrado.' });
        }

        const { name, price } = req.body;
        if (name) item.name = name;
        if (price) item.price = price;

        res.json(item);
    }
);

// DELETE - Remover um item por ID
app.delete('/items/:id', (req, res) => {
    const itemIndex = items.findIndex((i) => i.id === parseInt(req.params.id));
    if (itemIndex === -1) {
        return res.status(404).json({ message: 'Item não encontrado.' });
    }

    items.splice(itemIndex, 1);
    res.json({ message: 'Item removido com sucesso!' });
});

// Iniciar o servidor
app.listen(PORT, () => {
    console.log(`Servidor rodando em http://localhost:${PORT}`);
});
```


### Passo 3: Testar as rotas
Endpoints disponíveis:
POST /items: Cria um novo item (com validação).
Exemplo de corpo JSON:
```
{
    "name": "Produto A",
    "price": 10.5
}
```

GET /items: Retorna todos os itens.
GET /items/:id: Retorna um item específico.
PUT /items/:id: Atualiza um item (com validação).

Exemplo de corpo JSON:
```
{
    "name": "Produto Atualizado",
    "price": 12.0
}
```

**DELETE** `/items/:id`: Remove um item.

Use ferramentas como Postman ou Insomnia para testar os endpoints.

Este projeto é um exemplo básico de uma API RESTful com validação usando express-validator. Em projetos maiores, você pode separar as rotas, controladores e middleware em arquivos diferentes.

