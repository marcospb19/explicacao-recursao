> """Um texto explicando um assunto, o tema do texto é "Recursividade e stack overflow". Explique portanto como funciona a recursividade e sua relação com o problema de stack overflow."""

# Índice
- [Introdução à Recursividade](#Introduçao-a-Recursividade)
  - [Por quê?](#Por-que?)
  - [O que é recursão?](#O-que-é-recursão?)
  - [Dominando o loop](#Dominando-o-loop)
  - [Dominando o flow](#Dominando-o-flow)
  - [Fatorial](#Fatorial)

<!-- # Stack Overflow -->




# Introdução à Recursividade
## Por quê?
Mas calma, antes de aprendermos recursão, qual é a nossa motivação?

Recursão é um tema extremamente importante para nosso estudo de algorítmos, a recursão é necessária para uso de diversas estruturas de dados e é comumente utilizada em algorítmos de árvores, grafos, e de programação dinâmica.

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
#include <stdbool.h>

void imprimir(int i, int limite)
{
    while (true) {
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
- Função chamada com os argumentos (1, 5).
- `1 >= 5`? NÃO, entrar no `else` e imprimir `1`.
- Função chamada com os argumentos (2, 5).
- `2 >= 5`? NÃO, entrar no `else` e imprimir `2`.
- Função chamada com os argumentos (3, 5).
- `3 >= 5`? NÃO, entrar no `else` e imprimir `3`.
- Função chamada com os argumentos (4, 5).
- `4 >= 5`? NÃO, entrar no `else` e imprimir `4`.
- Função chamada com os argumentos (5, 5).
- `5 >= 5`? SIM, não chamar mais nada.

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
1. Faça imprimir do -2 até o 5.
2. Faça imprimir de 2 em 2.
3. Faça imprimir de 5 até 1 (ordem reversa, a solução está abaixo).
4. Faça um loop infinito acontecer (core dumped).

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
> Começo da chamada (5) \
> Começo da chamada (4) \
> Começo da chamada (3) \
> Começo da chamada (2) \
> Começo da chamada (1) \
> Começo da chamada (0) \
> Saída do (0) \
> Saída do (1) \
> Saída do (2) \
> Saída do (3) \
> Saída do (4) \
> Saída do (5) \

Na pilha de chamadas (chamando de `f` para simplificar):
> `f(5)` \
> `f(5) -> f(4)` \
> `f(5) -> f(4) -> f(3)` \
> `f(5) -> f(4) -> f(3) -> f(2)` \
> `f(5) -> f(4) -> f(3) -> f(2) -> f(1)` \
> `f(5) -> f(4) -> f(3) -> f(2) -> f(1) -> f(0)` \
> `f(5) -> f(4) -> f(3) -> f(2) -> f(1)` \
> `f(5) -> f(4) -> f(3) -> f(2)` \
> `f(5) -> f(4) -> f(3)` \
> `f(5) -> f(4)` \
> `f(5)` \


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

## Fatorial

Vamos agora fazer uma função recursiva que retorna o fatorial de um número, sabemos que o fatorial de `5` é:

> `Fatorial(5) = Fatorial(5)` \
> `Fatorial(5) = 5 * Fatorial(4)` \
> `Fatorial(5) = 5 * 4 * Fatorial(3)` \
> `Fatorial(5) = 5 * 4 * 3 * Fatorial(2)` \
> `Fatorial(5) = 5 * 4 * 3 * 2 * Fatorial(1)` \
> `Fatorial(5) = 5 * 4 * 3 * 2 * 1` \
> `Fatorial(5) = 120`



<!-- ## Stack Overflow

Se você completou o desafio de loop infinito, você recebeu essa mensagem no terminal:
> _segmentation fault (core dumped)_
 -->
