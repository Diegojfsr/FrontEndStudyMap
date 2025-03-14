

## 1.1 Projeto usando o hook `useState` no React para gerenciar o estado em um projeto

### Passo 1: Configurar o ambiente de desenvolvimento com Vite e TypeScript

Abra o terminal e execute:
``` npm create vite@latest contador-app -- --template react-ts```
Isso criará um projeto React com suporte a TypeScript.

Entre no diretório do projeto:
``` cd contador-app ```

Instale as dependências:
``` npm install ```

Inicie o servidor de desenvolvimento:
``` npm run dev ```

### Passo 2: Criar o componente Contador

#### Crie um novo arquivo chamado Contador.tsx na pasta src:
Código do componente:
```
import React, { useState } from "react";

const Contador: React.FC = () => {
  const [contador, setContador] = useState<number>(0); // Estado tipado como número

  const aumentar = () => setContador(contador + 1);
  const diminuir = () => setContador(contador - 1);

  return (
    <div style={{ textAlign: "center", marginTop: "50px" }}>
      <h1>Contador: {contador}</h1>
      <button onClick={aumentar}>Aumentar</button>
      <button onClick={diminuir}>Diminuir</button>
    </div>
  );
};

export default Contador;
```
Explicação detalhada do código.

Importação de módulos
```
import React, { useState } from "react";
```
- React: Importa a biblioteca principal para criar componentes e interfaces.
- useState: Um hook do React usado para gerenciar o estado de componentes funcionais. Aqui, ele será usado para armazenar e atualizar o valor do contador.

Definição do Componente Contador
```
const Contador: React.FC = () => { ... }
```
- const Contador: Define o componente funcional Contador.
- React.FC: Significa React Functional Component, e é utilizado para tipar o componente em projetos TypeScript.

Gerenciamento de Estado
```
const [contador, setContador] = useState<number>(0);
```
useState`<number>(0)`: Inicializa o estado contador com o valor 0, e o tipo do estado é number.
- contador: A variável que contém o estado atual.
- setContador: A função usada para atualizar o valor de contador.

Funções para Atualizar o Estado
```
const aumentar = () => setContador(contador + 1);
const diminuir = () => setContador(contador - 1);
```
- aumentar: Incrementa o valor do estado contador em 1.
- diminuir: Decrementa o valor do estado contador em 1.
- As funções usam setContador para alterar o estado, o que automaticamente re-renderiza o componente e atualiza a interface.

Retorno do JSX
```
return (
  <div style={{ textAlign: "center", marginTop: "50px" }}>
    <h1>Contador: {contador}</h1>
    <button onClick={aumentar}>Aumentar</button>
    <button onClick={diminuir}>Diminuir</button>
  </div>
);
```
Estrutura JSX: Define como o componente será exibido no navegador.
- `<div>`: Um contêiner para centralizar o conteúdo.
- `style={{ textAlign: "center", marginTop: "50px" }}`: Estilo inline para centralizar o texto e adicionar espaço superior.
- `<h1>`: Mostra o valor atual do contador (estado).
- `<button>`: Cada botão tem um evento de clique que chama as funções `aumentar` e `diminuir`

Exportação do Componente
```
export default Contador;
```
Permite que o componente seja importado e usado em outros arquivos do projeto.

Resumo
O estado é gerenciado pelo hook useState, começando em 0.
As funções aumentar e diminuir atualizam o estado e causam a re-renderização do componente.
O JSX define a interface com os botões e o valor do contador centralizado.

#### Atualize o arquivo App.tsx:
Substitua o conteúdo por:
```
import React from "react";
import Contador from "./Contador";

const App: React.FC = () => {
  return (
    <div>
      <Contador />
    </div>
  );
};

export default App;
```
Explicação detalhada do código

Importação de módulos
```
import React from "react";
import Contador from "./Contador";
```
- import React from "react": Importa a biblioteca React necessária para criar o componente App.
- import Contador from "./Contador": Importa o componente Contador do arquivo Contador.tsx ou Contador.jsx localizado na mesma pasta (./). O componente Contador será usado dentro de App.

Definição do componente App
```
const App: React.FC = () => {
  return (
    <div>
      <Contador />
    </div>
  );
};
```
- const App: Define o componente funcional chamado App.
- React.FC: Um tipo do TypeScript que significa React Functional Component (Componente Funcional do React). Isso ajuda a garantir que o componente segue as regras do React e melhora a autocompletação no editor de código.
- return: Indica o que será exibido no navegador.
- `<div>`: Um contêiner HTML que organiza o conteúdo do componente.
- `<Contador />`: Aqui o componente Contador é renderizado. Ele herda suas funcionalidades do código que você definiu anteriormente em Contador.tsx.

Exportação do Componente
```
export default App;
```
Exportação padrão: Permite que o componente App seja importado e usado em outros arquivos do projeto, como no ponto de entrada principal do React, geralmente main.tsx ou index.tsx.

Fluxo geral do código
- O componente App é o componente principal que organiza outros componentes dentro de si.
- Neste caso, o componente App usa o componente Contador, tornando-o parte da interface do aplicativo.
- Quando o aplicativo é iniciado, o navegador renderiza o conteúdo do componente App, que por sua vez mostra o componente Contador e suas funcionalidades.

### Passo 3: Estilizar o projeto (opcional)
Crie um arquivo CSS chamado Contador.css:
```
button {
  margin: 10px;
  padding: 10px 20px;
  font-size: 16px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
```

Importe o CSS no componente Contador.tsx:
```
import './Contador.css';
```

### Passo 4: Testar o projeto
Execute npm run dev novamente se necessário e veja o projeto funcionando no navegador.






