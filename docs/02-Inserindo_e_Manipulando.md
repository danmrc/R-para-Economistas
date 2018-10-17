#Inserindo e Manipulando Dados no R: O básico. 

Só podemos fazer análise de dados se tivermos... dados. Este capítulo ensina a colocar os dados no R e a manipular eles como para criar variáveis necessárias ou limpar os dados antes de iniciarmos a análise. 

Esse capítulo não tenta ser enciclopédico nem detalhista: pelo contrário, ele omite muitas coisas. A omissão mais grave é, sem dúvida alguma, os pacotes do *Tidyverse*. A omissão se deve a ignorância do autor em usar estes pacotes. 

##Arquivos excel/csv

O R não lê diretamente arquivos excel (.xls ou .xlsx), apesar de alguns pacotes permitirem o R ler estes arquivos. Mas esta não é a melhor opção: o ideal é salvar a planilha com os dados em outro formato, como .csv. Isto não é difícil: basta, no excel, ir em salvar como, e embaixo na opção de nome do arquivo há a opção de escolher o formato. O que queremos é **csv (separado por vírgula)**

Para carregar o arquivo no R, precisamos saber algumas poucas coisas:

* O seu excel usa ponto ou vírgula como separador decimal?
*Aonde está o arquivo

O item 1 importa porque, se o separador decimal for vírgula, usamos o comando `read.csv2`. Caso contrário, usamos `read.csv`. O comando é bem simples: basta passar o caminho (aquele C:/Usuário/...). Por exemplo, suponha que eu tenho um csv chamado dados, e quero importar ele para o R. Ele fica no C:/Usuário/Autor/Manual. Então, eu faria:

```
read.csv2("C:/Usuário/Autor/Manual/dados.csv")
```

Observe que o caminho para o arquivo está entre aspas e que você precisa colocar a extensão do arquivo no fim - o .csv. Mais uma observação: em geral, se você copiar e colar o caminho como o Windows dá o nome de arquivo com \ ao invés de /. O R só lê usando /, então você tem que alterar isto. 

Neste caso, o R só vai exibir os dados, e não vai salvar eles dentro do R. Você não vai poder fazer nada com os dados. Para podermos usar eles mais tarde, precisamos salvar ele no ambiente do R. Para isto, basta criar um objeto com os dados, como já fizemos no capítulo anterior. Por exemplo, podemos ser extremamente criativos e chamar o objeto de dados. Nesse caso:

```
dados <- read.csv2("C:/Usuário/Autor/Manual/dados.csv")
``` 

Se você não está familiarizado com usar o caminho dos arquivos, isto pode parecer excessivamente complicado. Felizmente, o R permite com que você escolha o caminho do arquivo de maneira mais usual, usando um menu e o mouse. Para isto, precisamos alterar o comando acima ligeiramente:

```
dados <- read.csv2(file.choose())
```

Isto vai abrir o menu e permitir que você escolha o arquivo como um menu do word. Entretanto, apesar de ser mais fácil, essa solução pode ser extremamente inconveniente: toda vez que você for rodar o programa você vai ter que escolher. Apesar de trabalhar com o caminho ser um pouco mais chato, isto poupa muito tempo. 

##Lendo arquivos do stata e outros pacotes estatísticos

Muitos arquivos com dados ainda são distribuídos em versão de programas estatísticos, como o stata. É fácil ler estes arquivos usando o pacote *foreign*. Normalmente este pacote já vem instalado, mas caso você não tenha, você pode instalar como qualquer outro pacote. Ele permite ler dados do SAS, SPSS, entre outros. A ideia é a mesma da seção anterior, mas com comandos diferentes para cada tipo de arquivo: o ideal é consultar o help do pacote.

Por exemplo, para ler um arquivo do stata, o comando no pacote foreign é `read.dta`. Suponha que, ao invés de ser um arquivo .csv, meus dados do exemplo anterior estivessem salvos em formato do stata. Bastaria fazer: 

```
dados <- read.dta("C:/Usuário/Autor/Manual/dados.dta")
```

**Porém**, o `read.dta` só lê arquivos criados pelo stata até a versão 12. Para versões posteriores do stata, existe um pacote chamado *readstata13*. Se, ao usar o `read.dta` você receber uma mensagem de erro, vale a pena checar o *readstata13*. 

##Lendo arquivos muito grandes

Algumas bases de dados podem ser muito grandes, e o R pode sofrer para abrir - mesmo em computadores com muita memória e muito processamento. Para driblar o problema, o pacote *data.table* ajuda a carregar arquivos grandes para o R. O pacote trás várias opções para trabalhar com os dados carregados, que não serão tratadas aqui. 

Outra opção é o pacote *readr*, que funciona de forma parecida com o comando padrão do R. Para ler um csv que usa vírgulas para separar decimais, o comando é `read\_csv2` - basicamente idêntico ao comando padrão do R, mas com uma linha no lugar do ponto. Ao carregar o arquivo, tudo funciona como o usual. Uma pequena diferença é que o nome das variáveis é preservado: assim, uma variável chamada "Nome da variável" continuará se chamando "Nome da variável", ao invés do padrão do `read.csv2` de transformar em "Nome.da.variável". Apesar de isso parecer bom a primeira vista, dificulta acessar as variáveis mais tarde, já que o R não entende nomes com espaço para objetos.  %falta explicar como carregar os arquivos

##Trabalhando com os dados

Uma vez carregado os dados, pode ser necessário manipular os dados de diversas maneiras. Esta seção tratará de algumas das maneiras mais comuns. 

###Selecionando linhas/colunas/elementos

Selecionar uma linha ou uma coluna específica de uma base de dados é essencial. Se quisermos rodar uma regressão e cada coluna da tabela é uma variável, então temos que ser capazes de informar ao R quais colunas serão usadas como variável explicada e quais como variável explicativa. O R usa a notação de matrizes com colchetes, então para selecionar a 4ª linha da base de dados chamada dados, basta fazer `dados[4,]`. Veja que colocamos a vírgula e depois deixamos em branco, informando ao R que queremos todas as colunas. Para obter todas as linhas da quarta coluna, fazemos `dados[,4]`.

E se quisermos apenas algumas linhas ou algumas colunas? Podemos passar um vetor dizendo quais são essas linhas e/ou colunas. Por exemplo, se quisermos as linhas 1 **a** 4, podemos passar um `dados[1:4,]`. E se quisermos as linhas 1 **e** 4, podemos fazer: `dados[c(1,4),]`

Outra maneira, bastante útil, de selecionar variáveis é pelo nome delas. Suponha que os dados vem com nomes id, renda, escolaridade. Para selecionar a variável renda, basta fazer `dados$renda`. Veja que isso exige saber como (e se) o R importou os nomes. Para isso, a função `names` permite saber quais os nomes das variáveis. Logo `names(dados)` vai retornar os nomes das variáveis. Em geral, os espaços são substituídos por pontos, logo uma variável anos de estudo se tornará `anos.de.estudo`. Veja que também podemos acessar as variáveis em um data.frame usando `nome do data.frame$nome da variável`. Assim, se temos um data.frame com nome dados e queremos acessar a variável renda, bastaria fazer `dados$renda`.

Veja que podemos querer selecionar apenas um elemento. No caso de vetor, é a única coisa que faz sentido: o vetor só tem uma dimensão (uma linha ou uma coluna), então só podemos pegar um elemento dele. Suponha que temos o vetor $\mathbf{v}$ e queremos o décimo elemento: basta fazer `v[10]`. Veja que não usamos vírgulas, que são usadas apenas para separar as dimensões. Se quisermos um elemento de uma matriz, basta passar a linha e a coluna dele, respectivamente. Por exemplo, o elemento da segunda linha e quinta coluna do dataframe dados é obtido usando `dados[2,5]`. 

Mas o R permite você selecionar o elemento de uma matriz como se fosse um vetor! Suponha uma matriz - chamaremos ela de $\mathbf{M}$ - com 5 linha e 5 colunas. O último elemento da matriz pode ser obtido com `M[5,5]` ou, equivalentemente, `M[25]`. Veja que o 25 não é a toa, no total a matriz tem 25 elementos: logo, o último tem que ser o membro 25.   

\subsection{Criando dummies}

As vezes, queremos transformar uma variável contínua em uma *dummy*. Pode ser o caso que queremos isolar apenas aqueles que recebem menos de um salário mínimo, e queremos que quem tiver menos de um salário mínimo tenha valor 1 e, caso contrário, 0. Suponha que o salário mínimo seja 678, e que a variável de salários se chame w. Então, bastaria fazer:

```
sal.min <- w < 678
```

Observe que o R vai gerar um vetor de Verdadeiros e Falso. É possível converter para numérico, mas não há nenhuma necessidade, uma vez que o R é capaz de interpretar o verdadeiro ou falso como uma *dummy* na regressão.

O que estamos fazendo é apenas uma operação de compara cada número do vetor ao número 678. Testamos se ele é menor (<), mas poderíamos ver que números são maiores (>), iguais (==)[^1], menor ou igual (<=) ou maior ou igual (>=). Estes operadores não são apenas úteis para criar *dummies*, mas também pode servir para escrever funções, que será tratado mais a frente. 

Pode ocorrer de a variável vir como um vetor de palavras ou siglas. Suponha que estamos trabalhando com um painel que tem variação temporal e por estado, e o vetor de estados vem com as siglas dos estados (RJ,SP,ES,MG,DF...). Se quisermos usar efeitos fixos de uma maneira extremamente ingênua, poderíamos criar dummies para cada estado e estimar o efeito fixo de cada estado. Esta não é uma maneira inteligente de fazer, já que existem pacotes para fazer estimação usando efeitos fixos com bem menos trabalho, que serão tratados no próximo capítulo. Mas, no momento, vamos ignorar esta opção e tentar criar uma dummy para cada estado.

Uma possível solução era criar um vetor para cada estado (Ou talvez uma matriz com n linha e o número de colunas sendo igual o número de estados), ler cada posição do vetor das siglas usando um loop e colocar um 1 na coluna correspondente, criando um vetor índice para o R buscar qual coluna é relacionada com cada estado... Se a explicação anterior bagunçou o seu cérebro, não se preocupe: ela é complicada, e o R não exige nada tão complicado[^2] 

Uma solução muito mais simples é usar o comando factors, que gera automaticamente dummies para cada categoria. Assim, RJ vira uma dummy, SP outra etc. Isso é automático, e podemos jogar direto numa regressão. Assim, suponha que temos uma base de dados chama dados e a coluna 2 é a coluna com os estados. Nesse caso: `estados <- factor(dados[,2])`. Poderíamos usar estados diretamente na regressão, que será tratada no capítulo seguinte. 

[^1]: Dois iguais seguidos

[^2]: Em geral. O autor já encontrou situações em que esse tipo de solução era estritamente necessária. A seção tópicos avançados discute este problema e como resolver.
