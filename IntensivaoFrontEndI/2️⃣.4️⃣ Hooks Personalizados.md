

Hooks personalizados no React são funções JavaScript que permitem reutilizar lógica entre diferentes componentes. Eles são chamados de "personalizados" porque você cria os seus próprios hooks além dos já fornecidos pelo React, como `useState` ou `useEffect`.

Esses hooks seguem as mesmas regras dos hooks padrão (por exemplo, devem começar com a palavra “use” e só podem ser chamados em componentes de função ou outros hooks). Sua principal utilidade é encapsular funcionalidades repetitivas em uma única função, tornando seu código mais limpo, modular e fácil de manter.

Exemplo de usos comuns:
- Gerenciar estados complexos compartilhados por vários componentes.
- Manipular assinaturas de eventos, como eventos de scroll ou resize.
- Fazer chamadas de API, com lógica de carregamento e tratamento de erros.
- Controlar formulários ou validar dados.

Benefícios:
- Reutilização: Evita duplicação de código.
- Organização: A lógica fica encapsulada e fácil de entender.
- Manutenção: Alterações na lógica só precisam ser feitas em um lugar.

## 1.1 Como criar seus próprios hooks

Criar seus próprios hooks pode ser uma ótima maneira de organizar e reutilizar lógica em projetos de React. Hooks personalizados permitem encapsular comportamentos e compartilhar entre componentes. 
Aqui está um guia simples para criar um hook:

- Identifique a Lógica Reutilizável: Escolha uma parte da lógica que deseja extrair. Por exemplo, controle de estado, manipulação de eventos ou chamadas de API.
- Crie um Arquivo para o Hook: Por convenção, salve o hook em um arquivo separado, como useMeuHook.js, na pasta hooks.
- Implemente o Hook: O nome do hook deve começar com "use" para seguir as regras do React e garantir que ele funcione corretamente.

Aqui está um exemplo de hook para gerenciar o estado da janela do navegador:

```
import { useState, useEffect } from "react";

function useJanelaTamanho() {
  const [tamanho, setTamanho] = useState({
    largura: window.innerWidth,
    altura: window.innerHeight,
  });

  useEffect(() => {
    function atualizarTamanho() {
      setTamanho({
        largura: window.innerWidth,
        altura: window.innerHeight,
      });
    }

    window.addEventListener("resize", atualizarTamanho);
    return () => window.removeEventListener("resize", atualizarTamanho);
  }, []);

  return tamanho;
}

export default useJanelaTamanho;
```

Use o Hook nos Componentes: Agora você pode usar seu hook personalizado em qualquer componente.

```
import useJanelaTamanho from "./hooks/useJanelaTamanho";

function Componente() {
  const { largura, altura } = useJanelaTamanho();

  return (
    <div>
      <p>Largura: {largura}</p>
      <p>Altura: {altura}</p>
    </div>
  );
}
```


### O que o Hook Faz:

Ele monitora as dimensões da janela (largura e altura) e atualiza essas informações automaticamente toda vez que o tamanho da janela muda.

Estado Inicial (useState): 
- Usamos o React useState para criar um estado chamado tamanho, que armazena a largura e a altura iniciais da janela.
- No início, as dimensões são definidas pelo tamanho atual da janela com window.innerWidth e window.innerHeight.
```
const [tamanho, setTamanho] = useState({
  largura: window.innerWidth,
  altura: window.innerHeight,
});
```

Monitoramento das Mudanças com useEffect:
- A função useEffect serve para configurar e limpar o monitoramento das mudanças de tamanho da janela.
- Dentro do useEffect, definimos uma função chamada atualizarTamanho que atualiza o estado (tamanho) com as novas dimensões da janela.
```
useEffect(() => {
  function atualizarTamanho() {
    setTamanho({
      largura: window.innerWidth,
      altura: window.innerHeight,
    });
  }
```

Adicionando e Removendo o Listener:
- Usamos window.addEventListener para ouvir o evento resize (disparado quando o tamanho da janela muda) e conectar a função atualizarTamanho.
- No retorno do useEffect, limpamos o listener com window.removeEventListener para evitar vazamento de memória quando o componente é desmontado.
```
window.addEventListener("resize", atualizarTamanho);
return () => window.removeEventListener("resize", atualizarTamanho);
```

Retorno do Hook:
Finalmente, o hook retorna o estado atual (tamanho), que contém as dimensões mais recentes da janela. Isso permite que os componentes que usam o hook tenham acesso direto às dimensões.
```
return tamanho;
```


Benefício:
Você encapsula toda a lógica para gerenciar o tamanho da janela dentro do hook, deixando os componentes mais limpos e focados apenas em como exibir os dados.


## 1.2 Uso de hooks personalizados para reutilizar lógica


Como já foi dito acima, um hook personalizado nada mais é do que uma função JavaScript que começa com o prefixo `use` e pode usar outros hooks internos como `useState`, `useEffect`, etc. Ele encapsula lógica complexa ou repetitiva e fornece resultados ou comportamentos aos componentes.

Exemplo Prático:
Vamos criar um hook personalizado chamado useContador que gerencia um contador.

```
import { useState } from "react";

function useContador(valorInicial = 0) {
  const [contador, setContador] = useState(valorInicial);

  const incrementar = () => setContador(contador + 1);
  const decrementar = () => setContador(contador - 1);
  const resetar = () => setContador(valorInicial);

  return { contador, incrementar, decrementar, resetar };
}

export default useContador;
```

Como Usar: 
Agora podemos utilizar o hook em um componente para gerenciar o estado do contador.

```
import React from "react";
import useContador from "./hooks/useContador";

function MeuComponente() {
  const { contador, incrementar, decrementar, resetar } = useContador(10);

  return (
    <div>
      <p>Contador: {contador}</p>
      <button onClick={incrementar}>Incrementar</button>
      <button onClick={decrementar}>Decrementar</button>
      <button onClick={resetar}>Resetar</button>
    </div>
  );
}

export default MeuComponente;
```


### O que o useContador faz:

Gera um estado (contador) para armazenar o valor atual do contador.
Fornece funções para: 
- Incrementar o contador.
- Decrementar o contador.
- Resetar o contador para o valor inicial.


### Explicação Detalhada:

Estado Inicial com useState:
- O useState cria o estado contador e define seu valor inicial como valorInicial.
- Exemplo: Se você passar 10 como argumento para o hook, o estado começará em 10.

```
const [contador, setContador] = useState(valorInicial);
```

Funções de Manipulação:
- incrementar: Adiciona 1 ao valor atual do contador.
- decrementar: Subtrai 1 do valor atual do contador.
- resetar: Restaura o contador ao valor inicial.

Retorno do Hook:
- O hook retorna um objeto contendo o valor atual do contador (contador) e as funções (incrementar, decrementar, resetar) para controlá-lo.
- Isso permite que outros componentes utilizem o hook facilmente.

```
return { contador, incrementar, decrementar, resetar };
```

### Como Funciona:

- Quando você clica em "Incrementar", a função incrementar é chamada, somando 1 ao estado atual do contador.
- Ao clicar em "Decrementar", a função decrementar subtrai 1 do valor atual.
- O botão "Resetar" redefine o contador ao valor inicial especificado ao criar o hook (neste caso, 5).

### Por que usar este hook?

- Reutilizável: Qualquer componente que precise de um contador pode usar este hook.
- Fácil de entender: A lógica do contador está encapsulada no hook, deixando o componente mais limpo.
- Flexível: Você pode passar um valor inicial diferente para cada uso do hook.



## 1.2 Criação de alguns hooks personalizados.

Aqui estão alguns exemplos de hooks personalizados que podem ser úteis para diferentes cenários:

### useFetch: Fazer chamadas de API.
Este hook encapsula a lógica de buscar dados de uma API, incluindo carregamento e tratamento de erros.

```
import { useState, useEffect } from "react";

function useFetch(url) {
  const [dados, setDados] = useState(null);
  const [carregando, setCarregando] = useState(true);
  const [erro, setErro] = useState(null);

  useEffect(() => {
    async function buscarDados() {
      try {
        setCarregando(true);
        const resposta = await fetch(url);
        if (!resposta.ok) throw new Error("Erro ao buscar os dados!");
        const resultado = await resposta.json();
        setDados(resultado);
      } catch (erro) {
        setErro(erro.message);
      } finally {
        setCarregando(false);
      }
    }
    buscarDados();
  }, [url]);

  return { dados, carregando, erro };
}

export default useFetch;
```

Como usar:

```
const { dados, carregando, erro } = useFetch("https://api.exemplo.com/usuarios");
```

### useLocalStorage: Gerenciar dados no Local Storage
Permite salvar e recuperar valores do armazenamento local do navegador.

```
import { useState } from "react";

function useLocalStorage(chave, valorInicial) {
  const [valor, setValor] = useState(() => {
    const armazenado = localStorage.getItem(chave);
    return armazenado ? JSON.parse(armazenado) : valorInicial;
  });

  const salvar = (novoValor) => {
    setValor(novoValor);
    localStorage.setItem(chave, JSON.stringify(novoValor));
  };

  return [valor, salvar];
}

export default useLocalStorage;
```

Como usar:

```
const [nome, setNome] = useLocalStorage("nomeUsuario", "Visitante");
```


### useToggle: Alternar estados booleanos
Este hook facilita alternar entre true e false.

```
import { useState } from "react";

function useToggle(valorInicial = false) {
  const [estado, setEstado] = useState(valorInicial);

  const alternar = () => setEstado((prev) => !prev);

  return [estado, alternar];
}

export default useToggle;
```

Como usar:

```
const [ativado, alternarAtivado] = useToggle(false);
```


### useDebounce: Reduz chamadas frequentes de funções
Ideal para otimizar eventos, como pesquisa ou rolagem.

```
import { useState, useEffect } from "react";

function useDebounce(valor, delay) {
  const [debouncedValor, setDebouncedValor] = useState(valor);

  useEffect(() => {
    const handler = setTimeout(() => setDebouncedValor(valor), delay);

    return () => clearTimeout(handler);
  }, [valor, delay]);

  return debouncedValor;
}

export default useDebounce;
```

Como usar:

```
const valorDebounced = useDebounce(valorDigitado, 500);
```

### useDarkMode: Gerenciar o modo escuro
Permite alternar entre o modo claro e escuro em uma aplicação.

```
import useLocalStorage from "./useLocalStorage";

function useDarkMode() {
  const [modoEscuro, setModoEscuro] = useLocalStorage("modoEscuro", false);

  useEffect(() => {
    document.body.className = modoEscuro ? "dark" : "light";
  }, [modoEscuro]);

  return [modoEscuro, setModoEscuro];
}

export default useDarkMode;
```

Como usar:

```
const [modoEscuro, alternarModoEscuro] = useDarkMode();
```

Estes são apenas alguns exemplos de hooks personalizados que podem ser adaptados a diferentes situações.


