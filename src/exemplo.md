Problema da Mochila 1/0
======

O que é?
---------

Nessa aula, será abordado dentro do tema de Divisão, Conquista e Programação Dinâmica, o uso do algoritmo dinâmico para resolver o problema da mochila. O problema da mochila, de forma sucinta, é um problema de otimização de espaço para o maior retorno possível.

Pareceu complicado? Vamos simplificar então.

Imagine uma mochila, que aguenta até um certo peso antes que rasgue, e vários objetos que queremos guardar nessa mochila, cada um tendo peso e valor próprio. Por causa do limite de peso, pode ser que tenhamos que deixar alguns objetos para trás. 

Mas como escolher quais objetos botar na mochila, e quais não? Como temos peso e valor do objeto, e o peso é usado para ver se a mochila aguenta os objetos, então o que determina se devemos guardá-los ou não é o seu valor! Quanto maior o valor, maior prioridade um objeto tem para ser guardado. 

Entendeu a lógica? Vamos praticar com um exercício então!

??? Questão 1

Considere uma mochila com limite de peso de 4kg com os seguintes itens:
* Item 1: 1 kg com valor de R$ 15,00
* Item 2: 2 kg com valor de R$ 20,00
* Item 3: 3 kg com valor de R$ 30,00

Intuitivamente, quais itens devem entrar na mochila para maximizar o seu valor?

!!! Aviso
Cada item só pode ser colocado UMA vez na lista. Ou seja, se o limite for 2 kg, NÃO pode colocar o Item 1 DUAS vezes. Também não pode colocar um item parcialmente caso adicionar o item todo ultrapasse o limite. Ou o objeto cabe na mochila, ou não cabe.
!!!

::: Gabarito
Os itens 1 e 3, chegando a um peso máximo de 4kg com valor R$ 40,00
:::

???

O problema parece simples para poucos itens com um limite de peso baixo. A partir do momento em que a lista de itens aumenta muito, e o limite de peso é elevado, fica mais difícil enxergar a solução otimizada. Com isso, fica necessário implementar a ajuda de um algoritmo para fazer essa otimização.

Opções de Algoritmo
---------

O problema da mochila é um problema de otimização, com aplicação real para empresas, e logo precisa de uma visão realista na hora de escolher qual algoritmo se deve usar para resolvê-lo. Embora haja algoritmos com melhor desempenho no que se refere à velocidade com que o programa roda, o algoritmo dinâmico é o mais confiável, pois ele sempre retorna a melhor resposta.
E como a confiabilidade dos resultados obtidos são muito valorizados por gestores, esse algoritmo é o melhor, principalmente em casos pequenos.

Agora que sabemos o POR QUÊ de usarmos esse algoritmo, está na hora de começarmos a entender COMO usamos esse algoritmo. Para isso, vamos montar a função. 

De forma mais técnica, o algoritmo de programação dinâmica para o problema da mochila recebe **três entradas** e devolve **uma saída**. 

A primeira entrada é uma lista de valores correspondente a cada objeto individualmente, e a segunda entrada uma lista de peso para cada um desses objetos. A posição que cada valor e peso ocupa nas listas e a mesma para um objeto, então nao precisamos nos preocupar com reorganização nesse sentido. 

A terceira entrada corresponde a quantidade de peso que a mochila pode carregar, sempre em forma de um inteiro. 

Por fim a saída corresponde ao valor final que a mochila vai ter no fim de todas as iterações, sendo que esse valor, para resolver o problema, tem que ser o maior possível.

Agora, vamos ver se você prestou atenção.

??? Questão 2

Quais devem ser os parâmetros que a função deverá receber?
Defina a chamada da função Mochila.

::: Gabarito

``` c
int Mochila(int L, int p[], int val[], int n){}
```
Onde:
* L é o limite de peso da mochila;
* p[ ] é a lista de pesos dos itens;
* val[ ] é a lista de valores dos itens;
* n é a quantidade de itens;

!!! Aviso
Para fins de simplificação, considera-se que a posição dos itens na lista de pesos e na lista de valores estão alinhados e se correspondem.
!!!
:::

???

Como o algoritmo é recursivo, isso implica calcular subproblemas derivados do problema principal e reutilizar esses resultados no futuro para fins de comparação. Isso funciona através do preenchimento de uma matriz limite x itens:

|                             | 0 kg     | 1 kg     | 2 kg     | 3 kg     | 4 kg     |
|-----------------------------|----------|----------|----------|----------|----------|
| **Item 1 (1 kg, R$ 15,00)** |          |          |          |          |          |
| **Item 2 (2 kg, R$ 20,00)** |          |          |          |          |          |
| **Item 3 (3 kg, R$ 30,00)** |          |          |          |          |          |

As células dessa matriz são preenchidas linhas por coluna com o valor máximo da mochila dado seu limite de peso e itens considerados.

??? Questão 3

Preencha a primeira linha da tabela. Ou seja, considerando apenas o item 1, qual é o valor da mochila para cada limite de peso?

::: Gabarito
Para a primeira coluna, como o limite de peso é de 0 kg, o item 1 não pode ser incluido pois nao cabe, portanto o valor máximo da mochila é R$ 0,00.

A partir da segunda coluna, onde o limite de peso é 1 kg, é possível incluir o item 1. Com isso, o valor das colunas restantes é de R$ 10,00.

|                             | 0 kg     | 1 kg     | 2 kg     | 3 kg     | 4 kg     |
|-----------------------------|----------|----------|----------|----------|----------|
| **Item 1 (1 kg, R$ 15,00)** | R$ 0,00  | R$ 15,00 | R$ 15,00 | R$ 15,00 | R$ 15,00 |
| **Item 2 (2 kg, R$ 20,00)** |          |          |          |          |          |
| **Item 3 (3 kg, R$ 30,00)** |          |          |          |          |          |
:::

???

O preenchimento da próxima linha é onde a recursão começa a importar. Até a segunda coluna, o preenchimento é igual a linha de cima, visto que não é possível incluir o item 2 quando o limite de peso é inferior ao seu peso. Na terceira coluna, há uma decisão a ser tomada: incluir o item 1 ou 2.

??? Questão 4

Preencha a segunda linha da tabela até a terceira coluna. Qual deve ser o item a ser incluido na terceira coluna? Por quê?

::: Gabarito

O item a ser incluido é o Item 2, pois o valor do item 2 é maior do que do item 1. <br>
<br>
Para tomar essa decisão, você acabou levando em consideração qual era o maior valor entre o item previamente incluido, o item 1, e o item a ser incluido, o item 2, para esse limite de peso.

|                             | 0 kg     | 1 kg     | 2 kg     | 3 kg     | 4 kg     |
|-----------------------------|----------|----------|----------|----------|----------|
| **Item 1 (1 kg, R$ 15,00)** | R$ 0,00  | R$ 15,00 | R$ 15,00 | R$ 15,00 | R$ 15,00 |
| **Item 2 (2 kg, R$ 20,00)** | R$ 0,00  | R$ 15,00 | R$ 20,00 |          |          |
| **Item 3 (3 kg, R$ 30,00)** |          |          |          |          |          |

:::

???

Nas últimas duas colunas, o valor máximo é a somatória dos valores dos itens 1 e 2, visto que há espaço suficiente para incluir ambos os itens na mochila. <br>

Para a terceira linha da tabela, assim como para o item 2, as primeiras 3 colunas são identicas a linha de cima, pois não é possível incluir o item 3 ainda.

??? Questão 5

Preencha a terceira linha da tabela até a quarta coluna. Qual deve ser o(s) item(ns) a ser(em) incluido(s) na quarta coluna? Por quê?

::: Gabarito

Os itens a serem incluidos são os Items 1 e 2, pois o valor da soma dos itens 1 e 2 é maior do que do item 3. <br>
<br>
Para tomar essa decisão, você acabou levando em consideração qual era o maior valor entre os itens previamente incluidos, os itens 1 e 2, e o item a ser incluido, o item 3, para esse limite de peso. <br> <br>
É importante entender que para esse caso, ao considerar a inclusão do item 2, sobrou espaço para incluir o item 1, o que possibilitou a sua inclusão. Ao selecionar o item 2, o problema se transforma em um subproblema da mochila onde o limite de peso é 3 kg (coluna atual) - 2 kg (incluir Item 2) = 1 kg. Ja foi calculado esse resultado previamente e foi determinado que apenas o item 1 podia ser incluido com um valor de R$ 15,00. Com isso, é possível tirar o máximo entre os dois valores: R$ 35,00 > R$ 30,00. Portanto, os itens 1 e 2 seguem na mochila e o item 3 não.

|                             | 0 kg     | 1 kg     | 2 kg     | 3 kg     | 4 kg     |
|-----------------------------|----------|----------|----------|----------|----------|
| **Item 1 (1 kg, R$ 15,00)** | R$ 0,00  | R$ 15,00 | R$ 15,00 | R$ 15,00 | R$ 15,00 |
| **Item 2 (2 kg, R$ 20,00)** | R$ 0,00  | R$ 15,00 | R$ 20,00 | R$ 35,00 | R$ 35,00 |
| **Item 3 (3 kg, R$ 30,00)** | R$ 0,00  | R$ 15,00 | R$ 20,00 | R$ 35,00 |          |

!!! Aviso
Não passe para frente antes de entender como funciona a recursão na tabela.
!!!

:::

???

??? Questão 6

Preencha a última célula restante. Qual deve ser o(s) item(ns) a ser(em) incluido(s) e qual é o valor final da mochila? Por quê?

::: Gabarito

Os itens a serem incluidos são os Items 1 e 3, pois o valor da soma dos itens 1 e 3 é maior do que a soma dos itens 1 e 2. <br>
O valor final da mochila é R$ 45,00 com peso de 4 kg.

|                             | 0 kg     | 1 kg     | 2 kg     | 3 kg     | 4 kg     |
|-----------------------------|----------|----------|----------|----------|----------|
| **Item 1 (1 kg, R$ 15,00)** | R$ 0,00  | R$ 15,00 | R$ 15,00 | R$ 15,00 | R$ 15,00 |
| **Item 2 (2 kg, R$ 20,00)** | R$ 0,00  | R$ 15,00 | R$ 20,00 | R$ 35,00 | R$ 35,00 |
| **Item 3 (3 kg, R$ 30,00)** | R$ 0,00  | R$ 15,00 | R$ 20,00 | R$ 35,00 | R$ 45,00 |

:::

???

Tutorial OG
---------

Para criar um parágrafo, basta escrever um texto contínuo, sem pular linhas.

Você também pode criar

1. listas;

2. ordenadas,

assim como

* listas;

* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta *img*.

![](logo.png)

Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| coluna a | coluna b |
|----------|----------|
| 1        | 2        |

Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md :` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

:bubble

Você também pode inserir código, inclusive especificando a linguagem.

``` py
def f():
    print('hello world')
```

``` c
void f() {
    printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```


!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!


??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???
