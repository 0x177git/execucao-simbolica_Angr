# Uma breve explicaçao do que eh a execuçao simbolica
## execuçao abstrata
a nossa queridissima execuçao simbolica nasceu a principio como uma forma de execuçao abstrata (basicamente um tipo de execuçao ou interpretaçao na qual podemos controlar o fluxo de dados e o fluxo de execuçao) 
a qual nos permite analisar o fluxograma de algum tipo de execuçao

### execuçao simbolica
este conceito nasceu de fato nos anos 70, mas tornu-se popular com nosso queridos manos da *DARPA* (os msm que modelaram a rede _onion_) os quai criaram um desafio que durou 2 anos 
no qual esperavam desenvolver um sistema de deteçao de vulnerabilidades em tempo-real, isso nos anos 2000 (e eu aqui soh querendo um _AMD Ryzen 7 5800_)

mas vamos ao que interessa

### principos e propiedades da execuçao simbolica

algo que precissamos comprender bem eh o seguinte, pra a execuçao simbolica, nosso binario ou code, eh apenas um fluxo de execuçao com diferentes estados (Maquina de turing nao-deterministica), entao, tendo uma ou multiplas entrada, esta pode tomar diferentes caminhos (podemos representar como uma arvores, onde a entrada de usuario ou alguma variavel resultante de uma funçao eh a raiz e nós sao fluxos compartivos (`if`|`swhitch`) entre as condicionais e cada escolha tomada com base na entrada eh um sub-nó ou folhas dependendo se determinado fluxo termina) 
isto pra testes de software eh uma maravilha, jah que permite testar as entradas esuas possiveis saidas, alem de testar se existem nós ou caminhos que podem resultar em erros ou bug ( ou seja determinar o comortamento de um sistema e se este age como o esperado)

* *uma das formas existentes de testar isto eh a geraçao aleatoria* na qual testamos dados de entrada aleatorios afim de ver os possiveis caminhos, mas e se quissermos tomar um caminho (ou um conjunto) de fluxos sem sabermos diretamente a entrada?? (ou seja, podemos saber a localizaçao, ou o tipo de dado, mas nao sabemos especificamente os dados de entrada pra tomar o fluxo de caminho que desejarmos)

* *eh neste ponto que entra a execuçao simbolica* tendo este fluxo de caminhos e sabendo que existe determinado conjunto de caminhos que desejarmos testar, podemos atraves desta tenica injetar valores simbolicos pra adentrarnos dentro de determinado fluxo

vou deixar um grafico aqui pra ficar melhor de esclarecer, jah que aqui começa a se tornar um pouco abstrato 






[source](https://arxiv.org/pdf/1610.00502.pdf)
