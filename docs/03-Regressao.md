#Regressão Básica

O coração de econometria é o modelo de regressão linear, estimado por Mínimos Quadrados Ordinários. Mas muitos outros métodos são úteis, como modelos logit, probit, variáveis instrumentais e modelos para painel. Este capítulo trata - finalmente! - de modelos de regressão no R. 

##Mínimos quadrados ordinários

Suponha que carregamos uma base de dados (chamada dados), e que esta base tem as variáveis $y, x.1,x.2,x.13$ e que nosso objetivo é estimar um modelo da forma: $y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \beta_3 x_3 + \epsilon$. O comando que faz estimativa por mínimos quadrados é o `lm` e para estimarmos a regressão proposta basta fazer:

```
reg <- lm(y ~ x.1 + x.2 + x.3 + x.4, data = dados)
```  

Usamos o ~ para separar a variável explicada (à esquerda do til) das explicativas (à direita do til), e para separar as explicativas usamos o +. A opção *data* diz ao R onde buscar as variáveis.

Agora, o objeto reg[^1] tem o modelo estimado. Para obter uma tabela usual de regressão, com valor do coeficiente, erro padrão, estatística t e p-valor, $R^2$ ajustado, e o teste F para os coeficientes basta usar `summary(reg)`. No contexto de regressão linear, podemos querer fazer uma série de coisas, que são explicadas a seguir.

**Atenção: Séries Temporais e o lm**
Objetos de série temporal são armazenados pelo R de uma maneira especial - uma vez que você transforma ele em um objeto de série temporal (algo que esse manual ainda não trata). Entretanto, você *não* deve passar um objeto de série temporal para o `lm()`, já que o `lm` vai ignorar o formato de série temporal. Assim, estimar um modelo AR(1) usando `lm(y ~ lag(y))` vai gerar uma regressão com coeficiente 1 para lag(y) e $R^2 = 1$. De fato, a regressão feita foi y em y - o que não é uma regressão muito emocionante.   

###Testes de hipótese

Suponha que queremos testar a significância conjunta de x.2 e x.3. Precisamos fazer um teste F. Uma maneira é estimar um novo modelo, que chamaremos de reg.2, só com o x.1: `reg.2 <- lm(y ~ x.1)`

Agora, para testar a significância conjunta de x.2 e x.3 basta fazer `anova(reg,reg.2)` e o R reportará os valores do teste (incluindo o p valor)

Testes mais gerais também estão disponíveis, pelo pacote car[^2]. O comando é `linearHypotesis` com o primeiro argumento sendo o objeto com o modelo. O segundo comando é a hipótese que estamos testando, entre aspas, e entramos ele de maneira extremamente intuitiva: suponha que no exemplo acima queremos testar se o coeficiente de x.2 é igual ao coeficiente de x.4. Nesse caso, o comando se resumiria a `linearHypotesis(reg,"x.2 = x.4")`. Várias opções podem ser usadas, como usar estimadores de erro padrão robustos a heterocedasticidade. Recomendamos que o leitor olhe o help do comando no R.

###Erros padrões robustos

Na presença de heterocedasticidade ou autocorrelação, os erros padrões usuais não são confiáveis. Infelizmente, o comando `summary` não exibe erros robustos por default e nem permite alterar os erros padrões exibidos. Felizmente, o pacote lmtest traz uma opção para o sumário padrão do R. A primeira coisa a fazer é carregar o pacote sandwich (`library(sandwich)`). O comando é coeftest e a sintaxe é curiosa. No caso do nosso exemplo, se quisermos obter erros robustos a heterocedasticidade:

```
coeftest(reg, vcov. = vcovHC(reg))
```

Uma explicação: O comando coeftest chama o comando vcovHC do pacote sandwich. Por sua vez, o comando vcovHC precisa saber qual o modelo que vai ter erros robustos. Por isso uma função que recebe uma função. Veja que se quisermos erros robustos a heterocedasticidade e autocorrelação, o comando vira vcovHAC.  

###Logs
 
 Muitas vezes queremos fazer as regressões não com as variáveis em nível, mas com as variáveis em log. Nesse caso, suponha que queremos a variável dependente - y - em log. Para isso, basta fazer `log.y <- log(y)` e a variável log.y vai ser a versão em log da variável y. Você pode reescrever a regressão como `lm(log.y ~ x.1 + x.2 + x.3)`.
 
 Agora, pode ocorrer de em alguns casos o vetor y ter algum elemento zero. Mas $\log(0) = -\infty$. O R tem um elemento Inf (e - Inf) para esses casos, mas o comando lm vai acusar um erro ao receber um vetor com algum elemento Inf. A solução é trocar esse valor para alguma coisa, como um NA, que o R vai ignorar[^3]. Para fazer isso, suponha que o vetor com os Inf seja o log.y do paragrafo anterior. Precisamos explicar para o R quais casos nós queremos substituir, e isso é incrivelmente fácil: assim como podemos usar `log.y[1]` para escolher o primeiro elemento do vetor `log.y`, podemos colocar entre colchetes uma condição, por exemplo os elementos do vetor log.y que são iguais a infinito. Já tratamos disso: `log.y == -Inf` faz esse trabalho. Assim, se quisermos substituir os elementos de log.y que são iguais a -Inf por NA, basta usar o seguinte trecho de código:
 
```
 log.y[log.y == -Inf] <- NA
```

##Probits e Logits
 
 Probits e logits também são úteis quando nossa variável dependente é uma dummy. Sempre podemos usar um modelo de probabilidade linear, e nesse caso o comando a ser utilizado é o `lm`. Mas em casos que queremos usar um probit ou logit, precisamos recorrer ao `glm`, que tem sintaxe muito parecida com o `lm`. Mas além de especificar as variáveis dependentes e independentes, também precisamos especificar o tipo de regressão, basicamente a distribuição da variável dependente. No caso de probits e logits, a variável dependente tem distribuição binomial. Depois, temos que especificar se a função de probabilidade da variável dependente é probit ou logit. Os comandos para estimar probits e logits são ilustrados abaixo:
 
```
mod.1 <- glm(y ~x.1 + x.2 + x.3, family = binomial(link = 'probit'))
mod.2 <- glm(y ~x.1 + x.2 + x.3, family = binomial(link = 'logit'))
```
 
E como de praxe, podemos usar o comando `summary` para obter os coeficientes, desvios padrões e estatísticas t. 
 
##Variável instrumental
 
Métodos de variável instrumental são muito úteis e populares, especialmente em casos de endogenidade. Existem várias implementações, mas para o mínimos quadrados de dois estágios usual, o pacote *AER* oferece um comando `ivreg`. A sintaxe é similar ao `lm`, mas com uma alteração na formula para inserir os instrumentos, que são separados dos regressores por |.
 
 Por exemplo, suponha que temos a variável dependente y, as variáveis endogenas x.1 e x.2, a variável exogena x.3, e os instrumentos z.1 e z.2. Nesse caso, o ivreg seria usado:
 
```
modelo <- ivreg(y ~x.1 + x.2 + x.3|z.1+z.2+x.3)
```

E podemos usar o `summary` para ver o valor dos coeficientes, erros padrão e estatísticas t. Observe que o pacote não mostra o valor do teste F para o primeiro estágio nem de teste de sobre identificação.   

##Dados em painel

Em muitas aplicações usamos dados em painel - i.e., com dimensão temporal e cross section. Em geral, esse tipo de aplicação acaba envolvendo o uso de efeitos fixos. Existem duas maneiras de fazer: usando o pacote *plm* ou "na mão", usando o lm. Não há nenhuma vantagem de usar o lm "na mão", em geral, exceto em casos que temos mais de duas dimensões ou por algum motivo o *plm* não funciona. Exploraremos primeiro o uso do plm, que deve satisfazer a maioria dos usuários. A solução na mão vem depois e pode ser ignorada sem perda de continuidade.

Supondo que o pacote já foi instalado e carregado, precisamos (i) explicar para o pacote quais colunas são as colunas com efeitos fixos de tempo e unidade e (ii) rodar a regressão propriamente dita. Suponha, como de praxe, que temos um dataframe carregado no R com nome "dados". Suponha que as colunas com as datas e um índice para unidades se chamam datas e unidades, respectivamente. Veja que essas colunas podem estar em formato de carácter, e que isso não deve nos preocupar no momento: poderia ser o caso de a unidade ser estados do país e o código da unidade ser o código do estado (DF,RJ,SP,...). O pacote *plm* disponibiliza o comando *plm.data*, que converte um data frame de forma que quando rodarmos a regressão, o R saiba quem são os efeitos fixos. Assim, vamos criar um dataframe chamado **dd**:

```
dd <- plm.data(dados,c('unidades','datas'))
```   

O primeiro argumento da função é o data.frame a ser convertido, que contém as colunas para as quais criaremos efeitos fixos. O segundo argumento da função são as colunas com os efeitos fixos. Veja que a ordem é unidade e depois a variável temporal. Agora, a estimação do modelo pode ser feita usando o comando plm, que tem sintaxe muito similar ao lm: passamos uma formula com a variável dependente e as independentes, informamos a base de dados - que nesse caso é o objeto **dd**, não o objeto dados. Mas temos algumas novas opções: o método de estimação (em geral estamos interessados em within, o padrão), mas mais relevante é que efeitos fixos queremos colocar: só para indivíduo, só para tempo ou ambos. Abaixo, mostramos a sintaxe para cada um dos casos, respectivamente:

```
mod.1 <- plm(y ~x.1 + x.2,data = dd, effect = 'individual')
mod.2 <- plm(y ~x.1 + x.2,data = dd, effect = 'time')
mod.3 <- plm(y ~x.1 + x.2,data = dd, effect = 'twoways')
```

Como de praxe, podemos usar o comando `summary` para obter um sumário da regressão. 

###Painel usando lm*

Suponha que não conseguimos usar o *plm* por alguma razão. Por exemplo, podemos querer três efeitos fixos: se tivermos microdados de escola, podemos querer ter efeito fixo de aluno, escola e tempo. Podemos implementar isso no braço usando o `lm`. Lembre-se que, no fundo, efeitos fixos são mera *dummies*, então se fizermos um modelo linear com dummies, devemos obter resultados parecidos.

Para ficarmos em terreno conhecido, suponha que só temos dois efeitos fixos que nos interessam: unidade e tempo. Cada um desses vem codificado em duas colunas: uma com a data e outra com algum código para a unidade. Lembrem-se da discussão no capítulo anterior que o `lm` é capaz de usar isso e entender como dummies, sem a necessidade de criar várias variáveis com 0 e 1. Logo, se queremos explicar y usando x como variável explicativa e efeitos fixos de unidade e tempo, a seguinte regressão deve bastar:

```
modelo <- lm(y ~x +tempo + unidade, data = dados)
```

E y,x,tempo e unidade estão no dataframe chamado dados, como de praxe. Algumas diferenças devem ser notadas para o comando plm:

(@) O sumário vai ser mais confuso no caso do `lm`: o `plm` esconde os efeitos fixos, o que não ocorre no caso do `lm`

Mas mais importante:

(@) Devido a maneira como o `plm` estima o modelo (por *within*, em geral), o `plm` usa menos graus de liberdade e pode fazer estimações mais precisas. Isso deve impactar mais nos desvios padrões que no valor dos coeficientes, especialmente quando o número de variáveis for muito grande. 


<table>
<tr>
<th bgcolor="#adadeb">Hands on!</th>
</tr>

<tr>
<td bgcolor="#adadeb">
É uma boa hora de checar se os resultados do plm e do lm são de fato similares. O pacote *AER* traz a base de dados do exemplo de dados em painel sobre cigarros tirado do livro do Stock e Watson. Para carregar, basta digitar `data("CigarettesSW")`. Os efeitos fixos são para estado e ano, e vem em colunas com nomes state e year, respectivamente. As variável packs nos traz o número de pacotes consumido naquele ano e estado, e income a renda do estado naquele ano. Uma regressão packs em income com efeitos fixos para estado e ano pode ser feita usando os dois métodos da seção anterior: o `plm` e o `lm`. Ambos devem dar, aproximadamente, o mesmo valor para o coeficiente do efeito da renda sobre pacotes $(-9.070e-08)$.
</td>
</tr>
</table>

[^1]: Se você leu a discussão anterior de tipos de objetos, vale observar que reg é uma lista

[^2]:Nada haver com carro: é uma sigla para Companion to Applied Reggresion

[^3]:Nem sempre o R vai ignorar um NA. Isso depende do pacote e das configurações do R
