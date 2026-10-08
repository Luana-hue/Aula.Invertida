<div align="center">

### CST EM ENGENHARIA DE SOFTWARE

<br><br><br>

**JÉSSICA CAROLINO LIMONGE – 24529342-2**  
**LUANA TAINA CAMARGO BUENO – 25349593-2**

<br><br><br><br>

# DESAFIO PRÁTICO DE SALA DE AULA INVERTIDA:
# ARQUITETURA E CONCORRÊNCIA

<br><br><br><br>

**LONDRINA**  
**2026**

</div>

---

<div align="center">

**JÉSSICA CAROLINO LIMONGE – 24529342-2**  
**LUANA TAINA CAMARGO BUENO – 25349593-2**

<br><br><br><br>

# DESAFIO PRÁTICO DE SALA DE AULA INVERTIDA:
# ARQUITETURA E CONCORRÊNCIA

</div>

<br><br><br>

<div align="right">

Desafio Prático de Sala de Aula Invertida,  
apresentado à disciplina de Sistemas  
Operacionais do Curso de Engenharia de  
Software para obtenção parcial de nota  
semestral.

</div>

<br><br><br>

<div align="center">

**LONDRINA**  
**2026**

</div>

---

## 1. Síntese Teórica

### 1.1 Processo e Thread

Um processo pode ser entendido como um programa que está sendo executado e que possui recursos próprios para funcionar, como espaço de memória e arquivos abertos. Já uma thread é uma linha de execução que existe dentro de um processo.

Um mesmo processo pode ter várias threads realizando tarefas diferentes. Nesse caso, elas compartilham o mesmo espaço de endereçamento e os recursos do processo, mas cada thread mantém algumas informações próprias, como registradores, contador de programa e pilha.

Essa diferença ajuda a entender por que criar threads é mais leve do que criar vários processos. Quando um novo processo é criado, é necessário preparar uma estrutura independente para ele. Já as threads conseguem aproveitar recursos que pertencem ao processo em que estão sendo executadas, principalmente o espaço de memória compartilhado.

### 1.2 Threads em Modo Usuário e Modo Núcleo

As threads podem ser gerenciadas de formas diferentes. Nas **threads em Modo Usuário**, o controle é feito no espaço do próprio programa, normalmente por uma biblioteca ou pelo ambiente de execução. Nesse caso, o kernel não acompanha diretamente cada thread e trabalha principalmente com o processo.

Nas **threads em Modo Núcleo**, o próprio Sistema Operacional conhece e controla cada thread. O kernel pode escolher diretamente qual thread será executada e também controlar o tempo de CPU destinado a ela.

Uma diferença importante é que, no modo núcleo, se uma thread ficar bloqueada esperando uma operação de entrada e saída, outras threads do mesmo processo ainda podem continuar sendo executadas. Por outro lado, esse gerenciamento possui um custo maior, pois envolve diretamente o kernel.

## 2. Diagnóstico do Problema

### 2.1 Condição de Corrida no Assento A-15

Uma **condição de corrida**, também chamada de *Race Condition*, acontece quando duas ou mais execuções acessam um mesmo recurso compartilhado ao mesmo tempo e o resultado acaba dependendo da ordem em que essas operações acontecem.

No sistema de ingressos, isso pode ocorrer quando o Usuário A e o Usuário B clicam em **Comprar** praticamente no mesmo instante para o Assento A-15.

Uma situação possível seria:

<img width="449" height="402" alt="image" src="https://github.com/user-attachments/assets/d017700c-1db8-4863-8971-4445275a560b" />



Se não existir nenhum controle para esse acesso simultâneo, o sistema pode acabar registrando duas compras para o mesmo assento. Isso causaria uma inconsistência nos dados e dois clientes poderiam receber a confirmação de compra para o mesmo lugar.

O problema acontece porque a informação do Assento A-15 é um recurso compartilhado e está sendo consultada e alterada por mais de uma execução ao mesmo tempo.

## 3. Solução Arquitetural

### 3.1 Exclusão Mútua e Região Crítica

Para evitar a condição de corrida, a parte do sistema responsável por verificar e alterar a situação do assento pode ser tratada como uma **região crítica**.

A região crítica é a parte do programa em que ocorre o acesso a um recurso compartilhado. Nesse caso, o recurso compartilhado é a informação que indica se o Assento A-15 está disponível ou vendido.

Para controlar esse acesso pode ser utilizada a **exclusão mútua**, que permite que apenas uma execução por vez entre nessa região crítica.

No caso do Assento A-15, o funcionamento seria:

<img width="240" height="575" alt="image" src="https://github.com/user-attachments/assets/3982744d-d502-4de6-8247-b96e90ad2eaa" />

Um mecanismo que pode ser utilizado para fazer esse controle é o **Mutex**, que funciona como uma trava para impedir que duas execuções alterem o mesmo recurso compartilhado ao mesmo tempo.

O importante nesse caso é proteger tanto a verificação quanto a atualização do assento. Assim, uma segunda compra só pode continuar depois que a primeira terminar sua operação sobre aquele recurso.

Dessa forma, a exclusão mútua evita que o mesmo assento seja vendido duas vezes e ajuda a manter as informações do sistema consistentes mesmo quando existem vários usuários acessando o sistema ao mesmo tempo.

## 4. Referências

MAZIERO, Carlos Alberto. **Sistemas operacionais: conceitos e mecanismos**. Curitiba: Editora da UFPR, 2019. 456 p. Disponível em: https://wiki.inf.ufpr.br/maziero/lib/exe/fetch.php?media=socm:socm-livro.pdf. Acesso em: 8 out. 2026.

COUTO, Rodrigo de Souza. **EEL770 - 19 - Introdução aos problemas de concorrência**. YouTube, 5 out. 2020. 1 vídeo. Disponível em: https://www.youtube.com/watch?v=ohCMTCfUBUs. Acesso em: 8 out. 2026.

CAFÉ DEBUG. **#167 Threads, Paralelismo e SO na Prática para Devs**. Café Debug seu podcast de tecnologia, 14 jul. 2025. Podcast. Disponível em: https://cafedebug.com.br/detalhes-epis%C3%B3dio?guid=16eec9ae-a6c4-4d28-9919-a773ee3c8738. Acesso em: 8 out. 2026.
