React — Sistema de Feedback com um Único Estado

1. O problema inicial

Na nossa aplicação de tarefas, percebemos que algumas ações importantes deveriam dar feedback visual ao utilizador.

Por exemplo:

- Ao adicionar uma tarefa → "Tarefa adicionada com sucesso!"
- Ao salvar uma tarefa → "Tarefa salva com sucesso!"
- Ao eliminar → "Tarefa eliminada!"
- Ao concluir → "Tarefa concluída!"
- Ao desfazer a conclusão → "Tarefa desmarcada!"
- Ao favoritar → "Tarefa adicionada aos favoritos!"
- Ao desfavoritar → "Tarefa removida dos favoritos!"
- Ao tentar criar uma tarefa sem título → "Escreva um título para adicionar nova tarefa."

Além disso, decidimos que os feedbacks de sucesso deveriam aparecer durante alguns segundos e depois desaparecer.

Isso levantou uma questão importante:

«Quem deve controlar o feedback?»

---

2. Quem controla o feedback?

Começámos analisando a responsabilidade de cada componente.

O "TaskForm" possui os campos e sabe quando o utilizador submeteu o formulário.

Mas quem realmente adiciona ou altera as tarefas é o "App".

Por exemplo:

function addTask(newTask) {
    setTasks([...tasks, newTask])
}

O "App" possui:

const [tasks, setTasks] = useState(...)

Portanto, o "App" é quem controla o estado principal da aplicação.

A mesma lógica vale para:

function saveEditingTask(...) {
    setTasks(updatedTasks)
}

function deleteTask(taskId) {
    setTasks(updatedTasks)
}

function toggleChecked(taskId) {
    setTasks(updatedTasks)
}

function toggleFavorite(taskId) {
    setTasks(updatedTasks)
}

Então chegámos à seguinte regra:

«Quem controla a ação deve controlar o feedback dessa ação.»

Assim, o "App" torna-se responsável pelo feedback das operações que alteram "tasks".

---

3. A primeira solução: um estado para cada feedback

Uma solução possível seria criar vários estados:

const [addFeedback, setAddFeedback] = useState("")
const [saveFeedback, setSaveFeedback] = useState("")
const [deleteFeedback, setDeleteFeedback] = useState("")
const [completeFeedback, setCompleteFeedback] = useState("")
const [favoriteFeedback, setFavoriteFeedback] = useState("")

Funcionaria.

Mas começámos a perceber um problema.

Todos esses estados representam praticamente a mesma coisa:

«Uma mensagem temporária de feedback para o utilizador.»

Apenas mudam:

- o texto;
- o tipo da mensagem.

Por exemplo:

addFeedback
saveFeedback
deleteFeedback
completeFeedback
favoriteFeedback

Todos representam:

"Existe uma mensagem de feedback para mostrar?"

Então surgiu uma ideia melhor:

«Será que precisamos realmente de vários estados para representar a mesma ideia?»

---

4. A ideia de um único estado

Em vez de termos:

const [addFeedback, setAddFeedback] = useState("")

e depois outros estados para as restantes ações, podemos ter:

const [feedback, setFeedback] = useState(null)

Agora o estado "feedback" representa:

«O feedback atualmente apresentado ao utilizador.»

Inicialmente:

feedback = null

Isso significa:

«Não existe nenhum feedback neste momento.»

---

5. Mas precisamos de mais do que uma mensagem

Surge então outra questão.

Não queremos apenas saber o texto.

Também queremos saber que tipo de feedback é esse.

Por exemplo:

Tarefa adicionada!

poderia ser um feedback de sucesso.

Enquanto:

Erro ao adicionar tarefa.

seria um feedback de erro.

Então podemos guardar duas informações dentro do mesmo estado:

{
    type: "success",
    message: "Tarefa adicionada com sucesso!"
}

O estado deixa de ser apenas uma string.

Agora ele é um objeto.

---

6. Estrutura do estado

Podemos pensar no estado assim:

const [feedback, setFeedback] = useState(null)

Quando não existe feedback:

feedback = null

Quando existe um feedback:

feedback = {
    type: "success",
    message: "Tarefa adicionada com sucesso!"
}

Ou:

feedback = {
    type: "error",
    message: "Não foi possível realizar a operação."
}

Portanto:

feedback
   │
   ├── type
   │     └── "success"
   │
   └── message
         └── "Tarefa adicionada com sucesso!"

---

7. Por que "type" é importante?

O "message" diz o que devemos escrever.

O "type" diz como devemos tratar visualmente essa mensagem.

Por exemplo:

{
    type: "success",
    message: "Tarefa adicionada com sucesso!"
}

Pode resultar numa mensagem verde.

Enquanto:

{
    type: "error",
    message: "Escreva um título."
}

pode resultar numa mensagem vermelha.

Assim, o mesmo componente de feedback consegue lidar com diferentes situações.

---

8. Pensando em português antes do React

Antes de escrever código, podemos descrever o comportamento:

«Quando uma ação for concluída com sucesso, guardar no estado de feedback o tipo "success" e a mensagem correspondente.»

Em português:

Adicionar tarefa
      ↓
Tarefa foi adicionada
      ↓
feedback recebe:
"success"
"Tarefa adicionada com sucesso!"
      ↓
React renderiza a mensagem
      ↓
Depois de alguns segundos
      ↓
feedback volta para null

O mesmo conceito pode ser utilizado para salvar:

Salvar tarefa
      ↓
Tarefa foi salva
      ↓
feedback recebe:
"success"
"Tarefa salva com sucesso!"
      ↓
Mensagem aparece
      ↓
Depois de alguns segundos
      ↓
feedback = null

---

9. O "setTimeout"

Queremos que a mensagem desapareça automaticamente.

O fluxo é:

setFeedback(...)
      ↓
mensagem aparece
      ↓
espera 3 segundos
      ↓
setFeedback(null)
      ↓
mensagem desaparece

Em JavaScript, usamos:

setTimeout(() => {
    setFeedback(null)
}, 3000)

O "3000" representa milissegundos:

1000 ms = 1 segundo

3000 ms = 3 segundos

Portanto:

setFeedback({
    type: "success",
    message: "Tarefa adicionada com sucesso!"
})

setTimeout(() => {
    setFeedback(null)
}, 3000)

---

10. Uma função para evitar repetição

Começámos então a perceber outro problema.

Se fizermos isso em todas as funções:

function addTask(...) {
    setFeedback({
        type: "success",
        message: "Tarefa adicionada!"
    })

    setTimeout(() => {
        setFeedback(null)
    }, 3000)
}

Depois:

function saveEditingTask(...) {
    setFeedback({
        type: "success",
        message: "Tarefa salva!"
    })

    setTimeout(() => {
        setFeedback(null)
    }, 3000)
}

Depois:

function deleteTask(...) {
    setFeedback({
        type: "success",
        message: "Tarefa eliminada!"
    })

    setTimeout(() => {
        setFeedback(null)
    }, 3000)
}

Estamos repetindo a mesma lógica.

Então podemos criar uma função responsável por mostrar feedback.

Por exemplo:

function showFeedback(type, message) {
    setFeedback({
        type: type,
        message: message
    })

    setTimeout(() => {
        setFeedback(null)
    }, 3000)
}

Agora as outras funções podem simplesmente dizer:

showFeedback("success", "Tarefa adicionada com sucesso!")

ou:

showFeedback("success", "Tarefa salva com sucesso!")

ou:

showFeedback("success", "Tarefa eliminada!")

---

11. O ganho dessa abordagem

Agora temos:

const [feedback, setFeedback] = useState(null)

em vez de:

const [addFeedback, setAddFeedback] = useState("")
const [saveFeedback, setSaveFeedback] = useState("")
const [deleteFeedback, setDeleteFeedback] = useState("")
const [completeFeedback, setCompleteFeedback] = useState("")
const [favoriteFeedback, setFavoriteFeedback] = useState("")

E existe um único conceito:

feedback

que pode representar qualquer feedback da aplicação.

---

12. O componente visual

Depois precisamos decidir:

«Onde o feedback será apresentado?»

O "App" controla o estado.

Então podemos ter algo como:

{feedback && (
    <p className={...}>
        {feedback.message}
    </p>
)}

Aqui aparece um conceito que já aprendemos:

feedback && (...)

Significa:

«Se existir um feedback, renderiza a mensagem.»

Quando:

feedback = null

nada aparece.

Quando:

feedback = {
    type: "success",
    message: "Tarefa adicionada!"
}

o React renderiza a mensagem.

---

13. O "type" também pode controlar o CSS

Podemos usar o "type" para decidir a classe CSS.

Por exemplo:

className={feedback.type === "success"
    ? styles.success
    : styles.error
}

Assim:

feedback.type
      │
      ├── success → estilo de sucesso
      │
      └── error → estilo de erro

Podemos até pensar em outros tipos no futuro:

success
error
warning
info

Por exemplo:

{
    type: "warning",
    message: "Esta ação precisa de atenção."
}

O sistema já estaria preparado para crescer.

---

14. O CSS

Podemos ter estilos diferentes para cada tipo.

Por exemplo:

.success {
    text-align: center;
    margin: 16px auto 0;
    padding: 12px 16px;
    max-width: 500px;

    background-color: #ECFDF5;
    color: #047857;

    border: 1px solid #A7F3D0;
    border-radius: 8px;

    font-size: 16px;
    font-weight: 500;
}

.error {
    text-align: center;
    margin: 16px auto 0;
    padding: 12px 16px;
    max-width: 500px;

    background-color: #FEF2F2;
    color: #B91C1C;

    border: 1px solid #FECACA;
    border-radius: 8px;

    font-size: 16px;
    font-weight: 500;
}

Agora o conteúdo e a aparência estão separados.

O React decide:

Qual mensagem?
Qual tipo?

O CSS decide:

Como esse tipo deve aparecer?

---

15. O fluxo completo

A arquitetura mental ficou assim:

                    UTILIZADOR
                         │
                         ▼
                 clica em Adicionar
                         │
                         ▼
                  TaskForm envia
                    nova tarefa
                         │
                         ▼
                     App recebe
                    nova tarefa
                         │
                         ▼
                     setTasks()
                         │
                         ▼
                  tasks é atualizado
                         │
                         ▼
                 showFeedback()
                         │
                         ▼
              setFeedback({
                  type: "success",
                  message: "Tarefa adicionada!"
              })
                         │
                         ▼
                    React renderiza
                     o feedback
                         │
                         ▼
                    espera 3 segundos
                         │
                         ▼
                 setFeedback(null)
                         │
                         ▼
                  feedback desaparece

---

16. O mais importante: o padrão mental

O principal aprendizado não é simplesmente:

const [feedback, setFeedback] = useState(null)

O mais importante é aprender a perguntar:

1. O que estou tentando representar?

Neste caso:

«Uma mensagem temporária de feedback.»

2. Quantos estados realmente preciso?

Percebemos que não precisávamos de um estado para cada ação.

Precisávamos de um estado que represente o conceito "feedback atual".

3. Que informação esse estado precisa carregar?

Descobrimos:

type
message

4. Quem controla esse estado?

O "App", porque ele controla as operações principais da aplicação.

5. O que acontece quando o estado muda?

O React renderiza novamente e o feedback aparece.

6. Quando o estado deve desaparecer?

Depois de 3 segundos:

setTimeout(...)

---

17. Uma observação importante sobre o nosso projeto

Não significa que:

«"No React, devemos sempre juntar vários estados num único estado."»

Não.

Se os estados representam conceitos diferentes, é perfeitamente correto separá-los.

Por exemplo:

const [tasks, setTasks] = useState([])
const [editingTaskId, setEditingTaskId] = useState(null)
const [feedback, setFeedback] = useState(null)

São três estados diferentes porque representam três conceitos diferentes:

tasks
  ↓
dados das tarefas

editingTaskId
  ↓
qual tarefa está sendo editada

feedback
  ↓
mensagem temporária para o utilizador

O objetivo não é ter "poucos estados".

O objetivo é:

«Cada estado deve representar um conceito coerente da interface/aplicação.»

---

18. A evolução do nosso raciocínio

Começámos com:

Preciso mostrar uma mensagem depois de adicionar.

↓

Pensámos:

Vou criar addFeedback.

↓

Depois:

Também preciso de saveFeedback.
deleteFeedback.
completeFeedback...

↓

Percebemos:

Todos representam a mesma ideia:
FEEDBACK

↓

Então:

const [feedback, setFeedback] = useState(null)

↓

Percebemos que precisamos distinguir os tipos:

{
    type: "success",
    message: "Tarefa adicionada!"
}

↓

Criámos um conceito reutilizável:

showFeedback(type, message)

↓

E finalmente:

Qualquer ação
      ↓
showFeedback()
      ↓
feedback
      ↓
React renderiza
      ↓
3 segundos
      ↓
feedback = null

Esse foi o caminho de análise → decisão → modelo mental → React.

E é exatamente esse tipo de raciocínio que queremos continuar praticando no projeto.