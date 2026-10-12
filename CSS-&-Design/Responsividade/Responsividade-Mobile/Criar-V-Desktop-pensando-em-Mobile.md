# Perguntas Básicas Ao Aplicar Antes da Aplicação da Responsividade  Mobile

Quando estamos desenvolvendo um site e primeiro começamos no desktop e ao mesmo tempo pensar no Mobile, ou seja, prever que design usando no desktop poderá facilitar na hora da responsividade mobile. Essa abordagem tem até um nome pra ela no mercado: **"desktop-first com mentalidade responsiva".**

 Não é abandonar o mobile, é adiar a implementação dele sem adiar o pensamento sobre ele.

Na prática, pra você seguir esse princípio a cada declaração CSS, algumas perguntas rápidas que valem virar hábito:

## 1. "Essa largura devia ser fixa ou relativa?"

Em vez de ```width```: 400px, pergunta se width: 40% ou max-width: 400px (que permite encolher) resolveria igual no desktop sem travar no mobile.

## 2. "Esse elemento está ao lado de outro por flex-direction: row?" 

Se sim, já é meio caminho andado: no mobile, você só muda pra column numa media query, sem reescrever a lógica toda.
    
## 3. "Esse posicionamento depende de coordenadas exatas (position: absolute com top/left em px)?" 

Se sim, é um sinal de alerta. Esses são os que mais quebram feio em telas menores. Prefira flex/grid sempre que possível pra posicionar elementos.
   
## 4. "Meu texto tem tamanho fixo (font-size: 32px)?" 

Considera clamp() (ex: font-size: clamp(20px, 4vw, 32px)) pra títulos grandes — isso faz o texto encolher suavemente conforme a tela diminui, sem precisar de media query nenhuma pra isso especificamente.

Se você for construindo com esses 4 hábitos em mente desde já, quando chegar a fase de responsividade de verdade, o trabalho vai ser muito mais "ajustar breakpoints" do que "reescrever seções inteiras". Ótima decisão de arquitetura pro processo.

