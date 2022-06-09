Algoritmo de Wilson para Construção de Labirintos
======

Um pouco de teoria dos grafos
---------

!!! Atenção

Comece o handout **após** a exposição inicical sobre grafos.

!!!


??? Checkpoint 1

Esse exemplo demonstra como um grafo pode representar objetos e como esses objetos se relacionam

Digamos que seis pessoas estão indo para uma festa logo apos uma semana cansativa de provas e projetos. Quando chegam à festa, a pessoa **A** comprimenta as pessoas **E** e **F**, **B** comprimenta **C**, e **F** comprimenta **B** e **E**. 

Como ficaria a representação dessas seis pessoas na festa e com quem cada uma interagiu?

::: Esquema
: graph
:::

::: Expressão Final
$$G = \left (\left\{A, B, C, D, E, F\right\}, \left\{\left\{A, E\right\}, \left\{A, F\right\}, \left\{B, C\right\}, \left\{F, B\right\}, \left\{F, E\right\}\right\}\right )$$
:::

???


Árvores Geradoras
---------

Agora que ja sabemos o que é um grafo, fica fácil de definir o conceito de árvore geradora (ou Spanning Tree).

Basicamente, para tornar um grafo uma árvore geradora basta escolher um subconjunto das arestas de forma que entre qauisquer dois vertices existe um, e **apenas um**, caminho. Ou seja não seria possível ir de um vértice ao outro de duas maneiras diferentes, muito parecido com o conceito de um **labirinto** não é mesmo?

Ou seja, uma Árvore Geradora é quase a mesma coisa que um grafo simples, mas existem apenas dois "poréns":

1. Esse grafo precisa conter **todos** os vértices.
    - Não podem existir vértices "soltos".
2. Todos os vértices possuem a **quantidade mínima** de arestas (não podem existir "ciclos").
    - Não pode ser possível ir de um vértice ao outro de duas maneiras diferentes.

Ou seja, se contássemos o numero total de arestas de uma árvore geradoras, ela sempre será igual a *__n - 1__*, onde *__n__* seria o número de vértices.

![](spanning_tree_gif.gif)

??? Checkpoint 2

No caso do primeiro *checkpoint*, seu grafo final poderia ser considerado uma árvore geradoras?

::: Gabarito
**Não**, já que ele não contém todos os seus vértices (deixando **D** de fora), e é possível ir de um vértice ao outro de duas maneiras diferentes, representando um ciclo (**A**, **E**, e **F**).
:::

???

??? Checkpoint 3

Dado abaixo os vértices de um grafo, como poderiam ser conectados de forma a se tornar uma árvore geradora? (lembrando que existe mais de um jeito possível)

![](nodes.png)

::: Gabarito
Aqui está uma solução possível:

:spanning_tree
:::

???

Com o exemplo acima, deve ficar mais claro ainda a semelhança de árvores geradoras com *__caminhos__*, e ainda por cima, caminhos *__únicos__*. E dessa ideia, surgiram inúmeras aplicações para esse tipo de grafo, como, por exemplo, no design de redes elétricas, computadores ou até mesmo de *__labirintos__*. Essa visão sobre *__caminhos únicos__* será muito importante no proximo tópico para o algorítimo que iremos introduzir!



Algoritmos de criação de labirintos
---------

Mas qual é a finalidade de construir labirintos por meio de um algoritmo?

* Construção de labirintos para entusiastas, deixando-os mais complexos;
* Criação de estruturas de dados enormes para testar e treinar algoritmos de *path finding*.

----------------------------------------------------------------------------

E como vamos montar o **grafo** para este algoritmo?

Para a construção de algoritmos, o grafo possui uma estrutura em formato de {red}(quadriculado), ou seja, os vértices são os centros dos quadrados, e as arestas entre quadrados são as arestas do grafo caso **não existam** ou indicam que não há ligação, paredes, **caso existam**.

![](Grafo_labirinto.png)

Para o {blue}(input), todas essas arestas entre os vérticies do grafo existem, ou seja, **todo vértice sabe quem são seus vizinhos**, e também possui uma variável responsável por saber se este já foi visitado ou não.

``` c
typedef struct {
    char visitado;
    int vizinhos[];
} vertice;
```

Nosso objetivo agora é construir os caminhos que serão passagens e esconder aqueles que não são, de modo a se transformarem em paredes. Para isso, vamos pensar em um exemplo muito simples na vida real:

Em um quarto muito amplo, precisamos construir nossas paredes do labirinto, mas só podemos andar nas **direções cardeais**. Para prosseguir, devemos escolher uma direção que {red}(não) seja parede ou um ponto no quarto que já se passou. Quando o movimento ocorrer, deve-se erguer uma parede para todo o resto do quarto que não conhecemos ao realizar o movimento. Essa escolha de movimento deve ser aleatória, como tentativa de ser o mais difícil possível de encontrar uma saída.

Este processo de andar aleatóriamente e ir construindo paredes é conhecido como [Random Walk](https://en.wikipedia.org/wiki/Random_walk). Mas para a nossa aplicação isso não é suficiente.

??? Checkpoint 5:

Sabendo que um caminho {red}(não) pode passar por vértices já visitados, o que deve acontecer quando o vértice **não possui mais vizinhos**?

**Obs.:** Neste momento não é necessário desenvolver o código, somente uma resposta de como ele funcionaria.

::: Gabarito

Deve ser criada uma **`md pilha` com os vértices que estão sendo visitados**, caso o vértice seguinte não possua mais vizinhos, deve-se retirar os vértices da pilha até encontrar um vértice com vizinhos não visitados e denominá-lo de vértice atual. Então definido, deve-se continuar o caminho a partir dele.

:::

???

Agora que a ideia do percurso foi montado, a animação abaixo mostra o funcionamento desta ideia na prática, como a teoria do {red}(Random Walk) mais a {red}(Pilha de Posições) para construir o caminho do labirinto:

:randomWalk

Esta construção de ligações sequenciais entre os elementos, garantindo a existência de um só caminho entre dois pontos, cria, ao final, uma árvore geradora!

!!! Atenção

Não continue pelo handout até entender completamente a ideia de labirinto e como pensar em sua construção, qualquer dúvida não hesite em pedir ajuda

!!!

----------------------------------------------------------------------------

**Agora vamos pensar...**

A ideia do RandomWalk é interessante, porém, se você testar construir alguns labirintos diferentes, vai se deparar com um problema, existe um padrão que nós não queremos: este algoritmo *sem querer* tem a preferência pela construção de `md caminhos longos`.

Isso é um problema porque estamos procurando por algoritmos **{green}(perfeitos)**, ou seja, assim como na árvore geradora uniforme, devem existir **chances iguais** para a construção de `md qualquer configuração` de caminhos possíveis. Como o algoritmo possui uma preferência, essa regra não é mais válida!

Para solucionar esse problema, foram então criados alguns algoritmos para evitar esse problema, um deles é o **Algoritmo de Wilson**, que será explicado mais à frente. Vamos entender um outro algoritmo primeiro, de modo a ser possível realizarmos algumas comparações; este é o *Algoritmo de Aldous-Broder*.

Algoritmo de Aldous-Broder
---------

Depois que entendemos e trabalhamos um pouco com o *Random Walk*, vamos começar pelo algoritmo de **{green}(Aldous-Broder)**.

Para garantir a aleatoriedade do sistema, este algoritmo escolhe aleatoriamente qualquer direção a percorrer, ou seja, não existe nenhuma proibição de avanço.

Pensando na estrutura do *Random Walk*, no algoritmo de Broder a escolha de que caminho seguir **não é levado em consideração os vizinhos já percorridos ou paredes criadas**, e, por isso, na maioria das vezes existem 4 caminhos possíveis para seguir.

??? Checkpoint 8

Pensando no funcionamento aleatório do sistema, qual é o grande problema causado por este quando a malha do labirinto é muito grande?

::: Gabarito

Pensando em uma malha maior de vértices disponíveis, embora ele percorra por vértices não visitados rapidamente no início, com o passar das iterações fica cada vez **mais difícil de encontrar um vértice que não esteja preenchido** de modo aleatório, o que indica o {red}(crescimento exponencial) dessa descoberta.

:::
???

Fica difícil de entender como este funciona e como essa aleatoriedade se torna preocupante com o tempo. Por isso recomendamos que vocês percebam este fator [neste vídeo](https://www.youtube.com/watch?v=-EZwuFdkJes&ab_channel=Ferenc).

Em relação ao funcionamento de preenchimento, quando o vértice a ser encontrado ainda não foi visitado, o algoritmo cria o caminho baseado naquele tomado para lá chegar. Caso esse vértice já tenha sido visitado, a parede continuará como separados do caminho.

![](AldousBroder.png)


Algoritmo de Wilson
---------

O algoritmo de Broder, realiza várias operações desnecessárias se considerarmos que os vértices
preenchidos não precisam ser mais percorridos pelo código novamente. Vamos ver um método que 
apenas percorre pelos vértices intocados.

Outra mudança importante que vamos adotar é que agora ao invés de pensar apenas nos "passos" do algoritmo,
vamos ter que pensar em seu "caminho", ou seja o conjunto de passos que juntos serão preenchidos.

Aqui temos o jeito antigo de preencher como visto no algoritmo de Aldous Broder onde o vértice é preenchido assim que 
chegamos nele

:aldousWay

O nosso novo algoritmo vai traçar um **{pink}(caminho)**, para depois todos esses pontos serem preenchidos.

:wilsonWay

??? Checkpoint 9

Um problema que você deve ter percebido, percorrendo um caminho apenas pelos vértices não preenchidos como podemos conectar eles ao resto do labirinto?

::: Gabarito
Podemos criar uma regra que quando esse caminho encontra o resto do labirinto ele é preenchido.
:::

???

Com a idéia básica do caminho do *random walk*, vamos introduzir agora a ideia do labirinto perfeito, onde todos os seus pontos são acessíveis, e 
entre dois deles há apenas um caminho. 

??? Checkpoint 10

Pensando na ideia acima, um algoritmo usando *random walk* **sempre** geraria labirintos perfeitos?

::: Gabarito
Não, porque pode acontecer de dois caminhos se cruzarem o que geraria um loop, permitindo assim que mais 
de uma rota existisse entre dois pontos!
:::

???

Como poderiamos prevenir a criação de loops? Precisariamos saber quando um vértice a ser preenchido cruza outro 
já definido e impedir que se encontrem apagando a operação.

??? Checkpoint 11

A ideia acima é um bom começo, mas apresenta um problema, qual? Como resolvê-lo?

::: Gabarito
Pode ocorrer o caso em que o vértice está "encurralado" e não tem para onde ir, entrando em um loop infinito. Para resolver o problema quando ocorre o cruzamento apagamos o loop e então seguimos o caminho a partir de sua base. 
:::

???

Esta ideia que desenvolvemos é o *loop-erased random walk*, a peça vital do algoritmo de Wilson.

Abaixo temos uma demonstração do *loop-erased random walk* em ação, caso não tenha entendido algum passo retorne para os checkpoints acima.

:loopErasedRandomWalk

A ideia de David Bruce Wilson para um algoritmo deste tipo é resumidamente, um programa que recebe
como entrada um grafo e escolhe um vértice não preenchido aleatório de início, então começa o 
*loop-erased random walk*. O processo se repete até obter-se uma arvore geradora completamente preenchida.

As vantagens dessa estratégia é que como resultado obtem-se um labirinto perfeito, ou seja, todos os vértices são acessíveis, e 
entre dois deles há apenas um caminho. Outro benefício do algoritmo de Wilson é que pelo caminho ser definido aleatóriamente
ele gera labirintos uniformes, ou seja não existe viés para tipos especificos de árvores geradoras, um problema presente em outros 
algoritmos de geração de labirintos como se pode ver nas duas figuras abaixo:

![](DepthFirstSearchMaze.png)

![](WilsonMaze.png)

A acima temos um labirinto gerado pelo algoritmo *depth-first search* que tem um viés para gerar labirintos com corredores longos e em baixo temos o algoritmo de Wilson.

"Mas porque a geração de labirintos uniformes é importante?" você pode estar se perguntando, bem, imagine que você é um desenvolvedor
de um jogo e quer fazer um algoritmo de pathfinding de um NPC para um jogo onde os mapas são gerados randômicamente. 
Como garantir que a entidade vai sempre pegar o caminho esperado? Esse parte é clara, testando oras! 

É aí que entra a importanciancia do labirinto uniforme, se as a probabilidade de acabar o programa com qualquer árvore geradora é a mesma, então você pode
garantir que seu programa funciona bem para um leque abrangente de casos, sem correr o risco de acabar com a impressão de que o código é melhor ou pior do
que realmente é, como seria o caso de um algoritmo gerador de labirintos com algum tipo de viés.     

Agora que tem uma ideia de como o algorimo funciona, que tal ver uma animação dele em ação? Temos o GIF abaixo e este [site](https://bl.ocks.org/mbostock/11357811) que contém um código que gera um novo labirinto toda vez que você carrega a tela.

![](wilson.gif)

Complexidade do algoritmo de Wilson e comparação com Aldous-Broder
---------
Pelo fato de ser um programa probabilistico, a complexidade do algoritmo de Wilson não é algo simples de ser definida, então vamos colocar os números de lado no momento
("Ufa, engenharia já tem números o suficiente.") e compará-lo com o algoritmo que vimos anteriormente, o algoritmo de Aldous-Broder. De ínicio vamos falar de sua similaridade, ambos utilizam alguma forma de *random walk* para ir preenchendo a árvore geradora, assim, no pior dos casos ambos vão ter a complexidade igual, porém isso
não significa que seus tempo de rodar são iguais. 

Para perceber isso basta analisar suas diferenças, o algoritmo de Aldous-Broder caminha tanto pelas células preenchidas quanto aquelas vazias, preenchendo-as no processo. Isso faz com que o preenchimento seja bem rápido no começo, já que há várias células vazias e poucas preenchidas, o que reduz as chances de já passar por caminho já traçado. Essa mesma qualidade causa o oposto quando esta próximo do fim, quando a maior parte do labirinto já está preenchida o que acaba resultando em várias caminhadas "sem rumo" onde nenhuma célula é adicionada ao labirinto.

O algoritmo de Wilson tem algumas mudanças como vimos, primeiramente os caminhos são apenas traçados por células vazias, só sendo preenchidas quando este encontra alguma célula definida, isto tem o comportamento inverso do Aldous-Broder, sendo mais devagar no começo, quando uma pequena parte do labirinto esta definida sendo difícil de ser encontrada pelo caminho do *loop-erased random walk*, e sendo muito mais rápida no final, quando as chances de encontrar um vértice já definido são muito maiores. 

Entretando embora os comportamentos sejam opostos não significa que ambos demorem o mesmo. O algoritmo de Wilson sempre está diminuindo as suas células disponíveis para percorrer com todas sendo válidas conforme vai preenchendo a árvore geradora. Enquanto o Aldous-Broder sempre tem ```n``` espaços para percorrer com 
o número de células válidas a serem preenchidas diminuindo gradativamente. Isso resulta que, na prática, que o algoritmo de Wilson seja bem mais rápido que o algoritmo de Aldous-Broder tornando-o mais atrativo já que ambos também apresentam as mesmas caracteristicas benéficas de gerarem labirintos perfeitos e com chances uniformes de serem gerados.


Para compravar a análise empirica, podemos ver o gráfico de complexidade de Wilson comparado com outro algoritmo de geração de labirintos: **Aldous-Broder**.

![](complexImg.png)