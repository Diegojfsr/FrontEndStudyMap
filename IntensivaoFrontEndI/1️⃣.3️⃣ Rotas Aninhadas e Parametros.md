
## 1.1 Criação de rotas aninhadas 

Rotas aninhadas no React Router permitem que você organize e navegue por páginas que possuem hierarquias ou relações de subpáginas. Elas permitem que você defina um componente "pai" que contenha um layout geral, enquanto os componentes "filhos" representam diferentes seções ou conteúdos dentro desse layout.
Exemplo básico de rotas aninhadas:

```
import { BrowserRouter, Routes, Route } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Rota Pai */}
        <Route path="/dashboard" element={<Dashboard />}>
          {/* Rotas Filhas */}
          <Route path="perfil" element={<Perfil />} />
          <Route path="configuracoes" element={<Configuracoes />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

function Dashboard() {
  return (
    <div>
      <h1>Painel</h1>
      <nav>
        <a href="/dashboard/perfil">Perfil</a>
        <a href="/dashboard/configuracoes">Configurações</a>
      </nav>
      {/* Renderiza os filhos */}
      <Outlet />
    </div>
  );
}

function Perfil() {
  return <h2>Perfil do Usuário</h2>;
}

function Configuracoes() {
  return <h2>Configurações</h2>;
}
```

Como funciona:
1. Componente Pai (Dashboard): Define o layout principal (menu de navegação, cabeçalho, etc.).
2. Outlet: É um placeholder que renderiza os componentes filhos da rota atual.
3. Rotas Filhas: As rotas dentro do Route pai (/dashboard) herdam o layout e são renderizadas dentro do Outlet.
4. Caminho Dinâmico: As rotas filhas adicionam apenas o "sufixo" ao caminho do pai, como /dashboard/perfil ou /dashboard/configuracoes.

Benefícios:
- Compartilhamento de layouts comuns entre diferentes páginas.
- Código mais organizado e reutilizável.


## 1.2 Trabalhando com parâmetros de rota.

No React Router, você pode usar parâmetros de rota para capturar valores dinâmicos diretamente da URL. Esses parâmetros são úteis para criar rotas dinâmicas, como páginas de perfil de usuários, detalhes de produtos, etc.
Exemplo básico de parâmetros de rota:

### Configurar a rota com parâmetros dinâmicos: 

Você pode usar :parametro no caminho da rota para definir parâmetros dinâmicos.

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
```

No exemplo acima, :id é um parâmetro de rota que pode variar (por exemplo, /usuario/1, /usuario/2).

### Acessar o parâmetro dentro do componente: 

Para acessar o valor do parâmetro, use o hook useParams.

```
import { useParams } from "react-router-dom";

function Usuario() {
  const { id } = useParams();

  return (
    <div>
      <h1>Detalhes do Usuário</h1>
      <p>ID do usuário: {id}</p>
    </div>
  );
}

export default Usuario;
```

Nesse exemplo:
- O useParams retorna um objeto com os parâmetros da rota.
- O valor de :id será capturado e exibido no componente.



### Trabalhar com múltiplos parâmetros

Se precisar de mais de um parâmetro, basta adicioná-los ao caminho da rota:`
```
<Route path="/usuario/:id/acao/:acaoId" element={<UsuarioAcao />} />
```
E dentro do componente:
```
function UsuarioAcao() {
  const { id, acaoId } = useParams();
  return (
    <div>
      <h1>Ação do Usuário</h1>
      <p>ID do usuário: {id}</p>
      <p>ID da ação: {acaoId}</p>
    </div>
  );
}
```

### Nota sobre validação

Se o parâmetro não estiver presente ou for inválido, você pode implementar validações ou redirecionar o usuário.
```
if (!id) {
  navigate("/erro");
}
```



## 1.3 Uso de hooks como useParams e useRouteMatch

Os hooks useParams e useRouteMatch são ferramentas poderosas no React Router que ajudam a lidar com parâmetros de rota e a verificar se uma rota corresponde a um padrão específico.

### Trabalhando com parâmetros dinâmicos

O hook useParams é usado para acessar os valores dos parâmetros dinâmicos definidos na URL da rota. Ele retorna um objeto contendo os parâmetros como pares chave-valor.

```
import { BrowserRouter, Routes, Route, useParams } from "react-router-dom";

function Usuario() {
  const { id } = useParams(); // Captura o valor do parâmetro "id" da URL

  return (
    <div>
      <h1>Usuário</h1>
      <p>ID do usuário: {id}</p>
    </div>
  );
}

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/usuario/:id" element={<Usuario />} />
      </Routes>
    </BrowserRouter>
  );
}
```

Nesse exemplo:
- A rota /usuario/:id define um parâmetro dinâmico chamado id.
- Se você acessar /usuario/123, o componente Usuario exibirá ID do usuário: 123.
Quando usar:
Para capturar informações dinâmicas diretamente da URL, como IDs de usuários, produtos, etc.

### Verificando correspondência de rotas
O useRouteMatch era usado no React Router v5 para verificar se a URL atual correspondia a uma rota específica ou padrão. No React Router v6, esse hook foi substituído em grande parte pela nova API de roteamento, mas ainda é relevante se você estiver usando a versão 5.

```
import { useRouteMatch } from "react-router-dom";

function Dashboard() {
  const match = useRouteMatch("/dashboard");

  return (
    <div>
      <h1>Dashboard</h1>
      {match && <p>A URL atual corresponde à rota /dashboard.</p>}
    </div>
  );
}
```

Nesse exemplo:
O useRouteMatch verifica se a URL atual corresponde ao padrão /dashboard.
O que ele retorna: O useRouteMatch retorna um objeto com informações sobre a correspondência, incluindo:
- path: O caminho da rota.
- url: A URL atual.
- Parâmetros capturados (se houver).

Substituto no React Router v6:
No React Router v6, o `useRouteMatch` foi substituído por outras ferramentas como:
- `useLocation`: Para acessar a localização atual.
- `matchPath`: Para verificar se uma rota corresponde a um padrão.

### Conclusão:

Use useParams para capturar parâmetros dinâmicos da URL.
Use useRouteMatch (ou alternativas no v6) para verificar a correspondência de rotas ou padrões.
