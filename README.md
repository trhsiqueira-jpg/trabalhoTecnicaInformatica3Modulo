# Trabalho Tecnico de Informatica 3 Modulo #
# Sistemas Operacionais #
![sistemas](https://blog.winco.com.br/wp-content/uploads/2018/02/sistemas-operacionais.jpg)

[Sistemas Operacionais](https://www.ibm.com/br-pt/think/topics/operating-systems)

Em termos técnicos, o sistema operacional é o software principal que gerencia todos os recursos de um computador ou dispositivo móvel, servindo de ponte entre o hardware e o usuário.

# O Kernel 

*  O que é o Kernel e qual a sua função Primordial na arquitetura de um SO?

   
O Kernel é a camada de software mais baixa e fundamental de um sistema operacional. Ele é o primeiro programa a ser carregado na memória após o boot e permanece lá até o computador ser desligado.
A Função Primordial: O Grande Tradutor
A função primordial do Kernel é servir como a ponte de comunicação entre o hardware e o software.

O hardware (processador, memória, disco) fala "eletricidade e binário", enquanto os aplicativos falam "lógica de alto nível". O Kernel traduz as solicitações dos programas em instruções que o hardware entende, garantindo que tudo funcione sem conflitos.

* Qual a diferença entre o gerenciamento de processos e gerenciamento de threads?

Um processo é uma instância de um programa em execução. Ele é uma unidade "pesada" porque o Sistema Operacional (através do Kernel) aloca um espaço de memória exclusivo para ele.

* Isolamento: Cada processo tem seu próprio endereço de memória. Se o Processo A travar, o Processo B continua rodando normalmente.

* Recursos: Possui seus próprios arquivos abertos, variáveis globais e pilha de dados.

* Custo: Criar ou trocar de um processo para outro (troca de contexto) é "caro" para a CPU, pois exige salvar e carregar todo esse estado isolado.

# O que seria uma thread?

Uma thread é a menor unidade de processamento que pode ser executada. Ela vive dentro de um processo. Um único processo pode ter múltiplas threads rodando simultaneamente (Multithreading).

* Compartilhamento: Todas as threads de um mesmo processo compartilham o mesmo espaço de memória e os mesmos recursos.

* Colaboração: Como compartilham a memória, elas se comunicam de forma muito rápida.

* Risco: Se uma thread cometer um erro grave e corromper a memória comum, ela pode derrubar o processo inteiro (a "fábrica" fecha por causa de um operário).

* Eficiência: São "leves". Criar uma thread é muito mais rápido do que criar um processo novo.

O Kernel gerencia ambos através do Escalonador. Ele decide qual thread ou processo vai usar o processador agora. Em processadores modernos com vários núcleos ($multicore$), o Kernel pode enviar diferentes threads de um mesmo processo para núcleos diferentes, fazendo com que o programa rode muito mais rápido.

# Como funciona o mecanismo de memoria virtual e qual o papel do "Paging" (Paginação) nesse processo?



##  O que é Memória Virtual?

A *memória virtual* é um mecanismo que permite que cada processo tenha a impressão de possuir *toda a memória do sistema disponível*, mesmo que a memória física (RAM) seja limitada.

Ela cria um *espaço de endereçamento virtual isolado* para cada processo.

### 🎯 Objetivos da Memória Virtual:

- 🔐 Isolamento entre processos
- 💾 Melhor aproveitamento da RAM
- 📦 Execução de programas maiores que a memória física
- ⚙️ Gerenciamento eficiente de memória

---

## 🏗️ Como Funciona?

Quando um programa acessa um endereço de memória:

1. Ele gera um *endereço virtual*
2. A CPU consulta a *MMU (Memory Management Unit)*
3. A MMU traduz o endereço virtual → endereço físico
4. A RAM é acessada

---


# O principal papel da Paginação (Paging)


*Paging (Paginação)* é a técnica usada para implementar memória virtual.

Ela divide:

- 📦 Memória virtual → em *páginas*
- 💾 Memória física → em *frames (quadros)*

Todas as páginas e frames possuem *tamanho fixo*.

---

##  Estrutura da Paginação

mermaid
flowchart TB
    subgraph Memória Virtual
        P1[Página 0]
        P2[Página 1]
        P3[Página 2]
    end

    subgraph Memória Física
        F1[Frame 5]
        F2[Frame 2]
        F3[Frame 9]
    end

    P1 --> F2
    P2 --> F1
    P3 --> F3


⚠️ As páginas *não precisam estar em ordem na memória física*.

---


# 🎯 Por que Paging é importante?

✅ Evita fragmentação externa  
✅ Permite multiprogramação  
✅ Permite uso de memória secundária (swap)  
✅ Garante isolamento entre processos  
✅ Melhora o uso da memória física  

---

# 🔎 Resumo Final

| Conceito | Função |
|----------|--------|
| Memória Virtual | Cria espaço de endereçamento isolado |
| MMU | Traduz endereços virtuais para físicos |
| Paging | Divide memória em páginas e frames |
| Page Table | Mapeia páginas para frames |
| Page Fault | Interrupção quando página não está na RAM |

---

# Defina o conceito de _"Deadlock"_ e cite as quatro condições necessárias para que ele ocorra.

O Deadlock (ou "impasse") é uma situação crítica no gerenciamento de processos onde um grupo de processos fica permanentemente impedido de prosseguir.

Isso acontece quando o Processo A está segurando um recurso que o Processo B precisa, enquanto o Processo B segura um recurso que o Processo A precisa. Ambos ficam esperando um ao outro indefinidamente, e nenhum deles libera o que já tem.


Imagine dois carros em uma rua estreita, um de frente para o outro: nenhum pode avançar porque o outro está no caminho, e nenhum pode voltar.


## As 4 Condições de _Coffman_ Para que um Deadlock ocorra, as quatro condições abaixo devem estar presentes simultaneamente.

Se você quebrar qualquer uma delas, o Deadlock é evitado

*  1 Exclusão Mútua (Mutual Exclusion)Pelo menos um recurso deve ser mantido em modo não compartilhável. Ou seja, apenas um processo pode usar o recurso por vez (ex: uma impressora ou um arquivo de escrita)
  
*  2 Posse e Espera (Hold and Wait)Um processo deve estar segurando pelo menos um recurso e, ao mesmo tempo, estar esperando por recursos adicionais que estão sendo ocupados por outros processos.
  
*  3 Inexistência de Preempção (No Preemption)O Kernel não pode "roubar" ou forçar a liberação de um recurso de um processo. O recurso só pode ser liberado voluntariamente pelo processo que o detém após ele completar sua tarefa.
  
*  4 Espera Circular (Circular Wait)Deve existir uma cadeia fechada de processos $\{P1, P2, ..., Pn\}$, onde $P1$ espera por um recurso de $P2$, $P2$ espera por $P3$, e assim por diante, até que $Pn$ esteja esperando por um recurso de $P1$.

# Qual a diferença fundamental entre sistemas de arquivos NTFS (Windows) e EXT4 (Linux) ?

* A diferença fundamental entre o NTFS (New Technology File System) e o EXT4 (Fourth Extended Filesystem) reside na arquitetura de metadados e na forma como lidam com a fragmentação e a segurança.

Enquanto o NTFS foi desenhado para ser robusto em ambientes corporativos Windows, o EXT4 foca na eficiência e no desempenho do ecossistema Linux.

## 1. Estrutura de Gerenciamento: MFT vs. Inodes
NTFS (Master File Table - MFT): Utiliza uma "tabela mestra" que funciona como um banco de dados relacional. Tudo no NTFS é um arquivo, inclusive a própria tabela MFT. Se a MFT for corrompida, o sistema tem sérios problemas para localizar os dados.

EXT4 (Inodes): Utiliza uma estrutura de Inodes (nós de índice). O sistema reserva um espaço fixo para os metadados (quem é o dono, permissões, onde estão os blocos) e os separa dos dados reais. É extremamente eficiente para lidar com milhões de arquivos pequenos.

## 2. Fragmentação: Onde a filosofia muda
A forma como eles escrevem no disco define se você precisará "desfragmentar" o PC ou não:

NTFS: Tenta escrever os dados no primeiro espaço vazio que encontra. Com o tempo, um arquivo grande acaba espalhado por vários "buracos" no disco, causando lentidão e exigindo desfragmentação.

EXT4: Utiliza Alocação Retardada (Delayed Allocation). Ele espera o máximo possível antes de gravar no disco para encontrar um bloco contínuo de espaço. Além disso, ele espalha os arquivos pelo disco propositalmente para que eles tenham espaço para "crescer" sem esbarrar uns nos outros. Por isso, sistemas Linux raramente precisam ser desfragmentados.

## 3. Sensibilidade a Maiúsculas (Case Sensitivity)
Esta é a diferença que mais confunde usuários que alternam entre sistemas:

NTFS: Geralmente não diferencia maiúsculas de minúsculas (Case-Insensitive). No Windows, você não pode ter Arquivo.txt e arquivo.txt na mesma pasta.

EXT4: É estritamente sensível (Case-Sensitive). Para o Linux, Arquivo.txt e arquivo.txt são dois objetos completamente diferentes e podem coexistir no mesmo diretório.

# O que são chamadas de sistema (System Calls) e por que elas são necessarias para as aplicações ?

System Calls (Chamadas de Sistema)* são mecanismos que permitem que um programa em modo usuário solicite serviços ao sistema operacional.

Elas funcionam como uma *ponte entre a aplicação e o kernel* do sistema operacional.

Em sistemas como Linux, Windows e macOS, as aplicações *não acessam diretamente o hardware*. Em vez disso, elas utilizam system calls para solicitar operações como:

- 📂 Acesso a arquivos
- 💾 Alocação de memória
- 🖥️ Criação de processos
- 🌐 Comunicação em rede
- ⏱️ Controle de tempo

---

##  Como funciona?

1. A aplicação executa uma função (ex: open(), read(), write()).
2. Essa função gera uma *interrupção controlada*.
3. O processador muda do *modo usuário* para o *modo kernel*.
4. O kernel executa a operação solicitada.
5. O resultado é retornado para a aplicação.

---

## 🔐 Por que as System Calls são necessárias?

### 1️⃣ Segurança
Aplicações não podem acessar diretamente hardware ou memória crítica.  
O kernel controla esse acesso para evitar falhas e ataques.

### 2️⃣ Abstração de Hardware
O programador não precisa saber como o disco ou a memória funcionam fisicamente.

### 3️⃣ Gerenciamento de Recursos
O sistema operacional controla:
- Uso da CPU
- Memória
- Dispositivos
- Processos

### 4️⃣ Estabilidade
Evita que um programa comprometa todo o sistema.



* Alunos

Thalles rafael dias de siqueira 

Anna Flavia



