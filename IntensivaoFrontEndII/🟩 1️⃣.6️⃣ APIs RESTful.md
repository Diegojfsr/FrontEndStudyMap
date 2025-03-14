
APIs RESTful são como "pontes" que permitem que diferentes sistemas ou aplicativos conversem entre si pela internet. Pense assim: você tem um site ou aplicativo que precisa buscar informações de outro lugar, como um banco de dados ou outro serviço. A API RESTful é o mecanismo que torna essa comunicação possível.

"RESTful" vem de REST, que significa Representational State Transfer. É um conjunto de regras que as APIs seguem para serem mais organizadas, rápidas e fáceis de usar.

Por exemplo, imagine que você tem um aplicativo de clima. Ele pode usar uma API RESTful para pedir as informações de previsão do tempo de um servidor. O servidor responde com os dados em um formato como JSON (que é parecido com uma lista organizada).

Basicamente:
- Cliente (seu app) faz um pedido para a API usando URLs e métodos (como GET para buscar dados ou POST para enviar dados).
- Servidor recebe o pedido, processa, e envia a resposta.

É como pedir algo em um restaurante: você (cliente) faz o pedido ao garçom (API), e a cozinha (servidor) prepara e entrega a comida (dados)!

## 1.1 Criação de APIs RESTful com Node.jse Express

O básico de como criar uma API RESTful com Node.js e o framework Express. É mais fácil do que parece, vamos lá!

### Instalar o Node.js e criar o projeto:
- Certifique-se de que o Node.js está instalado em sua máquina.
- Crie uma nova pasta para o seu projeto e abra-a no terminal.
- Execute npm init -y para criar o arquivo package.json.

### Instalar o Express:
- No terminal, execute o comando: npm install express.
- O Express é um framework que facilita o processo de criar servidores e rotas.

### Criar o servidor:
- Crie um arquivo chamado index.js (ou outro nome que você prefira).
- Escreva o seguinte código básico para configurar um servidor com Express:

```
const express = require('express');
const app = express();
const port = 3000;

// Middleware para lidar com JSON
app.use(express.json());

// Rota básica
app.get('/', (req, res) => {
  res.send('Bem-vindo à API RESTful!');
});

// Iniciar o servidor
app.listen(port, () => {
  console.log(`Servidor rodando em http://localhost:${port}`);
});
```

### Definir rotas:
As rotas representam os “endereços” que sua API vai atender. Por exemplo:
- GET: Para buscar dados.
- POST: Para criar novos dados.
- PUT/PATCH: Para atualizar dados existentes.
- DELETE: Para remover dados.

Aqui está um exemplo de como adicionar mais rotas:
```
// Rota para listar dados
app.get('/dados', (req, res) => {
  res.json([{ id: 1, nome: 'Item 1' }, { id: 2, nome: 'Item 2' }]);
});

// Rota para adicionar novos dados
app.post('/dados', (req, res) => {
  const novoDado = req.body; // Pega os dados do corpo da requisição
  res.status(201).json({ mensagem: 'Dado criado!', dado: novoDado });
});

// Rota para deletar dados
app.delete('/dados/:id', (req, res) => {
  const id = req.params.id; // Pega o ID da URL
  res.json({ mensagem: `Dado com id ${id} removido!` });
});
```

####  Rota para listar dados
```
app.get('/dados', (req, res) => {
  res.json([{ id: 1, nome: 'Item 1' }, { id: 2, nome: 'Item 2' }]);
});
```

app.get('/dados'): Aqui definimos uma rota para atender requisições HTTP do tipo GET no endpoint /dados. Isso significa que, quando um cliente acessar esse endereço, essa função será executada.

Função de callback (req, res) => {}:
- O req é o objeto da requisição (o que o cliente enviou).
- O res é o objeto de resposta (o que o servidor vai devolver).

res.json([...]): Utiliza o método json para enviar uma resposta em formato JSON. Neste caso, estamos enviando uma lista de objetos (uma "simulação" de dados).


Resultado: Quando alguém acessar o endpoint /dados, verá uma lista com dois itens:
```
[
  { "id": 1, "nome": "Item 1" },
  { "id": 2, "nome": "Item 2" }
]
```

#### Rota para adicionar novos dados
```
app.post('/dados', (req, res) => {
  const novoDado = req.body; // Pega os dados do corpo da requisição
  res.status(201).json({ mensagem: 'Dado criado!', dado: novoDado });
});
```

app.post('/dados'): Aqui estamos configurando uma rota do tipo POST no endpoint /dados. Isso é usado geralmente para criar ou enviar novos dados.

const novoDado = req.body;: Pegamos o conteúdo que foi enviado no corpo da requisição (é necessário usar middleware como express.json() para que isso funcione).

res.status(201).json(...):
- O método status(201) define o código de status HTTP como "201 Created", indicando que algo foi criado no servidor.
- O método json({...}) envia uma resposta com uma mensagem confirmando a criação e devolve o dado enviado.


Resultado: Quando você faz uma requisição POST com dados no corpo (por exemplo, { "id": 3, "nome": "Item 3" }), a resposta seria:
```
{
  "mensagem": "Dado criado!",
  "dado": {
    "id": 3,
    "nome": "Item 3"
  }
}
```


#### Rota para deletar dados
```
app.delete('/dados/:id', (req, res) => {
  const id = req.params.id; // Pega o ID da URL
  res.json({ mensagem: `Dado com id ${id} removido!` });
});
```

- app.delete('/dados/:id'): Esta rota atende requisições DELETE para o endpoint /dados/:id. O :id significa que o endpoint espera um parâmetro dinâmico na URL (o valor de id).
- req.params.id: Acessa o valor do parâmetro id da URL.
- res.json({...}): Envia uma resposta indicando que o dado com o ID fornecido foi removido.

Exemplo:
Se o cliente fizer uma requisição DELETE para /dados/2, o servidor responde:
```
{
  "mensagem": "Dado com id 2 removido!"
}
```

### Resumo
Este código mostra como criar três endpoints básicos para uma API:
- GET /dados - Para listar dados.
- POST /dados - Para adicionar novos dados.
- DELETE /dados/:id - Para deletar um dado específico.

### Testar a API:

#### Certifique-se de que o servidor está funcionando
No terminal, vá até o diretório do projeto onde está o arquivo index.js.
Execute o comando:
```
node index.js
```
Você deve ver uma mensagem no terminal, como: Servidor rodando em http://localhost:3000.

#### Use um navegador para testar rotas GET
Abra seu navegador e acesse http://localhost:3000/dados.
Você verá uma resposta como:
```
[
  { "id": 1, "nome": "Item 1" },
  { "id": 2, "nome": "Item 2" }
]
```
Essa é uma forma simples de testar rotas GET.

#### Ferramentas para testar outras rotas
Para rotas POST, DELETE, PUT, etc., você precisará de ferramentas que enviem requisições HTTP. Aqui estão as melhores opções:

#### Postman
Baixe e instale o Postman.
Crie uma nova requisição e configure:
- Método: GET, POST, DELETE, etc.
- URL: http://localhost:3000/dados.
- Body (para POST/PUT): selecione raw e formato JSON. Exemplo:
```
{ "id": 3, "nome": "Item 3" }
```
Envie a requisição e veja a resposta.

#### Insomnia
Outra ferramenta parecida com Postman. Instale pelo site Insomnia.
Configure a requisição do mesmo jeito que no Postman.

#### Curl (no terminal)
Você também pode usar o terminal para testar:

GET:
```
curl http://localhost:3000/dados
```
POST:
```
curl -X POST -H "Content-Type: application/json" -d '{"id": 3, "nome": "Item 3"}' http://localhost:3000/dados
```
DELETE:
```
curl -X DELETE http://localhost:3000/dados/1
```

#### Erro comum: “CORS”
Se você tentar testar a API a partir de outro domínio (como frontend em outra URL), pode ocorrer um erro de CORS (Cross-Origin Resource Sharing).

Para resolver, você pode instalar o pacote `cors` no Node.js:
```
npm install cors
```

E adicionar ao seu código:
```
const cors = require('cors');
app.use(cors());
```

#### Testar automação com Jest
Para testes automatizados do código, você pode usar o framework Jest ou outro similar, mas essa é uma etapa mais avançada.

#### Organização do Código:
À medida que sua API cresce, é uma boa prática dividir seu código em arquivos e usar um banco de dados (como MongoDB ou PostgreSQL) para persistir dados.


## 1.2 Uso de bibliotecas para validação de dados

O uso de bibliotecas para validação de dados em aplicativos é fundamental para garantir que as informações fornecidas pelos usuários estejam corretas, no formato esperado e seguras.
### O que é validação de dados?
Validação de dados é o processo de verificar se os dados recebidos atendem a certos requisitos ou padrões, como:
- Tipo (por exemplo, número, string, data).
- Formato (por exemplo, e-mail ou CPF válido).
- Limites (por exemplo, idade entre 18 e 100 anos).
- Presença ou obrigatoriedade (certos campos não podem ser deixados em branco).
### Por que usar bibliotecas?
Fazer validação manual em cada lugar do código pode ser trabalhoso e propenso a erros. Bibliotecas oferecem funções prontas e fáceis de usar, otimizando o trabalho e garantindo segurança

### Bibliotecas populares no Node.js
Aqui estão algumas das bibliotecas mais utilizadas para validação de dados:
#### Joi
É uma das bibliotecas mais populares para validação.
Permite definir esquemas para os dados e validá-los de forma elegante.
Exemplo:
```
const Joi = require('joi');

const schema = Joi.object({
  nome: Joi.string().min(3).required(),
  email: Joi.string().email().required(),
  idade: Joi.number().min(18).max(100)
});

// Dados para validar
const dados = { nome: 'Diego', email: 'diego@email.com', idade: 25 };

// Validação
const { error, value } = schema.validate(dados);
if (error) {
  console.log('Erro:', error.details[0].message);
} else {
  console.log('Dados válidos:', value);
}
```

#### Yup
Muito usado com bibliotecas de front-end (como React), mas também funciona bem no Node.js..
Similar ao Joi, mas com uma sintaxe ligeiramente diferente.
Exemplo:
```
const yup = require('yup');

const schema = yup.object().shape({
  nome: yup.string().min(3).required(),
  email: yup.string().email().required(),
  idade: yup.number().min(18).max(100)
});

// Dados para validar
schema.validate({ nome: 'Diego', email: 'diego@email.com', idade: 25 })
  .then((dados) => console.log('Dados válidos:', dados))
  .catch((erro) => console.log('Erro:', erro.message));
```

#### Express-Validator
Ideal para uso com Express, adicionando validação diretamente nas rotas da API.
Baseado em middlewares.
Exemplo:
```
const { body, validationResult } = require('express-validator');
const express = require('express');
const app = express();

app.use(express.json());

app.post('/dados', [
  body('nome').isString().isLength({ min: 3 }),
  body('email').isEmail(),
  body('idade').isInt({ min: 18, max: 100 })
], (req, res) => {
  const erros = validationResult(req);
  if (!erros.isEmpty()) {
    return res.status(400).json({ erros: erros.array() });
  }
  res.json({ mensagem: 'Dados válidos!', dados: req.body });
});

app.listen(3000, () => console.log('Servidor rodando!'));
```

### Quando usar cada biblioteca?
Joi e Yup: Se você precisa de validação independente do framework.
Express-Validator: Quando já está utilizando Express e quer integrar a validação diretamente nas rotas.

## 1.3 Documentação de APIs com Swagger

Documentar uma API é essencial para ajudar outros desenvolvedores (ou você mesmo no futuro) a entenderem como a API funciona. O Swagger é uma ferramenta incrível que facilita a criação de documentações claras e interativas para APIs RESTful.
### O que é o Swagger?
O Swagger é um conjunto de ferramentas que ajuda a documentar APIs. Ele inclui o Swagger UI, uma interface visual que exibe sua API de forma interativa, e o Swagger Editor, onde você pode escrever a documentação da API usando um padrão chamado OpenAPI Specification (OAS).
Com o Swagger:
- Você define os endpoints (rotas da API) e como cada um funciona.
- Os desenvolvedores podem testar as rotas diretamente na interface gerada.
- A documentação é legível tanto para humanos quanto para máquinas.
### Como usar o Swagger com Node.js e Express

#### Instalar Dependências:
No terminal, no diretório do projeto, instale o Swagger:
```
npm install swagger-jsdoc swagger-ui-express
```

#### Configurar o Swagger no seu projeto:
No seu arquivo principal (como index.js), adicione o seguinte:
```
const express = require('express');
const swaggerJsDoc = require('swagger-jsdoc');
const swaggerUi = require('swagger-ui-express');
const app = express();
const port = 3000;

// Configurações do Swagger
const swaggerOptions = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'API Exemplo',
      version: '1.0.0',
      description: 'Uma API simples para aprender Swagger',
    },
  },
  apis: ['./index.js'], // Arquivos que contêm as anotações
};

const swaggerDocs = swaggerJsDoc(swaggerOptions);
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocs));

// Rotas da API
/**
 * @swagger
 * /dados:
 *   get:
 *     summary: Retorna uma lista de dados
 *     responses:
 *       200:
 *         description: Lista de dados
 *         content:
 *           application/json:
 *             schema:
 *               type: array
 *               items:
 *                 type: object
 *                 properties:
 *                   id:
 *                     type: integer
 *                   nome:
 *                     type: string
 */
app.get('/dados', (req, res) => {
  res.json([{ id: 1, nome: 'Item 1' }, { id: 2, nome: 'Item 2' }]);
});

// Iniciar servidor
app.listen(port, () => {
  console.log(`Servidor rodando em http://localhost:${port}`);
  console.log(`Documentação disponível em http://localhost:${port}/api-docs`);
});
```

#### Entender as anotações:
No trecho acima, as anotações (comentários iniciados com @swagger) são usadas para descrever os endpoints da API.
Por exemplo:
- /dados: A rota.
- summary: Um resumo do que o endpoint faz.
- responses: As possíveis respostas da API (ex.: código 200 com descrição).

#### Testar a Documentação:
Após rodar o servidor (node index.js), acesse http://localhost:3000/api-docs no navegador.
Você verá uma interface bonita e interativa onde poderá explorar a API e testar as rotas diretamente.

### Por que usar Swagger?
Facilita o consumo da API: Outros desenvolvedores entendem rapidamente como usá-la.
Documentação interativa: É possível testar a API sem precisar de ferramentas externas como Postman.
Padrão: Seguindo o OpenAPI Specification, sua documentação será padronizada.

