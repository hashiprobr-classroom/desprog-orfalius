Bead sort
======

Introdução
---------

Existem diversos tipos de algoritmos de comparação. Ente eles temos:

* Selection sorts: Selection sort, Heapsort, Smoothsort...;

* Insertion sorts: Insertion sort Shell sort Splay sort...;

* Distribution sorts: American flag sort, Bead sort, Bucket sort...;

De modo geral, algoritmos de distribuição repartem os dados em diferentes espaços de memória (buckets), que são usados, posteriormente, para compor o output. 

Para iniciar a explicação do Bead Sort, precisamos pensar em um ábaco.

![](abaco.png)

De modo geral, ele possui todas as fileiras com a mesma quantidade de unidades e o usuário passa os pedaços de um lado para o outro fazendo algum tipo de contagem. No nosso caso, vamos pensar que as linhas são compostas apenas pela parte relevante, desconsiderando as unidades que sobram e não fazem parte do raciocínio. Considerando que as peças do lado esquerdo são as relevantes, podemos visualizar nosso ábaco da seguinte maneira.

![](meio_abaco.png)

??? Checkpoint 1

Quais valores podemos contar no ábaco?

::: Resposta
Apenas números positivos e inteiros. As restrições do ábaco também são aplicáveis ao Bead sort.
:::

???

??? Checkpoint 2

Com as nossas condições montadas, como podemos organizar nossas linhas em ordem crescente?

!!! Dica
Pense na mudança dos eixos.
!!!

::: Resposta
Se transformarmos os eixos horizontais em verticais, por força da gravidade as peças irão cair, preenchendo os espaços vazios. Daí vem o nome pelo qual o algoritmo é mais conhecido: Gravity Sort.
:::

???

!!! Mão na massa
Aqui temos uma [ferramenta interativa](https://www.youtube.com/watch?v=oavMtUWDBTM) que vai facilitar o entendimento de como a ideia de mudar os eixos do ábaco funciona. Essa parte é fundamental para você concretizar o que viu até agora.

!!!

Com a analogia feita, podemos partir para a implementação do algoritmo.

Implementação em vetor
---------

A segunda implementação que vamos mostrar é a utilizando um vetor. Nesta estratégia iremos pensar usando a ideia do ábaco. Ao trocarmos seu eixo e deixarmos as peças caírem, estamos ordenando visualmente. No entanto, como podemos transformar esse raciocínio em  código?

Como estamos implementando em vetor, precisamos transformar as linhas do ábaco em um único vetor. Para isso iremos transformar cada linha em um vetor de 0 ou 1 em que a quantidade de 1 é o valor da linha e então somar as linhas.

![](vector_loop1.png)

Detalhando um pouco mais temos as seguintes etapas:

1. Transformar as linhas em vetores de tamanho igual ao maior valor do input com zeros;

2. Preencher as posições da esquerda para a direita com 1 na quantidade de pedaços nas linhas;

3. Somar os vetores em um único vetor;

Na terceira parte do desenho temos a visualização do vetor que encontramos no processo feito acima. Seria como se tivéssemos invertido os eixos e então estivéssemos contando a quantidade de peças nas colunas.


``` txt
# Input do algoritmo -> [1, 4, 3]


# Econtrando o maior valor

maior_valor = 0;
para cada valor do input
    se o valor for maior que o maior_valor
        maior_valor = valor

# Criando o vetor auxiliar de tamanho do maior_valor

vetor_auxiliar = [0, 0, 0, 0]
para cada index do valor do input
    para cada valor que vai de 0 até o input[index]
        vetor_auxiliar[valor] += 1


# Resultado -> [3, 2, 2, 1]
```

Com a implementação lógica feita, podemos partir para a implementação em código:

```c
Codigo em C
```

Porém, nossos valores ainda não estão ordenados. A última etapa consiste em removermos a linha mais baixa do ábaco e contar o número de peças para assim encontrarmos o maior valor referente a nossa ordenação.

:vector_loop2

Com o processo mais claro, podemos partir para a pseudo-implementação em código. Nessa parte, passamos por todos os itens do vetor e sempre que ele for maior que zero, subtraímos em 1 o seu valor, e adicionamos 1 a uma variável de suporte que resultará no valor ordenado da iteração.

``` txt
# Input da segunda etapa -> [3, 2, 2, 1]


# pseudo codigo [esperando codigo do Davi]


# Resultado -> [4, 3, 1]
```

Com isso, temos nosso pseudo código implementado e podemos partir para a implementação efetiva em C.


```c
Codigo em C
```
