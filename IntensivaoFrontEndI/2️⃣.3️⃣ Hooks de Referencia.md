
## 1.1 Uso do hook useRef para acessar elementos DOM e armazenar valores mutáveis

O useRef é um hook do React muito útil que permite manipular diretamente elementos do DOM e armazenar valores mutáveis que não disparam uma re-renderização do componente quando alterados. Aqui está como ele funciona e alguns exemplos de uso:

Acessar elementos do DOM:
O useRef pode ser usado para criar uma referência a um elemento DOM, permitindo que você o manipule diretamente.

```
import { useRef } from "react";

function MeuComponente() {
  const inputRef = useRef(null);

  const focarInput = () => {
    inputRef.current.focus(); // Acessa o input diretamente
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focarInput}>Focar no Input</button>
    </div>
  );
}
```

Neste exemplo, usamos o useRef para criar uma referência a um campo de entrada (input). Com isso, conseguimos chamá-lo diretamente e utilizar métodos nativos do DOM, como focus().


Armazenar valores mutáveis:
Outra aplicação importante do useRef é manter valores mutáveis que não causam renderizações adicionais do componente. Isso é útil para rastrear estados que mudam frequentemente (como contadores ou dados de temporizadores).

```
import { useRef, useEffect } from "react";

function Temporizador() {
  const contadorRef = useRef(0);

  useEffect(() => {
    const intervalo = setInterval(() => {
      contadorRef.current += 1;
      console.log(`Contador: ${contadorRef.current}`);
    }, 1000);

    return () => clearInterval(intervalo); // Limpeza ao desmontar
  }, []);

  return <p>Confira o console para ver o contador</p>;
}
```

Aqui, usamos o useRef para armazenar um contador que é atualizado a cada segundo, sem disparar renderizações do componente.

Resumo das vantagens:
- Não dispara renderizações: As mudanças em ref.current não causam re-renders, diferente de usar o useState.
- Manipulação do DOM: Facilita o acesso direto a elementos DOM, substituindo o uso de document.querySelector.
- Preservação entre renderizações: Os valores armazenados em um useRef permanecem consistentes entre renderizações.


## 1.2 Uso do hook useMemo para memorização de valores

O useMemo é um hook do React que serve para memorizar valores derivados de cálculos custosos, de forma a evitar reexecuções desnecessárias toda vez que o componente renderiza. Ele ajuda a melhorar o desempenho de aplicações ao só recalcular o valor memorizado quando as dependências especificadas mudarem.

Sintaxe do useMemo: 

```
const valorMemorizado = useMemo(() => calcularAlgoComplexo(parametros), [dependencias]);
```

- Função de cálculo: A primeira parte do useMemo é uma função que executa o cálculo ou lógica que queremos memorizar.
- Array de dependências: O React só vai executar a função novamente se algum valor dentro do array de dependências mudar.

Exemplo Prático:
Imagine que temos uma lista longa de números e queremos calcular os números pares toda vez que a lista muda. Esse cálculo pode ser custoso, e recalculá-lo em todas as renderizações seria ineficiente.

```
import React, { useMemo, useState } from "react";

function ListaNumeros() {
  const [numeros, setNumeros] = useState([1, 2, 3, 4, 5, 6]);
  const [filtro, setFiltro] = useState("");

  // Memorizar os números pares
  const numerosPares = useMemo(() => {
    console.log("Calculando números pares...");
    return numeros.filter((num) => num % 2 === 0);
  }, [numeros]); // Só recalcula se "numeros" mudar

  return (
    <div>
      <h3>Números Pares:</h3>
      <ul>
        {numerosPares.map((num) => (
          <li key={num}>{num}</li>
        ))}
      </ul>
      <button onClick={() => setNumeros([...numeros, numeros.length + 1])}>
        Adicionar Número
      </button>
      <input
        placeholder="Teste de filtro (não afeta números pares)"
        value={filtro}
        onChange={(e) => setFiltro(e.target.value)}
      />
    </div>
  );
}
```

Neste exemplo:
- Função custosa: O cálculo dos números pares, numeros.filter(), é encapsulado no useMemo.
- Dependências: O cálculo só será refeito se o estado numeros mudar. Alterar o filtro não disparará a reexecução, pois ele não está nas dependências.

Vantagens do useMemo: 
- Otimização de performance: Reduz cálculos desnecessários em renderizações.
- Memorização condicionada: O valor é recalculado somente quando necessário.
- Adequado para cálculos pesados: Muito útil ao trabalhar com operações complexas ou grandes volumes de dados.


## 1.3 Diferenças entre useEffect e useLayoutEffect

Tanto o `useEffect` quanto o `useLayoutEffect` são hooks do React que permitem executar efeitos colaterais em componentes funcionais. No entanto, a diferença está **no momento em que são executados** no ciclo de vida do React.

### useEffect

- Quando é executado? Após a renderização do componente e após as alterações serem aplicadas ao DOM.
- Uso principal: Ideal para tarefas que não bloqueiam a pintura da interface, como:
- Buscar dados de uma API.
- Configurar assinaturas (event listeners, por exemplo).
- Atualizar estados que não afetam imediatamente o layout do componente.
- Comportamento: Não interfere no desenho do navegador; ou seja, a interface do usuário é atualizada antes que o código no useEffect seja executado.

Exemplo:

```
useEffect(() => {
  console.log("Efeito executado após a renderização!");
}, []);
```


### useLayoutEffect

- Quando é executado? Antes do navegador finalizar a pintura da tela, mas após o DOM ser atualizado.
- Uso principal: Ideal para tarefas que afetam o layout ou precisam ocorrer imediatamente após a atualização do DOM, como:
- Medir dimensões ou posicionamento de elementos no DOM.
- Manipular diretamente o DOM para sincronizar com os cálculos do layout.
- Comportamento: Pode bloquear a pintura da interface, então deve ser usado com cuidado para evitar degradação de desempenho.

Exemplo:

```
useLayoutEffect(() => {
  console.log("Efeito executado antes da pintura da tela!");
}, []);
```


### Diferenças em Resumo

| **Aspecto**           | `useEffect`                        | `useLayoutEffect`                                  |
| --------------------- | ---------------------------------- | -------------------------------------------------- |
| **Execução**          | Após a renderização e pintura      | Antes da pintura, logo após o DOM                  |
| **Impacto no Layout** | Não afeta o layout diretamente     | Afeta o layout e pode bloquear a pintura           |
| **Uso principal**     | Tarefas assíncronas ou secundárias | Manipulação síncrona que depende do DOM atualizado |

### Quando usar cada um?

- Use ` useEffect` para a maior parte dos casos, pois ele não bloqueia a renderização.
- Use `useLayoutEffect` apenas quando precisa garantir que os cálculos ou atualizações estejam sincronizados com o layout.
