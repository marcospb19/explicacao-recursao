# Recursividade

### Índice:
- [Introdução à Recursividade](#Introdução-à-Recursividade)
  - [Por quê?](#Por-quê)
  - [O que é recursão?](#O-que-é-recursão)
  - [Dominando o loop](#Dominando-o-loop)
  - [Dominando o flow](#Dominando-o-flow)
  - [Funções recursivas que retornam valores](#Funções-recursivas-que-retornam-valores)
    - [Fatorial](#Fatorial)
    - [Fibonnaci](#Fibonnaci)
  - [Stack Overflow](#Stack-Overflow)
    - [Como a stack funciona](#Como-a-stack-funciona)
    - [Como acontece?](#Como-acontece)


# Introdução à Recursividade
## Por quê?
Mas calma, antes de aprendermos recursão, qual é a nossa motivação?

Recursão é um tema extremamente importante para nosso estudo de algorítmos, a recursão é necessária para uso de diversas estruturas de dados e é comumente utilizada em algorítmos de árvores, grafos, programação dinâmica e melhores algorítmos de ordenação.

Um breve exemplo de uso cotidiano da recursão: copiar uma pasta no Windows ou Linux, imagine que para cada pasta que passamos para nossa função de copiar, precisamos antes verificar se dentro dela há alguma outra pasta, e assim por diante, até que todas as pastas (que estavam dentro da inicial) tenham sido copiadas.

## O que é recursão?
A recursão ocorre quando uma função (subrotina) chama a si mesma, veja o código genérico abaixo:
```c
void funcao_recursiva(...)
{
    ...
    if (CONDICAO) {
        funcao_recursiva(...);
    }
    ...
}

int main() {
    funcao_recursiva(...);
}
```

A função chamada `funcao_recursiva` recebe alguns argumentos, faz algo útil com eles, e chama `funcao_recursiva` novamente, e então nessa chamada, o processo se repetirá, e a função pode chamar-se várias vezes.

Porém não queremos que essa recursão cause um loop infinito, por isso vamos testar com um `if` se devemos continuar ou se chegamos na _condição de parada_. Caso a função deva continuar, será executado o que chamamos de _passo recursivo_.

Expressões importantes:
1. _Condição de parada_ - Quando a recursão chega ao seu fim.
2. _Passo recursivo_ - Parte da função que chama a si mesma.

Quando estamos utilizando recursão com números, é comum que vamos ter um limite superior ou inferior e esse número vai aumentar ou diminuir até atingir esse limite, então paramos.

A recursão causa um loop assim como a iteração (com `for` e `while`), soluções recursivas e iterativas vão ser mais aptas para resolver diferentes tipos de problemas. Vamos entender como funciona o loop e depois implementar funções que calculam fatoriais e números da sequência de Fibonacci usando recursão.

## Dominando o loop
Vamos tentar fazer um loop que imprime uma sequência de 4 números usando recursão.

**Saída desejada:**
> `1` \
> `2` \
> `3` \
> `4`

**Solução simples sem função:**
```c
int main()
{
    for (int i = 1; i < 5; ++i) {
        printf("%d\n", i);
    }
}
```

**Solução com função iterativa usando `for`:**
```c
void imprimir(int comeco, int limite)
{
    for (int i = comeco; i < limite; ++i) {
        printf("%d\n", i);
    }
}

int main()
{
    imprimir(1, 5);
}
```

**Solução com função iterativa usando `while (true)`:**
```c
#include <stdbool.h> // true e false

void imprimir(int i, int limite)
{
    while (true) {
        // Condição de parada
        if (i >= limite) {
            return; // ou break;
        }
        else {
            printf("%d\n", i);
            i++;
        }
    }
}

int main()
{
    imprimir(1, 5);
}
```

OBS: Embora a solução acima com `while (true)` seja estranha, é necessário entender seu fluxo para poder entender a solução recursiva abaixo.

**Solução recursiva:**
```c
void imprimir_rec(int i, int limite)
{
    // Se `i` chegou em `limite`, parar!
    if (i >= limite) {
        return;
    }
    else {
        printf("%d\n", i);
        imprimir_rec(i + 1, limite); // Roda de novo!
    }
}

int main()
{
    imprimir_rec(1, 5);
}
```

Veja, a nossa solução recursiva recebe `i` e `limite`, assim como as outras soluções, e imprime `i` enquanto ele não chega em `limite`, quando chega, saímos da função e paramos o loop.

Vamos tracejar a ordem de execução do nosso programa:
- Função chamada com os argumentos `(1, 5)`.
- `1 >= 5` == `false`, entrar no `else` e imprimir `1`.
- Função chamada com os argumentos `(2, 5)`.
- `2 >= 5` == `false`, entrar no `else` e imprimir `2`.
- Função chamada com os argumentos `(3, 5)`.
- `3 >= 5` == `false`, entrar no `else` e imprimir `3`.
- Função chamada com os argumentos `(4, 5)`.
- `4 >= 5` == `false`, entrar no `else` e imprimir `4`.
- Função chamada com os argumentos `(5, 5)`.
- `5 >= 5` == `true`, não chamar mais nada.

Se invertermos o `if` da nossa função, deixamos a solução mais parecida com a iterativa com o `for`:
```c
void imprimir_rec(int i, int limite)
{
    if (i < limite) {
        printf("%d\n", i);
        imprimir_rec(i + 1, limite);
    }
}
```

Veja, a condição `i < limite` é a mesma em ambos códigos.

Neste código a _condição de parada_ está **implícita**, mas ela existe! Ocorre quando `!(i < limite)` for verdade. É comum que a _condição de parada_ não seja um bloco explícito em funções recursivas, caso desejarmos deixar explícito podemos adicionar o `else`:
```c
void imprimir_rec(int i, int limite)
{
    if (i < limite) {
        printf("%d\n", i);
        imprimir_rec(i + 1, limite); // Passo Recursivo
    }
    // Condição de parada
    else {
        return;
    }
}
```

Deixei alguns comentários no código, dessa vez para mostrar o que caracterizamos como _condição de parada_ (seria o `else`, nesse caso) e _passo recursivo_.

Copie uma das soluções recursivas e mude o código para ver o que acontece.

Desafios:
1. _Faça imprimir do -2 até o 5._
2. _Faça imprimir de 2 em 2._
3. _Faça imprimir de 5 até 1 (ordem reversa, a solução está abaixo)._
4. _Faça um loop infinito acontecer (**core dumped**)._

Solução do desafio 3:
```c
void imprimir(int i, int limite)
{
    if (i < limite) {
        return;
    }
    else {
        printf("%d\n", i);
        imprimir(i - 1, limite);
    }
}

int main()
{
    imprimir(5, 1);
}
```

Veja, em todas as funções recursivas descritas acima mudamos o valor de `i` de 1 em 1 até que chegue no limite que nós estabelecemos.

## Dominando o flow
Agora que entendemos o loop, vamos entender o flow das chamadas recursivas.

Olhando o seguinte código, qual será a output?

```c
void imprimir_flow(int i)
{
    printf("Começo da chamada (%d)\n", i);
    if (i >= 1) {
        imprimir(i - 1); // Passo recursivo
    }
    printf("Saída do (%d)\n", i);
}

int main()
{
    imprimir_flow(5);
}
```

Output:
> `Começo da chamada (5)` \
> `Começo da chamada (4)` \
> `Começo da chamada (3)` \
> `Começo da chamada (2)` \
> `Começo da chamada (1)` \
> `Começo da chamada (0)` \
> `Saída do (0)` \
> `Saída do (1)` \
> `Saída do (2)` \
> `Saída do (3)` \
> `Saída do (4)` \
> `Saída do (5)`

Na pilha de chamadas (chamando de `f` para simplificar):

```py
f(5) # Começo
f(5) -> f(4) # Abre
f(5) -> f(4) -> f(3) # Abre
f(5) -> f(4) -> f(3) -> f(2) # Abre
f(5) -> f(4) -> f(3) -> f(2) -> f(1) # Abre
f(5) -> f(4) -> f(3) -> f(2) -> f(1) -> f(0) # Abre
f(5) -> f(4) -> f(3) -> f(2) -> f(1) -> f(0) # Fecha
f(5) -> f(4) -> f(3) -> f(2) -> f(1) # Fecha
f(5) -> f(4) -> f(3) -> f(2) # Fecha
f(5) -> f(4) -> f(3) # Fecha
f(5) -> f(4) # Fecha
f(5) # Fim
```

Perceba que na recursão onde `i = 0`, o `printf` é executado, porém o fluxo do programa não acessa o _passo recursivo_.

A condição de parada está mais uma vez implícita, vai acontecer quando `!(i >= 1)`, ou seja, `(i < 1)`, nessa condição a função não se chama novamente, ela completa a instrução após o `if` e então finaliza.

Isso acontece pois no _passo recursivo_, quando `imprimir_flow(i)` chama `imprimir_flow(i - 1)`, a sua execução é congelada naquela linha até que `imprimir_flow(i - 1)` termine.

Vamos fazer uma analogia para a ordem do começo e saída da chamada das funções com as bonecas russas.

![bonecas_russas](https://lh3.googleusercontent.com/proxy/6ziccRgpN2P_Vb4RPQPt446NJ-nWO-fCT_W4hKpU7WQ1aZZiGGlxFQUt3lXEBqS4yYfPdXzK0NfC3DMyz1K5DjgK7g9VigAKFxSlv0e9JUzVB-G8ijX5RBTaaFU)

Essas bonecas encaixam uma dentro da outra.

![bonecas_russas2](https://miro.medium.com/max/572/1*e0n2WjlNyS5ua1YTJR3apQ.png)

Portando, caso queiramos acessar a boneca mais interna, precisamos abrir as mais externas na ordem, isso corresponde ao nosso `printf("Começo...");`, depois que abrimos todas as bonecas, para fechar tudo, temos que fechar primeiro a mais interna, e depois seguir fechando até a mais externa, isso corresponde ao nosso `printf("Saída...");`.

Desafio:
1. Coloque ambos `prinft` dentro do `if`, o que muda na saída do programa?
2. Adicione um parâmetro `limite` para a assinatura da função, e use ele para fazer ir de `10` até `4`.

Agora que você entendeu o flow, vamos trabalhar com funções recursivas que não são `void`.

# Funções recursivas que retornam valores
## Fatorial
Vamos agora fazer uma função recursiva que retorna o fatorial de um número, vamos tentar expandir o fatorial de _5_:

```py
Fatorial(5) = Fatorial(5)
Fatorial(5) = 5 * Fatorial(4)
Fatorial(5) = 5 * 4 * Fatorial(3)
Fatorial(5) = 5 * 4 * 3 * Fatorial(2)
Fatorial(5) = 5 * 4 * 3 * 2 * Fatorial(1)
Fatorial(5) = 5 * 4 * 3 * 2 * 1
Fatorial(5) = 120
```

Isso se parece um pouco com o que estamos fazendo até agora com funções recursivas, mas agora vamos ter que retornar valores em cada chamada recursiva, vamos chamar o nosso argumento de numérico de `n`. Lembre-se que não existe fatorial de números negativos, então vamos assumir que `n >= 0` (se o nosso programa receber `n < 0` ele vai falhar).

Sabemos que o fatorial de _1_ e _0_ é igual a _1_, então vamos começar o código colocando essa essa condição.

```c
int fatorial(int n)
{
    // Para n == 0 ou n == 1
    if (n <= 1) {
        return 1;
    }
    ...
}

int main()
{
    int resultado = fatorial(5);
    printf("%d\n", resultado);
}
```

Perceba que já colocamos nossa _condição de parada_, como estamos trabalhando com números, essa condição de parada também é chamada de _caso base_, que é o ponto para aonde a recursão converge até ser parada.

Ok, agora precisamos terminar a função.

O que `fatorial(5)` deve retornar?

Voltando um pouco para a nossa expansão anterior:

> `Fatorial(5) = Fatorial(5)` \
> `Fatorial(5) = 5 * Fatorial(4)`

Então `fatorial(5)` precisa retornar `5 * fatorial(4)`, pois `4 == 5 - 1`, isso significa que estamos diminuindo de 1 em 1 até chegar no caso base!

Se quando `n == 5` o retorno é `5 * fatorial(4)`, então se trocarmos _5_ por `n`, obtemos:
`fatorial(n) = n * fatorial(n - 1)`

Descobrimos o passo recursivo! Agora no código:

```c
int fatorial(int n)
{
    // Para n == 0 ou n == 1
    // Caso base
    if (n <= 1) {
        return 1;
    }
    else {
        return n * fatorial(n - 1); // Passo recursivo
    }
}

int main()
{
    int resultado = fatorial(5);
    printf("%d\n", resultado);
}
```

Feito, obtemos _120_ como resultado.

Nossa definição recursiva final de fatorial concluiu como:
> `f(0) = f(1) = 1` \
> `f(n) = n * f(n - 1)`

## Fibonnaci
O algorítmo de Fibonacci surgiu para calcular o crescimento teórico de uma população de coelhos ao passar dos meses ([_ver história_](https://pt.wikipedia.org/wiki/Sequ%C3%AAncia_de_Fibonacci#Origens)).

Assim como fatorial, a sequência de Fibonacci não é definida para números negativos, então assuma que `n >= 0`.

A sequência é definida da seguinte maneira:
> `f(0) = 0` \
> `f(1) = 1` \
> `f(n) = f(n - 1) + f(n - 2)`

E seus primeiros elementos são:
> `0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55...`

Acima podemos ver a definição em ação `8 = 5 + 3`, `13 = 8 + 5`, `21 = 13 + 8`... etc..

Dessa vez temos dois _casos base_, quando `n == 0` e quando `n == 1`, (ou seja `0 <= n <= 1`), podemos então começar a nossa função recursiva, pois já sabemos quando ela deve parar.

```c
int fibonacci(int n)
{
    if (n == 0) {
        return 0;
    }
    else if (n == 1) {
        return 1;
    }
    ...
}

int main()
{
    int n = 8;
    int resultado = fibonacci(n);

    printf("%d\n", resultado);
}
```

_Casos base_ implementados, agora precisamos inserir a parte de `f(n) = f(n - 1) + f(n - 2)` para números que não são _0_ nem _1_.


```c
int fibonacci(int n)
{
    if (n == 0) {
        return 0;
    }
    else if (n == 1) {
        return 1;
    }
    else {
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
}
```

Feito, `fibonacci(8)` retorna _21_.

É possível fazer algumas simplificações no código, já que quando `n == 0` retorna-se _0_, e quando `n == 1` retorna _1_, podemos dizer que quando `n <= 1`, retorna-se o próprio `n`.

```c
int fibonacci(int n)
{
    if (n <= 1) {
        return n;
    }
    else {
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
}
```

Além disso, podemos remover o `else`, já que se o primeiro `return` for alcançado, as instruções seguintes não serão executadas.

```c
int fibonacci(int n)
{
    if (n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

Vamos ver como ficaria com o `if` invertido.

```c
int fibonacci(int n)
{
    if (n > 1) {
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
    return n;
}
```

Todos os exemplos são equivalentes!

Agora podemos mudar a `main` para mostrar a sequência (vamos de 0 até 10).

```c
int main()
{
    for (int i = 0; i <= 10; ++i) {
        printf("%d\n", fibonacci(i));
    }
}
```

Saída:
> `0` \
> `1` \
> `1` \
> `2` \
> `3` \
> `5` \
> `8` \
> `13` \
> `21` \
> `34` \
> `55`

Árvore de fluxo de chamadas recursivas:

![fiboarvore](https://i.stack.imgur.com/QVSdv.png)

# Stack Overflow
Caso você tenha feito os desafios, especificamente o de loop infinito, você causou um _**core dumped**_, mas por que esse loop infinito não continuou para sempre que nem no `for`?

> OBS: Caso não esteja recebendo o _**core dumped**_ com o código abaixo, certifique-se que não está passando argumentos extras para o compilador que tentam fazer truques e te ajudar.

Exemplo que causa loop infinito e leva ao _**core dumped**_:

```c
int novamente(int numero)
{
    return novamente(numero * 2) + novamente(numero - 1);
}
```

O seu programa "quebrou" porque você causou um estouro na pilha, ou seja, um _Stack Overflow_, para entender isso precisamos primeiro saber o que é essa pilha.

Antes de tudo, _Stack_ é a tradução de pilha, mas quando pensar na _Stack_, pense numa _pilha de livros_, e não em uma _pilha elétrica_. Existem dois blocos de memória quando executamos um programa em C: a `Heap`, e a `Stack`, vamos estudar sobre a `Heap` somente na aula de _alocação dinâmica_.


## Como a stack funciona
Quando criamos variáveis locais, elas são empilhadas no topo da `Stack`, tome esse código como exemplo:

```c
int funcao()
{
    int x, y;
    double f;

    scanf("%d %d %f", &x, &y, &f);

    if (x > y) {
        int soma = x + y;
        char c;
        ...
    }

    return ...;
}
```

Quando criamos `x`, `y`, `f`, e `soma`, essas variáveis são adicionadas (empilhadas) na `Stack` da seguinte maneira.

`Stack:`
`Stack: x{4}`
`Stack: x{4} | y{4}`
`Stack: x{4} | y{4} | f{8}`

Se `(x > y)`, entramos no `if` que declara a variável chamada `soma`:

`Stack: x{4} | y{4} | f{8} | soma{4}`
`Stack: x{4} | y{4} | f{8} | soma{4} | c{1}`

As chaves servem para denotar a quantidade de _bytes_ que cada tipo está consumindo, `int` ocupa _4_ bytes, e `double` ocupa _8_ bytes, vamos ver proporcionalmente o que isso signfica:

`Stack: xxxx|yyyy|ffffffff|soma|c`

Ao atingir o `return`, todas essas variáveis locais morrem, e a `Stack` limpa elas.

`Stack: ....|....|........|....|.`

A `Stack` está "limpa", mas o lixo da memória que estava lá antes ainda pode ser acessado, caso você use ponteiros de maneira errada, você vai poder ler novamente o conteúdo, e provavelmente receber lixo!

A barra vertical `|` está sendo usada somente para delimitar os espaços, não indica que estes espaços realmente existam entre a memória de cada variável local (embora em muitos casos seja necessário que o compilador reserve espaços que são potências de 2, veja [_memory padding_/_padding_](https://stackoverflow.com/a/381368/9982477)).

### Como acontece?

Além de variáveis locais, as chamadas de funções também entram na `Stack`, vamos tomar `imprimir_rec` como exemplo:

```c
void imprimir_rec(int i, int limite)
{
    if (i < limite) {
        printf("%d\n", i);
        imprimir_rec(i + 1, limite); // Roda de novo!
    }
}
```

Para cada chamada de `imprimir_rec`, temos a adição de mais memória na `Stack`:

`Stack: imprimir_rec{16} | i{4} | limite{4}`
`Stack: imprimir_rec{16} | i{4} | limite{4} | imprimir_rec{16} | i{4} | limite{4} `
`Stack: imprimir_rec{16} | i{4} | limite{4} | imprimir_rec{16} | i{4} | limite{4} | imprimir_rec{16} | i{4} | limite{4} `
...

E assim por diante, então se fizermos um loop infinito, a `Stack` vai crescer indefinidamente até que não vai ter mais memória disponível, e então vamos receber um **core dumped**, em outras linguagens a mensagem de erro é mais descritiva, por exemplo, em Javascript: `RangeError: Maximum call stack size exceeded`.
