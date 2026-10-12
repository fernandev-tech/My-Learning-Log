# Git — Comandos aprendidos

## `git status`

### O que faz?
Mostra o estado atual do repositório:
- branch em que estamos;
- arquivos modificados;
- arquivos na staging area;
- situação em relação ao commit.

### Que problema resolve?
> Como saber o que está acontecendo no meu repositório antes de executar qualquer ação?

---

## `git branch -a`

### O que faz?
Mostra as branches locais e as branches remotas conhecidas pelo repositório.

### Que problema resolve?
> Como descobrir quais branches existem antes de criar ou trocar para uma branch?

---

## `git switch -c <nome-da-branch>`

### O que faz?
Cria uma nova branch e muda imediatamente para ela.

### Que problema resolve?
> Como começar um trabalho isolado sem trabalhar diretamente na `main`?

---

## `git diff`

### O que faz?
Mostra as diferenças entre o estado atual dos arquivos e o último estado registrado pelo Git.

### Que problema resolve?
> Como verificar exatamente o que alterei antes de colocar essas alterações na staging area ou fazer um commit?

---

## `git add <arquivo>`

### O que faz?
Coloca uma alteração específica na staging area, preparando-a para o próximo commit.

### Que problema resolve?
> Como escolher quais alterações quero preparar para serem registradas no próximo commit?

---

## `git commit`

### O que faz?
Registra no histórico do Git as alterações que estão na staging area.

### Que problema resolve?
> Como criar um ponto registrado no histórico ao qual posso voltar ou que posso compartilhar com a equipa?