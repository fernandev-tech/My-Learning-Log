. Por que usamos template literal no className?

Não é simplesmente porque queremos duas classes. O motivo mais preciso é que precisamos montar dinamicamente uma única string contendo duas classes.

Queremos algo conceitualmente assim:

className="feedback success"

Mas o success depende do estado:

feedback.type

Então precisamos juntar:

styles.feedback

com:

styles[feedback.type]

Daí:

className={`${styles.feedback} ${styles[feedback.type]}`}

As crases permitem interpolar os valores dentro da string.

Se o feedback for:

{
    type: "success",
    message: "Tarefa adicionada com sucesso!"
}

temos:

styles.feedback + styles.success

Se for:

{
    type: "error",
    message: "Escreva um título."
}

temos:

styles.feedback + styles.error

Então o template literal é uma maneira conveniente de construir essa string dinamicamente.

Poderíamos ter duas classes fixas, por exemplo:

className={`${styles.feedback} ${styles.success}`}

e também funcionaria. Mas não serviria para nosso sistema porque o tipo pode mudar.

2. E concordo contigo: esse CSS está cru 😂

Aquilo era apenas para explicar a mecânica. Para a nossa aplicação, podemos fazer o feedback parecer realmente parte da interface.

Eu manteria a mesma ideia visual que já usamos no errorMessage, porque assim a aplicação começa a ter uma linguagem visual consistente.

Por exemplo:

.feedback {
    text-align: center;

    margin: 16px auto 0;
    padding: 12px 18px;

    max-width: 500px;

    border-radius: 10px;

    font-size: 15px;
    font-weight: 500;

    border: 1px solid transparent;

    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

Depois o tipo define a identidade visual.

Sucesso
.success {
    background-color: #ECFDF5;
    color: #047857;
    border-color: #A7F3D0;
}
Erro
.error {
    background-color: #FEF2F2;
    color: #B91C1C;
    border-color: #FECACA;
}

Assim:

.feedback
   │
   ├── estrutura comum
   │   ├── centralização
   │   ├── padding
   │   ├── tamanho
   │   ├── borda
   │   └── sombra
   │
   └── tipo
       │
       ├── success → verde
       │
       └── error → vermelho

E visualmente teríamos algo como:

✓ Tarefa adicionada com sucesso!

→ fundo verde muito claro + texto verde + borda verde suave.

Escreva um título para adicionar nova tarefa.

→ fundo vermelho muito claro + texto vermelho + borda vermelha suave.

Isso combina bastante bem com o que já fizemos no formulário.

E uma decisão importante

Eu não colocaria cores muito fortes no feedback.

Os botões podem ter cores mais fortes porque são elementos de ação. O feedback é informação. Portanto, fundos claros + texto mais escuro deixam a interface mais elegante e menos agressiva.

E depois, quando chegarmos ao setTimeout, podemos até pensar em uma pequena animação de entrada/saída para o feedback. Aí sim começamos a dar aquele acabamento de aplicação profissional. 👌


