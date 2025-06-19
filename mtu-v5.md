# Máquina de Turing v5.0

## Anotações

- Verificar onde se encaixa o q38
- Verifica a falta do q76

## O que falta?

- [ ]  (1) — Tratar o caso de não existir transições válidas, ou seja, deve rejeitar a palavra, desmarcar toda a fita e escrever `#R` ao final da fita.
- [ ]  (2) — O estado é compatível com a memoria, agora deve verificar se o cabeçote da palavra está no `b` ou `□`
- [ ]  (3) — O estado é compatível com a memoria, agora deve verificar se o cabeçote da palavra esta no `$`, ou seja, se no lugar do `$` existe `!`
- [x]  (4) — Se encontrar `a` ou `b` indica que o símbolo de leitura da transição foi completamente lido. Tratar o fato de ter lido completamente o símbolo de leitura da transição.
- [x]  (5) — Se encontrar vazio, indica que a palavra pode esta vazia ou o cabeçote não esta em um símbolo `A`, ou seja, deve tratar o caso em que os símbolos não são compatíveis.
- [x]  (6) — Se encontrar `A`, indica que pode começar a verifica a semelhança entre o símbolo de leitura na transição e o símbolo da palavra apontada pelo cabeçote.
- [x]  (7) — Se encontrar `!`, indica que o cabeçote esta em s, ou seja, tratar o caso em que o símbolo não é compatível.
- [x]  (8) — Tratar o fato de o número de 1s não na palavra não ser compatível com o número de 1s do símbolo de leitura na transição em verificação
- [x]  (9) — Se encontrar a, b ou □ do lado da palavra após a completa leitura do lado da transição, deve tratar a escrita do símbolo na palavra.
- [x]  (10) — Se encontrar `b`, deve tratar a substituição de `a…` por `b` na palavra
- [x]  (11) — Se encontrar `a` indica que o número de `1`s na palavra ira aumentar, diminuir ou permanecer o mesmo.
- [ ]  (12) — Tratar o movimento do cabeçote para a direita
- [ ]  (13) — Tratar o movimento do cabeçote para a esquerda

## Máquina de Turing

| ❗ | Transições | Observações |
| --- | --- | --- |
|  | q0 ; q ; q ; R ; q1 | Verifica se a entrada começa com `q1`  |
|  | q1 ; 1 ; 1 ; R ; q2 | Verifica se a entrada começa com `q1`  |
|  | q2 ; [a, b] ; ~ ; R ; q3 | Verifica se a entrada começa com `q1`  |
|  | q3 ; !$ ; ~ ; R ; q3 | MARCAÇÃO DO CABEÇOTE. Procura `$` para marcar o cabeçote na palavra. |
|  | q3 ; $ ; $ ; R ; q4 |  |
|  | q4 ; a ; A ; L ; q5 | MARCAÇÃO DO CABEÇOTE. Marca o cabeçote no primeiro símbolo da palavra se ele existir |
|  | q4 ; □ ; □ ; L ; q5 | MARCAÇÃO DO CABEÇOTE. Não marca o cabeçote se a palavra estiver vazia |
|  | q5 ; !□ ; ~ ; L ; q5 | CRIANDO MEMORIA. Volta para o inicio da fita para criar a memoria |
|  | q5 ; □ ; @ ; L ; q6 | CRIANDO MEMORIA. Escreve pela primeira vez o `@` |
|  | q6 ; □ ; t ; L ; q7 | CRIANDO MEMORIA. Escreve pela primeira vez o `t` |
|  | q7 ; □ ; 1 ; R ; q8 | CRIANDO MEMORIA. Inicia a memoria com estado `q1` |
|  | q8 ; !q ; ~ ; R ; q8 | Volta para a primeira transição de entrada |
|  | q8 ; q ; q ; S ; q9 | Inicia a leitura do estado gravado na memoria |
|  | q9 ; !@ ; ~ ; L ; q9 | LENDO A MEMORIA. Procura o começo da memoria.  |
|  | q9 ; @ ; @ ; L ; q10 |  |
|  | q10 ; t ; T ; L ; q11 | LENDO A MEMORIA. Marca t como T para indicar que começou a ler a memoria.  |
|  | q10 ; T ; T ; L ; q11 | LENDO A MEMORIA. Se encontrar T no lugar de t indica que a memoria já foi lida pelo menos uma vez. |
|  | q11 ; 3 ; 3 ; L ; q11 | LENDO A MEMORIA. Passa por todos os 1s já lidos |
|  | q11 ; 1 ; 3 ; R ; q12 | LENDO A MEMORIA. Marca um 1 como lido |
|  | q11 ; □ ; □ ; R ; q16 | LENDO A MEMORIA. Se encontrar □, já terminou de ler a memoria. Deve verificar se não existe mais 1s na transição |
|  | q12 ; !@ ; ~ ; R ; q12 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Volta para o inicio da fita para procurar o estado que condiz com a memoria |
|  | q12 ; @ ; @ ; R ; q13 |  |
|  | q13 ; q ; Q ; R ; q14 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Marca q como Q para indicar qual transição esta sendo verificada. Deve verificar se existe ao menos um 1 depois do q |
|  | q13 ; Q ; Q ; R ; q15 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Q indica que a transição já esta sendo verificada. Deve passar por todos os 1s já lidos e procurar ao menos 1 um para marcar |
|  | q13 ; F ; F ; R ; q23 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Indica que a transição já foi verificada e invalidada. Deve ir para a para a próxima transição |
|  | q14 ; 1 ; 1 ; S ; q15 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Verificando a existência de ao menos um 1 depois do q |
|  | q15 ; 2 ; 2 ; R ; q15 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Passar por todos os 1s na transição que já foram lidos |
|  | q15 ; 1 ; 2 ; L ; q9 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Marcar um 1 na transição como lido |
|  | q15 ; a ; a ; L ; q18 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Encontrou a, indica que o estado não combina com a memoria. Deve voltar e desmarcar tudo e marcar essa transição com F |
|  | q15 ; b ; b ; L ; q18 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Encontrou b, indica que o estado não combina com a memoria. Deve voltar e desmarcar tudo e marcar essa transição com F |
|  | q15 ; s ; s ; L ; q18 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Encontrou s, indica que o estado não combina com a memoria. Deve voltar e desmarcar tudo e marcar essa transição com F |
|  | q16 ; !Q ; ~ ; R ; q16 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. Voltar para transição para verificar se existe ou não ao menos um 1 não lido. Se existir o estado não combina, se não, o estado combina com a memoria |
|  | q16 ; Q ; Q ; R ; q17 |  |
|  | q17 ; 2 ; 2 ; R ; q17 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. Percorrer todos os 1s lidos |
|  | q17 ; 1 ; 1 ; L ; q18 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. Estado não compatível, pois existe ao menos um `1` que não foi lido na transição. |
|  | q17 ; a ; A ; R ; q67 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. Estado compatível, agora deve verificar se o símbolo da transição (`a`) é compatível com o símbolo apontado pelo cabeçote na palavra.  |
|  | q17 ; b ; B ; R ; q32 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. Estado compatível, agora deve verificar se o símbolo da transição (`b`) é compatível com o símbolo apontado pelo cabeçote na palavra.  |
|  | q17 ; s ; S ; R ; q62 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. Estado compatível, agora deve verificar se o símbolo da transição (`s`) é compatível com o símbolo apontado pelo cabeçote na palavra. |
|  | q18 ; 2 ; 1 ; L ; q18 | MARCANDO O ESTADO COMO NÃO COMPATÍVEL. Desmarcar todos os 1s lidos. |
|  | q18 ; Q ; F ; L ; q19 | MARCANDO O ESTADO COMO NÃO COMPATÍVEL. Marcando o Q como F, para informar que a transição não é compatível com a memoria. |
|  | q19 ; !@ ; ~ ; L ; q19 | MARCANDO O ESTADO COMO NÃO COMPATÍVEL. Procurando a memoria para desmarcar a memoria já lida. |
|  | q19 ; @ ; @ ; L ; q20 |  |
|  | q20 ; T ; T ; L ; q21 | MARCANDO O ESTADO COMO NÃO COMPATÍVEL. Pula o T e deixa como T, pois ainda não achou o estado compatível. |
|  | q21 ; 3 ; 1 ; L ; q21 | MARCANDO O ESTADO COMO NÃO COMPATÍVEL. Desmarcar todos os 1s lidos na memoria |
|  | q21 ; 1 ; 1 ; R ; q22 | MARCANDO O ESTADO COMO NÃO COMPATÍVEL. Terminou de desmarcar a memoria, agora deve voltar e procurar a próxima transição |
|  | q21 ; □ ; □ ; R ; q22 | MARCANDO O ESTADO COMO NÃO COMPATÍVEL. Terminou de desmarcar a memoria, agora deve voltar e procurar a próxima transição |
|  | q22 ; !@ ; ~ ; R ; q22 | MARCANDO O ESTADO COMO NÃO COMPATÍVEL. Voltando para o inicio da memoria, para retorna a busca |
|  | q22 ; @ ; @ ; L ; q10 | MARCANDO O ESTADO COMO NÃO COMPATÍVEL. Retorna a busca pela a transição que combina com a memoria |
|  | q23 ; [a, q, 1, f, b, s, R, L] ; ~ ; R ; q23 | PROCURANDO A PRÓXIMA TRANSIÇÃO. Pula todos os símbolos possíveis em uma transição até achar #, % ou $ |
|  | q23 ; # ; # ; R ; q13 | PROCURANDO A PRÓXIMA TRANSIÇÃO. Indica que achou uma próxima transição. Deve retorna e verifica-la. |
|  | q23 ; $ ; $ ; R ; q24 | PROCURANDO A PRÓXIMA TRANSIÇÃO. Encontrando $, indica que não encontrou transições compatíveis, deve retornar limpando a entrada e rejeitar a palavra (#R)  |
|  | q23 ; % ; % ; R ; q24 | PROCURANDO A PRÓXIMA TRANSIÇÃO. Encontrando %, indica que não encontrou transições compatíveis, deve retornar limpando a entrada e rejeitar a palavra (#R)  |
|  | q24 ; !□ ; ~ ; R ; q24 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Indo para o final da fita do lado direito para poder desmarcar toda a fita. |
|  | q24 ; □ ; □ ; L ; q25 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Iniciando a limpeza da palavra. |
|  | q25 ; 4 ; 1 ; L ; q25 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Limpando os 1s escritos. |
|  | q25 ; 2 ; 1 ; L ; q25 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Limpando os 2s escritos. |
|  | q25 ; [a, b, A, B, 1] ; ~ ; L ; q25 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Pulando o que não deve ser mexido durante a limpeza. |
|  | q25 ; $ ; $ ; L ; q26 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Iniciar a limpeza das transações  |
|  | q25 ; % ; $ ; L ; q26 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Iniciar a limpeza das transações e deixar a marcação indicando que o cabeçote esta no `s`. |
|  | q26 ; [a, b, s, 1, q, f, R, L, #] ; ~ ; L ; q26 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Passando pelo que o não deve ser mexido durante a limpeza |
|  | q26 ; 2 ; 1 ; L ; q26 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Desmarcando os 1s lidos |
|  | q26 ; F ; q ; L ; q26 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Desmarcando as transição não compatíveis com a memoria |
|  | q26 ; Q ; q ; L ; q26 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. |
|  | q26 ; B ; b ; L ; q26 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA.  |
|  | q26 ; S ; s ; L ; q26 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA.  |
|  | q26 ; A ; a ; L ; q26 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA.  |
|  | q26 ; r ; R ; L ; q26 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA.  |
|  | q26 ; l ; L ; L ; q26 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA.  |
|  | q26 ; @ ; @ ; S ; q27 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Iniciar a limpeza da memoria da máquina |
|  | q27 ; @ ; □ ; L ; q27 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA.  |
|  | q27 ; T ; □ ; L ; q27 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA.  |
|  | q27 ; t ; □ ; L ; q27 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA.  |
|  | q27 ; 3 ; □ ; L ; q27 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA.  |
|  | q27 ; 1 ; □ ; L ; q27 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA.  |
|  | q27 ; □ ; □ ; R ; q28 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Finalizou a limpeza da memoria |
|  | q28 ; !q ; ~ ; R ; q28 | LIMPANDO A FITA PARA REJEIÇÃO DA PALAVRA. Voltando para o inicio da entrada. |
|  | q28 ; q ; q ; R ; q29 |  |
|  | q29 ; !□ ; ~ ; R ; q29 | MARCAR A PALAVRA COMO REJEITADA. Ir ao final da entrada para escrever #R no final |
|  | q29 ; □ ; # ; R ; q30 | MARCAR A PALAVRA COMO REJEITADA. Escrevendo o # ao final da entrada |
|  | q30 ; □ ; R ; R ; **`q31`**  | MARCAR A PALAVRA COMO REJEITADA. Escrevendo R ao final da entrada e direcionando para o estado final da MTU. |
|  | q32 ; !□ ; ~ ; R ; q32  | VERIFICADO SE O CABEÇOTE ESTÁ NO VAZIO NA PALAVRA. Ir pra o final da entrada para procurar a verificar a palavra da direita pela esquerda.   |
|  | q32 ; □ ; □ ; L ; q33 | VERIFICADO SE O CABEÇOTE ESTÁ NO VAZIO NA PALAVRA. |
|  | q33 ; [1, b, a] ; ~ ; L ; q33 | VERIFICADO SE O CABEÇOTE ESTÁ NO VAZIO NA PALAVRA. Até o momento é compatível, porém ainda não achou o cabeçote. |
|  | q33 ; $ ; $ ; L ; q35 | VERIFICADO SE O CABEÇOTE ESTÁ NO VAZIO NA PALAVRA. Indica que é compatível, já que o cabeçote esta no primeiro espaço após o $.  |
|  | q33 ; B ; B ; L ; q45 | VERIFICADO SE O CABEÇOTE ESTÁ NO VAZIO NA PALAVRA. É compatível, já que o cabeçote está em um b. Deve retorna a transição e começar a escrita.  |
|  | q33 ; % ; % ; L ; q34 | VERIFICADO SE O CABEÇOTE ESTÁ NO VAZIO NA PALAVRA. Indica que não é compatível, já que o cabeçote está no `s`. Deve retorna a transição e marca-la como `F`.  |
|  | q33 ; A ; A ; L ; q34 | VERIFICADO SE O CABEÇOTE ESTÁ NO VAZIO NA PALAVRA. Indica que não é compatível, já que o cabeçote está no `A`. Deve retorna a transição e marca-la como `F`.  |
|  | q34 ; !B ; ~ ; L ; q34 | MARCANDO A TRANSIÇÃO COMO NÃO COMPATÍVEL. Procurando o B para desmarca-lo e marcar a transição como não compatível. |
|  | q34 ; B ; b ; L ; q18 | MARCANDO A TRANSIÇÃO  COMO NÃO COMPATÍVEL. Demarcando a leitura o b, pois o símbolo não bate com o símbolo apontado pelo cabeçote na palavra.  |
|  | q35 ; !B ; ~ ; L ; q35 | ESCREVEDO O SÍMBOLO NO VAZIO. Procurando B para escrever o símbolo da transição no primeiro espaço após o $.  |
|  | q35 ; B ; B ; R ; q36 |  |
|  | q36 ; a ; A ; R ; q39 | ESCREVEDO O SÍMBOLO NO VAZIO. Achou a, indica que deve escrever o símbolo a seguidos de ao menos um 1 no vazio após a palavra.  |
|  | q36 ; b ; B ; R ; q37 | ESCREVEDO O SÍMBOLO NO VAZIO. Achou b, indica que deve escrever o símbolo b no vazio após a palavra. |
|  | q37 ; !□ ; ~ ; R ; q37 | ESCREVEDO O SÍMBOLO NO VAZIO. Procurando pelo espaço vazio após o $ para escrever B. |
|  | q37 ; □ ; B ; L ; q54 | ESCREVEDO O SÍMBOLO NO VAZIO. Escrevendo B no primeiro espaço vazio após o $. Deve retorna a transição e verificar o movimento que deve fazer com o cabeçote.  |
|  | **q38 ; 1 ; 1 ; S ; q39** | ESCREVENDO SÍMBOLO NO VAZIO. Verificando se existe ao menos um 1 depois do a.  |
|  | q39 ; !□ ; ~ ; R ; q39 | ESCREVENDO SÍMBOLO NO VAZIO. Procurando o primeiro vazio depois da palavra para escrever o A na palavra |
|  | q39 ; □ ; A ; L ; q40 | ESCREVENDO SÍMBOLO NO VAZIO. Escrevendo o A na palavra |
|  | q40 ; !A ; ~ ; L ; q40 | ESCREVENDO SÍMBOLO NO VAZIO. Procurando o símbolo que deve ser escrito na palavra na transição. |
|  | q40 ; A ; A ; R ; q41 |  |
|  | q41 ; 2 ; 2 ; R ; q41 | ESCREVENDO SÍMBOLO NO VAZIO. Pula todos os 1s já escritos. |
|  | q41 ; 1 ; 2 ; R ; q42 | ESCREVENDO SÍMBOLO NO VAZIO. Marca um 1 que vai ser escrito na palavra. |
|  | q41 ; [R, L] ; ~ ; S ; q44 | ESCREVENDO SÍMBOLO NO VAZIO. Indica que terminou a escrita. Deve lidar com o movimento que o cabeçote deve realizar. Porém antes, deve desmarcar o 1s escritos na palavra. |
|  | q42 ; !□ ; ~ ; R ; q42 | ESCREVENDO SÍMBOLO NO VAZIO. Procurando o primeiro espaço vazio para escrever o 1 na palavra.  |
|  | q42 ; □ ; 4 ; L ; q43 | ESCREVENDO SÍMBOLO NO VAZIO. Escrevendo o 1 na palavra.  |
|  | q43 ; !2 ; ~ ; L ; q43 | ESCREVENDO SÍMBOLO NO VAZIO. Procurando o 1 já escrito da transição |
|  | q43 ; 2 ; 2 ; S ; q41 | ESCREVENDO SÍMBOLO NO VAZIO. Voltando para o loop de leitura e escrita.  |
|  | q44 ; R ; r ; R ; q82 | MOVIMENTAR O CABEÇOTE PARA A DIREITA. O cabeçote deve ir para a direita.  |
|  | q44 ; L ; l ; R ; q87 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. O cabeçote deve ir para a esquerda.  |
|  | q45 ; !B ; ~ ; L ; q45 | ESCREVENDO O SÍMBOLO X EM CIMA DO B. Procurando o símbolo que de ser escrito que deve ser escrita.   |
|  | q45 ; B ; B ; R ; q46 |  |
|  | q46 ; b ; B ; R ; q44 | ESCREVENDO O SÍMBOLO X EM CIMA DO B. Encontrou b, ou seja, não precisa escrever, pois ira manter o que já existe na palavra. |
|  | q46 ; a ; A ; R ; q47 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Encontrou um A, ou seja deve escrever A em cima do B, seguidos de 1s necessários.  |
|  | q47 ; !B ; ~ ; R ; q47 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Procura B na palavra para substituir por A |
|  | q47 ; B ; A ; L ; q48 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Substitui o B por A na palavra.  |
|  | q48 ; !A ; ~ ; L ; q48 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Procura o símbolo que deve ser escrito da transição |
|  | q48 ; A ; A ; R ; q49 |  |
|  | q49 ; 1 ; 1 ; S ; q50 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Verifica se existe ao menos um 1 depois do a.  |
|  | q50 ; 1 ; 2 ; R ; q51 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Lê o 1 do símbolo que deve ser escrito na palavra |
|  | q50 ; [R, L] ; ~ ; S ; q44 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. A escrita foi concluída, agora deve desmarcar os 1s na palavra e depois realizar o movimento do cabeçote.  |
|  | q51 ; !A ; ~ ; R ; q51 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Procura o A na palavra |
|  | q51 ; A ; A ; R ; q52 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Encontrou A na palavra. Agora deve verificar o que tem do lado, se é necessário deslocar o resto da palavra para a direita. |
|  | q52 ; 4 ; 4 ; R ; q52 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Passa por todos os 1s já escritos. |
|  | q52 ; [a, b] ; ~ ; R ; q55 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. O resto da palavra deve ser deslocada para a direita para abrir espaço para a escrita.  |
|  | q52 ; □ ; 4 ; L ; q53 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Encontrou vazio, ou seja, pode escrever e deve voltar para o loop de leitura e escrita do símbolo |
|  | q53 ; !2 ; ~ ; L ; q53 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Voltar para a transição para terminar de ler a palavra que deve ser escrita. |
|  | q53 ; 2 ; 2 ; R ; q50 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Voltar para a transição para terminar de ler a palavra que deve ser escrita. |
|  | q54 ; !B ; ~ ; L ; q54 | VERIFICANDO O DESLOCAMENTO DO CABEÇOTE  |
|  | q54 ; B ; B ; R ; q44 | VERIFICANDO O DESLOCAMENTO DO CABEÇOTE |
|  | q55 ; !□ ; ~ ; R ; q55 | DESLOCAR A PALAVRA PARA A DIREIRA. Procurando o final da palavra.    |
|  | q55 ; □ ; □ ; L ; q56 | DESLOCAR A PALAVRA PARA A DIREIRA. Achou o final da palavra |
|  | q56 ; 1 ; □ ; R ; q57 | DESLOCAR A PALAVRA PARA A DIREIRA. Lendo o símbolo 1, apagando e indo para a direita |
|  | q56 ; b ; □ ; R ; q58 | DESLOCAR A PALAVRA PARA A DIREIRA. Lendo o símbolo b, apagando e indo para a direita |
|  | q56 ; a ; □ ; R ; q59 | DESLOCAR A PALAVRA PARA A DIREIRA. Lendo o símbolo a, apando e indo para a direita |
|  | q56 ; A ; A ; R ; q60 | DESLOCAR A PALAVRA PARA A DIREIRA. Achou o símbolo A, ou seja, já abriu espaço. |
|  | q56 ; 4 ; 4 ; R ; q60 | DESLOCAR A PALAVRA PARA A DIREIRA. Achou o símbolo 4, ou seja, já abriu espaço |
|  | q57 ; □ ; 1 ; L ; q61 | DESLOCAR A PALAVRA PARA A DIREIRA. Escreve o 1 lido e apagado do lado direito de onde ele foi apagado. |
|  | q58 ; □ ; b ; L ; q61 | DESLOCAR A PALAVRA PARA A DIREIRA. Escreve o b lido e apagado do lado direito de onde ele foi apagado. |
|  | q59 ; □ ; a ; L ; q61 | DESLOCAR A PALAVRA PARA A DIREIRA. Escreve o a lido e apagado do lado direito de onde ele foi apagado. |
|  | q60 ; □ ; 4 ; L ; q53 | DESLOCAR A PALAVRA PARA A DIREIRA. Escreve o 4, pois já abriu o espaço necessário. Deve retornar ao loop de leitura e escrita da palavra.  |
|  | q61 ; □ ; □ ; L ; q56 | DESLOCAR A PALAVRA PARA A DIREIRA. Desloca para a esquerda um espaço em branco para verificar o que deve ser lido e apagado ou se já terminou de deslocar. |
|  | q62 ; !□ ; ~ ; R ; q62 | VERIFICA SE O CABEÇOTE ESTA NO S. Procura o final da palavra |
|  | q62 ; □ ; □ ; L ; q63 |  |
|  | q63 ; [a, b, 1, A, B] ; ~ ; L ; q63 | VERIFICA SE O CABEÇOTE ESTA NO S. Pula toda a palavra até encontrar o símbolo de inicio. |
|  | q63 ; $ ; $ ; L ; q64 | VERIFICA SE O CABEÇOTE ESTA NO S. Encontrou $, indica que o cabeçote não está no inicio, ou seja, a transição não é compatível, deve desmarcar e procurar a próxima.  |
|  | q63 ; % ; % ; L ; q65 | VERIFICA SE O CABEÇOTE ESTA NO S. Encontrou %, indica que o cabeçote está no inicio, ou seja, é compatível. Deve voltar de verificar qual o movimento que deve ser realizado.  |
|  | q64 ; !S ; ~ ; L ; q64 | MARCANDO A TRANSIÇÃO COMO NÃO COMPATÍVEL. Procurando o S da transição |
|  | q64 ; S ; s ; L ; q18 | MARCANDO A TRANSIÇÃO COMO NÃO COMPATÍVEL. Desmarcando o S da transição |
|  | q65 ; !S ; ~ ; L ; q65 | VERIFICANDO O DESLOCAMENTO DO CABEÇOTE |
|  | q65 ; S ; S ; R ; q44 | VERIFICANDO O DESLOCAMENTO DO CABEÇOTE |
|  | q67 ; !□ ; ~ ; R ; q67 | VERIFICANDO SE O CABEÇOTE ESTA NO A NA PALAVRA. Indo para o final da palavra.  |
|  | q67 ; □ ; □ ; L ; q68 |  |
|  | q68 ; [a, b, B, 1] ; ~ ; L ; q68 | VERIFICANDO SE O CABEÇOTE ESTA NO A NA PALAVRA. Passando por toda a palavra, da direita para a esquerda |
|  | q68 ; A ; A ; L ; q70 | VERIFICANDO SE O CABEÇOTE ESTA NO A NA PALAVRA. Encontrou A, ou seja a transição pode ser possível. Agora só deve verificar o número de 1 em ambos.  |
|  | q68 ; [$, %] ; ~ ; L ; q69 | VERIFICANDO SE O CABEÇOTE ESTA NO A NA PALAVRA. Encontrou $ ou %, indica que a transição não é compatível.  |
|  | q69 ; !A ; ~ ; L ; q69 | MARCANDO A TRANSIÇÃO COMO NÃO COMPATÍVEL. Procurando o A da transição |
|  | q69 ; A ; a ; L ; q18 | MARCANDO A TRANISIÇÃO COMO NÃO COMPATÍVEL. Desmarcando o A da transição |
|  | q70; !A ; ~ ; L ; q70 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Procurando o símbolo da transição |
|  | q70 ; A ; A ; R ; q71 |  |
|  | q71 ; 1 ; 1 ; S ; q72 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Verificando se existe ao menos um 1 depois do a. |
|  | q72 ; 1 ; 2 ; R ; q73 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Marcar 1 como lido na transição. Agora deve verificar se existi um 1 para ler na palavra |
|  | q72 ; [R, L] ; ~ ; R ; q77 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Encontrou R ou L, significa que terminou de ler o símbolo na transição, deve verificar se existe algum 1 não lido na palavra.  |
|  | q73 ; !A ; ~ ; R ; q73 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Ir até a palavra para ler um 1.  |
|  | q73 ; A ; A ; R ; q76 |  |
|  | q76 ; 2 ; 2 ; R ; q76 |  |
|  | q76 ; 1 ; 2 ; L ; q74 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Leu um 1 na palavra.  |
| * | q76 ; [a, b, □] ; ~ ; L ; q* | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Encontrou a, b ou vazio, significa que os símbolos não são compatíveis, ou seja, a transição não é compatível |
|  | q74 ; 2 ; 2 ; L ; q74 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Após ler um 1 da palavra deve voltar tudo para conseguir verificar o símbolo da transição.  |
|  | q74 ; A ; A ; L ; q75 |  |
|  | q75 ; !2 ; ~ ; L ; q75 |  |
|  | q75 ; 2 ; 2 ; R ; q72 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Voltar a transição para realizar o loop de leitura e confirmação.  |
|  | q77 ; !A ; ~ ; R ; q77 | COMFIRMANDO SE O SÍMBOLO A É COMPATÍVEL. Procurando o a na palavra |
|  | q77 ; A ; A ; R ; q78 |  |
|  | q78 ; 2 ; 2 ; R ; q78 | COMFIRMANDO SE O SÍMBOLO A É COMPATÍVEL. Passando por todos os 1s lidos e confirmados |
|  | q78 ; 1 ; 1 ; L ; q* | COMFIRMANDO SE O SÍMBOLO A É COMPATÍVEL. O símbolo não é compatível. Deve desmarcar tudo e marcar a transição como não compatível.  |
|  | q78 ; [a, b, ] ; ~ ; L ; q* | COMFIRMANDO SE O SÍMBOLO A É COMPATÍVEL. O símbolo é compatível, deve voltar e começar a escreve.  |
|  | q79 ; 2 ; 1 ; L q79 | MARCANDO A TRANSIÇÃO COMO NÃO COMPATÍVEL. Desmarca o símbolo na palavra como não compatível e depois desmarca a transição como não compatível. |
|  | q79 ; A ; A ; L ; q80 | MARCANDO A TRANSIÇÃO COMO NÃO COMPATÍVEL. Deixa o cabeçote da palavra no A. |
|  | q80 ; !2 ; ~ ; L ; q80 | MARCANDO A TRANSIÇÃO COMO NÃO COMPATÍVEL. Procura a transição que está sendo verificada. |
|  | q80 ; 2 ; 2 ; L ; q81 |  |
|  | q81 ; 2 ; 1 ; L ; q81 | MARCANDO A TRANSIÇÃO COMO NÃO COMPATÍVEL. Desmarca todos os 1s verificados na transição. |
|  | q81 ; A ; a ; L ; q18 | MARCANDO A TRANSIÇÃO COMO NÃO COMPATÍVEL. Desmarca o A para a e depois desmarca o estado. |
|  | q82 ; !□ ; ~ ; R ; q82 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. |
|  | q82 ; □ ; □ ; L ; q83 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. |
|  | q83 ; [a, b, 1] ; ~ ; L ; q83 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Passo pro todos os a, b, e 1s |
|  | q83 ; 4 ; 1 ; L ; q83 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Desmarca todos os 1s escritos |
|  | q83 ; A ; a ; R ; q85 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Tira o cabeçote do A |
|  | q83 ; B ; b ; R ; q85 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Tira o cabeçote do B |
|  | q83 ; % ; $ ; R ; q84 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. O cabeçote esta no s |
|  | q84 ; a ; A ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. O cabeçote foi para o inicio da fita. Agora deve voltar a transição e copiar o estado final para a memoria da máquina |
|  | q84 ; b ; B ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. O cabeçote foi para o inicio da fita. Agora deve voltar a transição e copiar o estado final para a memoria da máquina |
|  | q85 ; 1 ; 1 ; R ; q85 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Passa pro todos os 1s do símbolo na palavra |
|  | q85 ; □ ; B ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Encontrou vazio, deve voltar para a transição e gravar na memoria da máquina o estado final |
|  | q85 ; a ; A ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Encontrou a, deve voltar para a transição e gravar na memoria da máquina o estado final |
|  | q85 ; b ; B ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Encontrou b, deve voltar para a transição e gravar na memoria da máquina o estado final |
|  | q87 ; !□ ; ~ ; R ; q87 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Procura o final da palavra |
|  | q87 ; □ ; □ ; L ; q88 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Encontra o final da palavra.  |
|  | q88 ; [1 , a , b] ; ~ ; L ; q88 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Pula todos os 1, a e b |
|  | q88 ; 4 ; 1 ; L ; q88 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Desmarca os 1s escritos |
|  | **q88 ; % ; $ ; L ; q24** | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Encontrou %, indica que o cabeçote esta no s, deve rejeitar a palavra.  |
|  | q88 ; B ; b ; L ; q89 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Encontrou o cabeçote no B, desmarca e anda com ele para a esquerda |
|  | q88 ; A ; a ; L ; q89 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Encontrou o cabeçote no A, desmarca e anda para a esquerda |
|  | q89 ; 1 ; 1 ; L ; q89 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Pula todos os possíveis 1s.  |
|  | q89 ; a ; A ; L ; q86 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Marca o cabeçote para o primeiro símbolo que encontra do lado esquerdo.  |
|  | q89 ; b ; B ; L ; q86 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Marca o cabeçote para o primeiro símbolo que encontra do lado esquerdo.  |
|  | q89 ; $ ; % ; L ; q86 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Encontrou $, indica que o cabeçote foi para o s.  |
|  | q86 * |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  | q21 ; 1 ; 1 ; S ; q22 | Verifica se existe `1` após a leitura do `a` |
|  | q22 ; 2 ; 2 ; R ; q22 | Passa por todos os `1`s lidos |
|  | q22 ; 1 ; 2 ; R ; q23 | Lê um `1` do símbolo lido da transição |
|  | q22 ; {a, b}w ; w ; R ; q31  | Se encontrar `a` ou `b`, indica que o símbolo na transição foi completamente lido |
|  | q23 ; {q, 1, a, b, s, f, R, L, f, #}w ; w ; R ; q23 | Procura pelo inicio da palavra |
|  | q23 ; ! ; ! ; L ; q25 | O símbolo apontado pelo cabeçote não é compatível |
|  | q23 ; $ ; $ ; R ; q24 | Passar para o inicio da palavra. |
|  | q24 ; {a, 1, b, B}w ; w ; R ; q24 | Procurar pelo símbolo `A` na palavra que indica a posição do cabeçote e a inicio da semelhança com a transição. |
|  | q24 ; □ ; □ ; L ; q25 | A palavra não encontrou `A`, ou seja, não é compatível |
|  | q24 ; A ; A ; R ; q28 | Encontrou `A` apontado pelo cabeçote na palavra |
|  | q25 ; !2 ; ~ ; L ; q25 | Procurar pelo `2` na transição para iniciar a desmarcação da transição não compatível |
|  | q25 ; 2 ; 1 ; L ; q26 | Desmarca o `1` lido no símbolo de leitura da transição |
|  | q26 ; 2 ; 1 ; L ; q26 | Desmarcar todos os `1`s lidos no símbolo de leitura da transição |
|  | q26 ; A ; a ; L ; q17 | Desmarcar o `a` do símbolo de leitura da transição |
|  | q27 ; 1 ; 1 ; S ; q28 | Verifica a existência de `1` depois do `a` |
|  | q28 ; 2 ; 2 ; R ; q28 |  |
|  | q28 ; 1 ; 2 ; L ; q* | Verifica `1` no símbolo da palavra |
|  | q28 ; {a, b}w ; w ; L ; q30 | Encontrou `a` ou `b`, indica que não possui `1` para marcar, ou seja, não é compatível |
|  | q28 ; □ ; □ ; L ; q30 | Encontrou `□`, indica que não possui `1` para marcar, ou seja, não é compatível |
|  | q29 ; !2 ; ~ ; L ; q29 | Procura pelo símbolo sendo lido na transição |
|  | q29 ; 2 ; 2 ; R ; q22 | Encontra o símbolo sendo lido na transição |
|  | q30 ; 2 ; 1 ; L ; q30 | Desmarcar todos os `1`s lidos na palavra |
|  | q30 ; A ; A ; L ; q25 | Mantem o cabeçote no símbolo da palavra |
|  | q31 ; !2 ; ~ ; R ; q31 | Procura pelo símbolo da palavra que está sendo verificado |
|  | q31 ; 2 ; 2 ; R ; q32 | Encontrou o símbolo da palavra em verificação |
|  | q32 ; 2 ; 2 ; R ; q32 | Passa por todos os `1`s já verificados |
|  | q32 ; 1 ; 1 ; L ; q30 | Encontrou `1`, indica que o símbolo da palavra não combina com o símbolo de leitura da transição |
|  | q32 ; {a, b}w ; w ; L ; q33 | Se encontrar `a` ou `b`, indica que o símbolo da palavra e o símbolo da transição são iguais |
|  | q32 ; □ ; □ ; L ; q33 | Se encontrar `□`, indica que o símbolo da palavra e o símbolo da transição são iguais |
|  | q33 ; !2 ; ~ ; L ; q33 | Procurar pelo símbolo que deve ser escrito na transição |
|  | q33 ; 2 ; 2 ; L ; q34 | Encontrou o símbolo que deve ser escrito na transição |
|  | q34 ; b ; B ; R ; q35 | Encontrou `b`, indica que deve substituir o símbolo da palavra por b |
|  | q34 ; a ; A ; R ; q47 | Encontrou a, indica que deve aumentar, diminuir ou permanecer com a mesma quantidade de `1`s na palavra |
|  | q35 ; !A ; ~ ; R ; q35 | Procura o símbolo que deve ser substituído por `b` |
|  | q35 ; A ; B ; R ; q36 | Substitui o `a` por `B` , indicando que o cabeçote está no `b` por enquanto |
|  | q36 ; 2 ; 2 ; R ; q36 | Começa a puxa toda a palavra que estiver depois do `2` para a **ESQUERDA**, ou seja, trata de andar com a palavra para a esquerda. |
|  | q36 ; b ; 2 ; L ; q46 | Encontrou um `b` para copiar para esquerda |
|  | q36 ; a ; 2 ; L ; q42 | Encontrou um `a` para copiar para esquerda |
|  | q36 ; 1 ; 2 ; L ; q44 | Encontrou um `1` para copiar para a esquerda |
|  | q36 ; □ ; □ ; L ; q37 | Encontrou `□`, indica que já copiou toda a palavra para o lado esquerdo |
|  | q37 ; 2 ; □ ****; L ; q37 | Apagar o que sobra de `2` que indica os espaços “comidos” |
|  | q37 ; {B, b, 1}w ; w ; L ; q38 | Chegou no final da palavra |
|  | q38 ; {a, 1, b, A, B}w ; w ; L ; q38 | Procurar pelo inicio da palavra |
|  | q38 ; $ ; $ ; L ; q39 |  |
|  | q39 ; {a, 1, q, f, R, L, s, b, #}w ; w ; q39 | Procurar pelo movimento que deve fazer com o cabeçote na transição |
|  | q39 {B, 2}w ; w ; q40 |  |
| (12) | q40 ; R ; r ; R ; q* | Encontrou `R`, deve andar com o cabeçote para a direita |
| (13) | q40 ; L ; l ; L ; q* | Encontrou `L`, deve andar com o cabeçote para a esquerda |
|  | q41 ; 2 ; 2 ; L ; q41 | Passa por todos os `2`s para chegar até o primeiro `2`  |
|  | q41 ; {b, B, 1}w ; w ; R ; q42 |  |
|  | q42 ; 2 ; a ; R ; q36 | Escreve `a` no lugar do `2` |
|  | q43 ; 2 ; 2 ; L ; q43 | Passa por todos os `2`s para chegar até o primeiro `2`  |
|  | q43 ; a ; a ; R ; q44 |  |
|  | q44 ; 2 ; 1 ; R ; q36 | Escreve `1` no lugar do `2` |
|  | q45 ; 2 ; 2 ; L ; q45 | Passa por todos os `2`s para chegar até o primeiro `2`  |
|  | q45 ; {b, B, 1}w ; w ; q46 |  |
|  | q46 ; 2 ; b ; R ; q36 | Escreve `b` no lugar do `2` |
|  | q47 ; 1 ; 1 ; S ; q48 | Verifica se existe `1` depois do `a` na transição |
|  | q48 ; 2 ; 2 ; R ; q48 | Passa por todos os `1`s já lidos |
|  | q48 ; {R, L}w ; w ; R ; q52 | Já terminou de escrever o símbolo na palavra |
|  | q48 ; 1 ; 2 ; R ; q49 | Troca `1` por `2` para marcar a leitura de `1` no símbolo que deve ser escrito |
|  | q49 ; !A ; ~ ; R ; q49 | Procura pelo símbolo que deve ser substituído na palavra |
|  | q49 ; A ; A ; R ; q50 |  |
|  | q50 ; 4 ; 4 ; R ; q50 | Passa por todos os `1`s já escritos |
|  | q50 ; 2 ; 4 ; L ; q51  | Escreve um `1` na palavra |
|  | q50 ; □ ; 4 ; L ; q51 | Escreve um 1 na palavra |
|  | q50 ; {b, a}w ; w ; L ; q55 | Não tem mais espaço para escrever `1` na palavra, deve deslocar para direita a palavra.  |
|  | q51 ; !A ; ~ ; L ; q51 | Procura pelo símbolo de que deve ser escrito na transição |
|  | q51 ; A ; A ; R ; q48 |  |
|  | q52 ; !4 ; ~ ; R ; q52 | Procurar a palavra para verificar se existe algum `2` para deslocar a palavra pra esquerda |
|  | q52 ; 4 ; 4 ; R ; q53 | Percorrer o primeiro `1` escrito na palavra |
|  | q53 ; 4 ; 4 ; R ; q53  | Passar por todos os `1`s escritos na palavra |
|  | q53 ; 2 ; 2 ; R ; q36 | Encontrou `2`, indica que a palavra deve ser deslocada para a esquerda |
|  | q53 ; {a, b}w ; w ; L ; q54 | Encontrou `a` ou `b`, indica que a palavra não precisa de deslocamento |
|  | q54 ; !2 ; ~ ; L ; q54 | Procurar pelos `2` na transição |
|  | q54 ; 2 ; 2 ; R ; q39 |  |
|  | q55 ; !□ ; ~ ; R ; q55 | Percorrer até o final da palavra |
|  | q55 ; □ ; □ ; L ; q56 |  |
|  | q56 ; 1 ; □ ; R ; q57 | Troca 1 por vazio |
|  | q56 ; b ; □ ; R ; q58 | Troca b por vazio |
|  | q56 ; a ; □ ; R ; q59 | Troca `a` por vazio |
|  | q56 ; 4 ; 4 ; R ; q61 | Terminou de deslocar a palavra para a direita |
|  | q61 ; 4 ; 4 ; R ; q51 | Escreve o `1` na palavra, pois agora possui espaço |
|  | q57 ; □ ; 1 ; L ; q60 | Escreve `1` no vazio |
|  | q58 ; □ ; b ; L ; q60 | Escreve `b` no vazio |
|  | q59 ; □ ; a ; L ; q60 | Escreve `a` no vazio |
|  | q60 ; □ ; □ ; L ; q56 |  |

## Observações

1. 

## Entradas Testadas

|  | Entrada | Saída |
| --- | --- | --- |
|  | q1ba11Rqf$ | q1ba11Rqf$a11 |
| ✅ | q1ba11Rqf$a11 | q1ba11Rqf$a11#R |
| ✅ | q1ssRqf$a11 | Invalido |
|  | q1a11a111Rq11#q11a11a111Lqf#q1a1a11Rq11$a1a11 |  |
|  | q1a11a111Rq11#q11a11a111#q1a1a11Rq11$a1a11 |  |
|  | q1a1a1Rqf$a1 |  |
|  | q1a1a1Rqf$a1 |  |
|  | q1a1a11Rqf$a1 |  |
|  | q1a1a11Rqf$a1 |  |
|  | q1a1a1Rq11#q11ba1Lqf$a1 |  |
|  | q1a1bLqf$a1a1 |  |
|  | q1a1a1Xqf$a1 |  |
|  | q1a1a1Rqf#a1 |  |
|  | q1a1a1Rqf$a1c11 |  |
|  | q1a1a1Rq11$a11 |  |
|  | q1a1a1Rq1$a1 |  |
