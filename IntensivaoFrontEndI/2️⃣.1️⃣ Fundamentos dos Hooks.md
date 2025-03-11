

## 1.1 Introdução aos Hooks: O que são e por que usar

Hooks são uma funcionalidade introduzida no React 16.8, que permitem usar o estado e outros recursos do React em componentes funcionais, sem a necessidade de converter esses componentes em classes. Antes disso, componentes funcionais eram limitados em funcionalidade e não podiam gerenciar estados ou ciclos de vida de forma direta.
### Por que usar Hooks?

Simplicidade: Com Hooks, você pode escrever componentes mais enxutos e legíveis, eliminando a complexidade associada a classes.
Reaproveitamento de lógica: Hooks permitem extrair e reutilizar lógica de estado entre diferentes componentes por meio de hooks personalizados.
Maior controle: Com hooks como useEffect e useRef, é possível gerenciar o ciclo de vida do componente e interagir diretamente com o DOM de maneira funcional.
Transição gradual: Você pode começar a usar hooks em partes do seu código sem precisar reescrever tudo.

### Exemplos de Hooks Comuns:

- useState: Para gerenciar estados locais.
- useEffect: Para lidar com efeitos colaterais, como chamadas de API.
- useContext: Para acessar valores de contexto.
- useReducer: Alternativa ao useState para gerenciar estados mais complexos.

Os hooks tornam o código mais organizado e favorecem boas práticas no desenvolvimento moderno com React.


## 1.2 Uso do hook useState para gerenciar estado

O `useState` é um hook no React que permite gerenciar o estado local em componentes funcionais. Ele facilita a criação e atualização de estados sem a necessidade de usar classes. Aqui está uma explicação mais detalhada:

### Como funciona o useState
O useState é usado para declarar uma "variável de estado" e uma função que pode atualizar esse estado. Quando você inicializa o useState, você passa o estado inicial como argumento, e ele retorna um array com dois itens:
- O valor atual do estado.
- Uma função para atualizar esse valor.

Sintaxe básica: 

```
import { useState } from 'react';

function Exemplo() {
  const [contador, setContador] = useState(0); // estado inicial: 0

  const incrementar = () => {
    setContador(contador + 1); // atualiza o estado
  };

  return (
    <div>
      <p>Contador: {contador}</p>
      <button onClick={incrementar}>Incrementar</button>
    </div>
  );
}
```

Explicação do exemplo
- Estado inicial: useState(0) define que o estado inicial de contador é 0.
- Função de atualização: setContador é usada para alterar o valor de contador.
- Renderização: Sempre que setContador é chamado, o componente é re-renderizado com o novo valor de estado.

### Benefícios do useState

- Simplicidade ao lidar com estados locais.
- Mantém o estado isolado em componentes funcionais.
- Funciona bem com outros hooks, como useEffect.



## 1.3 Uso do hook useEffect para efeitos colaterais.

O `useEffect` é um hook poderoso no React que permite lidar com **efeitos colaterais** em componentes funcionais. Efeitos colaterais incluem coisas como buscar dados de uma API, manipular o DOM, configurar assinaturas ou até mesmo limpar recursos (como timers). 
É essencial para controlar o comportamento de um componente além de sua simples renderização.

### Como funciona o useEffect

- O useEffect aceita duas partes principais:
- Uma função: Representa o código que você quer executar como efeito colateral.
- Um array de dependências (opcional): Determina quando o efeito será executado.

### Sintaxe básica

```
import { useState, useEffect } from 'react';

function Exemplo() {
  const [dados, setDados] = useState([]);

  useEffect(() => {
    // Efeito colateral: Buscar dados de uma API
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then((response) => response.json())
      .then((data) => setDados(data));
  }, []); // Array de dependências vazio - executa uma vez ao montar o componente

  return (
    <div>
      <h1>Posts:</h1>
      <ul>
        {dados.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}
```

Explicação do exemplo
- Efeito colateral: A função dentro do useEffect busca dados de uma API e atualiza o estado com os dados recebidos.
- Array de dependências: O [] vazio indica que o efeito será executado apenas uma vez, quando o componente for montado (comportamento semelhante ao componentDidMount em classes).

### Objetivo do Código
Esse componente React faz o seguinte:
- Usa o hook useState para armazenar um estado chamado dados.
- Usa o hook useEffect para buscar dados de uma API externa quando o componente é montado.
- Renderiza esses dados em uma lista de itens HTML `(<li>)`, exibindo o título de cada elemento.

### Explicação do Código 

Importação dos Hooks: Essa linha importa os hooks `useState` (para gerenciar estado) e `useEffect` (para lidar com efeitos colaterais, como buscar dados).
```
import { useState, useEffect } from 'react';
```

Declaração do Estado: Aqui, o `useState` é usado para criar uma variável de estado chamada `dados`, com valor inicial sendo um array vazio (`[]`). A função `setDados` é usada para atualizar o valor de `dados`.
```
const [dados, setDados] = useState([]);
```


Uso do useEffect: 
```
useEffect(() => {
  // Efeito colateral: Buscar dados de uma API
  fetch('https://jsonplaceholder.typicode.com/posts')
    .then((response) => response.json())
    .then((data) => setDados(data));
}, []);
```
O que acontece aqui:
- Função de Efeito: A função dentro do useEffect será executada após o componente ser montado na tela.
- Chamada da API: A função fetch faz uma requisição HTTP para a API no endereço 'https://jsonplaceholder.typicode.com/posts'.
- Resolução da Promessa: O método .then((response) => response.json()) transforma a resposta em formato JSON.
- Atualização do Estado: O resultado (um array de objetos) é passado para setDados, atualizando o estado dados.
- Dependências ([]): O array vazio indica que o useEffect executará o código apenas uma vez, quando o componente for montado.

Renderização dos Dados:
```
return (
  <div>
    <h1>Posts:</h1>
    <ul>
      {dados.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  </div>
);
```

Título da Lista: O elemento `<h1>` exibe o título "Posts:".
Exibição da Lista:
- O método .map() percorre o array dados e cria um item `<li>` para cada elemento.
- Cada item `<li>` exibe o title do objeto post.
- A propriedade key usa o id de cada objeto post para garantir uma identificação única de cada item (importante para eficiência do React).

### Comportamento Final

Quando o componente é montado:
- O useEffect é executado e busca os dados da API.
- Os dados recebidos são armazenados no estado dados.
- O componente é renderizado novamente, exibindo a lista dos títulos dos "posts" retornados pela API.

## 1.4 Casos de uso comuns do useEffect

- Chamada de API: Para buscar ou enviar dados.
- Manipulação do DOM: Alterar propriedades ou adicionar event listeners.
- Configuração de timers: Iniciar ou parar contadores.
- Limpeza de recursos: Remover assinaturas ou listeners para evitar vazamentos de memória.

### Limpeza de efeitos

Se o seu efeito criar algo que precise ser desmontado (como um event listener), você pode retornar uma função de limpeza dentro do useEffect:

```
useEffect(() => {
  const handleResize = () => console.log('Redimensionado!');
  window.addEventListener('resize', handleResize);

  // Limpeza do efeito ao desmontar ou atualizar
  return () => {
    window.removeEventListener('resize', handleResize);
  };
}, []); // Executa ao montar e limpa ao desmontar
```

Esse código utiliza o useEffect para configurar um efeito colateral em um componente React. Vamos detalhar passo a passo o que o código faz:

- Adiciona um event listener: O useEffect registra o evento resize no objeto global window. A função handleResize é chamada toda vez que a janela é redimensionada, exibindo no console a mensagem "Redimensionado!".
- Remove o event listener (limpeza): Quando o componente for desmontado ou o efeito precisar ser "atualizado", o React executa a função de limpeza retornada pelo useEffect. Nesse caso, o listener do evento resize é removido para evitar comportamentos indesejados ou vazamentos de memória.
- Executa apenas uma vez: O array vazio [] como dependência indica que o efeito será executado apenas ao montar o componente (uma vez) e será limpo ao desmontá-lo.

### Descrição detalhada do código de Limpeza de efeitos

Função do evento: Esta função é chamada sempre que o evento `resize` é disparado.
```
const handleResize = () => console.log('Redimensionado!');
```

Adicionando o listener: Isso vincula a função `handleResize` ao evento `resize` da janela.
```
window.addEventListener('resize', handleResize);
```

Função de limpeza: Ao desmontar o componente, o React chama essa função de retorno para remover o listener, prevenindo problemas como múltiplos eventos acumulados no `window`.
```
return () => {
  window.removeEventListener('resize', handleResize);
};
```

Controle de execução: O array vazio indica que este efeito não depende de variáveis externas, sendo configurado apenas na montagem do componente.
```
}, []);
```


A limpeza no useEffect é crucial para evitar:
- Vazamentos de memória: Listeners acumulados ocupam recursos desnecessários.
- Erros inesperados: Ao interagir com componentes desmontados.

Por fim, o `useEffect` é muito flexível e pode ser adaptado a diversas situações.


