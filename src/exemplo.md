Algoritmo de Wilson para Construção de Labirintos
======

Um pouco de teoria dos grafos
---------

O que é um grafo?

Um grafo de teoria dos grafos, em particular, é um assunto essêncial para entender o handout de hoje. Na teoria dos grafos, um grafo é um par ordenado que consiste em um conjunto de vértices e, em seguida, em um conjunto de arestas. 

$$G = \left ( v, e \right )$$

Os grafos são frequentemente representados como diagramas, com pontos ou circulos representando vértices e linhas representando arestas. Cada aresta une dois vértices, de modo que as linhas no diagrama de um grafo vão de um vértice a outro vértice. Então, o conjunto de arestas de um grafo consiste em subconjuntos de dois elementos do conjunto de vértices, pois em um grafo simples, cada aresta é inteiramente definida pelos vértices que une. 

![](simpleGraph.png)

$$G = \left ( \left\{ 1, 2, 3, 4 \right\}, \left\{ \left\{ 1, 2 \right\}, \left\{ 1, 3 \right\}, \left\{ 1, 4 \right\}, \left\{ 2, 3 \right\} \right\} \right )$$

Ah, a propósito, estamos falando apenas de grafos simples, que são os tipos de grafos mais bem estudados na teoria dos grafos, e geralmente são chamados apenas de grafos. Entre outras restrições, grafos simples não permitem loops, multi-arestas ou arestas direcionadas. Para ficar mais fácil a compreensão, veja o exemplo abaixo:


??? Checkpoint 1

Esse exemplo demonstra como um grafo pode representar objetos e como esses objetos se relacionam

Digamos que seis pessoas estão indo para uma festa logo apos uma semana cansativa de provas e projetos. Quando chegam à festa, a pessoa **A** comprimenta as pessoas **E** e **F**, **B** comprimenta **C**, e **F** comprimenta **B** e **E**. 

Como ficaria a representação dessas seis pessoas na festa e com quem cada uma interagiu?

::: Esquema
: graph
:::

::: Equação Final
$$G = \left (\left\{A, B, C, D, E, F\right\}, \left\{\left\{A, E\right\}, \left\{A, F\right\}, \left\{B, C\right\}, \left\{F, B\right\}, \left\{F, E\right\}\right\}\right )$$
:::

???


Arvores de Extensão
---------

Agora que ja sabemos o que é um grafo, fica fácil de definir o conceito de Arvore de Extensão (ou Spanning Tree).

Uma Arvore de Estensão, também chamada de sub-grafo de extenção, é quase a mesma coisa que um grafo simples, mas existem apenas dois "poréns":

1. Esse grafo precisa conter **todos** os vértices.
2. Todos os vértices possuem a **quantidade mínima** de arestas (não podem existir "ciclos").

Ou seja, se contássemos o numero total de arestas de uma árvore de extensão, ela sempre será igual a *__n - 1__*, onde *__n__* seria o número de vértices.

![](spanning_tree_gif.gif)

??? Checkpoint 2

No caso do primeiro *checkpoint*, seu grafo final poderia ser considerado uma árvore de extensão?

::: Gabarito
**Não**, já que ele não contém todos os seus vértices (deixando **D** de fora), e seus vértices não possuem a quantidade mínima de arestas, representando um ciclo (**A**, **E**, e **F**).
:::

???

??? Checkpoint 3

Dado abaixo os vértices de um grafo, como poderiam ser conectados de forma a se tornar uma árvore de extensão (lembrando que existem mais de um jeito possível)?

![](nodes.png)

::: Gabarito
Aqui está uma solução possível:

:spanning_tree
:::

???

Se parar para pensar, uma característica muito interessante das árvores de extensão é que se parecem muito com *__caminhos__*, e ainda por cima, *__únicos__*. E dessa ideia, surgiram inúmeras aplicações para esse tipo de grafo, como, por exemplo, no design de redes elétricas ou até mesmo de computadores. Essa visão sobre *__caminhos únicos__* será muito importante no proximo tópico para o algorítimo que iremos introduzir.



Algoritmos de criação de labirintos
---------

Mas qual é a finalidade de construir labirintos por meio de um algoritmo?

* Construção de labirintos para entusiastas, deixando-os mais complexos;
* Criação de estruturas de dados enormes para testar e treinar algoritmos de *path finding*.

----------------------------------------------------------------------------

E como vamos montar o **grafo** para este algoritmo?

Para a construção de algoritmos, o grafo possui uma estrutura em formato de {red}(quadriculado), ou seja, os vértices são os centros dos quadrados, e as arestas entre quadrados são as arestas do grafo caso **não existam** ou indicam que não há ligação, paredes, **caso existam**.

Para o {blue}(input), todas essas arestas entre os vérticies do grafo existem, ou seja, **todo vértice sabe quem são seus vizinhos**, e também possui uma variável responsável por saber se este já foi visitado ou não.

``` c
typedef struct {
    char visitado;
    int vizinhos[];
} vertice;
```

??? Checkpoint 4:

Sabendo que para construir um labirinto os caminhos devem ser escolhidos aleatoriamente para garantir que existam vários tipos de construção para a mesma estrutura, como você construiria a função que percorre pelos vértices do grafo para marcar o caminho?

**Obs. 1:** Neste momento não é necessário desenvolver o código, somente uma resposta de como ele funcionaria.

**Obs. 2:** Esta função deve ser pensada para rodar por vértice, então pense como funcionaria dentro de um deles.

::: Gabarito

O código deve, a partir de um determinado vértice, escolher `md aleatoriamente` um **vizinho** que será o próximo a ser analisado, marcando o atual como um vértice já visitado. Depois devemos redefinir os vizinhos para serem somente o anterior a ele e o próximo escolhido, de modo a formar o caminho por onde o labirinto passa.

:::

???

Este conceito de varredura aleatória para a construção de labirintos é conhecido como [Random Walk](https://en.wikipedia.org/wiki/Random_walk), mas possui uma diferença:

??? Checkpoint 5:

Sabendo que um caminho {red}(não) pode passar por vértices já visitados, o que deve acontecer quando o vértice **não possui mais vizinhos**?

**Obs.:** Neste momento não é necessário desenvolver o código, somente uma resposta de como ele funcionaria.

::: Gabarito

Deve ser criada uma **`md pilha` com os vértices que estão sendo visitados**, caso o vértice seguinte não possua mais vizinhos, deve-se retirar os vértices da pilha até encontrar um vértice com vizinhos não visitados e denominá-lo de vértice atual. Então definido, deve-se continuar o caminho a partir dele.

:::

???

Agora que a ideia do percurso foi montado, a animação abaixo mostra o funcionamento desta ideia na prática, como a teoria do {red}(Random Walk) mais a {red}(Pilha de Posições) para construir o caminho do labirinto:

:randomWalk

!!! Atenção

Não continue pelo handout até entender completamente a ideia do algoritmo, qualquer dúvida não hesite em pedir ajuda

!!!

----------------------------------------------------------------------------

Agora, com a ideia do algoritmo compreendida, vamos montar o **{red}(código)**!

Pensando em toda a construção feita até agora sobre este algorítmo, vamos dividir também as funções do código. A que estamos particularmente interessados é na procura dos vértices e a sequência aleatória do percurso.

??? Checkpoint 6

Construa a função **{green}(proximo_vertice(vertice *atual))** para procurar no vértice atual os vizinhos disponíveis e escolher aleartoriamente, entre estes, qual é o próximo vértice no caminho do labirinto e substituir o vértice atual por esse selecionado.

**DICA:** Procure pela função ```c rand()``` para ajudar na escolha aleatória.

::: Gabarito

```c
void proximo_vertice(vertice *atual){
    possíveis vértices <- array para guardar possíveis vértices
    para cada vizinho do array{
        verificar se já foi visitado
            se não, adicionar no array de possíveis vértices
    }
    usar a função rand() para escolher um aleatório
    marca como visitado o atual
    *atual <- próximo vértice
    return
}
```
:::

???

Avaliando a função, encontramos o problema já resolvido teoricamente, o que acontece quando {red}(não) temos nenhum vizinho disponível?

??? Checkpoint 7

Implemente na função a pilha de vértices percorridos

**DICA:** Passe como argumento da função o ponteiro para a pilha

::: Gabarito

```c
void proximo_vertice(vertice *atual, stack_int *s){
    anterior <- recebe ponteiro do vértice anterior
    possíveis vértices <- array para guardar possíveis vértices
    n possíveis vértices <- número de possíveis vértices
    para cada vizinho do array{
        verificar se já foi visitado
            se não, adicionar no array de possíveis vértices
            n possíveis vértices ++
    }
    se n < 1{
        vértice *atual = pop( *s )
        return
    }
    usar a funcao rand() para escolher um aleatório
    marca como visitado o atual
    push(*s, *atual)
    *atual <- próximo vértice
    return
}
```

:::

???


Algoritmo de Wilson
---------

EXPLICAR MELHOR LABIRINTOS PERFEITOS, O QUE SÃO E PORQUE É BOM -Check
---------
COMPARAR WILSON E ALDOUS-BRODER
---------
EXPLICAR VELOCIDADE DO WILSON PARA O ALDOUS
---------

O algoritmo de Broder, realiza várias operações desnecessárias se considerarmos que os vértices
não preenchidos não precisam ser mais percorridos pelo código novamente. Vamos ver um método que 
apenas percorre pelos vértices intocados.

Antes de tudo uma mudança importante que vamos adotar é que agora ao invés de pensar apenas nos "passos" do algoritmo,
vamos ter que pensar em seu "caminho", ou seja o conjunto de passos que juntos serão preenchidos.

INSERIR FOTOS EXPLICATIVAS
---------

??? Checkpoint 8

Um problema, agora percorrendo um caminho apenas pelos vértices não preenchidos como podemos conectar eles ao resto do labirinto?

::: Gabarito
Podemos criar uma regra que quando esse caminho encontra o resto do labirinto ele é preenchido.
:::

???

Com a idéia básica do caminho do *random walk*, vamos introduzir agora a ideia do labirinto perfeito, onde todos os seus pontos são acessíveis, e 
entre dois deles há apenas um caminho. 

??? Checkpoint 9

Pensando na ideia acima, um algoritmo usando *random walk* **sempre** geraria labirintos perfeitos?

::: Gabarito
Não, porque pode acontecer de dois caminhos se cruzarem o que geraria um loop, permitindo assim que mais 
de uma rota existisse entre dois pontos!
:::

???

Como poderiamos prevenir a criação de loops? Precisariamos saber quando um vértice a ser preenchido cruza outro 
já definido e impedir que se encontrem apagando a operação.

??? Checkpoint 10

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

Agora que tem uma ideia de como o algorimo funciona, que tal ver uma animação dele em ação? este [site](https://bl.ocks.org/mbostock/11357811) contém um código 
que toda vez que você carrega a página um novo labirinto começa a ser gerado.

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