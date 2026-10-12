# Learning Log --- Formulários HTML, GET, POST, FormData e searchParams no Next.js

## Contexto: o que estamos a construir?

No projecto da plataforma de turismo, temos uma barra de pesquisa
reutilizada na página inicial e na página `/pesquisa`.

A barra contém dois campos:

-   **Pesquisa:** o texto que a pessoa procura, por exemplo, `hotel`.
-   **Localização:** a província escolhida, por exemplo, `Luanda`.

O objectivo é que, quando a pessoa clicar em **Pesquisar**, o navegador
navegue para `/pesquisa` e leve os critérios na URL. Depois, a página
`/pesquisa` poderá ler esses critérios e apresentar os resultados
correspondentes.

Este percurso permite aprender vários conceitos que trabalham em
conjunto. Vamos estudá-los um de cada vez.

------------------------------------------------------------------------

## 1. O elemento `<form>`

O elemento HTML `<form>` agrupa campos que recolhem dados do utilizador
e define como esses dados podem ser submetidos.

Na nossa barra, começámos com um `<div>`:

``` tsx
<div className="...">
    {/* campos e botão */}
</div>
```

Um `div` é um contentor genérico. Organiza elementos visualmente, mas
não cria, por si só, o comportamento nativo de submissão de um
formulário.

Quando substituímos o `div` por um `form`, passámos a usar um elemento
HTML com um propósito específico:

``` tsx
<form className="...">
    {/* campos e botão */}
</form>
```

**Ideia principal:** usamos `form` quando os campos pertencem a uma
operação de recolha e envio de dados.

### O botão `type="submit"`

Dentro de um formulário, podemos usar um botão de submissão:

``` tsx
<button type="submit">
    Pesquisar
</button>
```

Ao clicar nele, o navegador tenta submeter o formulário. A tecla Enter
também pode submeter um formulário em determinadas circunstâncias, como
quando o utilizador está a escrever num campo de texto.

Antes da alteração, o botão tinha `type="button"`. Esse tipo cria um
botão normal e não desencadeia automaticamente a submissão do
formulário.

**Importante:** mudar o botão para `type="submit"` não define, por si
só, o destino nem o método de envio. Para isso existem `action` e
`method`.

------------------------------------------------------------------------

## 2. O atributo `name`

Cada campo que queremos enviar precisa de um nome.

Exemplo:

``` tsx
<input
    name="pesquisa"
    type="text"
    placeholder="O que procura?"
/>
```

O atributo `name="pesquisa"` identifica o campo nos dados submetidos.

O campo de localização usa:

``` tsx
<select name="localizacao">
    {/* opções */}
</select>
```

Aqui, `name="localizacao"` identifica o valor escolhido no menu.

O `name` não é o mesmo que `placeholder`:

-   `name` identifica o campo nos dados enviados.
-   `placeholder` é apenas uma indicação visual apresentada dentro do
    campo quando está vazio.

Por exemplo, `O que procura?` pode ser o placeholder, enquanto
`pesquisa` é o nome que aparecerá nos dados enviados.

Em geral, os controlos precisam de ter um `name` para que os seus
valores sejam incluídos na submissão nativa do formulário. O valor
enviado depende do controlo: num `input`, costuma ser o texto escrito;
num `select`, é o valor da opção seleccionada.

------------------------------------------------------------------------

## 3. `action`: para onde vão os dados?

O atributo `action` define o destino para o qual o navegador submete o
formulário.

Na nossa aplicação:

``` tsx
<form action="/pesquisa">
```

Isto indica que o destino é a rota `/pesquisa`.

No App Router do Next.js, essa rota corresponde ao ficheiro:

``` text
src/app/(publico)/pesquisa/page.tsx
```

A pasta `(publico)` é um *route group*: organiza os ficheiros, mas não
aparece no endereço público. Por isso, a URL continua a ser `/pesquisa`,
não `/(publico)/pesquisa`.

Uma forma fácil de recordar:

**`action` responde à pergunta: «Para onde?»**

`action="/pesquisa"` não significa que os dados são entregues ao próprio
elemento `form`. Significa que o navegador submete os dados para o
destino indicado, que neste caso é a rota `/pesquisa`.

------------------------------------------------------------------------

## 4. `method`: como são enviados os dados?

O atributo `method` define o método HTTP usado para submeter o
formulário.

Na nossa pesquisa, vamos usar:

``` tsx
<form action="/pesquisa" method="GET">
```

**`method` responde à pergunta: «Como?»**

Os dois métodos que precisamos de distinguir inicialmente são `GET` e
`POST`.

### 4.1. GET --- consultas e pesquisas

Com `GET`, os dados do formulário são normalmente acrescentados à URL
como parâmetros de consulta (*query parameters*).

Por exemplo, se o utilizador escrever `hotel` e escolher `Luanda`, o
navegador poderá navegar para:

``` text
/pesquisa?pesquisa=hotel&localizacao=Luanda
```

Nesta URL:

-   `/pesquisa` é o caminho da página.
-   `?` marca o início dos parâmetros de consulta.
-   `pesquisa=hotel` é um par nome/valor.
-   `&` separa os parâmetros.
-   `localizacao=Luanda` é outro par nome/valor.

O navegador codifica os valores conforme necessário. Por exemplo,
espaços e caracteres especiais podem aparecer codificados na URL.

O `GET` é adequado para pesquisas, filtros e consultas que queremos
poder recarregar, guardar nos favoritos ou partilhar através do
endereço.

Os dados enviados por `GET` ficam visíveis na URL. Por isso, não devemos
usá-lo para enviar palavras-passe ou outros dados que precisem de
confidencialidade.

### 4.2. POST --- envio de dados no corpo do pedido

Com `POST`, os dados do formulário são enviados no corpo do pedido HTTP,
em vez de serem acrescentados à URL como acontece normalmente com `GET`.

Exemplo:

``` tsx
<form action="/registo" method="POST">
    {/* campos de registo */}
</form>
```

`POST` é frequentemente utilizado para criar registos ou enviar dados
para processamento, como num formulário de inscrição.

Atenção: `POST` não torna automaticamente os dados seguros ou privados.
A segurança também depende de HTTPS, validação, autorização e de como o
servidor trata os dados.

### 4.3. Comparação rápida

  -----------------------------------------------------------------------
  Pergunta                GET                     POST
  ----------------------- ----------------------- -----------------------
  Onde ficam normalmente  Na URL, como parâmetros No corpo do pedido
  os dados do formulário? de consulta             

  Usos frequentes         Pesquisas e filtros     Registos e envio de
                                                  dados para
                                                  processamento

  É fácil partilhar a     Sim                     Não da mesma forma
  consulta através da                             
  URL?                                            

  Devemos enviar          Não                     O envio deve ser feito
  palavras-passe?                                 por HTTPS e com
                                                  tratamento seguro;
                                                  POST, por si só, não
                                                  garante segurança
  -----------------------------------------------------------------------

A distinção mais importante para o nosso caso é esta:

-   `action` define o destino.
-   `method` define o método de envio.

Não confundas `GET` com «obter a página». A página de destino é indicada
por `action`; `GET` é o método HTTP escolhido para enviar os critérios
de pesquisa.

------------------------------------------------------------------------

## 5. O percurso completo da nossa barra de pesquisa

O formulário pode ficar assim:

``` tsx
<form action="/pesquisa" method="GET">
    <input
        name="pesquisa"
        type="text"
        placeholder="O que procura?"
    />

    <select name="localizacao" defaultValue="">
        <option value="" disabled>
            Selecciona uma província
        </option>
        {/* opções de províncias */}
    </select>

    <button type="submit">
        Pesquisar
    </button>
</form>
```

Este é um exemplo reduzido para estudar o comportamento; no projecto
mantemos as classes Tailwind e geramos as opções do `select` a partir da
constante `provincias`.

O percurso é:

1.  O utilizador escreve um termo e escolhe uma localização.
2.  Clica em **Pesquisar**.
3.  O navegador identifica os campos através de `name`.
4.  Como o método é `GET`, o navegador inclui os valores na URL.
5.  O navegador navega para `/pesquisa` com os parâmetros.
6.  A página `/pesquisa` precisa de ler esses parâmetros e utilizá-los
    para apresentar os resultados.

Por exemplo:

``` text
/pesquisa?pesquisa=hotel&localizacao=Luanda
```

Não é o `form` que filtra as instituições. O formulário transporta os
critérios; a página de resultados terá a responsabilidade de interpretar
os critérios e encontrar os resultados correspondentes.

------------------------------------------------------------------------

## 6. O que é `FormData`?

`FormData` é uma API JavaScript que permite criar um objecto com os
dados de um formulário. Depois, o código pode consultar esses dados ou
utilizá-los num pedido personalizado.

Um exemplo:

``` tsx
const formData = new FormData(formElement);
```

Vamos interpretar a linha:

-   `const formData`: declara uma constante chamada `formData`.
-   `new FormData(...)`: cria um novo objecto `FormData`.
-   `formElement`: representa o elemento `<form>` cujos campos queremos
    recolher.

O nome `formData` é uma escolha nossa; poderíamos escolher outro nome.
`FormData`, com maiúsculas, é o nome da API fornecida pelo JavaScript do
navegador.

### 6.1. Como obter um valor?

Depois de criar o objecto, podemos consultar um campo pelo seu `name`:

``` tsx
const pesquisa = formData.get("pesquisa");
const localizacao = formData.get("localizacao");
```

`get("pesquisa")` procura o valor associado ao campo cujo nome é
`pesquisa`. Da mesma forma, `get("localizacao")` consulta o campo de
localização.

O resultado de `FormData.get()` é um valor do tipo `FormDataEntryValue`
ou `null`: pode ser uma string, um `File` ou `null`, dependendo do campo
e da existência de um valor. Num formulário de texto como o nosso,
esperamos normalmente uma string ou `null`.

### 6.2. De onde vem `event.currentTarget`?

Quando tratamos um evento de submissão em JavaScript, o evento fornece
informações sobre o que aconteceu.

-   `event` representa o evento recebido pela função.
-   `currentTarget` é o elemento ao qual o nosso manipulador de evento
    está associado.
-   Se o manipulador estiver associado ao evento de submissão do
    formulário, `event.currentTarget` será o formulário.

Por exemplo, num manipulador React de submissão, poderíamos escrever:

``` tsx
function handleSubmit(event: React.FormEvent<HTMLFormElement>) {
    event.preventDefault();

    const form = event.currentTarget;
    const formData = new FormData(form);

    const pesquisa = formData.get("pesquisa");
    const localizacao = formData.get("localizacao");
}
```

Este exemplo serve para explicar a API. Não é uma instrução para
substituir a implementação actual por este código.

`event.preventDefault()` impede o comportamento nativo de submissão. É
utilizado quando queremos controlar o que acontece através de
JavaScript. Se o quisermos fazer, teremos de implementar nós próprios o
comportamento seguinte, como validar os dados e navegar para a URL
pretendida.

### 6.3. Precisamos de `FormData` na nossa primeira versão?

Não necessariamente.

Com:

``` tsx
<form action="/pesquisa" method="GET">
```

o navegador já sabe recolher os campos com `name`, construir a query
string e navegar para o destino. Para esta necessidade, usar `FormData`
manualmente seria trabalho adicional sem benefício imediato.

`FormData` torna-se útil quando precisamos de controlar a submissão,
validar valores antes de prosseguir, preparar um pedido personalizado ou
enviar dados através de JavaScript.

**Lição:** aprender uma API não significa que temos de a utilizar em
todos os casos. A escolha técnica deve corresponder à necessidade real.

------------------------------------------------------------------------

## 7. O que é `searchParams` no Next.js?

Até agora, aprendemos como os critérios chegam à URL. Falta aprender
como a página `/pesquisa` os lê.

O Next.js App Router disponibiliza à página uma propriedade chamada
`searchParams`, que representa os parâmetros de consulta da URL.

Para a URL:

``` text
/pesquisa?pesquisa=hotel&localizacao=Luanda
```

os valores que queremos ler são:

-   `pesquisa` → `hotel`
-   `localizacao` → `Luanda`

A ideia é que a página consiga ler esses valores sem precisar de
importar a barra de pesquisa nem de receber directamente o estado dos
campos.

### 7.1. Porque é que `searchParams` é assíncrono?

Nas versões actuais do Next.js, incluindo o Next.js 16, a propriedade
`searchParams` de uma página é uma `Promise`. Isso significa que não
devemos tratá-la imediatamente como se já fosse o objecto com os
parâmetros.

Por isso, a página pode precisar de ser uma função `async` e de utilizar
`await`.

Uma forma simplificada de ilustrar a assinatura é:

``` tsx
type PesquisaPageProps = {
    searchParams: Promise<{
        pesquisa?: string | string[];
        localizacao?: string | string[];
    }>;
};

export default async function PesquisaPage({
    searchParams,
}: PesquisaPageProps) {
    const parametros = await searchParams;

    // A partir daqui, podemos consultar os parâmetros.
    return (
        <section>
            <h1>Pesquisa</h1>
        </section>
    );
}
```

Este é um exemplo didáctico de tipagem e acesso aos parâmetros. Ainda
não implementa a filtragem dos resultados, e a estrutura final deverá
ser adaptada ao código existente na página.

### 7.2. O papel de `async`

A palavra-chave `async` declara uma função assíncrona. Uma função
`async` devolve uma `Promise`, mesmo quando o seu resultado final é um
valor normal.

No nosso caso, escrevemos:

``` tsx
export default async function PesquisaPage(...)
```

porque a função precisa de aguardar a resolução de `searchParams`.

### 7.3. O papel de `await`

`await` permite esperar pela resolução de uma `Promise` dentro de uma
função assíncrona.

``` tsx
const parametros = await searchParams;
```

Podemos ler esta linha assim: «Espera que `searchParams` esteja
resolvido e guarda o resultado em `parametros`.»

Depois disso, `parametros` contém os valores disponíveis na query
string.

### 7.4. Porque é que o tipo permite `string[]`?

Uma query string pode conter o mesmo nome mais do que uma vez. Por
exemplo:

``` text
/pesquisa?categoria=hotel&categoria=resort
```

Por isso, o Next.js pode representar um parâmetro como uma string ou
como um array de strings. Se o nosso formulário só enviar um valor por
campo, normalmente receberemos uma string, mas a tipagem deve considerar
que os parâmetros podem repetir-se.

O tipo também pode ser `undefined` quando um parâmetro não existe na
URL. Isso é importante para permitir que `/pesquisa`, sem filtros,
apresente todos os resultados.

------------------------------------------------------------------------

## 8. Como os conceitos se ligam

Podemos resumir a arquitectura da pesquisa assim:

``` text
BarraPesquisa
    |
    | form + name + action + method="GET"
    v
URL /pesquisa?pesquisa=hotel&localizacao=Luanda
    |
    | searchParams
    v
Página /pesquisa
    |
    | interpretar critérios e filtrar os dados
    v
Resultados apresentados ao utilizador
```

Cada parte tem uma responsabilidade:

-   **`form`** agrupa os campos.
-   **`name`** identifica os campos submetidos.
-   **`action`** define o destino.
-   **`method`** define como os dados são enviados.
-   **`FormData`** permite consultar os dados através de JavaScript
    quando precisamos de controlar a submissão.
-   **`searchParams`** permite à página Next.js ler os critérios que
    chegaram na URL.
-   **A lógica de pesquisa** utilizará os critérios para encontrar e
    apresentar as instituições correspondentes.

------------------------------------------------------------------------

## 9. O que ainda não implementámos

Nesta fase, estamos a aprender a transportar e receber os critérios.
Ainda não concluímos:

-   A validação dos campos.
-   O tratamento de parâmetros vazios.
-   A filtragem dos dados de instituições.
-   A apresentação de resultados e de mensagens quando não existem
    correspondências.
-   A integração com o contrato do backend, que ainda aguardamos.

Vamos implementar esses pontos gradualmente, à medida que forem
necessários, sem criar arquitectura prematura.

## 10. Resumo para revisão

-   `form`: agrupa campos que podem ser submetidos.
-   `name`: identifica cada campo nos dados submetidos.
-   `type="submit"`: desencadeia a submissão do formulário.
-   `action`: define para onde o formulário é submetido.
-   `method="GET"`: envia os critérios como parâmetros da URL.
-   `method="POST"`: envia os dados no corpo do pedido.
-   `FormData`: permite consultar dados de um formulário através de
    JavaScript.
-   `searchParams`: permite à página do App Router ler os parâmetros da
    URL.
-   `async`: declara uma função assíncrona.
-   `await`: espera pela resolução de uma `Promise`.

**Princípio de trabalho:** compreender o problema, escolher a solução
mais simples que o resolve e só depois acrescentar complexidade quando
houver uma necessidade real.
