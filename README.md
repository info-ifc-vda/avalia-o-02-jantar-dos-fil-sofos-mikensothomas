1. Garfo como Semáforo:
Cada garfo é representado por um semáforo com valor 1 **threading.Semaphore(1)**, garantindo que apenas um filósofo possa pegar um determinado garfo por vez. O filósofo precisa pegar dois garfos (o da esquerda e o da direita) para começar a comer.

2. Controle dos Filósofos que Podem Comer:
Para resolver problemas de deadlock e starvation, os filósofos que podem comer ao mesmo tempo são divididos em grupos. Apenas filósofos de um grupo podem comer em um dado momento. A alternância entre os grupos é feita através da função alternar_grupo(), que rotaciona entre três grupos diferentes de filósofos (grupo_comendo0 [0, 2], `grupo_comendo1 [1, 3], grupo_comendo2 [4, 2]).

3. Exclusão Mútua:
O objeto bloqueio (um lock) é utilizado para controlar a alternância entre os grupos e garantir que o acesso à variável compartilhada grupo_comendo seja feito de forma segura, sem que múltiplos filósofos tentem acessar ou modificar essa variável ao mesmo tempo.

Descrição do Funcionamento:
Ciclo de Vida dos Filósofos:
Cada filósofo alterna entre pensar e comer:

Pensar: O filósofo simula o ato de pensar por um tempo (time.sleep(tempo_de_pensar)).
Verificação do Grupo: O filósofo só pode tentar pegar os garfos se ele fizer parte do grupo ativo no momento (grupo_comendo). Isso é controlado pelo **_while True_** interno que verifica se ele pertence ao grupo permitido a comer. O **_bloqueio_** garante que essa verificação e troca de grupo sejam feitas de maneira segura.
2. Pegando os Garfos:
O filósofo tenta pegar dois garfos:

O garfo à sua esquerda é o garfo de sua posição (garfos[posicao]).
O garfo à sua direita é o garfo da próxima posição, calculado de forma circular com (posicao + 1) % NUM_FILOSOFOS.
Ele adquire os dois garfos usando garfo_esquerdo.acquire() e garfo_direito.acquire(), garantindo que ele tenha ambos antes de começar a comer.

3. Comendo e Liberando os Garfos:
Após adquirir os garfos, o filósofo come por um período time.sleep(tempo_de_comer). Em seguida, ele libera os garfos com garfo_esquerdo.release() e garfo_direito.release(), permitindo que outros filósofos possam usá-los.

4. Alternância dos Grupos:
Após terminar de comer, o filósofo aciona o mecanismo de troca de grupo através da função alternar_grupo(), controlada por um bloqueio (bloqueio), garantindo que o próximo grupo de filósofos tenha a chance de comer.

Como a Solução Resolve os Problemas:

1. Deadlock:
O deadlock é evitado pela estratégia de dividir os filósofos em grupos. Apenas os filósofos que pertencem ao grupo ativo podem pegar os garfos. Isso impede que todos os filósofos tentem pegar garfos ao mesmo tempo, eliminando o risco de cada filósofo ficar esperando indefinidamente que o próximo libere um garfo.

2. Starvation:
O problema da starvation é mitigado pela alternância justa entre os grupos. A função alternar_grupo() rotaciona entre três grupos, o que garante que todos os filósofos tenham a oportunidade de comer periodicamente.

Considerações Finais:
A solução utiliza uma combinação de semáforos para controlar os garfos e um mecanismo de alternância de grupos para evitar que múltiplos filósofos entrem em deadlock ou fiquem famintos. A implementação com grupos garante que o acesso aos recursos seja ordenado e equitativo, enquanto os semáforos gerenciam o uso dos garfos de forma segura.[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/-Z5ovbbf)
