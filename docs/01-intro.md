#Primeiros Passos

##Instalação

###Instalando o R e o RStudio

Instalar o R é trivial: basta ir no http://cran.r-project.org/ e baixar a versão para o seu computador. Exceto se você usar alguma distribuição de Linux (Ubuntu, por exemplo): ai é mais difícil, mas o próprio CRAN dá instruções de como fazer. Depois, recomenda-se baixar o R Studio, disponível em https://www.rstudio.org/. O R Studio é uma IDE (Integrated Development Enviroment), que facilita muito a vida na hora de programar - especialmente dando sugestões de comandos e mostrando quais variáveis estão salvas no ambiente do R. Assim, sugiro instalar o R Studio, que é bem tranquilo. Para usar o R Studio, você precisa ter o R. 

###Uma alternativa

Se o seu computador tem um processador multi-core, pode ser interessante instalar o Microsoft R, disponível em http://mran.microsoft.com/. Ele é idêntico ao R, mas vem com uma biblioteca que tira vantagem dos vários núcleos do processador, o que o R padrão não faz. A principal desvantagem é que ele é atualizado com menos frequência, e a biblioteca que usa mais de um núcleo não está disponível para o Mac. Ele funciona com o R Studio. 

###Interface

A interface do R Studio mostra 4 espaços diferentes: no canto esquerdo superior, existe uma tela chamada source (se ela não estiver lá, tente usar ctrl + shift + n para abrir); a direita dela, o ambiente; no canto inferior esquerdo, está o console; e no canto inferior direito está uma tela multiuso, que deve vir com as abas plot, files. Cada uma dessas será explicada, por alto, nesta seção

A que mais nos interessa, em um primeiro momento, é o console. Nele, você pode passar comandos direto para o R. Digitando 2 + 2 nele e clicando em enter, o resultado deve aparecer na tela. Em geral, é nele que você vai trabalhar. Entretanto, escrever código muito longos no console é muito ruim. O console é desorganizado, não permite salvar o código passado para ele para ser usado mais tarde e não permite com que você corrija erros com facilidade. O source serve justamente para escrever um código longo - uma função ou uma simulação, por exemplo - que pode ser executado no console. Para isso, basta selecionar o conteúdo e dar ctrl + enter ou chegar no fim da linha e usar ctrl + enter. 

A tela do canto direito inferior é uma "geleia geral": a aba plots é onde os gráficos que faremos vão aparecer; a aba files permite você ver arquivos em diferentes pastas do computador. Estas são as abas mais importantes e que mais serão usadas. Em cima desta tela, há a tela *environment*, que mostra as variáveis que foram criadas e estão disponíveis para o R usar

###Pacotes

O grande atrativo do R são os pacotes. Para instalar um pacote, basta digitar no console: 

``` 
install.packages("nome-do-pacote")
``` 

É necessário colocar as aspas para o pacote instalar. Uma vez instalado, o pacote não é carregado automaticamente. Para carregar o pacote, basta digitar no console:

``` 
library(nome-do-pacote)
``` 
 
 Agora as aspas não são necessárias. 
 
 Instalar pacote por pacote pode ser uma tarefa chata. Além do mais, isto exige que você saiba quais são os pacotes que fazem cada coisa. Felizmente, o CRAN mantém coleções de pacotes de determinados temas, chamados de Task Views. Existe um de econometria, e para instalar ele é necessário instalar o pacote ctv , e depois o task view de econometria: 
 
``` 
install.packages("ctv")
library(ctv)
install.views("Econometrics")
```
 
 Como são muitos pacotes, esta operação pode tomar algum tempo. Em geral, muitos dos pacotes nos task view não são tão úteis, então pode ser interessante ir no cran, e visitar os task views para selecionar quais pacotes de lá fazem o que você precisa fazer. Existem vários pacotes, para diferentes áreas. Para os economistas, além do pacote de econometria, os pacotes de Time Series, Bayesian, Finance, Machine Learning podem ser de interesse. De fato, com a expansão das áreas de pesquisas, muitos outros task views podem ser de interesse! Recomenda-se visitar o cran para ter uma visão do que está disponível.

<table>
<tr>
<th bgcolor="#adadeb">Hands on!</th>
</tr>

<tr>
<td bgcolor="#adadeb">Vamos instalar um primeiro pacote que adiciona vários comandos importantes para econometria e alguns datasets de livros de econometria (como o Stock Watson). O nome do pacote é AER. Ele vai instalar vários outros pacotes que ele necessita para funcionar, então a instalação pode demorar um pouco. Ele será usado de agora por diante, então tenham certeza que ele está carregado, ou seja, que vocês sempre estão usando o `library(AER)` quando abrem o R.</td>
</tr>
</table>

 
##Ajuda

Em 90% do tempo você vai precisar de olhar o help: seja para lembrar que opções estão disponíveis em um comando, ou lembrar exatamente o que o comando faz, ou descobrir qual o comando para fazer alguma coisa específica que você só tem uma palavra chave: ``Como gera números saídos de uma distribuição normal mesmo?''[^1] 

Se você sabe o comando e quer ler o help deste comando, basta fazer `?comando`. Por exemplo, se você quiser saber o que o comando rnorm faz e quais as opções ele oferece, basta digitar no console `?rnorm`. Se você não sabe o nome do comando, mas quer saber todos os comandos que estão relacionados a uma determinada palavra chave, use `??palavra`. Por exemplo, se você quiser saber todos os comandos que envolvem a distribuição normal, basta usar `??normal`. Observe que, se você tiver muitos pacotes, isto pode demorar um pouco, já que exige que o R procure cada pacote que se referencie àquela palavra. 

Você também pode estar interessado em todos os comandos de um pacote. Neste caso, a melhor solução é é usar `help(package=``nome-do-pacote'')`. 

##Comentando 

Em geral, quando se escreve um código, é importante explicar o que algumas partes do código fazem. Isso não é exclusivo para o caso em que o código vai ser distribuído: pelo contrário, o você do futuro vai agradecer muito o você do passado se você comentar o seu código. Para comentar o código no R, usa-se o #. Tudo depois do # vira um comentário é ignorado pelo R. Assim, suponha que queremos explicar o que o parâmetro a abaixo é: suponha que ele vem de um outro estudo, de Cleese et. al. (1975). Então:

```
	a <- 1 #Retirado de Cleese et. al. (1975)
```   

A regra geral para comentários é: eles não devem ser óbvios (ex.: `1 + 1 # somando dois números`) mas devem ajudar o leitor - que eventualmente será você mesmo - a entender o que está sendo feito - especialmente em momentos mais obscuros.  

##Objetos*

Como qualquer linguagem de programação, existem vários tipos de objetos no R. Objetos são maneiras diferentes de guardar os dados. Os mais usados e mais comuns são: 

* Vetor
* Matriz
* Dataframe
* Lista

Em geral, para dar um nome a um objeto, usamos uma setinha, `<- `, que é o sinal de menor e o menos. Podemos usar o igual também, mas é preferível a seta. Então, se fizermos `a <- 1` e digitarmos a, o R vai mostrar 1. Veja que por padrão o R não mostra o valor de objetos que você acabou de criar, e vai mostrar apenas se você pedir. 


```r
a <- 1
a
```

```
## [1] 1
```

Vetores são só coleções unidimensionais  de "coisas". Para criar um vetor, basta usar `c()` e separar os elementos por vírgula. Suponha que queremos listar os 4 primeiros números primos, então poderíamos fazer:


```r
primos <- c(2,3,5,7)
```

Assim, ao digitar no console `primos`:


```r
primos
```

```
## [1] 2 3 5 7
```

Podemos fazer operações com vetores, como somar, subtrair, multiplicar e dividir. Observe que multiplicar um vetor não é multiplicar uma matriz: o R vai multiplicar elemento a elemento. Então `c(1,2,3)*c(1,2,3)` gera como resultado: 


```r
c(1,2,3)*c(1,2,3)
```

```
## [1] 1 4 9
```

Para multiplicar dois vetores como multiplicariamos usualmente, usamos `%*%`:


```r
c(1,2,3)%*%c(1,2,3)
```

```
##      [,1]
## [1,]   14
```

Podemos agrupar vetores em matrizes, usando os comandos cbind e rbind, que transforma cada vetor em uma coluna ou em uma linha, respectivamente. Assim, se fizermos `cbind(c(1,2),c(3,4))`, teríamos:


```r
cbind(c(1,2),c(3,4))
```

```
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
```

Se usarmos o `rbind(c(1,2),c(3,4))`, teríamos:


```r
rbind(c(1,2),c(3,4))
```

```
##      [,1] [,2]
## [1,]    1    2
## [2,]    3    4
```

Existe outra maneira de fazer matrizes, com o comando `matrix`. Veja que as operações `*`e `%*%` funcionam assim como elas funcionam com vetores: `*`multiplica elemento a elemento a matriz e `%*%`multiplica a matriz da maneira usual: 


```r
A <- matrix(c(1,2,3,4),ncol = 2)
B <- matrix(c(1,1,1,1),ncol = 2)
A*B
```

```
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
```

```r
A%*%B
```

```
##      [,1] [,2]
## [1,]    4    4
## [2,]    6    6
```

Observe que matrizes forçam todos os elementos a serem do mesmo tipo. Suponha que você quer fazer uma matriz com nomes e notas de alunos, e quer tirar a média das notas. O R vai acusar um erro, porque as notas não serão do tipo numérico, e sim do tipo *character*, que é o tipo do nome dos alunos. Para contornar este problema e poder agregar vetores com diferentes tipos - como é o caso do exemplo de notas de alunos e notas é que existe o objeto do tipo dataframe. O comando data.frame funciona como um cbind, que permite diferente juntar vetores de diferentes tipos em uma "matriz". 

Observe que, para formar matrizes e dataframes, os vetores tem que ter o mesmo tamanho, o que nem sempre é possível ou desejável. Neste contexto, existem as listas, que são um *anything goes*. Você pode ter uma lista de vetores, uma lista de variáveis, uma lista de listas etc. Além disto, você pode dar nomes as coisas dentro dela e chamar pelo nome. Por exemplo, se fizermos:


```r
um.teste <- list(Ola = "Olá", Dia = date())
```

E depois digitarmos `um.teste$Dia`, ele deve exibir qual a data atual. Digite `um.teste$Ola` e ele deve exibir um Olá na sua tela:


```r
um.teste$Dia
```

```
## [1] "Wed Jul 04 15:08:42 2018"
```

```r
um.teste$Ola
```

```
## [1] "Olá"
```

Observem que eu usei um "Olá", entre aspas na lista. Aspas são bastante importante. Por exemplo, faça: `c("Bom","Dia")`. O R vai mostrar na tela as duas palavras. Agora, suponha que você esquecesse as aspas no Dia. Agora, o R dará um erro: ele afirma que o objeto Dia não foi encontrado.

Assim, se quisermos digitar palavras, frases, letras, devemos colocar eles entre aspas. Caso contrário, o R vai buscar o objeto com aquele nome. De maneira bastante grosseira, expressões desse tipo são chamadas de string.  

Familiarizados com como instalar, pedir ajuda, e os principais objetos do R, podemos proceder para a primeira etapa de qualquer análise de dados: como inserir os dados no R.

[^1]: Resposta: rnorm
