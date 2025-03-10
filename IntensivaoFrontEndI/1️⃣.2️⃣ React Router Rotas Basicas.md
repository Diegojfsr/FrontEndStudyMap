
## 1.1 Criação de rotas simples e navegação entre elas. 

Rotas Simples no React referem-se à configuração básica de rotas em uma aplicação usando a biblioteca React Router. As rotas permitem que você navegue entre diferentes páginas ou componentes da sua aplicação com base na URL. Por exemplo, você pode criar uma rota para a página inicial, outra para a página de contato, e assim por diante.

No React Router, você usa o componente Route para definir essas rotas. Cada rota tem um caminho (path) e um componente associado que será renderizado quando o caminho for acessado. Aqui está um exemplo básico:

```
import { BrowserRouter, Routes, Route } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/sobre" element={<Sobre />} />
        <Route path="/contato" element={<Contato />} />
      </Routes>
    </BrowserRouter>
  );
}
```

Nesse exemplo:
1. O caminho / renderiza o componente Home.
2. O caminho /sobre renderiza o componente Sobre.
3. O caminho /contato renderiza o componente Contato.

Essa abordagem é chamada de "Rotas Simples" porque não envolve funcionalidades mais avançadas, como rotas aninhadas ou protegidas.


## 1.2 Uso do componente Link para navegação 

No React Router, o componente Link é usado para criar links de navegação dentro da sua aplicação, sem recarregar a página. Ele substitui a tag HTML `<a>` quando você está trabalhando com SPA (Single Page Applications), garantindo que a navegação seja mais rápida e eficiente.

Aqui está um exemplo básico do uso do Link:
```
import { Link } from "react-router-dom";

function Navbar() {
  return (
    <nav>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/sobre">Sobre</Link></li>
        <li><Link to="/contato">Contato</Link></li>
      </ul>
    </nav>
  );
}
```

Detalhes importantes:
1. Prop to: Define o caminho para onde o link deve direcionar o usuário (como o atributo href em `<a>`).
2. Navegação sem recarga: O Link utiliza a API de histórico do navegador para evitar recarregar a página, melhorando a performance.
3. Estilização: O Link é estilizado como qualquer outro elemento usando classes CSS ou estilos inline.

Exemplo do CSS:

```
nav ul li {
  display: inline;
  margin-right: 10px;
}

nav ul li a {
  text-decoration: none;
  color: blue;
```

Diferença entre Link e `<a>`:
1. Usar `<a>` em uma aplicação React Router causa uma nova solicitação HTTP e recarrega a página inteira, o que vai contra o conceito de SPA.
2. O Link mantém o estado da aplicação e permite transições suaves entre páginas.


## 1.3 Uso de navegação programática para redirecionamento.

A navegação programática no React Router permite que você redirecione os usuários para uma determinada rota usando código, em vez de depender de cliques em links. Isso pode ser útil em situações como após o envio de um formulário, autenticação de usuários, ou outras ações que exigem uma mudança de página.

No React Router v6, você pode usar o hook useNavigate para implementar a navegação programática. Aqui está um exemplo simples:

Exemplo básico:

```
import { useNavigate } from "react-router-dom";

function Login() {
  const navigate = useNavigate();

  const handleLogin = () => {
    // Lógica de autenticação (exemplo fictício)
    const isAuthenticated = true; // Imagine que o login foi bem-sucedido
    if (isAuthenticated) {
      navigate("/dashboard"); // Redireciona para o painel
    } else {
      alert("Usuário ou senha incorretos.");
    }
  };

  return (
    <div>
      <h1>Login</h1>
      <button onClick={handleLogin}>Entrar</button>
    </div>
  );
}
```

Como funciona:
1. **Importando** `useNavigate`**:** Esse hook do React Router fornece a função `navigate` para redirecionar o usuário.
2. **Chamando** `navigate`**:** Basta passar o caminho desejado para o `navigate("/caminho")`.
3. **Lógica condicional:** Você pode controlar para onde o usuário será redirecionado com base em condições específicas (ex.: autenticação).

## 1.4 Personalização adicional:

**Adicionar estado:** Você pode passar dados para a próxima página utilizando um objeto como segundo argumento, assim:
`navigate("/dashboard", { state: { userId: 123 } });`

**Navegação "para trás":** O mesmo hook também pode ser usado para navegar de volta na história do navegador:
`navigate(-1); // Retorna para a página anterior`

Essa abordagem é super flexível e é frequentemente usada em funcionalidades dinâmicas ou apps que exigem controle mais refinado sobre navegação.

