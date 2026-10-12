# Git — Branches e fluxo de trabalho em equipa

## Contexto

Ao começar o estágio, recebi acesso ao repositório GitHub de um projeto real em Next.js.

Uma orientação importante do gestor foi:

> "Sobe em uma branch que não seja a main ou a master."

Isso introduziu um conceito de Git que ainda não tinha estudado no curso: **branches**.

Neste momento, o projeto já foi clonado para o computador e estou na branch `main`, com o repositório limpo. **As dependências do projeto ainda não foram instaladas.**

---

# 🌿 O que é uma branch?

Uma **branch** é uma linha de desenvolvimento independente dentro do histórico do Git.

A ideia é evitar trabalhar diretamente na `main`, que normalmente representa uma linha de código que a equipa quer manter estável.

Podemos imaginar inicialmente:

```text
main
  │
  A ─── B ─── C
```

O projeto está atualmente no commit `C`.

Quando recebo uma tarefa, em vez de continuar diretamente na `main`:

```text
main
  │
  A ─── B ─── C ─── D ─── E
```

crio uma nova branch:

```text
                    tarefa
                      │
                      D ─── E
                     /
A ─── B ─── C ──────
          │
         main
```

A nova branch começa a partir da `main`, mas passa a ter sua própria linha de desenvolvimento.

Assim:

```text
main
```

continua apontando para o estado estável que existia antes da tarefa, enquanto:

```text
tarefa
```

é onde faço as alterações.

---

# 🧠 Branch e commit são conceitos diferentes

É importante não confundir os dois.

## Commit

Um **commit** registra um estado/alteração no histórico do projeto.

```text
A ─── B ─── C
```

Cada letra representa um ponto registrado na história.

Posso pensar num commit como uma **fotografia de um momento do projeto**.

Por exemplo, se uma alteração feita no commit `D` causar um problema, posso analisar o histórico e encontrar um estado anterior que ainda funcionava.

---

## Branch

Uma **branch** representa uma linha de desenvolvimento.

```text
                 tarefa
                    ↓
A ─── B ─── C ─── D ─── E
      ↑
     main
```

Uma forma simples de memorizar:

> **Commit = fotografia de um momento.**
>
> **Branch = caminho/linha de desenvolvimento.**

---

# 🛠️ Fluxo de trabalho esperado no estágio

O fluxo que estou aprendendo é aproximadamente:

```text
1. Atualizar a main
        ↓
2. Criar uma branch para a tarefa
        ↓
3. Trabalhar na tarefa
        ↓
4. Verificar as alterações com git status
        ↓
5. git add
        ↓
6. git commit
        ↓
7. git push
        ↓
8. Criar Pull Request
        ↓
9. A equipa/gestor revisa
        ↓
10. Merge
```

Esse fluxo permite que cada tarefa tenha sua própria linha de desenvolvimento.

---

# 🔄 Antes de criar uma branch

A ideia é começar a tarefa a partir de uma `main` atualizada.

Primeiro verificamos o estado:

```bash
git status
```

No meu caso, o Git mostrou:

```text
On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

Isso significa que:

* estou na branch `main`;
* a minha `main` está sincronizada com `origin/main`;
* não existem alterações locais pendentes;
* o projeto está num estado limpo.

Depois, normalmente, podemos atualizar a `main`:

```bash
git pull origin main
```

Esse comando procura alterações novas na `main` remota e atualiza a cópia local.

---

# 🌱 Criando uma branch

Supondo que a tarefa seja corrigir um formulário de inscrição:

```bash
git switch -c correcao-formulario-inscricao
```

Esse comando realiza duas ações:

```text
switch
   ↓
muda para uma branch

-c
   ↓
cria uma nova branch
```

Portanto:

```bash
git switch -c correcao-formulario-inscricao
```

significa:

> Criar uma nova branch chamada `correcao-formulario-inscricao` e mudar para ela.

---

# 🔎 Verificar a branch atual

Podemos usar:

```bash
git branch
```

Por exemplo:

```text
* correcao-formulario-inscricao
  main
```

O `*` indica a branch atual.

Também podemos usar:

```bash
git status
```

que mostrará algo como:

```text
On branch correcao-formulario-inscricao
```

---

# 📤 Enviando a branch para o GitHub

Depois de implementar e testar a tarefa, podemos registrar as alterações:

```bash
git add ...
```

Depois criar um commit:

```bash
git commit -m "Corrige formulário de inscrição"
```

E enviar a branch para o GitHub:

```bash
git push origin correcao-formulario-inscricao
```

A partir desse momento, a branch também estará disponível no GitHub.

Conceitualmente:

```text
GitHub

main
  │
  └── correcao-formulario-inscricao
```

---

# 🔀 Pull Request

Depois do `push`, normalmente é criado um **Pull Request (PR)**.

O Pull Request permite apresentar as alterações à equipa para revisão antes de elas serem integradas na `main`.

A ideia é:

```text
Minha branch
     │
     │ Pull Request
     ▼
   revisão
     │
     ▼
   main
```

Assim, não sou eu simplesmente colocando código diretamente na `main`.

A equipa pode analisar:

* o que foi alterado;
* quais arquivos foram modificados;
* os commits realizados;
* se a implementação está adequada;
* se existem problemas ou melhorias necessárias.

---

# 🧩 Aplicação no projeto do estágio

O projeto em que estou trabalhando é o **Formizi**, uma plataforma de inscrições e candidaturas para o mercado angolano.

O repositório foi clonado com:

```bash
git clone https://github.com/izitecdev/inscreva-portal.git
```

Depois entrei no projeto:

```bash
cd inscreva-portal
```

E confirmei o estado:

```bash
git status
```

Resultado:

```text
On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

Portanto, atualmente estou neste estado:

```text
main
│
└── working tree clean
```

O projeto já está no computador, mas **as dependências ainda não foram instaladas** e ainda não comecei a implementação da tarefa.

---

# 🎯 Objetivo de aprendizagem

Como ainda estou no início do estudo de Git, não quero apenas copiar comandos.

Quero entender o raciocínio:

```text
Recebi uma tarefa
      ↓
Qual branch devo usar?
      ↓
Como nomear a branch?
      ↓
A partir de onde ela deve ser criada?
      ↓
Faço as alterações
      ↓
Verifico o estado
      ↓
Registro commits
      ↓
Envio a branch
      ↓
Crio o Pull Request
```

A intenção é chegar ao ponto em que consigo participar de um projeto em equipa entendendo **por que estou executando cada comando**, e não apenas sabendo decorá-los.

---

# 📌 Estado atual

* [x] Repositório clonado
* [x] Projeto localizado no computador
* [x] `git status` verificado
* [x] `main` sincronizada
* [x] Working tree limpa
* [ ] Instalar dependências
* [ ] Executar o projeto localmente
* [ ] Receber/analisar a tarefa
* [ ] Criar branch da tarefa
* [ ] Implementar
* [ ] Testar
* [ ] Commit
* [ ] Push
* [ ] Pull Request
* [ ] Revisão/merge

**Próximo passo:** entender a tarefa recebida antes de começar a implementação.
