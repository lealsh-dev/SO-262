## Atividade
Execute o algoritmo dos Banqueiros definido na aula anterior. 

Gere arquivos que evidenciam a execução no GitHub na pasta atividade-07 como definido abaixo:
1) Explicando cada teste proposto e sua saída gerada;
2) Print da execução de cada Teste.
3) Identifique o seu nome nos prints.

## Explicação dos Testes
### Teste 1:
O objetivo do Teste 1 é demonstrar o comportamento do algoritmo ao processar e aprovar um pedido que não compromete a estabilidade do sistema.
Para isso, processo P1 solicita o vetor de recursos [1, 0, 2]. Assim, o algoritmo valida que a quantia pedida não ultrapassa a necessidade máxima declarada por P1 nem os recursos físicos disponíveis no momento.
Em seguida, ele realiza uma alocação provisória e simula a execução do sistema. Ao confirmar que o cenário resultante é seguro, ou seja, após encontrar uma sequência onde todos os processos conseguem finalizar, a requisição é concedida.

### Teste 2:
O objetivo do Teste 2 é demonstrar o mecanismo de proteção imediato do sistema ao barrar um pedido que não possui recursos físicos suficientes para ser atendido no momento, evitando travamentos.
Para isso, o processo P4 solicita o vetor de recursos [3, 3, 0]. Desse modo, o algoritmo compara os recursos solicitados com o vetor de disponíveis atualizado após o Teste 1, que passou a ser [2, 3, 0]. Como P4 exige 3 unidades do recurso R0 e o sistema só possui 2, a validação de disponibilidade falha instantaneamente. Por isso, o algoritmo sequer tenta fazer a alocação provisória, e o processo é bloqueado (colocado em espera) até que outros processos liberem recursos.
