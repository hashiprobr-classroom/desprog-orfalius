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

Algoritmo de Aldous-Broder
---------

Depois que entendemos e trabalhamos um pouco com o *Random Walk*, vamos começar pelo algoritmo de **{green}(Aldous-Broder)**.

Para garantir a aleatoriedade do sistema, este algoritmo escolhe aleatoriamente qualquer direção a percorrer, ou seja, não existe nenhuma proibição de avanço.

Pensando na estrutura do *Random Walk*, no algoritmo de Broder a escolha de que caminho seguir **não é levado em consideração os vizinhos já percorridos ou paredes criadas**, e, por isso, na maioria das vezes existem 4 caminhos possíveis para seguir.

??? Checkpoint X

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

Vamos introduzir agora a ideia do labirinto perfeito, onde todos os seus pontos são acessíveis, e 
entre dois deles há apenas um caminho. 

??? Checkpoint 8

Pensando na ideia acima, um algoritmo usando *random walk* **sempre** geraria labirintos perfeitos?

::: Gabarito
Não, porque pode acontecer de dois caminhos se cruzarem o que geraria um loop, permitindo assim que mais 
de uma rota existisse entre dois pontos!
:::

???

Como poderiamos prevenir a criação de loops? Precisariamos saber quando um vértice a ser preenchido cruza outro 
já definido e impedir que se encontrem.

??? Checkpoint 9

A ideia acima é um bom começo, mas apresenta um problema, qual?

::: Gabarito
Pode ocorrer o caso em que o vértice está "encurralado" e não tem para onde ir, entrando em um loop infinito. 
:::

???

Para começar a resolver este impasse mudamos um pouco o funcionamento do programa, ao invés de ir preenchendo um caminho
de uma raiz definida e avançando vértice por vértice, podemos escolher qualquer um marcado como vazio e ir construindo um
caminho, ainda não preenchido, até cruzarmos um vértice já definido que é quando preenchemos todo o caminho.

??? Checkpoint 10

A esquemática do algoritmo já esta começando a ser formada, porém ainda não resolvemos o problema do caminho ser
"encurralado" em um loop com si mesmo, como resolveriamos isso?

::: Gabarito
Quando ocorre o cruzamento apagamos o loop e então seguimos o caminho a partir de sua base. 
:::

???

Esta ideia que desenvolvemos é o *loop-erased random walk*, a peça vital do algoritmo de Wilson.

Abaixo temos uma demonstração do *loop-erased random walk* em ação.

:loopErasedRandomWalk

A ideia de David Bruce Wilson para um algoritmo deste tipo é resumidamente, um programa que recebe
como entrada um grafo e escolhe um vértice não preenchido aleatório de início, então começa o 
*loop-erased random walk*. O processo se repete até obter-se um labirinto completamente preenchido.

As vantagens dessa estratégia é que como resultado obtem-se um labirinto perfeito, ou seja, todos os vértices são acessíveis, e 
entre dois deles há apenas um caminho. Outro benefício do algoritmo de Wilson é que pelo caminho ser definido aleatóriamente
ele é imparcial entre caminhos curtos ou longos, um problema de alguns outro algoritmos de geração de labirintos como se pode ver
nas duas figuras abaixo:

![](DepthFirstSearchMaze.png)

![](WilsonMaze.png)

A acima temos um labirinto gerado pelo algoritmo *depth-first search* que tem um viés para gerar corredores longos e em baixo temos o algoritmo de Wilson.

Agora que tem uma ideia de como o algorimo funciona que tal ver uma animação dele em ação? este [site](https://bl.ocks.org/mbostock/11357811) contém um código 
que toda vez que você carrega a página um novo labirinto começa a ser gerado.

Complexidade do algoritmo de Wilson
---------
Pelo fato de ser um programa probabilistico, a complexidade do algoritmo de Wilson não é algo simples de ser definida, entretanto podemos chegar a um numero esperado médio que leva para o grafo ser preenchido.

A probabilidade do programa ficar preso em um loop infinito é nula, requerindo que nunca encontre algum ponto pré definido o que é impossível já que sempre existe esta probabilidade. A probabilidade de percorrer apenas algumas vezes todos os vértices é maior, e conforme o grafo é preenchido ela só cresce. Assim é de se esperar que o tempo que leva para percorrer é diretamente proporcional ao tamanho do grafo portanto pelas regras de simplificação nos que a conclusão que a complexidade do algorítmo de Wilson é O(n).

Para compravar a análise empirica, podemos ver o gráfico de complexidade de Wilson comparado com outro algoritmo de geração de labirintos: **Aldous-Broder**.

![](complexImg.png)