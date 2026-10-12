# Learning Log — JavaScript Assíncrono: Promise, async e await

## 1. Contexto: por que estamos a aprender isto?

No projecto `plataforma-turismo`, estamos a desenvolver uma barra de pesquisa que permite procurar instituições turísticas por nome ou localização.

O formulário envia os critérios através da URL. Por exemplo:

```text
/pesquisa?pesquisa=hotel&localizacao=Luanda
```

O próximo passo é fazer com que a página `/pesquisa` consiga ler esses valores.

No Next.js App Router, a propriedade `searchParams` de uma página é assíncrona. Para compreender como trabalhar com ela, precisamos primeiro de conhecer três conceitos de JavaScript:

- `Promise`
- `async`
- `await`

O objectivo deste documento não é memorizar palavras. É compreender o problema que estes conceitos resolvem.

---

## 2. O que significa síncrono?

Uma operação síncrona é executada de forma que o código seguinte espera que essa operação termine antes de continuar.

Consideremos:

```js
console.log("Primeiro");
console.log("Segundo");
console.log("Terceiro");
```

Resultado:

```text
Primeiro
Segundo
Terceiro
```

As instruções são executadas por ordem.

Podemos imaginar uma pessoa que prepara o pequeno-almoço: coloca o pão na torradeira, espera que fique pronto e só depois passa a manteiga.

Esta analogia representa uma sequência em que uma tarefa espera pela conclusão da anterior.

### Ideia principal

**Síncrono:** a execução segue a sequência das operações, esperando pela conclusão de cada operação síncrona antes de continuar.

---

## 3. O que significa assíncrono?

Uma operação assíncrona permite que o programa aguarde um resultado sem bloquear necessariamente toda a execução do JavaScript.

Imaginemos outra situação:

1. Colocamos o pão na torradeira.
2. Enquanto o pão fica pronto, preparamos o chá.
3. Quando o pão estiver pronto, continuamos o pequeno-almoço.

Não precisamos de ficar parados a olhar para a torradeira.

Em programação, podemos ter situações semelhantes quando:

- Pedimos dados a uma API.
- Consultamos uma base de dados através de uma operação assíncrona.
- Lemos um ficheiro através de uma API assíncrona.

Por exemplo, a nossa plataforma de turismo poderá pedir ao servidor uma lista de hotéis. Como a resposta pode demorar, precisamos de uma forma de representar a operação enquanto aguardamos.

É aqui que entram as Promises.

### Ideia principal

**Assíncrono:** uma operação pode continuar pendente enquanto outras tarefas são executadas.

Atenção: assíncrono não significa que tudo acontece simultaneamente. Significa que podemos gerir operações que não terminam imediatamente sem bloquear necessariamente toda a execução.

---

## 4. O que é uma Promise?

`Promise` significa «promessa» em inglês.

Em JavaScript, uma Promise é um objecto que representa o resultado de uma operação que poderá ficar disponível mais tarde.

Imaginemos que pedimos uma lista de hotéis ao servidor.

A lista pode ainda não ter chegado. No entanto, podemos ter uma Promise que representa essa operação.

Uma Promise pode estar em três estados:

### 4.1. `pending` — pendente

A operação ainda não terminou.

Exemplo: fizemos um pedido ao servidor e continuamos à espera da resposta.

### 4.2. `fulfilled` — cumprida

A operação terminou com sucesso e produziu um resultado.

Exemplo: recebemos a lista de hotéis.

### 4.3. `rejected` — rejeitada

A operação terminou com uma falha.

Exemplo: o pedido falhou e não conseguimos receber os dados.

### Uma distinção importante

A Promise não é necessariamente o resultado em si.

Se pedirmos uma lista de hotéis, podemos distinguir:

- A Promise representa a operação e o seu resultado.
- A lista de hotéis é o valor que poderemos obter quando a operação terminar com sucesso.

Uma Promise também pode ser criada a partir de um resultado que já está disponível; não significa obrigatoriamente que haverá uma espera demorada.

---

## 5. O que é `async`?

`async` é uma palavra-chave do JavaScript utilizada para declarar uma função assíncrona.

Exemplo:

```js
async function apresentarMensagem() {
    return "Olá, Fernando!";
}
```

Vamos interpretar o código:

- `async`: declara a função como assíncrona.
- `function`: declara uma função.
- `apresentarMensagem`: é o nome escolhido para a função.
- `return`: indica o resultado que a função pretende devolver.
- `"Olá, Fernando!"`: é o valor devolvido.

A parte importante é esta:

**Uma função `async` devolve sempre uma Promise.**

Mesmo que escrevamos:

```js
return "Olá, Fernando!";
```

o resultado da chamada à função será uma Promise cumprida com essa string, e não directamente a string.

Isto permite utilizar `await` para trabalhar com o valor.

### Ideia principal

`async` declara uma função assíncrona e permite utilizar `await` no seu corpo.

---

## 6. O que é `await`?

`await` é uma palavra-chave que permite esperar pela resolução de uma Promise e obter o valor resultante.

Vejamos um exemplo completo:

```js
async function apresentarMensagem() {
    return "Olá, Fernando!";
}

async function iniciar() {
    const mensagem = await apresentarMensagem();

    console.log(mensagem);
}

iniciar();
```

Resultado:

```text
Olá, Fernando!
```

### Vamos analisar passo a passo

**Passo 1 — Declaramos uma função assíncrona**

```js
async function apresentarMensagem() {
    return "Olá, Fernando!";
}
```

A função devolve uma Promise que será cumprida com o valor `"Olá, Fernando!"`.

**Passo 2 — Chamamos a função**

```js
apresentarMensagem()
```

A chamada devolve uma Promise.

**Passo 3 — Utilizamos `await`**

```js
const mensagem = await apresentarMensagem();
```

O JavaScript espera pela resolução da Promise e obtém o valor resultante.

**Passo 4 — Guardamos o valor**

```js
const mensagem = ...
```

A constante `mensagem` recebe a string `"Olá, Fernando!"`.

**Passo 5 — Apresentamos o resultado**

```js
console.log(mensagem);
```

O resultado apresentado é:

```text
Olá, Fernando!
```

### O que acontece quando usamos `await`?

`await` suspende a execução da função assíncrona naquele ponto até que a Promise seja resolvida.

Isso não significa que todo o JavaScript fica bloqueado. Outras tarefas podem continuar enquanto a função aguarda.

No JavaScript moderno, `await` é utilizado normalmente dentro de funções `async`. Existem também contextos de módulos JavaScript que permitem `await` no nível superior.

### Ideia principal

- `async`: permite declarar uma função assíncrona.
- `await`: espera pela resolução de uma Promise para obter o seu resultado.

---

## 7. Como `async`, `await` e `Promise` trabalham juntos?

Consideremos:

```js
async function obterNome() {
    return "Hotel Luanda";
}

async function iniciar() {
    const nome = await obterNome();

    console.log(nome);
}

iniciar();
```

O percurso é:

1. A função `obterNome` é chamada.
2. A função devolve uma Promise.
3. `await` espera pela resolução dessa Promise.
4. O valor `"Hotel Luanda"` é guardado na constante `nome`.
5. `console.log(nome)` apresenta o valor.

Podemos resumir assim:

| Conceito | Função |
|---|---|
| `Promise` | Representa uma operação e o seu resultado futuro ou falha. |
| `async` | Declara uma função assíncrona, que devolve uma Promise. |
| `await` | Espera pela resolução de uma Promise e permite obter o seu resultado. |

Não são três palavras que fazem exactamente a mesma coisa. Cada uma tem um papel diferente.

---

## 8. Aplicação no projecto `plataforma-turismo`

Agora vamos ligar os conceitos ao nosso código.

Na página de pesquisa do Next.js, pretendemos utilizar uma estrutura semelhante a esta:

```tsx
export default async function PesquisaPage({
    searchParams,
}: PesquisaPageProps) {
    const parametros = await searchParams;

    // ...
}
```

Ainda não estamos a implementar a filtragem das instituições. Estamos apenas a estudar a leitura dos parâmetros da URL.

### 8.1. O que é `searchParams`?

`searchParams` é uma propriedade que o Next.js disponibiliza à página para representar os parâmetros de consulta da URL.

Por exemplo:

```text
/pesquisa?pesquisa=hotel&localizacao=Luanda
```

Os parâmetros são:

- `pesquisa`: `hotel`
- `localizacao`: `Luanda`

No Next.js 16, a propriedade `searchParams` de uma página é assíncrona e é recebida como uma Promise.

### 8.2. Por que a função da página utiliza `async`?

```tsx
export default async function PesquisaPage({
    searchParams,
}: PesquisaPageProps) {
```

A palavra `async` permite que a função utilize `await` para aguardar a resolução de `searchParams`.

### 8.3. Por que utilizamos `await`?

```tsx
const parametros = await searchParams;
```

Vamos interpretar a linha:

- `const`: declara uma constante.
- `parametros`: é o nome que escolhemos para a constante.
- `await`: espera pela resolução da Promise.
- `searchParams`: é a propriedade que contém a Promise.

Depois de resolvida, a constante `parametros` contém o objecto com os parâmetros disponíveis.

Por exemplo, para a URL:

```text
/pesquisa?pesquisa=hotel&localizacao=Luanda
```

poderemos consultar:

```tsx
parametros.pesquisa
parametros.localizacao
```

Os valores serão, neste exemplo, `"hotel"` e `"Luanda"`.

Se um parâmetro não existir, o seu valor poderá ser `undefined`. Também podem existir parâmetros repetidos, que o Next.js representa através de arrays de strings.

### Atenção

A declaração `PesquisaPageProps` serve para definir os tipos das propriedades da página em TypeScript. Não é uma palavra-chave especial de JavaScript.

A estrutura exacta da tipagem deverá corresponder à versão do Next.js e ao código do projecto.

---

## 9. O percurso completo da pesquisa

A funcionalidade pode ser entendida como uma sequência:

```text
Barra de pesquisa
       |
       | form + name + action + method="GET"
       v
URL com os critérios de pesquisa
       |
       | searchParams
       v
Promise com os parâmetros da URL
       |
       | await
       v
Objecto com os parâmetros resolvidos
       |
       | interpretar os critérios
       v
Filtrar os dados das instituições
       |
       v
Apresentar os resultados
```

Cada parte tem uma responsabilidade:

- O formulário recolhe os critérios.
- `action` indica o destino.
- `method="GET"` permite enviar os critérios através da URL.
- `searchParams` permite à página ler os parâmetros.
- `async` e `await` permitem trabalhar com a propriedade assíncrona.
- A lógica de pesquisa utiliza os critérios para encontrar os resultados.

---

## 10. O que ainda preciso de compreender?

Não preciso de memorizar tudo imediatamente.

Nesta fase, quero conseguir explicar progressivamente:

1. O que significa uma operação assíncrona.
2. O que é uma Promise.
3. Por que uma função `async` devolve uma Promise.
4. Para que serve `await`.
5. Por que utilizamos `async` e `await` na página de pesquisa.

Posso voltar aos exemplos e executá-los no Node.js para observar os resultados.

O importante é construir uma compreensão que me permita reconhecer estes conceitos em projectos reais.

---

## 11. Resumo final

**Síncrono:** uma operação segue a execução normal do código, e as instruções seguintes esperam pela sua conclusão.

**Assíncrono:** uma operação pode aguardar um resultado enquanto outras tarefas continuam a ser executadas.

**Promise:** objecto que representa uma operação cujo resultado pode ficar disponível mais tarde ou terminar em falha.

**`async`:** declara uma função assíncrona e faz com que ela devolva uma Promise.

**`await`:** espera pela resolução de uma Promise e permite utilizar o valor obtido.

**`searchParams`:** propriedade da página Next.js que permite receber os parâmetros de consulta da URL.

**Princípio de aprendizagem:** compreender primeiro o problema, depois perceber a função de cada conceito e só então aplicá-lo no código do projecto.
