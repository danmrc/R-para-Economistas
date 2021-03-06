--- 
title: "R: Uma Introdução para economistas"
author: "Daniel Coutinho"
date: "04/07/2018"
site: bookdown::bookdown_site
output: bookdown::gitbook
documentclass: book
biblio-style: apalike
link-citations: yes
---

#Introdução

Existem muitos softwares de estatística, mas o R é um dos mais populares. O R é um software gratuito, o que justifica a sua escolha. Mas além disso, a comunidade é muito ativa, desenvolvendo muitos pacotes - inclusive para econometria. O R não é tão amigável quanto o gretl: não existem menus para escolher como estimar. Entretanto, ele é muito mais flexível. Assim, o R tem ganho cada vez mais espaço entre aqueles que fazem econometria.

Este manual foi criado para ajudar a introduzir economistas ao R. Com isso, ele é mais voltado para exemplos de tratamento de dados e ferramentas estatísticas usadas pelos economistas. Existem outros excelentes livros que ensinam a usar o R, e alguns aplicados à econometria. Eles estão listados nas referências deste manual. Mas nenhum deles é em português, e muitos são muito detalhistas e longos. Este manual tenta servir como algo menos extenso que os livros. Portanto, uma consulta aos livros pode ser muito útil.

Esta é a segunda versão do manual, que foi extensivamente reescrito em meados de 2017 e início de 2018. Não é necessário nenhuma experiência anterior com programação, mas é necessário saber (alguma) econometria. O autor agradece as muitas pessoas na PUC-Rio que ajudaram com o R, bem como os seis meses trabalhando no Dlab. As duas versões anteriores foram escritas, originalmente, em LaTeX, o *gold standard* da edição de texto acadêmica.Essa versão foi escrita usando o bookdown, um pacote do R para produzir livros para a internet.  

Algumas palavras sobre a filosofia por trás desse manual são necessárias: a ideia é tirar o leitor do chão e colocar ele pronto para fazer as coisas básicas de econometria rapidamente. Isso envolve notórias omissões. O autor muitas vezes sugere consultar o help, o que deve ser visto não como preguiça, mas sim como a única maneira de fazer as coisas: existem mais de 12 mil pacotes disponíveis para o R, e nenhum ser humano jamais conseguirá explorar e entender todos. O autor usou, diretamente, uma dúzia pacotes, se tanto. E invariavelmente é necessário consultar o help para saber qual o nome do argumento que faz alguma coisa específica. Assim, a ideia aqui é entender as ideias gerais do R, os comandos básicos e o mínimo do vocabulário para saber consultar/ler o help. 

Definitivamente, esse manual deve ser lido com o R aberto e tentando entender cada um dos comandos que são ditos aqui. Uma leitura comparativa entre o help e o que eu coloco no manual sobre cada comando é provavelmente a melhor maneira de proceder. Tentar replicar tudo que eu faço é certamente uma maneira garantida de aprender.As seções Hands on propõe exercícios específicos para serem feitos no R. 

O manual é dividido em três partes: a primeira trata de como fazer econometria no R: é o coração do manual e é de principal interesse dos economistas; a segunda parte trata de como programar no R, o que é útil em algumas atividades um pouco mais avançadas. A terceira parte será dedicada a algumas coisas de matemática no R. Seções marcadas com um * são mais indigestas para a primeira leitura.
