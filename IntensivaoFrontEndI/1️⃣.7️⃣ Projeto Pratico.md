
## 1.1 Crie um pequeno projeto para aplicar o que aprendeu.

Aqui está um projeto com React, TypeScript, Vite, e utilizando rotas com links de navegação para páginas como "Home", "Sobre" e "Contato".

### Passo 1: Criar o Projeto com Vite
Execute o comando para criar o projeto:

```
npm create vite@latest meu-projeto -- --template react-ts
```
Aqui, --template react-ts configura o projeto com React e TypeScript.

Acesse o diretório do projeto:
```
cd meu-projeto
```

Instale as dependências:
```
npm install
```


### Passo 2: Instalar o React Router
Instale o React Router para lidar com as rotas da aplicação:
```
npm install react-router-dom
```

### Passo 3: Estruturar o Projeto
Dentro do diretório do projeto, crie uma estrutura básica de arquivos:
```
src/
├── components/
│   ├── Navbar.tsx
├── pages/
│   ├── Home.tsx
│   ├── Sobre.tsx
│   ├── Contato.tsx
├── App.tsx
├── main.tsx
```

### Passo 4: Criar os Componentes
Navbar.tsx: Crie um componente de navegação para mudar entre as páginas:

```
import { Link } from "react-router-dom";

const Navbar = () => {
  return (
    <nav>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/sobre">Sobre</Link></li>
        <li><Link to="/contato">Contato</Link></li>
      </ul>
    </nav>
  );
};

export default Navbar;
```

Home.tsx: Página inicial:

```
const Home = () => {
  return (
    <div>
      <h1>Bem-vindo à Home</h1>
      <p>Esta é a página inicial da aplicação.</p>
    </div>
  );
};

export default Home;
```

Sobre.tsx: Página "Sobre":

```
const Sobre = () => {
  return (
    <div>
      <h1>Sobre</h1>
      <p>Esta é a página sobre o projeto.</p>
    </div>
  );
};

export default Sobre;
```

Contato.tsx: Página "Contato":

```
const Contato = () => {
  return (
    <div>
      <h1>Contato</h1>
      <p>Entre em contato conosco através desta página.</p>
    </div>
  );
};

export default Contato;
```

### Passo 5: Configurar o React Router
No arquivo App.tsx, configure as rotas:

```
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Navbar from "./components/Navbar";
import Home from "./pages/Home";
import Sobre from "./pages/Sobre";
import Contato from "./pages/Contato";

const App = () => {
  return (
    <BrowserRouter>
      <Navbar />
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/sobre" element={<Sobre />} />
        <Route path="/contato" element={<Contato />} />
      </Routes>
    </BrowserRouter>
  );
};

export default App;
```

No arquivo **main.tsx**, renderize o componente `App`:

```
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./index.css";

ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

### Passo 6: Estilizar a Navegação (Opcional)
Adicione estilos básicos no arquivo index.css:
```
nav ul {
  display: flex;
  list-style: none;
  padding: 0;
}

nav ul li {
  margin-right: 10px;
}

nav ul li a {
  text-decoration: none;
  color: blue;
}

nav ul li a:hover {
  text-decoration: underline;
}
```

### Passo 7: Iniciar o Servidor
Execute o comando para rodar o projeto:

```
npm run dev
```

Acesse o projeto no navegador em `http://localhost:5173`.
Você terá uma aplicação com três páginas: "Home", "Sobre" e "Contato".
O usuário pode navegar entre essas páginas usando os links na barra de navegação.


