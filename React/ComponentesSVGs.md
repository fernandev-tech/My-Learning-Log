# Registro de Estudo — Ícones de Botão (Adicionar/Salvar) no TaskFlow

## Contexto inicial

**Minha pergunta:**
Pedi um SVG para o botão "Adicionar" ([+ Adicionar]), no estilo tradicional, com o "+" branco, para um botão com fundo azul (`background-color: #2563EB`).

**Resposta:**
SVG do "+" tradicional:

```jsx
<svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
  <path d="M12 5V19M5 12H19" stroke="white" strokeWidth="2.5" strokeLinecap="round"/>
</svg>
```

Dica: `strokeWidth="2.5"` deixa o traço mais "cheio". Se o texto do botão for mais fino, pode-se usar `2`.

---

## Escolhendo o ícone de "Salvar"

**Minha pergunta:**
Apresentei várias opções (💾 disquete, ✓ check, ↑ upload, ✎ edit, 🔄) e não sabia decidir entre ✓ Salvar ou 💾 Salvar.

**Resposta:**
A escolha deve ser pensada pelo **contexto de uso**, não só pela tradição do ícone:
- **✓ (check)** → comunica "confirmar e concluir a ação". Bom quando o botão fecha um formulário.
- **💾 (disquete)** → comunica "gravar dados", ícone mais antiquado mas ainda universalmente reconhecido.

Recomendação: usar **✓**, por dois motivos:
1. Visualmente mais leve, combina com o traço fino do "+".
2. Cria consistência visual — dois ícones geométricos simples (+ e ✓) em vez de misturar geométrico com emoji de objeto (disquete).

SVG do check, no mesmo estilo do "+":

```jsx
<svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
  <path d="M5 13L9 17L19 7" stroke="white" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"/>
</svg>
```

---

## O problema: botão único que alterna entre "Adicionar" e "Salvar"

**Minha pergunta:**
O botão de adicionar e salvar é o mesmo componente. Quando estou editando uma tarefa, o texto vira "Salvar"; quando estou criando, vira "Adicionar". Eu já tinha um ternário para o texto:

```jsx
{editingTask ? "Salvar" : "Adicionar"}
```

Queria saber como aplicar a mesma lógica para trocar o **ícone** junto com o texto.

**Conceitos apresentados antes da resposta:**
- Um ternário sempre *retorna um valor* — esse valor pode ser uma string, número, ou **um elemento JSX inteiro**.
- Em JSX, tudo dentro de `{ }` é uma expressão JavaScript normal. O React não distingue entre uma string e um elemento JSX na hora de renderizar.
- Perguntas para refletir: "o que impede o SVG de seguir a mesma lógica do ternário de texto?", "o resultado de um ternário pode ser só texto, ou pode ser outra coisa também?"

**Conceito para pesquisar recomendado:** *"conditional rendering with JSX expressions"*

**Resposta (depois de eu decidir por dois ternários separados):**

```jsx
<button>
  {editingTask ? (
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M5 13L9 17L19 7" stroke="white" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"/>
    </svg>
  ) : (
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M12 5V19M5 12H19" stroke="white" strokeWidth="2.5" strokeLinecap="round"/>
    </svg>
  )}
  {editingTask ? "Salvar" : "Adicionar"}
</button>
```

**Explicação da estrutura:**
- Primeiro ternário → escolhe qual SVG renderizar.
- Segundo ternário → escolhe qual texto renderizar.
- Os dois ficam lado a lado dentro do mesmo `<button>`, cada um resolvendo seu próprio pedaço.

**Detalhe de sintaxe convencional:** quando o valor de um lado do ternário é um elemento JSX com múltiplas linhas (como o `<svg>`), é convenção envolver cada ramo em parênteses `( )`. Isso é só legibilidade — o JavaScript não exige, mas é o estilo padrão em código React.

---

## Transformando os SVGs em componentes reutilizáveis

**Minha pergunta:**
Fiquei curioso sobre a sugestão de usar os ícones como componentes separados (`<PlusIcon />` e `<CheckIcon />`) em vez de repetir o SVG inline.

**Conceito apresentado:**
Qualquer JSX pode virar um componente, incluindo um SVG sozinho — porque, no fundo, um componente é só uma função que retorna JSX. Não importa se esse JSX é uma `<div>` grande ou um `<svg>` pequeno.

Comparação para entender por analogia:
```jsx
// Isso já era familiar:
function TaskItem() {
  return <li>Minha tarefa</li>;
}

// Isso é exatamente a mesma ideia:
function PlusIcon() {
  return (
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M12 5V19M5 12H19" stroke="white" strokeWidth="2.5" strokeLinecap="round"/>
    </svg>
  );
}
```

O botão fica mais limpo:
```jsx
<button>
  {editingTask ? <CheckIcon /> : <PlusIcon />}
  {editingTask ? "Salvar" : "Adicionar"}
</button>
```

**Por que isso é vantajoso (para eu refletir, não decorar):**
1. Reutilização — se precisar do "+" em outro botão do app, só chamo `<PlusIcon />` de novo, sem copiar o SVG inteiro.
2. Manutenção centralizada — mudar cor/tamanho do ícone no futuro exige alteração em um único lugar.
3. Legibilidade — `<PlusIcon />` comunica intenção melhor do que um bloco de `<path>` com coordenadas.

**Pergunta para pesquisar (próximo passo):**
Se eu quisesse reutilizar o `PlusIcon` em outro botão com fundo diferente (não azul), como ele saberia usar outra cor de stroke?
**Conceito para pesquisar:** *"props"* aplicado a esse caso.

---

## Erros práticos ao implementar (aplicação real no código)

**Minha situação:**
Tentei separar os ícones em um arquivo `TaskIcons.js`, mas declarei `PlusIcon` e `CheckIcon` **dentro** da função `TaskIcons`, e não conseguia importar/usar no `TaskForm`.

**Diagnóstico e explicação antes da correção:**
- `PlusIcon` e `CheckIcon`, declaradas dentro do corpo de `TaskIcons`, só existem no escopo daquela função — ninguém de fora enxerga essas funções, nem o arquivo que importa `TaskIcons`.
- Analogia usada: é como guardar uma ferramenta dentro de uma gaveta trancada — a gaveta (`TaskIcons`) foi exportada, mas o que está dentro dela não.
- O que era necessário: cada ícone precisa ser **seu próprio componente**, exportado separadamente.

**Conceito para pesquisar:** *"named exports vs default export"* — um arquivo pode ter várias exportações nomeadas (`export function X`) além de, no máximo, uma exportação padrão (`export default`).

**Correção proposta para `TaskIcons.js`:**
```jsx
export function PlusIcon() {
    return (
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M12 5V19M5 12H19" stroke="white" strokeWidth="2.5" strokeLinecap="round" />
        </svg>
    )
}

export function CheckIcon() {
    return (
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M5 13L9 17L19 7" stroke="white" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round" />
        </svg>
    )
}
```

**Importação corrigida no `TaskForm.js`:**
```jsx
// Antes (default export):
import TaskIcons from './TaskIcons'

// Depois (named exports):
import { PlusIcon, CheckIcon } from './TaskIcons'
```

**Detalhe técnico apontado:** atributos SVG com hífen (`stroke-width`, `stroke-linecap`) precisam virar **camelCase** em JSX (`strokeWidth`, `strokeLinecap`) — mesma regra do `class` → `className`.

**Conceito para pesquisar:** *"SVG attributes in JSX camelCase"*

---

## Bug de lógica identificado no código

No botão implementado, havia uma inversão:
```jsx
{editingTask ? <PlusIcon /> : <CheckIcon />}
{editingTask ? "Salvar" : "Adicionar"}
```

Quando `editingTask` é verdadeiro (modo edição), o texto vira "Salvar", mas o ícone escolhido era o `PlusIcon` — invertido em relação ao texto. A correção correta segue a mesma lógica do texto:
```jsx
{editingTask ? <CheckIcon /> : <PlusIcon />}
```

---

## Resumo dos conceitos para revisar

1. Ternários em JSX podem retornar qualquer tipo de valor, incluindo elementos JSX inteiros — não só strings.
2. Convenção de parênteses em ternários com JSX multilinha.
3. Qualquer JSX pode virar componente reutilizável (mesmo um SVG isolado).
4. `named exports` vs `default export` — e como isso afeta a visibilidade de funções entre arquivos.
5. Atributos SVG em JSX seguem camelCase.
6. Importância de revisar a correspondência lógica entre condições relacionadas (texto e ícone precisam mudar *juntos* e na *mesma direção*).
7. Próximo conceito a explorar: **props**, para tornar ícones reutilizáveis com cores/tamanhos diferentes.


