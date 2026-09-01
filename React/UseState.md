# Anotação sobre aprendizado de UseState

# Índice
1. O que é (estado) UseStae
2. Por que usar estado
3. Vantagens de Usar useState
4. Quando usar?

# 1. O que é Estado

Estado = Memória do componente

É onde o react guarda informações que podem mudar enquanto a aplicação está a rodar.

Exemplo:

Variável normal = esquece valor quando a tela recarrega.

Estado useState = O react lembra e ainda atualiza a tela sozinho quando muda.

---
# 2. Por que Usar estado

Porque na web as coisas mudam.

Se usares só variávl normal a tela não atualiza sozinha.

Com estado - useState, o React redezenha a tela automaticamente. 

```JSX

import { useState} from 'react'

function contador (){
           //Valor  FunçãoParaMudar useState(valor inicial)
     const [contador, setContador] = useState(0)
     
     return (
    <div>
        <p>Você clicou {contador} vezes</p>
        <button onclick={()=> setContador(contador + 1)}>Clicar</button>
    </div>
     )
}
```

1. contador = o valor atualcomeça em 0.
2. setContador = função para mudar o valor.
3. Quando clicas o contador muda e a tela atualiza.

# 3. Vantagens de Usar useState

1. A tela atualiza sozinha. Não precisas recarregar na página.
2. O React lembra do valor mesmo se o componente renderizar de novo.
3. Deixa a aplicação dinâmica e interativa. Diferente de uma página HTML estática.

# 4. Quando usar useState

Use useState quando essa informação: 

1. muda com o tempo
    exemplo: input do usuário, abrir/fechar menu, darkmode
2. E quando mudar a tela precisa mudar junto

Então: 

- Usamos useState quando precisamos guardar dados que mudam e fazer a tela reagir a essa mudança. 