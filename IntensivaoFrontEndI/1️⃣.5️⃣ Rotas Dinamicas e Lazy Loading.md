
## 1.1 Criação de rotas dinâmicas

Rotas dinâmicas no React Router permitem que você defina caminhos que incluem variáveis ou parâmetros, tornando possível trabalhar com URLs que mudam dinamicamente, como perfis de usuários, detalhes de produtos ou páginas baseadas em identificadores únicos.

### Como criar rotas dinâmicas

**Defina parâmetros de rota no caminho (**`path`**)**: No React Router, você pode usar `:parametro`

```
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Usuario from "./Usuario";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/usuario/:id" element={<Usuario />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

Nesse exemplo:
A rota /usuario/:id cria uma rota dinâmica onde :id é o parâmetro que será substituído por um valor real (ex.: /usuario/123).


Acesse os parâmetros dinâmicos com useParams: Use o hook useParams para capturar os valores dos parâmetros diretamente na URL. Exemplo do componente Usuario:

```
import { useParams } from "react-router-dom";

function Usuario() {
  const { id } = useParams(); // Captura o valor do parâmetro "id"

  return (
    <div>
      <h1>Perfil do Usuário</h1>
      <p>ID do usuário: {id}</p>
    </div>
  );
}

export default Usuario;
```

Se a URL for /usuario/456, o valor de id será 456.


### Exemplo com múltiplos parâmetros

Você pode definir mais de um parâmetro em uma rota. Por exemplo:

```
<Route path="/usuario/:id/projeto/:projetoId" element={<DetalhesDoProjeto />} />
```

E no componente:

```
import { useParams } from "react-router-dom";

function DetalhesDoProjeto() {
  const { id, projetoId } = useParams();

  return (
    <div>
      <h1>Projeto do Usuário</h1>
      <p>ID do usuário: {id}</p>
      <p>ID do projeto: {projetoId}</p>
    </div>
  );
}
```

### Casos de uso comuns

Páginas de detalhes: Páginas como /produto/123 para mostrar detalhes de produtos com base no ID.
Navegação personalizada: URLs que variam de acordo com entradas dinâmicas, como /usuario/:id/configuracoes.
APIs baseadas em IDs: Use os parâmetros capturados para buscar dados de uma API.

### Validação de parâmetros

Você pode adicionar validações ou lógicas para verificar se os parâmetros dinâmicos são válidos. Por exemplo:

```
function Usuario() {
  const { id } = useParams();

  if (isNaN(id)) {
    return <p>O ID do usuário não é válido.</p>;
  }

  return <p>ID do usuário: {id}</p>;
}
```

Rotas dinâmicas são muito úteis para criar aplicações flexíveis e interativas.


## 1.2 Carregamento preguiçoso de componentes(Lazy Loading)

O **Lazy Loading** (ou Carregamento Preguiçoso) é uma técnica usada no desenvolvimento de aplicações para carregar componentes ou recursos apenas quando eles são realmente necessários, em vez de carregar tudo no início. No contexto do React, isso significa que partes da interface (como componentes, páginas ou recursos) só são carregadas quando o usuário navega até elas, reduzindo o tempo inicial de carregamento da aplicação e melhorando a performance.

### Como funciona no React?

O React oferece suporte ao Lazy Loading de componentes através da função `React.lazy()` e do componente `Suspense`. Aqui está um exemplo básico:

```
import React, { Suspense } from "react";

// Lazy load do componente
const PaginaSobre = React.lazy(() => import("./PaginaSobre"));

function App() {
  return (
    <div>
      <h1>Bem-vindo à Aplicação</h1>
      <Suspense fallback={<div>Carregando...</div>}>
        <PaginaSobre />
      </Suspense>
    </div>
  );
}

export default App;
```

Explicação:
- React.lazy(): Usado para carregar dinamicamente um componente. Neste caso, PaginaSobre será carregado somente quando for renderizado.
- Suspense: Envolve o componente carregado dinamicamente e define um fallback (como um spinner ou mensagem) que será exibido enquanto o componente está sendo carregado.


### Benefícios do Lazy Loading:

Melhora a performance inicial: Apenas os recursos essenciais são carregados ao abrir a aplicação, reduzindo o tempo de carregamento inicial.
Carregamento sob demanda: Os componentes ou páginas só são carregados quando o usuário realmente acessa eles.
Otimização da experiência do usuário: A navegação é mais fluida, pois os recursos desnecessários não impactam o carregamento inicial.


### Uso comum de Lazy Loading:

Rotas em aplicações grandes: Em aplicações com muitas páginas, você pode carregar cada página apenas quando o usuário navegar para ela, usando React.lazy() junto com o React Router.

```
import React, { Suspense } from "react";
import { BrowserRouter, Routes, Route } from "react-router-dom";

const PaginaHome = React.lazy(() => import("./PaginaHome"));
const PaginaSobre = React.lazy(() => import("./PaginaSobre"));
const PaginaContato = React.lazy(() => import("./PaginaContato"));

function App() {
  return (
    <BrowserRouter>
      <Suspense fallback={<div>Carregando...</div>}>
        <Routes>
          <Route path="/" element={<PaginaHome />} />
          <Route path="/sobre" element={<PaginaSobre />} />
          <Route path="/contato" element={<PaginaContato />} />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}

export default App;
```

Imagens e recursos pesados: Carregue imagens, gráficos ou vídeos apenas quando forem visíveis no viewport do usuário (também conhecido como "lazy loading de mídia").

Bibliotecas grandes: Divida bibliotecas externas (como gráficos ou mapas) para carregar apenas nas páginas que utilizam essas funcionalidades.

O Lazy Loading é uma estratégia fundamental para construir aplicações modernas, especialmente para melhorar a experiência do usuário em dispositivos mais lentos ou com conexões limitadas.


## 1.3 Uso de Suspense e Error Boundaries para melhorar a experiência do usuário.

O React oferece dois recursos valiosos para melhorar a experiência do usuário: **Suspense** e **Error Boundaries**. Esses recursos são usados para lidar com o carregamento de componentes de forma mais elegante e capturar erros que possam ocorrer durante a renderização de uma aplicação.

Uso de Suspense:  O Suspense é usado para exibir uma interface de carregamento enquanto um componente ou recurso está sendo carregado. É particularmente útil em conjunto com o Lazy Loading de componentes.

Como funciona: O componente Suspense envolve partes da interface e exibe um conteúdo de fallback (como uma mensagem ou um spinner de carregamento) enquanto o React carrega os componentes necessários.

```
import React, { Suspense } from "react";

// Carregamento preguiçoso de um componente
const PaginaSobre = React.lazy(() => import("./PaginaSobre"));

function App() {
  return (
    <div>
      <h1>Bem-vindo à aplicação</h1>
      <Suspense fallback={<div>Carregando...</div>}>
        <PaginaSobre />
      </Suspense>
    </div>
  );
}

export default App;
```

Explicação:
- React.lazy(): Carrega dinamicamente o componente PaginaSobre apenas quando ele for necessário.
- Fallback: O conteúdo fornecido no atributo fallback será exibido enquanto o componente estiver sendo carregado (neste caso, a mensagem "Carregando...").

Esse comportamento melhora a experiência do usuário, garantindo que a aplicação continue funcional enquanto o conteúdo é carregado.


### Uso de Error Boundaries

Error Boundaries (Limites de Erro) são uma forma de capturar e tratar erros JavaScript que ocorrem durante a renderização de componentes. Eles evitam que um erro em parte da aplicação quebre toda a interface.

Como funciona: 
- Um componente de Error Boundary envolve outros componentes.
- Se um erro ocorrer nos componentes filhos, o Error Boundary exibe uma interface alternativa (como uma mensagem de erro) em vez de deixar a aplicação falhar.

```
import React from "react";

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Atualiza o estado para exibir a mensagem de fallback
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // Você pode registrar os erros em um serviço de monitoramento, como Sentry
    console.error("Erro capturado:", error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Ocorreu um erro. Por favor, tente novamente mais tarde.</h1>;
    }

    return this.props.children;
  }
}

function ComponenteQuePodeFalhar() {
  throw new Error("Algo deu errado!");
}

function App() {
  return (
    <ErrorBoundary>
      <ComponenteQuePodeFalhar />
    </ErrorBoundary>
  );
}

export default App;
```

Explicação:
- getDerivedStateFromError: Atualiza o estado interno do Error Boundary quando um erro é detectado.
- componentDidCatch: Permite registrar informações do erro para diagnóstico.
- Fallback: Exibe uma interface de fallback em caso de falha (como a mensagem "Ocorreu um erro").

### Quando usar:

Suspense: Para lidar com carregamento de componentes ou dados (especialmente com React.lazy e bibliotecas como React Query ou Relay).

Error Boundaries: Para capturar erros inesperados em seções específicas da aplicação, garantindo que o resto do aplicativo continue funcionando.

Melhorando a experiência do usuário:
Suspense garante que o carregamento seja suave e evita "brancos" na interface.
Error Boundaries evitam que a aplicação inteira falhe, exibindo mensagens amigáveis e ajudando no rastreamento de erros.

