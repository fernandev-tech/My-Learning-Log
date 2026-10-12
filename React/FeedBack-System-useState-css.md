Sim. Essa linha parece assustadora no início 😄, mas quando a desmontamos em português, ela fica bem lógica.

Estamos falando desta:

<p className={`${styles.feedback} ${styles[feedback.type]}`}>
    {feedback.message}
</p>
1. Primeiro, esquece o feedback.type

Vamos imaginar que no estado temos:

feedback = {
    type: "success",
    message: "Tarefa adicionada com sucesso!"
}

E no CSS Module temos:

.feedback {
    text-align: center;
    padding: 12px 16px;
}

.success {
    background-color: #ECFDF5;
    color: #047857;
}

.error {
    background-color: #FEF2F2;
    color: #B91C1C;
}

Nós queremos que o <p> tenha duas classes:

<p class="feedback success">

Uma classe fornece o estilo geral:

feedback
↓
espaçamento
alinhamento
tamanho
bordas
etc.

E a outra fornece o estilo específico do tipo:

success
↓
verde

error
↓
vermelho
2. A primeira parte
styles.feedback

Como estamos usando CSS Modules:

import styles from './Feedback.module.css'

O React não usa diretamente:

className="feedback"

Em CSS Modules usamos:

styles.feedback

Isso significa:

"Pegue a classe .feedback que está no meu CSS Module."

3. A parte mais interessante

Agora temos:

styles[feedback.type]

Vamos olhar novamente para o estado:

feedback = {
    type: "success",
    message: "Tarefa adicionada!"
}

Então:

feedback.type

produz:

"success"

Agora temos:

styles[feedback.type]

que na prática vira:

styles["success"]

Ou seja:

styles.success

Portanto, se:

feedback.type === "success"

pegamos:

.success

Se fosse:

feedback.type === "error"

teríamos:

styles["error"]

que corresponde a:

.error
É por isso que usamos [].

Porque estamos dizendo:

"O nome da classe que quero pegar está guardado dentro desta variável."

Isso é acesso dinâmico a uma propriedade de objeto.

4. Agora entra o template literal

Temos:

`${styles.feedback} ${styles[feedback.type]}`

As crases:

`

permitem montar uma string usando ${...}.

Então imagine:

styles.feedback = "feedback_x7a91"
styles.success = "success_k29fd"

O React pode acabar recebendo algo equivalente a:

className="feedback_x7a91 success_k29fd"

Ou seja:

Duas classes no mesmo elemento.

5. Traduzindo a linha inteira
<p className={`${styles.feedback} ${styles[feedback.type]}`}>

Podemos ler assim:

"Crie um parágrafo que tenha a classe geral feedback e também a classe correspondente ao tipo de feedback."

Se o tipo for:

"success"

fica:

feedback + success

Se for:

"error"

fica:

feedback + error

É justamente isso que permite termos um componente de feedback, mas vários estilos.

6. E esta parte?
{feedback.message}

É muito mais simples.

Se:

feedback = {
    type: "success",
    message: "Tarefa adicionada!"
}

então:

{feedback.message}

mostra:

Tarefa adicionada!

Se depois tivermos:

feedback = {
    type: "error",
    message: "Escreva um título."
}

o mesmo <p> mostra:

Escreva um título.
7. E o &&?

Aqui temos outro conceito importante.

Poderíamos fazer:

{feedback && (
    <p className={`${styles.feedback} ${styles[feedback.type]}`}>
        {feedback.message}
    </p>
)}

Lê-se:

"Se existir feedback, renderiza esse <p>."

Lembra que decidimos começar com:

const [feedback, setFeedback] = useState(null)

Então inicialmente:

feedback = null

O React encontra:

feedback && (...)

Como feedback não existe, o conteúdo depois do && não é renderizado.

Depois fazemos:

setFeedback({
    type: "success",
    message: "Tarefa adicionada!"
})

Agora existe um objeto.

Então:

feedback && (...)

permite renderizar o <p>.

8. Deves usar sempre &&?

Não. Essa é uma coisa importante.

O && não é "a forma de fazer condições no React".

Ele é especialmente útil quando queremos dizer:

Se isto existir/for verdadeiro, mostre isto.

Por exemplo:

{editingTask && <button>Cancelar</button>}

Significa:

Se existe uma tarefa sendo editada, mostra o botão Cancelar.

Foi exatamente o que já fizeste no teu projeto.

Quando usar &&

Quando tens uma situação do tipo:

Se existir X
    ↓
mostra Y

Exemplo:

{feedback && <Feedback />}

ou:

{editingTask && <button>Cancelar</button>}
9. Quando usar ternário

Quando precisas de escolher entre duas coisas:

Se X
 ↓
A

senão
 ↓
B

Aí o ternário é muito adequado:

{editingTask ? "Salvar" : "Adicionar"}

Que significa:

Existe editingTask?
       ↓
     SIM → "Salvar"
     NÃO → "Adicionar"

Tu já estás usando isso aqui:

{editingTask ? <CheckIcon /> : <PlusIcon />}

Perfeito.

10. Uma diferença que quero que guardes

Pensa assim:

&&
condição && elemento

"Se existir, mostra."

Ternário
condição ? elementoA : elementoB

"Se existir, mostra A; senão, mostra B."

No nosso feedback

Como não temos uma segunda coisa para mostrar quando não existe feedback:

Existe feedback?
      │
      ├── SIM → mostrar feedback
      │
      └── NÃO → não mostrar nada

O && é perfeito:

{feedback && (
    <p>...</p>
)}

E olha como isso combina muito bem com nosso estado:

const [feedback, setFeedback] = useState(null)
null
 ↓
não mostra

objeto
 ↓
mostra

É uma relação muito limpa.

E não precisas decorar a linha gigante de className agora. O mais importante é entender as três peças separadamente:

styles.feedback

→ classe geral.

styles[feedback.type]

→ classe escolhida dinamicamente pelo type.

feedback && (...)

→ só renderiza quando existe feedback.

