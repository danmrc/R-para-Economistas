#Paralelizando

Muitas tarefas no R consomem muito tempo. *Loops* longos são um caso particular disso. Se cada iteração do loop demora 1s e você pede para repetir a iteração mil vezes, você gasta mil segundos (16 minutos)! Infelizmente, muitas operações acabam caindo em problemas que são *loops*. Felizmente, existe uma maneira de agilizar o processo.

Isso se deve a dois fatos. O primeiro é que a estrutura do *for* muitas vezes permite que cada tarefa seja feita separadamente. Isso é muito frequente em simulações, que serão tratadas em mais detalhes mais a frente. O outro fato é que o R não aproveita 100% da capacidade da maioria dos computadores atuais. Os computadores atuais vem com processadores com mais de um núcleo (*multi core*[^1]). Cada núcleo age como um pequeno processador e o computador distribui as tarefas entre estes núcleos.

Em um *for*, poderíamos dar cada iteração do loop para núcleo do processador, e quando o núcleo termina a iteração ele devolve a resposta recebe uma nova iteração para fazer. Isso é chamado de paralelização e agiliza muito situações em que precisamos de um *loop*. 

Podemos pensar em uma situação equivalente bastante prática: suponha que queremos multiplicar todos os números de 1 a 10. Se sentarmos uma única pessoa para fazer a conta, esta pessoa vai demorar. Entretanto, se tivermos quatro pessoas na sala, podemos deixar a primeira pessoa multiplicar 1,2,3; a segunda 3,4,5; a terceira 6,7,8; e a quarta 9,10. No fim, pegamos o resultado e pedimos para a primeira e a quarta pessoa multiplicarem seus resultados com os resultados da segunda e da terceira, respectivamente. E por último multiplicamos o resultado que elas obtiveram. De fato, antes da existência dos computadores, contas complicadas eram feitas assim! 

O tratamento dessa seção deve muito a este [documento](http://michaeljkoontz.weebly.com/uploads/1/9/9/4/19940979/parallel.pdf)

##Desafios computacionais de paralelização

A primeira coisa a se observar é que paralelizar coloca um enorme peso sobre o computador. O R gera novos processos e cada um vai para um núcleo do processador. Isso deixa o computador sobre uma carga brutal de trabalho. Assim, rodar código paralelizado usualmente requer que você deixe o computador fazendo apenas o que você pediu para o R e nada mais. 

Outro problema é que processos paralelizados gastam muita memória RAM. Assim, é importante ficar de olho no consumo de RAM (via gerenciador de tarefas). Tratarei desse problema mais a frente, mas uma boa regra de bolso é que cada núcleo precisa de mais ou menos um 1,5GB de RAM. Mas isso vai variar de sistema para sistema.

##Configurando o R para paralelizar

Vamos precisar de dois pacotes para a paralelização: o **foreach** e o **doParallel**. Carregue os dois pacotes. É sempre bom saber quantos núcleos nós temos, e isso é possível via o comando `detectcores()`. Na sequência só precisamos definir o número de núcleos, que no código a seguir foi definido pela variável `n.cores`. O resto dos comandos é padrão e não temos que entender o que cada um faz:

```
n.cores <- 3
cl <- makeCluster(n.cores)
registerDoParallel(cl)
```

Observe que até podemos definir um número de núcleos maior do que o que `detectcores()` mostra, mas isso vai ser extremamente problemático: nós teremos 30 seções do R disputando por recursos do computador. Como regra de bolso, `n.cores` deve ser, no máximo, como o número de núcleos que o `detectcores()` encontrou menos 1. 

Para checar se o R registrou corretamente e consegue usar os processos paralelizados, use o comando `getDoParWorkers()`. Ele irá indicar o mesmo número que você colocou no `n.cores` se tudo tiver dado certo.

##Usando a paralelização

Uma vez configurado, temos mais uma etapa: o comando `for` usual do R não consegue usar as vantagens da paralelização. Por isso, precisamos usar o comando `foreach`, que é muito semelhante, mas tem diferenças importantíssimas. A primeira é que o `foreach` vai gerar um objeto, ao contrário do for. Você pode explicar qual o objeto vai ser gerado usando a opção .combine entre as opções. O default é criar uma lista. Também é importante entender que o que vai ser colocado no objeto que o R vai gerar com o `foreach` é o último comando dentro do foreach **que não é a criação de um objeto**.

A sintaxe do foreach também é diferente: não usamos o in do for e precisamos colocar um `%dopar%` entre o parênteses e as chaves. Assim, para repetir alguma coisa n vezes: 

```
objeto <- foreach(i=1:n) %dopar% {

comandos

}
```

Um exemplo deve clarificar. Suponha que queremos tirar a raiz quadrada de todos os números de 1 a 20 e queremos paralelizar isso. Escreveríamos o código da seguinte maneira:

```
raizes <- foreach(i = 1:20) %dopar%  {

sqrt(i)

}
```

Veja que o R vai devolver uma lista. Se quissemos que ele devolvesse um vetor (que parece mais razoável no caso), teriamos que ter alterado o parêntese para (i = 1:20, .combine = c). Veja que se tivéssemos usado `a <- sqrt(i)`, ao invés de `sqrt(i)`, o R teria devolvido uma lista vazia. 

[^1:]Dai dual core, quad core etc.
