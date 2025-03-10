
## 1.1 Implementação de rotas protegidas

A proteção de rotas é uma técnica usada em aplicações React (ou outras frameworks) para restringir o acesso a determinadas páginas ou áreas com base em condições específicas, como autenticação de usuários ou permissões.

Como funciona a proteção de rotas?
A ideia central é verificar se o usuário atende a certos critérios (como estar autenticado) antes de permitir que ele acesse uma rota específica. Caso contrário, ele pode ser redirecionado para uma página de login, erro ou qualquer outra página apropriada.
### Criar um componente para proteger rotas
O componente de rota protegida age como um "guarda", verificando as condições antes de renderizar a página.

```
import { Navigate } from "react-router-dom";

function RotaProtegida({ isAutenticado, children }) {
  if (!isAutenticado) {
    return <Navigate to="/login" replace />;
  }
  return children;
}

export default RotaProtegida;
```

Nesse exemplo:
- Se isAutenticado for false, o usuário será redirecionado para a rota /login.
- Se isAutenticado for true, a rota protegida será renderizada.

### Implementar em suas rotas

Você pode envolver as rotas protegidas com o componente `RotaProtegida`.

```
import { BrowserRouter, Routes, Route } from "react-router-dom";
import RotaProtegida from "./RotaProtegida";
import Login from "./Login";
import Dashboard from "./Dashboard";

function App() {
  const isAutenticado = false; // Alterne para `true` para testar.

  return (
    <BrowserRouter>
      <Routes>
        {/* Rota pública */}
        <Route path="/login" element={<Login />} />
        
        {/* Rota protegida */}
        <Route 
          path="/dashboard" 
          element={
            <RotaProtegida isAutenticado={isAutenticado}>
              <Dashboard />
            </RotaProtegida>
          } 
        />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

Conceitos principais:
- Navigate (React Router v6): Redireciona o usuário se ele não tiver permissão de acesso.
- Autenticação: O estado de autenticação (isAutenticado) geralmente é gerenciado por meio de uma API, estado global (como Redux), ou Context API no React.
- Flexibilidade: O componente RotaProtegida pode ser estendido para verificar permissões específicas (exemplo: isAdmin) ou outras condições.


### ### Proteção avançada:

Se precisar de proteção mais específica, como verificar papéis de usuário ou permissões de acesso:
```
function RotaProtegida({ isAutenticado, permissaoNecessaria, role, children }) {
  if (!isAutenticado) {
    return <Navigate to="/login" replace />;
  }
  if (role !== permissaoNecessaria) {
    return <Navigate to="/sem-acesso" replace />;
  }
  return children;
}
```

A proteção de rotas é essencial para aplicações modernas que lidam com usuários e dados sensíveis.


## 1.2 Controle de acesso a rotas com autenticação

O controle de acesso a rotas com autenticação é essencial para garantir que apenas usuários autorizados acessem determinadas áreas de sua aplicação, como painéis administrativos ou perfis pessoais. Ele combina a verificação de autenticação (se o usuário está logado) e, opcionalmente, a verificação de permissões (se o usuário tem direitos adequados para acessar um recurso específico).

### Implementação de Controle de Acesso em React
Criando uma Rota Protegida
Um componente de rota protegida verifica se o usuário está autenticado antes de permitir o acesso à página. Caso contrário, redireciona o usuário para uma página de login.
Exemplo de uma rota protegida básica:
```
import { Navigate } from "react-router-dom";

function RotaProtegida({ isAutenticado, children }) {
  if (!isAutenticado) {
    return <Navigate to="/login" replace />;
  }
  return children;
}

export default RotaProtegida;
```

Nesse exemplo:
- O estado de autenticação (isAutenticado) é recebido como uma prop.
- Se isAutenticado for false, o usuário será redirecionado para a rota /login.


### #### **Integração nas Rotas**

Envolva as rotas que precisam de proteção com o componente `RotaProtegida`.

```
import { BrowserRouter, Routes, Route } from "react-router-dom";
import RotaProtegida from "./RotaProtegida";
import Login from "./Login";
import Dashboard from "./Dashboard";

function App() {
  const isAutenticado = false; // Simulação do estado de autenticação

  return (
    <BrowserRouter>
      <Routes>
        {/* Rota Pública */}
        <Route path="/login" element={<Login />} />
        
        {/* Rota Protegida */}
        <Route 
          path="/dashboard" 
          element={
            <RotaProtegida isAutenticado={isAutenticado}>
              <Dashboard />
            </RotaProtegida>
          } 
        />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```


### Gerenciamento do Estado de Autenticação
O estado de autenticação pode ser gerenciado usando:
- React Context API: Para compartilhar o estado globalmente na aplicação.
- Redux: Para gerenciamento mais avançado de estado.
- API ou Local Storage: Para verificar tokens de sessão ou cookies.
Exemplo usando Context API:

```
import React, { createContext, useContext, useState } from "react";

// Criação do contexto
const AuthContext = createContext();

export function AuthProvider({ children }) {
  const [isAutenticado, setIsAutenticado] = useState(false);

  return (
    <AuthContext.Provider value={{ isAutenticado, setIsAutenticado }}>
      {children}
    </AuthContext.Provider>
  );
}

// Hook personalizado para usar autenticação
export function useAuth() {
  return useContext(AuthContext);
}
```

E no componente de proteção:

```
import { Navigate } from "react-router-dom";
import { useAuth } from "./AuthContext";

function RotaProtegida({ children }) {
  const { isAutenticado } = useAuth();

  if (!isAutenticado) {
    return <Navigate to="/login" replace />;
  }
  return children;
}
```


### Controle de Permissões

Para adicionar níveis de acesso ou permissões, expanda o componente de proteção:

```
function RotaProtegida({ isAutenticado, role, permissaoNecessaria, children }) {
  if (!isAutenticado) {
    return <Navigate to="/login" replace />;
  }
  if (role !== permissaoNecessaria) {
    return <Navigate to="/sem-acesso" replace />;
  }
  return children;
}
```

### Benefícios do Controle de Acesso

- **Segurança:** Protege informações sensíveis e impede acesso indevido.
- **Organização:** Mantém uma estrutura clara para páginas públicas e privadas.
- **Escalabilidade:** Facilmente adaptável para incluir diferentes níveis de permissão.


## 1.3 Uso de redirecionamento condicional

O redirecionamento condicional no React Router é uma técnica utilizada para direcionar o usuário dinamicamente para uma rota específica com base em uma condição. Essa abordagem é útil em situações como verificar autenticação, permissões ou qualquer lógica personalizada que determine para onde o usuário deve ser levado.

### Como funciona o redirecionamento condicional?
Usando o componente Navigate
O componente Navigate do React Router é usado para redirecionar programaticamente os usuários. Ele pode ser inserido diretamente dentro de um componente que verifica uma condição.

```
import { Navigate } from "react-router-dom";

function Painel({ isAutenticado }) {
  if (!isAutenticado) {
    return <Navigate to="/login" replace />;
  }

  return <h1>Bem-vindo ao Painel!</h1>;
}

export default Painel;
```
Nesse exemplo:
- Se isAutenticado for false, o usuário será redirecionado para a rota /login.
- Caso contrário, a mensagem "Bem-vindo ao Painel!" será exibida.

Usando o hook useNavigate
O hook useNavigate permite redirecionar o usuário programaticamente dentro de funções ou manipuladores de eventos.

```
import { useNavigate } from "react-router-dom";

function Login() {
  const navigate = useNavigate();

  const handleLogin = () => {
    const isAutenticado = true; // Simulação de login bem-sucedido
    if (isAutenticado) {
      navigate("/dashboard");
    }
  };

  return (
    <div>
      <h1>Login</h1>
      <button onClick={handleLogin}>Entrar</button>
    </div>
  );
}

export default Login;
```

Nesse exemplo:
- O usuário é redirecionado para /dashboard após a simulação de um login bem-sucedido.
- navigate("/dashboard") realiza o redirecionamento.


### Quando usar redirecionamento condicional?

Autenticação: Redirecionar usuários para a página de login caso não estejam autenticados.
Fluxos personalizados: Direcionar o usuário para a próxima etapa em um processo (ex.: formulário em etapas).
Permissões: Redirecionar usuários para uma página de erro ou área limitada caso não possuam as permissões necessárias.

Exemplo avançado: Proteção com redirecionamento condicional
Aqui está um exemplo completo de uso de redirecionamento condicional dentro de um componente de proteção:

```
import { Navigate } from "react-router-dom";

function RotaProtegida({ isAutenticado, children }) {
  if (!isAutenticado) {
    return <Navigate to="/login" replace />;
  }
  return children;
}

// Uso da RotaProtegida
function App() {
  const isAutenticado = false;

  return (
    <Routes>
      <Route path="/login" element={<Login />} />
      <Route 
        path="/dashboard" 
        element={
          <RotaProtegida isAutenticado={isAutenticado}>
            <Dashboard />
          </RotaProtegida>
        } 
      />
    </Routes>
  );
}
```

Essa abordagem é flexível e se adapta a muitas situações.

