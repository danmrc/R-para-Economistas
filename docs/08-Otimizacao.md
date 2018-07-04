#Otimização

Muitas vezes precisamos resolver problemas que são problemas de maximização ou minimização. Por exemplo, o problema de uma regressão por MQO pode ser escrito como minimizar a soma do quadrado dos erros. O problema de máxima verossimelhança envolve escolher paramêtros que maximizam uma função de densidade conjunta. Esse capítulo explica algumas ideias básicas de como fazer otimização no R. 

Antes de começarmos, é necessário um aviso importante: otimização no computador raramente é feita tirando a primeira derivada e igualando a zero. Existe uma variedade de algoritmos que fazem otimização e o tema é excessivamente amplo para ser coberto com calma. O *Numerical Methods in Economics*, de Kenneth Judd, trata de alguns desses métodos - mas saiba que o tema não é simples. Esse capítulo foi escrito de maneira que você pode pular a seção *Mais sobre otimização númerica*. Mas eu recomendo fortemente a leitura. 

##Optimize e Optim

O R vem, por padrão, com duas funções básicas para otimização: o `optimize` e o `optim`. A diferença entre eles é que o primeiro é para quando se tem uma variável para ser otimizada, e o segundo para mais de uma variável. Em ambos os casos, entretanto, temos que escrever uma função antes, então começaremos por ai. Vamos trablhar com duas funções para entender como usar os comandos: $f(x)=x^2$ e $g(x,y)=x^2+y^2$. Veja que sabemos que o mínimo em ambas é colocar todos os argumentos igual a zero. Já aprendemos a escrever funções. **Uma idiosincrasia importante para o caso de mais de uma variável** é que precisamos ter apenas um argumento para as variáveis a serem otimizadas, que deve ter a forma de um vetor. Assim, se tivermos dois argumentos na função matemática, teremos um argumento na função do R que vai ser um vetor com duas posições. Escrevemos as funções $f(x)$ e $g(x,y)$:


```r
f <- function(x){x^2}
g <- function(x) {x[1]^2+x[2]^2}
```

Vamos começar com o `optimize`: o primeiro argumento é a função e o segundo é o intervalo da busca. Sabemos que o mínimo é igual a zero, então vamos colocar o intervalo entre -2 e 2:


```r
optimize(f,c(-2,2))
```

```
## $minimum
## [1] -5.551115e-17
## 
## $objective
## [1] 3.081488e-33
```

Observe que, apesar de normalmente usarmos a função no R como `f(x)`, o comando recebe apenas `f`. A resposta tem dois componentes: o valor da variável que minimiza a função (`$minimum`) e o valor da função nesse ponto ( `$objective`). Veja que o R não nos devolveu 0 como o valor que minimiza: a minimização é numérica e vai ser sempre um valor aproximado. Mas $-5.55 10^{-17}$ é, para todos os efeitos, zero. 

Vamos trabalhar agora com a nossa função com duas variáveis. Usamos o comando optimize que recebe primeiro um chute inicial e depois a função. O chute incial deve ser um vetor. Sabemos que o ótimo é o vetor $(0,0)$, então para dar algum trabalho para o computador vamos chutar $(-1,1)$:


```r
optim(c(-1,1),g)
```

```
## $par
## [1]  0.0001134426 -0.0001503306
## 
## $value
## [1] 3.546849e-08
## 
## $counts
## function gradient 
##       55       NA 
## 
## $convergence
## [1] 0
## 
## $message
## NULL
```

Veja que a resposta nos trás os valores que minimizam( `$par`), o valor da função nesse ponto( `$value`) e algumas outras informações: a mais importante é o `$convergence`, que nos diz se o algoritmo convergiu ou não - isso é, se encontramos um ótimo ou não. Em geral, se temos um 0 é que tudo ocorreu bem. Outros valores indicam erro e cada algoritmo dá um significado para cada valor. 

Infelizmente, o chute inicial pode ser muito importante em alguns casos. Pegue a função $h(x,y)=x^3+y^2+x*y$. Vamos testar dois chutes inciais: $(0,0)$ e $(-10,0)$:


```r
h <- function(x){x[1]^3+x[2]^2+x[1]*x[2]}

optim(c(0,0),h)
```

```
## $par
## [1]  0.16666670 -0.08333335
## 
## $value
## [1] -0.002314815
## 
## $counts
## function gradient 
##       99       NA 
## 
## $convergence
## [1] 0
## 
## $message
## NULL
```

```r
optim(c(-10,0),h)  
```

```
## $par
## [1] -3.183805e+56  2.684172e+56
## 
## $value
## [1] -3.2273e+169
## 
## $counts
## function gradient 
##      501       NA 
## 
## $convergence
## [1] 1
## 
## $message
## NULL
```

Veja que, no primeiro caso, temos convergência; no segundo, a coisa dá totalmente errada e não há convergência (por isso o código 1 ali no `$convergence`).

Veja que pode ser o caso de que queremos encontrar o máximo de uma função. O optimize tem uma opção `maximum`, e basta alterar para `TRUE`. Entretanto, o `optim` não tem essa opção. A solução é simples: multiplique a função por $-1$. Por exemplo, o máximo de $j(x)=- x^2$ é o mesmo ponto que o mínimo de $f(x)=x^2$. A única coisa que vai mudar é o valor da função no ponto.


