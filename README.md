# Multiprogamação e Programação Concorrente.

## Multiprogramação

### Multiprogramação é uma forma de simultaneidade de processamento utilizando um único processador [TANENBAUM]
### •Conhecido como multiprocessamento.
### •Técnica utilizada para melhor aproveitamento dos recursos de um sistema operacional.

### Tipicamente, um programa realiza operações de CPU e  I/O. Se um único programa tomasse posse do sistema até o final da sua execução, existiria  uma grande desperdício de recursos. Por exemplo, quando um programa (mais precisamente um processo) realiza um operação de I/O (ex: aguarda a entrada de um dado), a CPU fica ociosa. Assim, cabe ao SO executar outro processo na CPU, enquanto o anterior aguarda ser atendido pelo I/O. Essa estratégia de otimização do uso da CPU é chamada de multiprogramação.

### Em sistemas multiprogramados, vários processos são mantidos na memória. Aqueles que não podem ser armazenados na memória principal (por limitação de espaço), permanecem em disco. Assim que for possível, o processo sairá do disco e será carregado na memória principal.

### Um conceito fundamental em sistemas operacionais é o conceito de processo. Um processo é basicamente um programa em execução. Ele consiste de um programa executável, os seus dados e pilha, o seu stack pointer e registradores, enfim todas as informações necessárias para executar o programa.

### Periodicamente o sistema operacional decide se a execução de um processo deve ser interrompida e a execução de um outro processo deve ser iniciada--pela razão do primeiro já ter tido mais do que a sua fatia de tempo de CPU. Em um sistema de multiprogramação a CPU fica se alternando entre a execução de vários processos, cada um por dezenas ou centenas de milisegundos.

### Um processo pode estar em um dos seguintes estados:

### Running: usando a CPU naquele instante;
### Ready: pronto para ser executado, temporariamente parado para que outro processo possa ser executado; e
### Blocked: impossibilitado de ser executado até que algum evento externo ocorra.

## Multiprogramação com Partições Fixas

### A maneira mais simples de implementar a multiprogramação, em termos de memória, é dividir a mesma, primeiramente em uma parte para uso do sistema operacional e outra para uso dos processos de usuários. Depois, a parte dos usuários é dividida em n partições de tamanhos diferentes, porém com valores fixos. Quando um programa chega, há duas possibilidades: ele é colocado em uma fila de entrada da menor partição capaz de armazená-lo ou ele é colocado em uma fila de entrada única.

### Com o esquema das partições de tamanho fixos, qualquer espaço não ocupado por um programa é perdido. Na maioria das vezes o programa a ser executado é menor do que a partição alocada para ele. A partição alocada não pode ser menor que o tamanho do programa e é improvável que o tamanho do programa seja exatamente igual ao da partição alocada. Assim, é muito provável que ocorra desperdício de memória para cada programa alocado. Esse desperdício é chamado fragmentação interna, ou seja, perde-se memória dentro do espaço alocado ao processo.

### Há outra possibilidade de desperdício de espaço de memória, chamada de fragmentação externa. Ela ocorre quando se desperdiça memória fora do espaço ocupado por um processo. Por exemplo, suponha que há duas partições disponíveis e o processo necessita de uma partição de tamanho maior que qualquer uma das duas livres, e ainda, menor que o total de memória livre, somando-se o tamanho de todas partições livres. Neste caso, o processo não será executado devido ao esquema que a memória é gerenciada, mesmo que exista memória total livre disponível.

### Outra desvantagem de classificar os programas em entradas separadas é apresentada quando uma fila para uma partição grande está vazia e filas para partições pequenas estão muito cheias. Uma possível alternativa é colocar todos os programas em uma única fila de entrada e sempre que uma partição encontrar-se livre, alocar para o próximo programa da fila. Para não desperdiçar espaço pode-se realizar uma pesquisa para selecionar o programa que melhor se ajuste ao tamanho da partição. No entanto, isto pode deixar programas pequenos de fora, o que também é indesejável. Neste caso, é interessante dispor de pelo menos uma partição pequena para programas pequenos ou criar uma regra que limite o número de vezes que um programa pode ser ignorado, obrigando que o mesmo seja selecionado em um determinado momento.

### Dois conceitos importantes devem ser introduzidos quando há ocorrência de multiprogramação: realocação e proteção.

### Programas (jobs) diferentes são colocados em endereços diferentes (partições). Quando um programa é vinculado, o linkeditor deve saber em que endereço o programa deve começar na memória. Como o gerenciador de memória só decide em qual partição (endereço) o programa vai executar quando este chegar, não há garantia sobre em qual partição um programa vai realmente ser executado. Este problema é conhecido como realocação: modificação dos endereços especificados dentro do programa de acordo com a partição onde ele foi colocado.

### Outra questão importante está na proteção: programas diferentes não podem ter acesso a dados e/ou instruções fora de sua partição. Sem esta proteção, a construção de sistemas maliciosos seria facilitada. Uma possível alternativa para ambos os problemas é equipar a máquina com dois registradores especiais de hardware chamados registradores de base (RB) e registradores de limite (RL). Quando um processo é agendado, o registrador de base é carregado com o endereço de início de sua partição e o registrador de limite com o comprimento de sua partição. Assim, instruções são verificadas em relação ao seu endereço de armazenamento e para verificar se o mesmo não está fora da partição. O hardware protege os RB e RL para impedir que programas de usuário os modifiquem.

## Programação Concorrente

### A programação concorrente foi usada inicialmente na construção de sistemas operacionais. Atualmente, ela é usada para desenvolver aplicações em todas as áreas da computação. Este tipo de programação tornou-se ainda mais importante com o advento dos sistemas distribuídos e das máquinas com arquitetura paralela. Neste capítulo serão apresentados os conceitos básicos e os mecanismos clássicos da programação concorrente.

### A grande maioria dos programas escritos são programas seqüenciais. Nesse caso, existe somente um fluxo de controle (fluxo de execução, linha de execução, thread) no programa. Isso permite, por exemplo, que o programador realize uma "execução imaginária" de seu programa apontando com o dedo, a cada instante, o comando que está sendo executada no momento.

### Um programa concorrente pode ser visto como se tivesse vários fluxos de execução. Para o programador realizar agora uma "execução imaginária", ele vai necessitar de vários dedos, um para cada fluxo de controle.

### A programação concorrente é mais complexa que a programação seqüencial. Um programa concorrente pode apresentar todos os tipos de erros que aparecem nos programas seqüenciais e, adicionalmente, os erros associados com as interações entre os processos. Muitos erros dependem do exato instante de tempo em que o escalonador do sistema operacional realiza um chaveamento de contexto. Isso torna muitos erros difíceis de reproduzir e de identificar.

### Apesar da maior complexidade, existem muitas áreas nas quais a programação concorrente é vantajosa. Em sistemas nos quais existem vários processadores (máquinas paralelas ou sistemas distribuídos), é possível aproveitar esse paralelismo e acelerar a execução do programa. Mesmo em sistemas com um único processador, existem razões para o seu uso em vários tipos de aplicações.


