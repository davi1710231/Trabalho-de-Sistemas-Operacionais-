# Trabalho-de-Sistemas-Operacionais-

---

# Documentação — Trabalho Prático 1: Comunicação e Sincronização de Processos e Threads

**Disciplina:** Sistemas Operacionais

**Curso:** Engenharia de Software — PUC Minas

---

## 🛠️ Tecnologias e Conceitos Utilizados

* **Linguagem:** Java (JDK 8 ou superior)
* **Concorrência:** Programação multithread nativa (`Thread`)
* **Sincronização:** Semáforos da biblioteca `java.util.concurrent.Semaphore`
* **Prevenção de Deadlocks e Condições de Corrida:** Exclusão mútua em regiões críticas com semáforos e sem o uso de espera ocupada (*busy wait*).

---

## 💈 Letra b) Problema do Barbeiro Dorminhoco

### 1. Descrição do Problema

O problema consiste em simular a operação de uma barbearia com **1 barbeiro**, **1 cadeira de atendimento** e **$N$ cadeiras na sala de espera**.

* Se não houver clientes, o barbeiro dorme.


* Quando chega um cliente e o barbeiro está dormindo, o cliente o acorda e senta-se na cadeira de corte.


* Se chegam outros clientes enquanto o barbeiro está ocupado:
* Sentam em uma das cadeiras vagos na sala de espera.


* Se todas as cadeiras da sala de espera estiverem ocupadas, o cliente vai embora sem cortar o cabelo.





### 2. Modelagem e Estrutura

* **Threads:**
* `Barbeiro`: Executa em loop infinito, bloqueando-se para dormir quando não há clientes e acordando para atender quando recebe a sinalização.


* `Cliente`: Tenta entrar na barbearia e verifica se há espaço livre na sala de espera.




* **Recursos Compartilhados e Região Crítica:**
* `waiting`: Variável inteira que armazena a quantidade atual de clientes na sala de espera. A leitura e modificação dessa variável é uma **região crítica** e deve ser protegida por exclusão mútua.




* **Mecanismos de Sincronização (Semáforos):**
* `mutex` (`Semaphore(1)`): Garante acesso exclusivo à variável compartilhada `waiting`.


* `customers` (`Semaphore(0)`): Conta quantos clientes estão aguardando na barbearia. Bloqueia o barbeiro até que um cliente chegue.


* `barbers` (`Semaphore(0)`): Indica a disponibilidade do barbeiro para atender um cliente que está na sala de espera.





---

## 📚 Letra c) Problema dos Leitores e Escritores

### 1. Descrição do Problema

Várias threads (tarefas concorrentes) tentam acessar uma área de memória compartilhada (um vetor de inteiros $M$) para realizar leituras ou escritas:

* **Leitores:** Podem ler o vetor simultaneamente em paralelo, pois leituras concorrentes não alteram os dados e não causam conflito.


* **Escritores:** Precisam de acesso **exclusivo** ao vetor $M$, pois alterações simultâneas (ou durante leituras) geram condições de corrida (*race conditions*) e corrupção de dados.



### 2. Modelagem e Estrutura

* **Threads:**
* `Leitor`: Acessa dados no vetor $M$ em paralelo com outros leitores.
* `Escritor`: Altera valores em posições do vetor $M$ com acesso exclusivo.


* **Recursos Compartilhados e Região Crítica:**
* `M`: Vetor de inteiros compartilhado (`int[] M = {3, 7, 1, 9}`).


* `readCount`: Variável de controle do número de leitores ativos na área compartilhada.


* **Mecanismos de Sincronização (Semáforos):**
* `mutex` (`Semaphore(1)`): Protege a atualização do contador de leitores (`readCount`).
* `db` (`Semaphore(1)`): Garante exclusão mútua para escrita no vetor $M$. O primeiro leitor a chegar adquire o semáforo `db` para impedir que escritores modifiquem os dados enquanto houver leituras ativas, e o último leitor a sair o libera.



---

### **Instruções para Teste e Execução**

1. Acesse qualquer compilador Java online (ex.: [OnlineGDB](https://www.onlinegdb.com/online_java_compiler) ou [JDoodle](https://www.jdoodle.com/online-java-compiler)).
2. Cole o código correspondente no editor.
3. Execute e verifique a saída no console:
* **Barbeiro Dorminhoco:** Observe os clientes lotando as cadeiras, o barbeiro acordando/dormindo e os clientes sendo dispensados quando as cadeiras estão ocupadas.


* **Leitores e Escritores:** Observe múltiplos leitores lendo sem bloquear uns aos outros e os escritores exigindo bloqueio exclusivo para modificação de dados.





---

Podemos prosseguir para a **letra d (Problema dos Fumantes)**?
