

##  1.1 Introdução ao React Router: O que é e por que usar.

O **React Router** é uma excelente opção para gerenciamento de rotas em projetos **React**, permitindo uma navegação mais fluida e sem a necessidade de fazer novas solicitações ao servidor. Com ele, podemos criar rotas facilmente e criar a navegação entre as páginas do nosso aplicativo de forma simples e intuitiva.

---
## 1.2 Instalação e Configuração: Instale o React Router e configure seu ambiente de desenvolvimento.

O React se tornou a biblioteca JavaScript ideal para construir interfaces de usuário dinâmicas e interativas. Seja você um desenvolvedor experiente ou apenas iniciante, configurar seu ambiente de desenvolvimento para React é uma etapa inicial crucial para garantir uma experiência de desenvolvimento tranquila. Neste guia abrangente, nós o guiaremos por cada etapa em detalhes para configurar seu ambiente de desenvolvimento React.

### Instalação do Node.jse npm:

Primeiro, certifique-se de ter o Node.jse o npm instalados no seu sistema. Você pode baixar e instalar a versão mais recente do Node.js [aqui.](https://nodejs.org/pt)

### Criação de um novo projeto React:
Abra o terminal e navegue até o diretório onde deseja criar seu projeto. Em seguida, execute o comando:
```
npm create vite@latest my-react-app --template
```

### Navegar até o diretório do projeto:
Após a criação do projeto, navegue até o diretório do projeto:
```
cd my-react-app
```

### Instalação do React Router:

Para instalar o React Router, execute o comando:
```
npm install react-router-dom
```


## 1.3 Primeira Rota: Crie uma rota básica e entenda como funciona.

### Configuração do React Router:

**Configure o React Router:** Crie um arquivo `src/routes.tsx` com o seguinte conteúdo:
```
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Home from './components/Home';
import About from './components/About';

const AppRoutes: React.FC = () => {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Router>
  );
};

export default AppRoutes;

```

Crie os componentes Home e About: Crie um arquivo src/components/Home.tsx com o seguinte conteúdo:

```
import React from 'react';

const Home: React.FC = () => {
  return <h2>Home Page</h2>;
};

export default Home;
```

Crie um arquivo src/components/About.tsx com o seguinte conteúdo:

```
import React from 'react';

const About: React.FC = () => {
  return <h2>About Page</h2>;
};

export default About;
;
```

Integre as rotas na sua aplicação principal: No arquivo src/main.tsx, substitua o conteúdo por:

```
import React from 'react';
import ReactDOM from 'react-dom/client';
import AppRoutes from './routes';
import './index.css';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <AppRoutes />
  </React.StrictMode>
);
```

Inicie o servidor de desenvolvimento:

```
npm run dev
```

Após iniciar o servidor de desenvolvimento com npm run dev, você poderá acessar sua aplicação no navegador. Por padrão, Vite inicia o servidor em http://localhost:3000. 
Aqui estão os passos para visualizar as rotas:

Acesse a rota inicial:
Abra seu navegador e vá para http://localhost:3000. Isso deve exibir o componente Home, que tem o conteúdo "Home Page".

Acesse a rota "About":
No navegador, altere a URL para http://localhost:3000/about. Isso deve exibir o componente About, que tem o conteúdo "About Page".

### Extra:
Além disso, lembre-se de que você pode adicionar links de navegação para facilitar a transição entre as páginas. Por exemplo, você pode adicionar um menu de navegação no seu arquivo `src/routes.tsx`:

```
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom';
import Home from './components/Home';
import About from './components/About';

const AppRoutes: React.FC = () => {
  return (
    <Router>
      <nav>
        <ul>
          <li><Link to="/">Home</Link></li>
          <li><Link to="/about">About</Link></li>
        </ul>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Router>
  );
};

export default AppRoutes;
```


