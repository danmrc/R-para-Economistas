#Funções

Nesse capítulo explicamos o que são as funções, como e porque usar. Também damos algumas dicas de como criar as funções.

A primeira coisa que deve ser observada é que funções não se limitam a funções matemáticas: uma função recebe alguns inputs, faz algumas operações e devolve um output. Isso pode ser tão geral quanto necessário: de funções que recebem um valor de $x$ e retornam $x^2+x+4$ até funções que recebem matrizes e fazem operações complicadíssimas. Por exemplo, o comando `lm`, que usamos para estimar uma regressão linear, é uma função que recebe a variável dependente e a variável independente e faz uma série de operações com elas e devolve os coeficientes e seus erros padrões.

Em geral, funções são criadas para tarefas que serão repetidas várias vezes. Não faz nenhum sentido escrever uma função que só será usada uma única vez.

##Um exemplo simples: uma função matemática

Suponha que queremos uma função $x^2+2x+4$, que iremos chamar de funcao. Nesse caso:


```r
funcao <- function(x){x^2+2*x+4}
```

Veja que, depois de function, entre parêntese, temos o nome da variável. Entre chaves o que a função de fato faz. Podemos criar uma função $ G = x^2 + y^2$. Nesse caso, o código é:


```r
G <- function(x,y){x^2+y^2}
```

Veja que podemos precisar de definir funções como essas quando queremos integrar ou derivar usando o R. Mas em geral, um exemplo mais interessante de como (e porque) usar funções são exemplos não numéricos. 

##Caso geral e exemplos

Em geral, funções são feitas da seguinte forma:

```
nome_da_funcao <- function(variaveis){
comandos
}
```

Veja que podemos ter muitas variáveis - e em geral as funções do R tem dezenas de variáveis. Os comandos podem ser absolutamente qualquer coisa que o R faz: de rodar uma regressão até operações com arquivos. O importante ao escrever funções - e provavelmente o mais difícil - é estruturar as coisas de maneira geral: os inputs são as variáveis da função e não coisas que estão neste momento no ambiente do R. 

Existem duas coisas importantes sobre funções que devem ser explicitadas antes de seguirmos para o próximo exemplo:

(@) Variáveis podem ter defaults quando a função é construída. Por exemplo, na função $G(x,y) = x^2+y^2$, podia ser o caso de que, exceto se o usuários especificasse ao contrário, o padrão fosse $y = 0$. Para estabelecer essa mudança, bastaria alterar o código para:


```r
G <- function(x,y=0){x^2+y^2}
```
Assim:


```r
G(1)
```

```
## [1] 1
```

```r
G(1,0)
```

```
## [1] 1
```

(@) Os exemplos de funções matemáticas foram muito simples e o R sabe exatamente qual resultado deve ser exibido. Isso nem sempre é verdade, e em funções mais complicadas usamos o comando `return`. O comando só funciona dentro de funções e diz o que a função deve retornar como resposta. No exemplo da função numérica, poderíamos ter escrito: 
```
g <- function(x,y){return(x^2+y^2)}
```

Vamos aplicar essas duas ideias. Suponha que queremos gerar duas amostras de normais independentes que podem ter médias e variâncias diferentes. Também queremos poder mudar o tamanho da amostra. No fim, essa função deve retornar uma matriz $n \times 2$, onde n é o tamanho da amostra. E ainda queremos que, por padrão, as duas variáveis sejam uma normal padrão (com média zero e variância 1). Podemos escrever:


```r
amostrador <- function(n,media_x = 0,media_y = 0,variancia_x = 1,variancia_y = 1){
sd_x <- sqrt(variancia_x)
sd_y <- sqrt(variancia_y)
amostra.x <- rnorm(n,media_x,sd_x)
amostra.y <- rnorm(n,media_y,sd_y)
return(cbind(amostra.x,amostra.y))
}
amostrador(100)
```

```
##           amostra.x    amostra.y
##   [1,] -1.380298821 -0.147131584
##   [2,] -0.591277762  0.798598451
##   [3,] -0.231567267  0.071561977
##   [4,]  1.698680471 -0.057202424
##   [5,]  0.584985286  0.423163855
##   [6,]  1.742563656  0.113208578
##   [7,]  0.800433226  1.761994723
##   [8,] -0.046062883  0.145826533
##   [9,]  0.356897961  0.807075063
##  [10,]  0.369645294 -0.457378193
##  [11,]  1.096467014  0.182427549
##  [12,]  0.868614133 -0.234188208
##  [13,]  2.278019809 -0.394705833
##  [14,]  0.288617014  1.476778064
##  [15,]  1.188947649 -0.254454226
##  [16,]  0.012881909 -0.061980945
##  [17,]  0.060578832 -0.271236165
##  [18,]  2.771361605  1.709343772
##  [19,]  0.651168686  0.244707640
##  [20,] -1.066015589 -0.009647945
##  [21,] -1.068579644  0.488515259
##  [22,] -2.612009042  0.856348631
##  [23,] -0.695347296  0.519688548
##  [24,]  0.399685994  0.082116425
##  [25,]  1.956223149 -0.064897785
##  [26,] -1.946855324 -1.120970352
##  [27,]  0.321964330  0.600095465
##  [28,]  0.004287931  0.067440580
##  [29,] -1.243092256 -0.485752065
##  [30,]  0.088480483  0.355431684
##  [31,] -0.998412313  0.193244997
##  [32,]  0.004525607 -0.642118798
##  [33,]  2.253755525  0.430688704
##  [34,] -0.421194412  1.160670203
##  [35,]  0.863350501  0.633020786
##  [36,] -1.298717848 -0.321868719
##  [37,]  0.202832420  0.331478881
##  [38,]  0.016561968 -1.564544163
##  [39,]  0.083115000 -1.292964154
##  [40,]  0.877247101  1.043816619
##  [41,]  1.786720628  0.143457622
##  [42,]  0.650435658 -0.486036493
##  [43,] -1.094530260 -0.700882306
##  [44,]  0.631077143 -0.246866238
##  [45,] -2.267088616  1.877112544
##  [46,]  0.059613298 -2.109554281
##  [47,]  0.932077800  0.758715935
##  [48,] -0.238374864  0.629888969
##  [49,] -0.495940215  0.382888300
##  [50,]  0.085213702 -0.320761316
##  [51,] -0.935642427  2.197078567
##  [52,]  0.723984922 -0.779464662
##  [53,]  0.162860174  1.511023127
##  [54,] -0.896498424 -1.286588876
##  [55,] -0.799021867 -1.551254765
##  [56,] -1.602901796 -0.488391174
##  [57,]  0.048276470 -0.132943527
##  [58,] -1.289955803  0.522893834
##  [59,] -0.452483344  1.033501741
##  [60,]  0.928226161 -0.372438378
##  [61,]  0.458090282 -0.766071842
##  [62,]  0.355474646  1.296266207
##  [63,] -0.886989593  0.217180634
##  [64,]  1.149309452 -0.025247124
##  [65,]  0.234358047  0.746640444
##  [66,] -0.578536077  0.226235026
##  [67,]  0.375885797 -0.717955001
##  [68,]  0.029400387  0.069747663
##  [69,] -0.270167814 -0.775950208
##  [70,] -0.226542401 -0.622222236
##  [71,] -0.746828584 -0.494314292
##  [72,] -1.045852502  0.119834823
##  [73,]  0.629063557  0.831475121
##  [74,] -0.872191214 -0.649460585
##  [75,] -0.719897983  0.130151365
##  [76,]  2.823044829  0.576313325
##  [77,]  1.578071751 -1.934064298
##  [78,] -0.297786108  2.989490594
##  [79,] -1.547789191  1.622190310
##  [80,]  0.505574843 -1.041959706
##  [81,]  0.957057184 -0.027948978
##  [82,]  1.459018971 -0.915524983
##  [83,]  1.282118029 -1.527306980
##  [84,] -0.376318774  3.029963648
##  [85,]  0.759908394 -1.781739428
##  [86,]  0.630168904  0.616048646
##  [87,] -1.216789994 -0.031489406
##  [88,]  0.543019280 -0.719786667
##  [89,] -1.039207096 -1.070896788
##  [90,]  0.650998875 -0.256397782
##  [91,] -0.709730927 -0.357204711
##  [92,]  0.278491021 -0.823542924
##  [93,]  2.732879986 -0.646170021
##  [94,]  0.391175672  0.299459349
##  [95,]  0.393560782 -2.076223485
##  [96,] -0.340618443 -0.436367548
##  [97,] -1.052526306 -1.084989750
##  [98,] -0.922990521  2.102133576
##  [99,] -3.257367720 -0.590250612
## [100,] -0.805980435 -3.354158387
```

Veja que, como o `rnorm` recebe o desvio padrão da distribuição e a minha função tem como parâmetro a variância da distribuição, criei uma variável na função que tira a raiz quadrada. 

**Atenção: checando argumentos**
É importante notar que em todos os casos, os argumentos de uma função são restritos de alguma forma: no exemplo acima, $n$ tem que ser um número inteiro; a variância não pode ser negativa. Aqui, isso não é tão importante por dois motivos: (1) eu estou assumindo que só você vai usar a função e você sabe o que cada argumento recebe, (2) o `rnorm` daria um erro caso você violasse essas duas restrições. Isso nem sempre é verdade e pode gerar muitos problemas, então em geral desenvolvedores sérios criam ifs que checam o tipo do argumento passado. Isso vai além do escopo do manual.

##Por que escrever funções?

Escrever funções nem sempre é fácil, especialmente para iniciantes. Uma função te força a escrever tudo em termos gerais: um n que não existe, um vetor que ainda não foi criado, etc. Como dito no inicio deste capítulo, funções são criadas para tarefas que serão repetidas várias vezes. Em geral, há uma tentação de copiar e colar o código várias vezes fazendo as alterações necessárias ``no braço''. 

Não caia nesta tentação! Há dois motivos básicos para isso:


* O seu código vai ser ilegível em pouco tempo, com milhares de linhas repetidas que você é incapaz de ver qual a diferença
* É muito fácil esquecer de mudar uma parte do código e a coisa dar erro, ou ainda pior, rodar e te dar um resultado errado.

Este autor já precisou rodar de novo várias partes de código que demoravam horas por esquecer de mudar alguma pequena coisa no código copiado e colado. Usar funções é uma maneira muito mais razoável de resolver o problema e, uma vez criada e debbugada, a dor de cabeça é muito menor. Quando o autor aprendeu R, ele foi informado de que escrever tudo em funções era o ideal e ignorou. Esta parece ser uma das muitas coisas que só se aprende cometendo o erro.

##Como escrever funções

Um dos principais motivos para iniciantes no R ignorarem a sugestão da seção anterior é que escrever funções pode ser desafiador. Escrever a função exige, de antemão, que você saiba o tamanho dos vetores, matrizes, como o código irá se comportar etc. Existe uma maneira simples de mitigar este desafio: comece escrevendo o caso específico e depois generalize. No exemplo das duas amostras normais, nós poderiamos ter começado escrevendo apenas:

```
amostra.x <- rnorm(100,0,1)
amostra.y <- rnorm(100,2,2)
```

E depois ter generalizado: sabemos que 100 é o tamanho da amostra, então troque por n. Para permitir médias diferentes (como o código acima fez), colocamos duas variáveis no lugar - com nomes que evidenciam o que elas fazem. Etc. No fim, só precisamos colocar, no começo, o `nome.da.funcao <- function(argumentos){`e fechar com o `return` de maneira a amarrar as duas amostras (cbind, talvez).   

Outro desafio é que o `return` só aceita um único objeto, ou seja, funções no R só retornam uma única coisa. Em alguns casos isso é um desafio. Voltando ao exemplo das duas amostras, `return(amostra.x,amostra.y)` daria um erro. A solução aqui é simples: cole os dois vetores em uma matriz. Mas nem sempre isso é possível: e se os vetores tiverem dimensão diferente? E se a função gera várias matrizes?

Nesses casos, lembre que `list()` recebe qualquer coisa. Você pode ter um elemento na lista que é só um número, outro que é um vetor e até uma lista! O comando `lm` retorna, de fato, uma lista. Dentro da lista há um vetor com os coeficientes, uma matriz de variância covariância e outras coisas. 

Uma terceira dica é quebrar uma função grande em funções menores. O R permite que uma função chame outra, o que facilita em muito o processo de criação de funções complicadas. Em um exemplo extremo, poderíamos imaginar que você quer fazer uma função que faz OLS para você. Poderiamos quebrar isso em vários pedaços: uma que estima os coeficientes, outra que calcula o erro padrão, uma terceira que faz a conta do $R^2$...e finalmente uma última que amarra todas essas funções e devolve uma lista com os coeficientes, erros padrões e tudo mais. 
