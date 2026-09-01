# Algumas Perguntas ao Fazer-se Ao Construir Projetos Com IA

🧠 Pergunta 1 — O que precisa acontecer?

Exemplo:

“Quero editar uma tarefa. O que precisa acontecer?”

Resposta:

O usuário precisa conseguir alterar título e descrição.

🧠 Pergunta 2 — Que dado está mudando?

“O que muda?”

Resposta:

título

descrição

🧠 Pergunta 3 — Preciso guardar essa mudança?

“Esse valor precisa ser lembrado pelo React?”

Se sim:

useState
🧠 Pergunta 4 — O que deve aparecer na interface?

“Quem diz ao input o que mostrar?”

Resposta:

value
🧠 Pergunta 5 — Quando o usuário modificar?

“Como descubro que o valor mudou?”

Resposta:

onChange
🧠 Pergunta 6 — Quando alguma coisa mudar, preciso provocar outra ação?

“Quando editingTask mudar, preciso inicializar os campos?”

Sim.

Então pensamos:

useEffect
🧠 Pergunta 7 — Quem possui o estado principal?

“Quem possui tasks?”

App.

Então:

“Quem deve modificar tasks?”

App.

Isso evita começares a colocar lógica aleatoriamente nos componentes.

12. Uma espécie de mapa mental para usares nos projetos

Quando encontrares um problema em React, começa a pensar:

O que aconteceu?
      ↓
Que dado mudou?
      ↓
Quem precisa saber disso?
      ↓
Onde esse dado deve viver?
      ↓
A interface precisa reagir?
      ↓
Como o usuário altera esse dado?
      ↓
Como essa alteração chega ao estado principal?
      ↓
Como o estado principal será atualizado?
      ↓
O que a interface deve renderizar depois?

E começa a associar:

_________
Pergunta |	Ferramenta que pode aparecer
Preciso guardar um valor que muda?|	useState
Preciso | reagir a uma mudança/efeito?	useEffect
Usuário clicou?	onClick
Valor de input mudou?	onChange
Formulário foi enviado?	onSubmit
Preciso encontrar um item?	find()
Preciso transformar uma lista?	map()
Preciso remover itens?	filter()
Preciso mostrar algo dependendo de uma condição?	ternário / &&
Preciso enviar informação para um filho?	props
Filho precisa avisar o pai?	função passada por prop