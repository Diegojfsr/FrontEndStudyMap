
Trabalhar com bancos de dados em Node.js é muito eficiente e flexível, graças à ampla gama de bibliotecas e ferramentas disponíveis para integração. O Node.js é compatível com uma ampla variedade de bancos de dados, tanto relacionais quanto não relacionais. Aqui estão algumas opções populares.
### O que é um banco de dados?
Um banco de dados é uma coleção organizada de informações que permite armazenar, recuperar e manipular dados de forma eficiente. Eles são amplamente usados em aplicativos para guardar informações como dados de usuários, produtos, transações, etc.

Existem dois tipos principais de bancos de dados:
- Relacionais (SQL): Dados organizados em tabelas, com linhas e colunas (e.g., MySQL, PostgreSQL).
- Não relacionais (NoSQL): Dados armazenados em formatos flexíveis, como documentos, gráficos ou pares chave-valor (e.g., MongoDB, Redis).

### Node.js e bancos de dados
Node.js é muito popular para desenvolvimento backend por causa de sua arquitetura não bloqueante (non-blocking I/O), o que significa que ele pode lidar com múltiplas solicitações simultaneamente. Ele interage com bancos de dados para ler e gravar informações essenciais para aplicativos. As bibliotecas disponíveis facilitam a conexão e manipulação de dados.

### Ferramentas e bibliotecas comuns
- MySQL: Pode ser usado com a biblioteca mysql2 ou sequelize (que é um ORM – Mapeador Objeto-Relacional).
- PostgreSQL: Suportado por pacotes como pg ou typeorm.
- MongoDB: Amplamente usado em conjunto com a biblioteca mongoose.
- Redis: Conectado com pacotes como redis, ótimo para armazenamento em cache.
- SQLite: Leve e acessado com a biblioteca sqlite3.

### Como funciona na prática?
Aqui está um exemplo básico de conexão com um banco de dados MySQL no Node.js:

```
const mysql = require('mysql2');

// Configuração de conexão
const connection = mysql.createConnection({
  host: 'localhost',
  user: 'seu_usuario',
  password: 'sua_senha',
  database: 'nome_do_banco'
});

// Testando a conexão
connection.connect((err) => {
  if (err) {
    console.error('Erro ao conectar:', err);
    return;
  }
  console.log('Conectado ao banco de dados!');
});
```

Da mesma forma, cada tipo de banco de dados tem suas bibliotecas específicas e sintaxe para interagir com Node.js..

### Escolhendo o banco de dados certo
A escolha depende do seu projeto:
- Relacional (SQL): Ideal para aplicativos que precisam de consistência e transações estruturadas, como sistemas financeiros.
- Não relacional (NoSQL): Melhor para aplicações flexíveis, como redes sociais, onde os dados não são tão estruturados.

## 1.1 Introdução ao MongoDB e instalação

O MongoDB é um banco de dados NoSQL amplamente utilizado, conhecido por sua flexibilidade e escalabilidade. Ao contrário de bancos de dados relacionais, ele armazena dados em um formato baseado em documentos, geralmente em JSON, tornando-o ideal para aplicações que lidam com dados dinâmicos e não estruturados.

### Características principais do MongoDB
- Baseado em documentos: Os dados são armazenados em documentos BSON (uma extensão do JSON).
- Modelo NoSQL: Não requer esquemas fixos, permitindo maior flexibilidade.
- Escalabilidade horizontal: Pode ser expandido facilmente para grandes volumes de dados.
- Consultas ricas: Oferece uma linguagem de consulta poderosa e flexível.
Agora, vamos à instalação e configuração do MongoDB.
### Passo a passo para instalar o MongoDB
Aqui estão as instruções gerais para instalar o MongoDB em diferentes sistemas operacionais:

#### Windows

- Acesse o site oficial do MongoDB: https://www.mongodb.com/try/download/community.
- Faça o download do instalador para Windows.
- Execute o instalador e siga as etapas de configuração:
- Escolha a instalação "Complete".
- Durante a configuração, ative a opção "Install MongoDB Compass" (uma interface gráfica para gerenciar o MongoDB).
- Após a instalação, adicione o MongoDB ao PATH do sistema para acessá-lo pelo terminal.
- Inicie o servidor MongoDB com o comando:

```
mongod
```


#### Linux

- Importar a chave pública GPG:
```
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
```

- Criar o arquivo de lista para o repositório MongoDB:
```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```

- Atualizar os pacotes e instalar:
```
sudo apt-get update
sudo apt-get install -y mongodb-org
```

- Inicie o serviço com:
```
sudo systemctl start mongod
```

#### macOS

- Instale o Homebrew, caso não tenha:
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

- Use o Homebrew para instalar o MongoDB:
```
brew tap mongodb/brew
brew install mongodb-community@6.0
```

- Inicie o serviço:
```
brew services start mongodb/brew/mongodb-community
```

Com o MongoDB instalado, você pode começar a trabalhar com ele.


## 1.2 Uso do Mongoose para modelagem de dados

O Mongoose é uma biblioteca poderosa para Node.js que facilita a interação com bancos de dados MongoDB. Ele é um **ODM** (Object Data Modeling), ou seja, um mapeador de objetos para documentos. Com o Mongoose, você pode criar modelos que definem a estrutura de documentos em uma coleção MongoDB, simplificando a manipulação de dados.

### Por que usar o Mongoose?

- Definição de esquemas: O Mongoose permite definir esquemas para seus dados, o que ajuda a garantir consistência mesmo em um banco NoSQL.
- Validação: Ele oferece validação embutida para garantir que os dados armazenados sigam determinadas regras.
- Middlewares: Suporte para middlewares (pré e pós-gancho), permitindo executar lógica em diferentes etapas, como antes de salvar ou excluir dados.
- Consultas simplificadas: Interface intuitiva para criar, ler, atualizar e excluir dados (CRUD).

### Usando o Mongoose - Passo a Passo

#### Instalando o Mongoose
Certifique-se de que você tem o Node.js instalado. Em seguida, adicione o Mongoose ao seu projeto com o seguinte comando:
```
npm install mongoose
```

#### Conectando ao MongoDB
Aqui está um exemplo básico de como conectar ao seu banco de dados:
```
const mongoose = require('mongoose');

// Conectando ao banco
mongoose.connect('mongodb://localhost:27017/nomeDoBanco', {
  useNewUrlParser: true,
  useUnifiedTopology: true
}).then(() => {
  console.log('Conectado ao MongoDB!');
}).catch((err) => {
  console.error('Erro ao conectar ao MongoDB:', err);
});
```

#### Definindo um esquema
Com o Mongoose, você pode criar esquemas para organizar os dados:
```
const { Schema } = mongoose;

// Criando um esquema para uma coleção de usuários
const usuarioSchema = new Schema({
  nome: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  idade: { type: Number, min: 0 },
  dataCriacao: { type: Date, default: Date.now }
});

// Criando um modelo baseado no esquema
const Usuario = mongoose.model('Usuario', usuarioSchema);
```


## 1.3 Implementação de operações CRUD (Create, Read, Update, Delete)

Depois de criar seu modelo, você pode usá-lo para manipular os dados no MongoDB.
Aqui estão exemplos de operações básicas.
### Criar um documento:
```
const novoUsuario = new Usuario({
  nome: 'Diego',
  email: 'diego@email.com',
  idade: 28
});

novoUsuario.save()
  .then(() => console.log('Usuário salvo com sucesso!'))
  .catch((err) => console.error('Erro ao salvar usuário:', err));
```

### Ler documentos:
```
Usuario.find({ idade: { $gte: 18 } })
  .then((usuarios) => console.log('Usuários encontrados:', usuarios))
  .catch((err) => console.error('Erro ao buscar usuários:', err));
```

### Atualizar um documento:
```
Usuario.updateOne({ nome: 'Diego' }, { idade: 29 })
  .then(() => console.log('Usuário atualizado!'))
  .catch((err) => console.error('Erro ao atualizar usuário:', err));
```

### Deletar um documento:
```
Usuario.deleteOne({ nome: 'Diego' })
  .then(() => console.log('Usuário deletado!'))
  .catch((err) => console.error('Erro ao deletar usuário:', err));
```


### Conclusão
O Mongoose facilita a gestão de dados no MongoDB, adicionando estrutura, validações e funcionalidades úteis para simplificar o desenvolvimento.

Essas operações CRUD (Criar, Ler, Atualizar, Deletar) são fundamentais para trabalhar com bancos de dados no Node.js. . Elas são a base para gerenciar dados em qualquer aplicação, desde sites simples até sistemas complexos. No contexto do Node.js, ferramentas como o Mongoose facilitam bastante a execução dessas operações ao interagir com um banco de dados MongoDB.

Aqui está o que torna essas operações essenciais no Node.js:
- Criar (Create): Inserir novos dados no banco, como adicionar um novo usuário ou produto.
- Ler (Read): Buscar e visualizar dados existentes, como listar todos os usuários ou um usuário específico.
- Atualizar (Update): Alterar dados já existentes no banco, como atualizar o e-mail de um usuário.
- Deletar (Delete): Remover dados desnecessários ou incorretos, como apagar registros duplicados.

Essas operações são implementadas usando métodos e bibliotecas específicas, como o Mongoose para MongoDB ou pacotes como `mysql2` e `pg` para outros bancos de dados.