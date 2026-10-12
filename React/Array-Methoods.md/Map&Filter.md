# Entre map e filter qual o melhor método para criar uma Lista de Favoritos numa To-do List

Tanto map() quanto filter() podem produzir um novo array. A diferença é qual é o objetivo dessa nova lista.

map() responde:

"Quero pegar todos os elementos e transformar cada um deles."

filter() responde:

"Quero pegar somente alguns elementos que satisfazem uma condição."

E no nosso caso queremos:

"Pegue somente as tarefas que são favoritas."

Isso é literalmente um filtro.

## Vamos usar um exemplo

Temos:

tasks

```bash
Tarefa 1 → favorite: false
Tarefa 2 → favorite: true
Tarefa 3 → favorite: false
Tarefa 4 → favorite: true
```

Queremos:

favoriteTasks

```bash
Tarefa 2
Tarefa 4
```

Então estamos selecionando alguns elementos e descartando outros.

Isso é:

```JS
filter()
```

Mentalmente:
```

                  tasks
                    │
          ┌─────────┼─────────┐
          ↓         ↓         ↓
       false      true      false ...
          ✗         ✓          ✗
                    │
                    ↓
              nova lista
              de favoritos
```

## E o map()?

Imagina:

```JSX
tasks.map(task => {
  return {
    ...task,
    title: task.title.toUpperCase()
  }
})
```

Aqui eu quero:

```bash
Tarefa 1 → transforma
Tarefa 2 → transforma
Tarefa 3 → transforma
Tarefa 4 → transforma
```

Ninguém é eliminado da lista.

Tenho aproximadamente a mesma quantidade de elementos antes e depois.

```JSX
4 tarefas
   ↓
map()
   ↓
4 tarefas transformadas

Enquanto com filter():

4 tarefas
   ↓
filter(favorite === true)
   ↓
2 tarefas
```

# A pergunta certa para escolher entre os dois

Quando estiveres diante de um problema, pensa:

"Quero modificar/transformar os elementos?"

```JS
→ map()
```

"Quero selecionar somente alguns elementos?"

```JS
→ filter()
```

No nosso caso:

Quero selecionar somente as tarefas favoritas.

Logo:

```JS
filter()
```

E sim, o resultado será uma nova lista.

Por exemplo:

const favoriteTasks = tasks.filter(task => task.favorite === true)

Temos:

```JS
tasks
 ↓
filter()
 ↓
favoriteTasks
```

Não estamos dizendo que ```filter()``` "não cria uma lista".

Ele cria.

A questão é como essa lista é construída.