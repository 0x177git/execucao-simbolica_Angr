# Uma breve explicaçao sobre o que eh a execuçao simbolica
## execuçao abstrata
a nossa queridissima execuçao simbolica nasceu a principio como uma forma de execuçao abstrata (basicamente um tipo de execuçao ou interpretaçao na qual podemos controlar o fluxo de dados e o fluxo de execuçao) 
a qual nos permite analisar o fluxograma de algum tipo de execuçao

### execuçao simbolica
este conceito nasceu de fato nos anos 70, mas tornu-se popular com nosso queridos manos da *DARPA* (os msm que modelaram a rede _onion_) os quai criaram um desafio que durou 2 anos 
no qual esperavam desenvolver um sistema de deteçao de vulnerabilidades em tempo-real, isso nos anos 2000 (e eu aqui soh querendo um _AMD Ryzen 7 5800_)

mas vamos ao que interessa

### principos e propiedades da execuçao simbolica

algo que precissamos comprender bem eh o seguinte, pra a execuçao simbolica, nosso binario ou code, eh apenas um fluxo de execuçao com diferentes estados (*Maquina de turing nao-deterministica*), entao, tendo uma ou multiplas entrada, esta pode tomar diferentes caminhos (podemos representar como uma arvore, onde a entrada de usuario ou alguma variavel resultante de uma funçao eh a raiz e nós sao fluxos compartivos (`if`|`swhitch`) entre as condicionais e cada escolha tomada com base na entrada eh um sub-nó ou folhas) 
isto pra testes de software eh uma maravilha, jah que permite testar as entradas e suas possiveis saidas, alem de testar se existem nós ou caminhos que podem resultar em erros ou bug ( ou seja determinar o comportamento de um sistema e se este age como o esperado)

* *uma das formas existentes de testar isto eh a geraçao pseudo-aleatoria* na qual testamos dados de entrada aleatorios afim de ver os possiveis caminhos, mas e se quissermos tomar um caminho (ou um conjunto) de fluxos sem sabermos diretamente a entrada?? (ou seja, podemos saber a localizaçao, ou o tipo de dado, mas nao sabemos especificamente os dados de entrada pra tomar o fluxo de caminho que desejarmos)

* *eh neste ponto que entra a execuçao simbolica* tendo este fluxo de caminhos e sabendo que existe determinado conjunto de caminhos que desejarmos testar, podemos atraves desta tecnica injetar valores simbolicos pra adentrarnos dentro de determinado fluxo

> [Arvix (A Survey of Symbolic Execution Techniques) ref.2](https://arxiv.org/pdf/1610.00502.pdf)




### vou deixar um grafico aqui pra ficar melhor de esclarecer, jah que aqui começa a se tornar um pouco abstrato 

![arvore-binaria incompleta](https://github.com/exoForce01/execucao-simbolica_Angr/blob/main/binary-tree.png?raw=true)

temos aqui uma arvore binaria (lembrando que esta eh apenas uma representaçao, em *radare2* veremos como grafos por exemplo)

* a nossa raiz (1) seria uma entrada de usuario ou variavel na qual este segmento ou funçao ira executar comparaçoes pra trazer diferentes resultados
então, imaginemos que desejamos tomar o seguinte fluxo (caminho)
* queremos passar pela sub-arvore (2)
*  dps pelo sub-nó (4)
*  afim de chegar na folha (saida) (8)

o que a nossa *execuçao simbolica* projetaria fazer seria inserir os simbolos necessarios pra executarmos o caminho (2 -> 4 -> 8), a diferença por exemplo da geraçao pseudo-aleatoria que iria testar varios caminhos atraves de entradas aleatorias (que vem de um dominio)

as vantagens disto em comparaçao de outras tipos de teste seriam 

* podemos escolher um ou varios conjuntos de caminhos, podemos analisar atraves de um controle de fluxo mais preciso e especifico, e atraves de pontos de controle podemos verificar os simbolos inseridos pra adentrarmos de determinado fluxo (nó)

porem ha algums coisas que precisso comentar, e eh que demasiados conjuntos de caminhos poderiam dar tempos de respotas literalmente maiores que a idade do universo, alem de que a execuçao simbolica nao diferencia entre resultados de erro e caminhos que se tornam inviaveis por adentrar-se em funçoes ou fluxos nao esperados

> * [Resumo de alguns pontos, ref.1](https://www.tutorialspoint.com/software_testing_dictionary/symbolic_execution.htm)

*assim precisamos a principo conhecer os fluxos de comparaçoes a fim de evitar estes problemas e evitar aplicar este tipo de metodo em fluxos exageradamente grandes*
  
### exemplo rapido 

![reversing in binary](https://github.com/exoForce01/execucao-simbolica_Angr/assets/138733317/214852da-bdaa-466e-a178-9b20e759b572)

se vc jah conhece um pouco a sintaxe do *radare 2* jah deve ter entendido o que acontece aqui, mas vamos lah, temos aqui uma binario que iremos chamar de *_crackme_*

temos a principo uma comparaçao de tamanho, no qual recebemos uma variavel que eh o resultado da saida de uma funçao 
```
if (iVar2 == 0x20) {        #iVar2 eh um inteiro que recebe o tamanho do noss input
```
aqui podemos definir esta como a primeira comparaço, ou melhor como a sub-arvore do nosso pedaço de code, na qual comparamos o tamanho e se este eh igual a hex(0x20(31 bytes)) ou seja se nosso *array* possui 31 elementos e retorna _True_

logo dps temos um pedaço no qual chamamos o `argv` ou seja a entrada do usuario dentro da `pcVar1` e dps eh que começa os nossos kaoticos (com _k_ msm) condicionais comparativos, onde ocorrem 31 fluxos condicionais e se a entrada e saida das operaçoes em _stack_ forem iguais nos 31 elementos, este ira nós retornas a mensagem **license correct** que eh o que queremos

        em um mundo onde nossos modelos abstratos de teste nao existem, teriamos que resolver atraves de analise estatico condicional por condicional

porem jah conhecendo a teoria da nossa execuçao simbolica, sabemos que existe uma teoria de execuçao abstrata (**_modelo de teste_**) com a qual podemos direccionar um conjunto de caminhos a fim de atingir um resultado especifico (*_em nossa represantaçao de arvore binaria, podemos imaginar como a folha (8)_*), neste caso, atraves de entradas simbolicas durante os teste comparativos 

## sabendo a mecanica de execuçao simbolica, as propiedades que precisamos, o conjunto de caminhos que precisamos atingir e os que precisamos evitar, podemos entao ir pra a segunda parte 
eu apresento a vcs, [**Angr**](https://angr.io/) um framework de analise estatica e motor de execuçao simbolica criado em python na universidade santa monica, vamos lah implementar o que acabamos de conhecer aqui??


