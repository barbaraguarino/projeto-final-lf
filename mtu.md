# Máquina de Turing v3.0

## Anotações

- Deve analisa o que foi mudado na tabela e passar para o JFLAP.

## O Fazer?

- [x]  Escrever A em cima de A
- [x]  Escrever B em cima de A
- [x]  Copiar o estado final para a memoria da máquina
- [x]  Configurar para identificar o estado final da transição
- [x]  Escrever aceitar palavra na fita

## Máquina de Turing

| ❗ | Transições | Observações |
| --- | --- | --- |
|  | q0 ; q ; q ; R ; q1 | Verifica se a entrada começa com `q1`  |
|  | q1 ; 1 ; 1 ; R ; q2 | Verifica se a entrada começa com `q1`  |
|  | q2 ; [a, b] ; ~ ; R ; q3 | Verifica se a entrada começa com `q1`  |
|  | q3 ; !$ ; ~ ; R ; q3 | MARCAÇÃO DO CABEÇOTE. Procura `$` para marcar o cabeçote na palavra. |
|  | q3 ; $ ; $ ; R ; q4 | MARCAÇÃO DO CABEÇOTE.  |
|  | q4 ; a ; A ; L ; q5 | MARCAÇÃO DO CABEÇOTE. Marca o cabeçote no primeiro símbolo da palavra se ele existir |
| M | q4 ; □ ; B ; L ; q5 | MARCAÇÃO DO CABEÇOTE. Não marca o cabeçote se a palavra estiver vazia |
|  | q5 ; !□ ; ~ ; L ; q5 | CRIANDO MEMORIA. Volta para o inicio da fita para criar a memoria |
|  | q5 ; □ ; @ ; L ; q6 | CRIANDO MEMORIA. Escreve pela primeira vez o `@` |
| M | q6 ; □ ; T ; L ; q7 | CRIANDO MEMORIA. Escreve pela primeira vez o `t` |
|  | q7 ; □ ; 1 ; R ; q8 | CRIANDO MEMORIA. Inicia a memoria com estado `q1` |
|  | q8 ; !q ; ~ ; R ; q8 | Volta para a primeira transição de entrada |
|  | q8 ; q ; q ; S ; q9 | Inicia a leitura do estado gravado na memoria |
|  | q9 ; !@ ; ~ ; L ; q9 | LENDO A MEMORIA. Procura o começo da memoria.  |
|  | q9 ; @ ; @ ; L ; q10 | LENDO A MEMORIA. |
| D | q10 ; t ; T ; L ; q11 | LENDO A MEMORIA. Marca t como T para indicar que começou a ler a memoria.  |
|  | q10 ; T ; T ; L ; q11 | LENDO A MEMORIA. Se encontrar T no lugar de t indica que a memoria já foi lida pelo menos uma vez. |
|  | q11 ; 3 ; 3 ; L ; q11 | LENDO A MEMORIA. Passa por todos os 1s já lidos |
|  | q11 ; 1 ; 3 ; R ; q12 | LENDO A MEMORIA. Marca um 1 como lido, deve ir até a transição e marcar um 1 no estado inicial dela |
|  | q11 ; □ ; □ ; R ; q16 | LENDO A MEMORIA. Se encontrar □, já terminou de ler a memoria. Deve verificar se não existe mais 1s na transição |
|  | q12 ; !@ ; ~ ; R ; q12 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Volta para o inicio da fita para procurar o estado que condiz com a memoria |
|  | q12 ; @ ; @ ; R ; q13 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA.  |
|  | q13 ; q ; Q ; R ; q14 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Marca q como Q para indicar qual transição esta sendo verificada. Deve verificar se existe ao menos um 1 depois do q |
|  | q13 ; Q ; Q ; R ; q15 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Q indica que a transição já esta sendo verificada. Deve passar por todos os 1s já lidos e procurar ao menos 1 um para marcar |
|  | q13 ; F ; F ; R ; q23 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Indica que a transição já foi verificada e invalidada. Deve ir para a para a próxima transição |
|  | q14 ; 1 ; 1 ; S ; q15 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Verificando a existência de ao menos um 1 depois do q |
|  | q15 ; 2 ; 2 ; R ; q15 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Passar por todos os 1s na transição que já foram lidos |
|  | q15 ; 1 ; 2 ; L ; q9 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Marcar um 1 na transição como lido |
|  | q15 ; [a, b, s] ; ~ ; L ; q18 | PROCURANDO ESTADO COMPATÍVEL COM A MEMORIA. Encontrou a, b ou s, indica que o estado não combina com a memoria. Deve voltar e desmarcar tudo e marcar essa transição com F |
|  | q16 ; !Q ; ~ ; R ; q16 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. Voltar para transição para verificar se existe ou não ao menos um 1 não lido. Se existir o estado não combina, se não, o estado combina com a memoria |
|  | q16 ; Q ; Q ; R ; q17 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. |
|  | q17 ; 2 ; 2 ; R ; q17 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. Percorrer todos os 1s lidos |
|  | q17 ; 1 ; 1 ; L ; q18 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. Estado não compatível, pois existe ao menos um `1` que não foi lido na transição. |
|  | q17 ; a ; A ; R ; q67 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. Estado compatível, agora deve verificar se o símbolo da transição (`a`) é compatível com o símbolo apontado pelo cabeçote na palavra.  |
|  | q17 ; b ; B ; R ; q32 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. Estado compatível, agora deve verificar se o símbolo da transição (`b`) é compatível com o símbolo apontado pelo cabeçote na palavra.  |
|  | q17 ; s ; S ; R ; q62 | COMFIRMANDO SE O ESTADO É COMPATÍVEL COM A MEMORIA. Estado compatível, agora deve verificar se o símbolo da transição (`s`) é compatível com o símbolo apontado pelo cabeçote na palavra. |
|  | q18 ; 2 ; 1 ; L ; q18 | MARCANDO O ESTADO INICIAL COMO NÃO COMPATÍVEL. Desmarcar todos os 1s lidos. |
|  | q18 ; Q ; F ; L ; q19 | MARCANDO O ESTADO INICIAL COMO NÃO COMPATÍVEL. Marcando o Q como F, para informar que a transição não é compatível com a memoria. |
|  | q19 ; !@ ; ~ ; L ; q19 | DESMARCANDO A MEMORIA. Procurando a memoria para desmarcar a memoria já lida. |
|  | q19 ; @ ; @ ; L ; q20 | DESMARCANDO A MEMORIA. |
|  | q20 ; T ; T ; L ; q21 | DESMARCANDO A MEMORIA. Pula o T e deixa como T, pois ainda não achou o estado compatível. |
|  | q21 ; 3 ; 1 ; L ; q21 | DESMARCANDO A MEMORIA. Desmarcar todos os 1s lidos na memoria |
|  | q21 ; 1 ; 1 ; R ; q22 | DESMARCANDO A MEMORIA. Terminou de desmarcar a memoria, agora deve voltar e procurar a próxima transição |
|  | q21 ; □ ; □ ; R ; q22 | DESMARCANDO A MEMORIA. Terminou de desmarcar a memoria, agora deve voltar e procurar a próxima transição |
|  | q22 ; !@ ; ~ ; R ; q22 | Voltando para o inicio da memoria, para retorna a busca |
|  | q22 ; @ ; @ ; L ; q10 | Retorna a busca pela a transição que combina com a memoria |
|  | q23 ; [a, q, 1, f, b, s, R, L] ; ~ ; R ; q23 | PROCURANDO A PRÓXIMA TRANSIÇÃO. Pula todos os símbolos possíveis em uma transição até achar #, % ou $ |
|  | q23 ; # ; # ; R ; q13 | PROCURANDO A PRÓXIMA TRANSIÇÃO. Indica que achou uma próxima transição. Deve retorna e verifica-la. |
| C | q23 ; [$, %] ; ~ ; R ; q24 | PROCURANDO A PRÓXIMA TRANSIÇÃO. Encontrando $ ou %, indica que não encontrou transições compatíveis, deve retornar limpando a entrada e rejeitar a palavra (#R)  |
|  | q24 ; !□ ; ~ ; R ; q24 | REJEITAR A PALAVRA. Indo para o final da fita do lado direito para poder desmarcar toda a fita. |
| M | q24 ; □ ; N ; L ; q25 | REJEITAR A PALAVRA. Iniciando a limpeza da palavra e marcando o final como N para indicar que deve escrever `#R` no final da fita |
|  | q25 ; [a, b, A, B, 1] ; ~ ; L ; q25 | LIMPANDO A FITA. Pulando o que não deve ser mexido durante a limpeza. |
|  | q25 ; 4 ; 1 ; L ; q25 | LIMPANDO A FITA. Limpando os 1s escritos. |
|  | q25 ; 2 ; 1 ; L ; q25 | LIMPANDO A FITA. Limpando os 2s escritos. |
|  | q25 ; $ ; $ ; L ; q26 | LIMPANDO A FITA. Iniciar a limpeza das transações  |
| M | q25 ; % ; $ ; L ; q26 | LIMPANDO A FITA. Iniciar a limpeza das transações e deixar a marcação indicando que o cabeçote esta no `s`. |
|  | q26 ; [a, b, s, 1, q, f, R, L, #] ; ~ ; L ; q26 | LIMPANDO A FITA. Passando pelo que o não deve ser mexido durante a limpeza |
|  | q26 ; 2 ; 1 ; L ; q26 | LIMPANDO A FITA. Desmarcando os 1s lidos |
|  | q26 ; F ; q ; L ; q26 | LIMPANDO A FITA. Desmarcando as transição não compatíveis com a memoria |
|  | q26 ; Q ; q ; L ; q26 | LIMPANDO A FITA. |
|  | q26 ; B ; b ; L ; q26 | LIMPANDO A FITA.  |
| N | q26 ; 5 ; 1 ; L ; q26 | LIMPANDO A FITA.  |
|  | q26 ; S ; s ; L ; q26 | LIMPANDO A FITA. |
|  | q26 ; A ; a ; L ; q26 | LIMPANDO A FITA. |
|  | q26 ; r ; R ; L ; q26 | LIMPANDO A FITA. |
|  | q26 ; l ; L ; L ; q26 | LIMPANDO A FITA. |
|  | q26 ; @ ; @ ; S ; q27 | LIMPANDO A FITA. Iniciar a limpeza da memoria da máquina |
|  | q27 ; @ ; □ ; L ; q27 | LIMPANDO A FITA. |
|  | q27 ; T ; □ ; L ; q27 | LIMPANDO A FITA. |
|  | q27 ; t ; □ ; L ; q27 | LIMPANDO A FITA. |
|  | q27 ; 3 ; □ ; L ; q27 | LIMPANDO A FITA. |
|  | q27 ; 1 ; □ ; L ; q27 | LIMPANDO A FITA. |
|  | q27 ; □ ; □ ; R ; q28 | LIMPANDO A FITA. Finalizou a limpeza da memoria |
|  | q28 ; !q ; ~ ; R ; q28 | LIMPANDO A FITA. Voltando para o inicio da entrada. |
|  | q28 ; q ; q ; R ; q29 | LIMPANDO A FITA.  |
|  | q29 ; !□ ; ~ ; R ; q29 | VERIFICAR QUAL A MARCAÇÃO. Ir ao final da entrada para escrever #R no final |
| N | q29 ; □ ; □ ; L ; q66 | VERIFICAR QUAL A MARCAÇÃO. |
| D | q29 ; □ ; # ; R ; q30 | MARCAR A PALAVRA COMO REJEITADA. Escrevendo o # ao final da entrada |
| M | q30 ; □ ; R ; R ; **`q31`**  | MARCAR A PALAVRA COMO REJEITADA. Escrevendo R ao final da entrada e direcionando para o estado final da MTU. |
|  | q32 ; !□ ; ~ ; R ; q32  | VERIFICADO SE O CABEÇOTE ESTÁ NO B. Ir pra o final da entrada para procurar a verificar a palavra da direita pela esquerda.   |
|  | q32 ; □ ; □ ; L ; q33 | VERIFICADO SE O CABEÇOTE ESTÁ NO B. |
|  | q33 ; [1, b, a] ; ~ ; L ; q33 | VERIFICADO SE O CABEÇOTE ESTÁ NO B. Ainda não encontrou o cabeçote. |
| D | q33 ; $ ; $ ; L ; q35 | VERIFICADO SE O CABEÇOTE ESTÁ NO B. Indica que é compatível, já que o cabeçote esta no primeiro espaço após o $.  |
|  | q33 ; B ; B ; L ; q45 | VERIFICADO SE O CABEÇOTE ESTÁ NO B. É compatível, já que o cabeçote está em um b. Deve retorna a transição e começar a escrita.  |
|  | q33 ; [%, A] ; ~ ; L ; q34 | VERIFICADO SE O CABEÇOTE ESTÁ NO B. Indica que não é compatível, já que o cabeçote está no `s` ou `A`. Deve retorna a transição e marca-la como `F`.  |
|  | q34 ; !B ; ~ ; L ; q34 | DESMARCAR SÍMBOLO DE LEITURA B DA TRANSIÇÃO. Procurando o B para desmarca-lo e marcar a transição como não compatível. |
|  | q34 ; B ; b ; L ; q18 | DESMARCAR SÍMBOLO DE LEITURA B DA TRANSIÇÃO. Demarcando a leitura o b, pois o símbolo não bate com o símbolo apontado pelo cabeçote na palavra.  |
| N | q35 ; !S ; ~ ; L ; q35 | LE O SÍMBOLO DE ESCRITA DEPOIS DO S COMO SÍMBOLO DE LEITURA |
| N | q35 ; S ; S ; R ; q36 | LE O SÍMBOLO DE ESCRITA DEPOIS DO S COMO SÍMBOLO DE LEITURA |
| N | q36 ; s ; S ; R ; q44 | LE O SÍMBOLO DE ESCRITA DEPOIS DO S COMO SÍMBOLO DE LEITURA |
| N | q37 ; a ; A ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. O cabeçote foi para o inicio da fita. Agora deve voltar a transição e copiar o estado final para a memoria da máquina |
| N | q37 ; b ; B ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. O cabeçote foi para o inicio da fita. Agora deve voltar a transição e copiar o estado final para a memoria da máquina |
| N | q37 ; □ ; B ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Encontrou vazio, deve voltar para a transição e gravar na memoria da máquina o estado final |
| N | q38 ; q ; Q ; R ; q39 | COPIAR O ESTADO FINAL PARA A MEMORIA. Marca o q para Q |
| N | q39 ; 1 ; 1 ; S ; q42 | COPIAR O ESTADO FINAL PARA A MEMORIA.  |
| N | q42 ; 5 ; 5 ; R ; q42 | COPIAR O ESTADO FINAL PARA A MEMORIA.  |
| N | q42 ; 1 ; 5 ; L ; q41 | COPIAR O ESTADO FINAL PARA A MEMORIA. Indica a leitura de um 1 no estado final |
| N | q42 ; [#, $, %] ; ~ ; L ; q108 | COPIAR O ESTADO FINAL PARA A MEMORIA.  |
| N | q42 ; f ; f ; R ; q40 | COPIAR O ESTADO FINAL PARA A MEMORIA. Encontrou f, indica que vai para o estado final |
| N | q40 ; !□ ; ~ ; R ; q40 | COPIAR O ESTADO FINAL PARA A MEMORIA. Vai até o final da fita. |
| N | q40 ; □ ; Y ; L ; q25 | COPIAR O ESTADO FINAL PARA A MEMORIA. Marca Y no final da fita para indicar que a máquina aceita a palavra |
| N | q41 ; !T ; ~ ; L ; q41 | COPIAR O ESTADO FINAL PARA A MEMORIA.  |
| N | q41 ; T ; T ; L ; q43 | COPIAR O ESTADO FINAL PARA A MEMORIA |
| N | q43 ; 3 ; 1 ; R ; q107 | COPIAR O ESTADO FINAL PARA A MEMORIA |
| N | q43 ; □ ; 1 ; R ; q107 | COPIAR O ESTADO FINAL PARA A MEMORIA |
| N | q107 ; !5 ; ~ ; R ; q107 | COPIAR O ESTADO FINAL PARA A MEMORIA |
| N | q107 ; 5 ; 5 ; R ; q42 | COPIAR O ESTADO FINAL PARA A MEMORIA |
| D | q35 ; !B ; ~ ; L ; q35 | ESCREVEDO O SÍMBOLO NO VAZIO. Procurando B para escrever o símbolo da transição no primeiro espaço após o $.  |
| D | q35 ; B ; B ; R ; q36 |  |
| D | q36 ; a ; A ; R ; q39 | ESCREVEDO O SÍMBOLO NO VAZIO. Achou a, indica que deve escrever o símbolo a seguidos de ao menos um 1 no vazio após a palavra.  |
| D | q36 ; b ; B ; R ; q37 | ESCREVEDO O SÍMBOLO NO VAZIO. Achou b, indica que deve escrever o símbolo b no vazio após a palavra. |
| D | q37 ; !□ ; ~ ; R ; q37 | ESCREVEDO O SÍMBOLO NO VAZIO. Procurando pelo espaço vazio após o $ para escrever B. |
| D | q37 ; □ ; B ; L ; q54 | ESCREVEDO O SÍMBOLO NO VAZIO. Escrevendo B no primeiro espaço vazio após o $. Deve retorna a transição e verificar o movimento que deve fazer com o cabeçote.  |
| D | **q38 ; 1 ; 1 ; S ; q39** | ESCREVENDO SÍMBOLO NO VAZIO. Verificando se existe ao menos um 1 depois do a.  |
| D | q39 ; !□ ; ~ ; R ; q39 | ESCREVENDO SÍMBOLO NO VAZIO. Procurando o primeiro vazio depois da palavra para escrever o A na palavra |
| D | q39 ; □ ; A ; L ; q40 | ESCREVENDO SÍMBOLO NO VAZIO. Escrevendo o A na palavra |
| D | q40 ; !A ; ~ ; L ; q40 | ESCREVENDO SÍMBOLO NO VAZIO. Procurando o símbolo que deve ser escrito na palavra na transição. |
| D | q40 ; A ; A ; R ; q41 |  |
| D | q41 ; 2 ; 2 ; R ; q41 | ESCREVENDO SÍMBOLO NO VAZIO. Pula todos os 1s já escritos. |
| D | q41 ; 1 ; 2 ; R ; q42 | ESCREVENDO SÍMBOLO NO VAZIO. Marca um 1 que vai ser escrito na palavra. |
| D | q41 ; [R, L] ; ~ ; S ; q44 | ESCREVENDO SÍMBOLO NO VAZIO. Indica que terminou a escrita. Deve lidar com o movimento que o cabeçote deve realizar. Porém antes, deve desmarcar o 1s escritos na palavra. |
| D | q42 ; !□ ; ~ ; R ; q42 | ESCREVENDO SÍMBOLO NO VAZIO. Procurando o primeiro espaço vazio para escrever o 1 na palavra.  |
| D | q42 ; □ ; 4 ; L ; q43 | ESCREVENDO SÍMBOLO NO VAZIO. Escrevendo o 1 na palavra.  |
| D | q43 ; !2 ; ~ ; L ; q43 | ESCREVENDO SÍMBOLO NO VAZIO. Procurando o 1 já escrito da transição |
| D | q43 ; 2 ; 2 ; S ; q41 | ESCREVENDO SÍMBOLO NO VAZIO. Voltando para o loop de leitura e escrita.  |
|  | q44 ; R ; r ; R ; q82 | MOVIMENTAR O CABEÇOTE PARA A DIREITA. O cabeçote deve ir para a direita.  |
|  | q44 ; L ; l ; R ; q87 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. O cabeçote deve ir para a esquerda.  |
|  | q45 ; !B ; ~ ; L ; q45 | ESCREVENDO O SÍMBOLO X NO B. Procurando o símbolo que de ser escrito que deve ser escrita.   |
|  | q45 ; B ; B ; R ; q46 | ESCREVENDO O SÍMBOLO X NO B. |
|  | q46 ; b ; B ; R ; q44 | ESCREVENDO O SÍMBOLO B NO B. Encontrou b, ou seja, não precisa escrever, pois ira manter o que já existe na palavra. |
|  | q46 ; a ; A ; R ; q47 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Encontrou um A, ou seja deve escrever A em cima do B, seguidos do número de 1s necessários.  |
|  | q47 ; !B ; ~ ; R ; q47 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Procura B na palavra para substituir por A |
|  | q47 ; B ; A ; L ; q48 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Substitui o B por A na palavra.  |
|  | q48 ; !A ; ~ ; L ; q48 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Procura o símbolo que deve ser escrito da transição |
| M | q48 ; A ; A ; R ; q50 | ESCREVENDO O SÍMBOLO A EM CIMA DO B.  |
| D | q49 ; 1 ; 1 ; S ; q50 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Verifica se existe ao menos um 1 depois do a.  |
|  | q50 ; 1 ; 2 ; R ; q51 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Lê o 1 do símbolo que deve ser escrito na palavra |
|  | q50 ; [R, L] ; ~ ; S ; q44 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. A escrita foi concluída, agora deve desmarcar os 1s na palavra e depois realizar o movimento do cabeçote.  |
|  | q51 ; !A ; ~ ; R ; q51 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Procura o A na palavra |
|  | q51 ; A ; A ; R ; q52 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Encontrou A na palavra. Agora deve verificar o que tem do lado, se é necessário deslocar o resto da palavra para a direita. |
|  | q52 ; 4 ; 4 ; R ; q52 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Passa por todos os 1s já escritos. |
|  | q52 ; 2 ; 4 ; R ; q53 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. |
|  | q52 ; [a, b] ; ~ ; R ; q55 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. O resto da palavra deve ser deslocada para a direita para abrir espaço para a escrita.  |
|  | q52 ; □ ; 4 ; L ; q53 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Encontrou vazio, ou seja, pode escrever e deve voltar para o loop de leitura e escrita do símbolo |
|  | q53 ; !2 ; ~ ; L ; q53 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Voltar para a transição para terminar de ler a palavra que deve ser escrita. |
|  | q53 ; 2 ; 2 ; R ; q50 | ESCREVENDO O SÍMBOLO A EM CIMA DO B. Voltar para a transição para terminar de ler a palavra que deve ser escrita. |
|  | q54 ; !B ; ~ ; L ; q54 | VERIFICANDO O DESLOCAMENTO DO CABEÇOTE |
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
|  | q62 ; □ ; □ ; L ; q63 | VERIFICA SE O CABEÇOTE ESTA NO S.  |
| M | q63 ; [a, b, 1] ; ~ ; L ; q63 | VERIFICA SE O CABEÇOTE ESTA NO S. Pula toda a palavra até encontrar o símbolo % indicando que o cabeçote esta no inicio ou A ou B, indicando que não esta no inicio. |
| N | q63 ; [A, B, $] ; ~ ; L ; q64 | VERIFICA SE O CABEÇOTE ESTA NO S. Encontrou `A` ou `B`, indica que o cabeçote não está no inicio, ou seja, a transição não é compatível, deve desmarcar e procurar a próxima.  |
| D | q63 ; $ ; $ ; L ; q64 | VERIFICA SE O CABEÇOTE ESTA NO S. Encontrou $, indica que o cabeçote não está no inicio, ou seja, a transição não é compatível, deve desmarcar e procurar a próxima.  |
| M | q63 ; % ; % ; L ; q35 | VERIFICA SE O CABEÇOTE ESTA NO S. Encontrou %, indica que o cabeçote está no inicio, ou seja, é compatível. Deve voltar de verificar se o símbolo de escrita também é um `s` . |
|  | q64 ; !S ; ~ ; L ; q64 | DESMARCA O SÍMBOLO DE LEITURA S. Procurando o S da transição |
|  | q64 ; S ; s ; L ; q18 | DESMARCA O SÍMBOLO DE LEITURA S. Desmarcando o S da transição |
| D | q65 ; !S ; ~ ; L ; q65 | VERIFICANDO O DESLOCAMENTO DO CABEÇOTE |
| D | q65 ; S ; S ; R ; q44 | VERIFICANDO O DESLOCAMENTO DO CABEÇOTE |
| N | q66 ; Y ; # ; R ; q112 | VERIFICAR QUAL A MARCAÇÃO. Indica que deve marcar #A no final da fita |
| N | q66 ; N ; # ; R ; q30 | VERIFICAR QUAL A MARCAÇÃO. Indica que deve marcar #R no final da fita |
|  | q67 ; !□ ; ~ ; R ; q67 | VERIFICANDO SE O CABEÇOTE ESTA NO A NA PALAVRA. Indo para o final da palavra.  |
|  | q67 ; □ ; □ ; L ; q68 | VERIFICANDO SE O CABEÇOTE ESTA NO A NA PALAVRA.  |
| M | q68 ; [a, b, 1] ; ~ ; L ; q68 | VERIFICANDO SE O CABEÇOTE ESTA NO A NA PALAVRA. Passando por toda a palavra, da direita para a esquerda |
|  | q68 ; A ; A ; L ; q70 | VERIFICANDO SE O CABEÇOTE ESTA NO A NA PALAVRA. Encontrou A, ou seja a transição pode ser possível. Agora só deve verificar o número de 1 em ambos.  |
| M | q68 ; [$, %, B] ; ~ ; L ; q69 | VERIFICANDO SE O CABEÇOTE ESTA NO A NA PALAVRA. Encontrou $ ou %, indica que a transição não é compatível.  |
|  | q69 ; !A ; ~ ; L ; q69 | MARCANDO A TRANSIÇÃO COMO NÃO COMPATÍVEL. Procurando o A da transição |
|  | q69 ; A ; a ; L ; q18 | MARCANDO A TRANISIÇÃO COMO NÃO COMPATÍVEL. Desmarcando o A da transição |
|  | q70; !A ; ~ ; L ; q70 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Procurando o símbolo da transição |
| M | q70 ; A ; A ; R ; q72 |  |
| D | q71 ; 1 ; 1 ; S ; q72 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Verificando se existe ao menos um 1 depois do a. |
|  | q72 ; 1 ; 2 ; R ; q73 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Marcar 1 como lido na transição. Agora deve verificar se existi um 1 para ler na palavra |
| C | q72 ; [a, b] ; ~ ; R ; q77 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Encontrou R ou L, significa que terminou de ler o símbolo na transição, deve verificar se existe algum 1 não lido na palavra.  |
|  | q73 ; !A ; ~ ; R ; q73 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Ir até a palavra para ler um 1.  |
|  | q73 ; A ; A ; R ; q76 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. |
|  | q76 ; 2 ; 2 ; R ; q76 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. |
|  | q76 ; 1 ; 2 ; L ; q74 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Leu um 1 na palavra.  |
|  | q76 ; [a, b, □] ; ~ ; L ; q104 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Encontrou a, b ou vazio, significa que os símbolos não são compatíveis, ou seja, a transição não é compatível |
|  | q74 ; 2 ; 2 ; L ; q74 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Após ler um 1 da palavra deve voltar tudo para conseguir verificar o símbolo da transição.  |
|  | q74 ; A ; A ; L ; q75 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. |
|  | q75 ; !2 ; ~ ; L ; q75 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. |
|  | q75 ; 2 ; 2 ; R ; q72 | VERIFICANDO SE O SÍMBOLO A É COMPATÍVEL. Voltar a transição para realizar o loop de leitura e confirmação.  |
|  | q77 ; !A ; ~ ; R ; q77 | COMFIRMANDO SE O SÍMBOLO A É COMPATÍVEL. Procurando o a na palavra |
|  | q77 ; A ; A ; R ; q78 | COMFIRMANDO SE O SÍMBOLO A É COMPATÍVEL. |
|  | q78 ; 2 ; 2 ; R ; q78 | COMFIRMANDO SE O SÍMBOLO A É COMPATÍVEL. Passando por todos os 1s lidos e confirmados |
|  | q78 ; 1 ; 1 ; L ; q104 | COMFIRMANDO SE O SÍMBOLO A É COMPATÍVEL. O símbolo não é compatível. Deve desmarcar tudo e marcar a transição como não compatível.  |
|  | q78 ; [a, b, □] ; ~ ; L ; q79 | COMFIRMANDO SE O SÍMBOLO A É COMPATÍVEL. O símbolo é compatível, deve voltar e começar a escreve.  |
|  | q82 ; !□ ; ~ ; R ; q82 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. |
|  | q82 ; □ ; □ ; L ; q83 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. |
|  | q83 ; [a, b, 1] ; ~ ; L ; q83 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Passo pro todos os a, b, e 1s |
|  | q83 ; 4 ; 1 ; L ; q83 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Desmarca todos os 1s escritos |
|  | q83 ; A ; a ; R ; q85 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Tira o cabeçote do A |
| M | q83 ; B ; b ; R ; q37 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Tira o cabeçote do B |
|  | q83 ; % ; $ ; R ; q84 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. O cabeçote esta no s |
|  | q84 ; a ; A ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. O cabeçote foi para o inicio da fita. Agora deve voltar a transição e copiar o estado final para a memoria da máquina |
|  | q84 ; b ; B ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. O cabeçote foi para o inicio da fita. Agora deve voltar a transição e copiar o estado final para a memoria da máquina |
|  | q85 ; 1 ; 1 ; R ; q85 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Passa pro todos os 1s do símbolo na palavra |
|  | q85 ; □ ; B ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Encontrou vazio, deve voltar para a transição e gravar na memoria da máquina o estado final |
|  | q85 ; a ; A ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Encontrou a, deve voltar para a transição e gravar na memoria da máquina o estado final |
|  | q85 ; b ; B ; L ; q86 | MOVIMENTO DO CABEÇOTE PARA A DIREITA. Encontrou b, deve voltar para a transição e gravar na memoria da máquina o estado final |
| N | q86 ; !r ; ~ ; L ; q86 | Procurando o estado final da transição executada. |
| N | q86 ; r ; r ; R ; q38 | Procurando o estado final da transição executada. |
|  | q87 ; !□ ; ~ ; R ; q87 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Procura o final da palavra |
|  | q87 ; □ ; □ ; L ; q88 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Encontra o final da palavra.  |
|  | q88 ; [1 , a , b] ; ~ ; L ; q88 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Pula todos os 1, a e b |
|  | q88 ; 4 ; 1 ; L ; q88 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Desmarca os 1s escritos |
|  | q88 ; % ; $ ; L ; q24 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Encontrou %, indica que o cabeçote esta no s, deve rejeitar a palavra.  |
|  | q88 ; B ; b ; L ; q89 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Encontrou o cabeçote no B, desmarca e anda com ele para a esquerda |
|  | q88 ; A ; a ; L ; q89 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Encontrou o cabeçote no A, desmarca e anda para a esquerda |
| M | q89 ; a ; A ; L ; q96 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Marca o cabeçote para o primeiro símbolo que encontra do lado esquerdo.  |
| M | q89 ; b ; B ; L ; q96 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Marca o cabeçote para o primeiro símbolo que encontra do lado esquerdo.  |
| M | q89 ; $ ; % ; L ; q96 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Encontrou $, indica que o cabeçote foi para o s.  |
| C | q79 ; 2 ; 2 ; L ; q79 | Voltar para o inicio do símbolo verificado da palavra |
| C | q79 ; A ; A ; L ; q80 | Volta para o inicio do símbolo verificado da palavra |
| C | q80 ; !2 ; ~ ; L ; q80 | ESCREVENDO X EM CIMA DO A. Procurando o ultimo 1 do símbolo de leitura da transição |
| C | q80 ; 2 ; 2 ; R ; q81 | ESCREVENDO X EM CIMA DO A.  |
| C | q81 ; a ; A ; R ; q90 | ESCREVENDO A EM CIMA DO A. Marcando o A que deve ser escrito. |
| C | q81 ; b ; B ; R ; q91 | ESCREVENDO B EM CIMA DO A. Marcando o B que deve ser escrito. Deve voltar para a palavra e escrever.  |
| C | q89 ; 1 ; 1 ; L ; q89 | MOVIMENTAR O CABEÇOTE PARA A ESQUERDA. Pula todos os possíveis 1s.  |
| N | q90 ; 1 ; 1 ; S ; q50 | ESCREVENDO A EM CIMA DO A. Confirma que existe ao menos um 1 depois do a.  |
| C | q91 ; !A ; ~ ; R ; q91 | ESCREVENDO B EM CIMA DO A. Vai até a palavra. |
| C | q91 ; A ; B ; R ; q92 | ESCREVENDO B EM CIMA DO A. Transforma A na palavra para B. Agora deve apagar todos os 1s que existem depois do A, ou seja, deve deslocar a palavra para a esquerda.  |
| C | q92 ; 2 ; 2 ; R ; q92 | DESLOCAR A PALAVRA PARA A ESQUERDA. Pula todos os 1 que não serão sobrescrito |
| C | q92 ; b ; 2 ; L ; q94 | DESLOCAR A PALAVRA PARA A ESQUERDA. Encontrando b, substitui por 2 e anda para a esquerda. Deve procurar o primeiro 2 da palavra e substituir por b.  |
| C | q92 ; a ; 2 ; L ; q93 | DESLOCAR A PALAVRA PARA A ESQUERDA. Encontrando a, substitui por 2 e anda para a esquerda. Deve procurar o primeiro 2 da palavra e substituir por a.  |
| N | q92 ; 1 ; 2 ; L ; q99 | DESLOCAR A PALAVRA PARA A ESQUERDA. Encontrando 1, substitui por 2 e anda para a esquerda. Deve procurar o primeiro 2 da palavra e substituir por 1.  |
| C | q92 ; □ ; □ ; L ; q95 | DESLOCAR A PALAVRA PARA A ESQUERDA. Se encontrar □, indica que o símbolo substituído é o último símbolo da palavra. Deve voltar apagando os 2.  |
| N | q94 ; 2 ; 2 ; L ; q94 | DESLOCAR A PALAVRA PARA A ESQUERDA. Volta passando por todos os 2. |
| N | q94 ; [B, 1, b, 4] ; ~ ; R ; q100 | DESLOCAR A PALAVRA PARA A ESQUERDA. Anda para a direita para encontrar o primeiro 2 da palavra. |
| N | q100 ; 2 ; b ; R ; q92 | DESLOCAR A PALAVRA PARA A ESQUERDA. Substitui 2 por b e anda para direita e volta para o loop de deslocamento |
| N | q93 ; 2 ; 2 ; L ; q93 | DESLOCAR A PALAVRA PARA A ESQUERDA. Volta passando por todos os 2. |
| N | q93 ; [b , 1, B, 4] ; ~ ; R ; q101 | DESLOCAR A PALAVRA PARA A ESQUERDA. Anda para a direita para encontrar o primeiro 2 da palavra. |
| N | q101 ; 2 ; a ; R ; q92 | DESLOCAR A PALAVRA PARA A ESQUERDA. Substitui 2 por a e anda para direita e volta para o loop de deslocamento |
| N | q99 ; 2 ; 2 ; L ; q102 | DESLOCAR A PALAVRA PARA A ESQUERDA. Volta passando por todos os 2. |
| N | q102 ; [a, 1] ; ~ ; R ; q103 | DESLOCAR A PALAVRA PARA A ESQUERDA. Anda para a direita para encontrar o primeiro 2 da palavra. |
| N | q103 ; 2 ; 1 ; R ; q92 | DESLOCAR A PALAVRA PARA A ESQUERDA. Substitui 2 por 1 e anda para direita e volta para o loop de deslocamento |
| C | q95 ; 2 ; □ ; L ; q95 | DESLOCAR A PALAVRA PARA A ESQUERDA. Apaga todos os 2 encontrados na palavra.  |
| M | q95 ; [4, b, B, 1] ; ~ ; L  ; q97 | DESLOCAR A PALAVRA PARA A ESQUERDA. Volta para a transição para descobrir o movimento do cabeçote.  |
| N | q96 ; !l ; ~ ; L ; q96 | Procurando o estado final para copiar para a memoria |
| N | q96 ; l ; l ; R ; q38 | Procurando o estado final para copiar para a memoria |
| N | q97 ; !2 ; ~ ; L ; q97 | Procurar qual sera o movimento do cabeçote. |
| N | q97 ; 2 ; 2 ; R ; q98 | Procurar qual sera o movimento do cabeçote. |
| N | q98 ; B ; B ; R ; q44 | Procurar qual sera o movimento do cabeçote. |
| N | q98 ; [R, L] ; ~ ; S ; q44  | Procurar qual sera o movimento do cabeçote. |
| N | q104 ; 2 ; 1 ; L ; q104 | DESMARCANDO O SÍMBOLO LIDO NA PALAVRA. Limpar os 1s lidos na palavra |
| N | q104 ; A ; A ; L ; q105 | DESMARCANDO O SÍMBOLO LIDO NA PALAVRA. Encontrou o cabeçote da palavra. Deve procura o símbolo de leitura da transição. |
| N | q105 ; !2 ; ~ ; L ; q105 | DESMARCANDO O SÍMBOLO DE LEITURA NA TRANSIÇÃO. Procurando o símbolo de leitura na transição |
| N | q105 ; 2 ; 1 ; L ; q106 | DESMARCANDO O SÍMBOLO DE LEITURA NA TRANSIÇÃO. Limpando o 1 lido na transição |
| N | q106 ; 2 ; 1 ; L ; q106 | DESMARCANDO O SÍMBOLO DE LEITURA NA TRANSIÇÃO. Limpando os 1s lidos na transição |
| N | q106 ; A ; a ; L ; q18 | DESMARCANDO O SÍMBOLO DE LEITURA NA TRANSIÇÃO. Limpando o a lido na transição. Deve limpar o estado lido e marcar como F a transição.  |
| N | q108 ; !□ ; ~ ; R ; q108 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q108 ; □ ; □ ; L ; q109 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q109 ; [a, 1, b, A, B] ; ~ ; L ; q109 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q109 ; 4 ; 1 ; L ; q109 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q109 ; [%, $] ; ~ ; L ; q110 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q110 ; [#, 1, q, R, L, f, b, s] ; ~ ; L ; q110 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q110 ; 2 ; 1 ; L ; q110 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q110 ; F ; q ; L ; q110 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q110 ; 5 ; 1 ; L ; q110 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q110 ; r ; R ; L ; q110 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q110 ; l ; L ; L ; q110 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q110 ; B ; b ; L ; q110 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q110 ; A ; a ; L ; q110 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q110 ; Q ; q ; L ; q110 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q110 ; @ ; @ ; L ; q111 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q111 ; [T, 1] ; ~ ; L ; q111 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q111 ; 2 ; □ ; L ; q111 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q111 ; □ ; □ ; R ; q22 | LIMPAR A FITA PARA PROCURAR O PRÓXIMO ESTADO.  |
| N | q116 ; □ ; A ; L ; `q31` | MARCA ACEITAR PALAVRA.  |

## Legenda

| Legenda | Observação |
| --- | --- |
| M | Transição modificada |
| N | Transição Nova |
| D | Transição que deve ser apagada |
| C | Verificar a transição no JFLAP |
| A | Adicionar a tabela |
| P | Onde parei de escrever |

## Observações

1. Rejeitar a palavra → `q24`
2. Leitura da memória → `q9` (Procura o `@` do lado esquerdo)
3. Limpa a fita → `q25` (Limpa a fita da direita para a esquerda, depois volta para escrever `#A` ou `#R`)
4. Desmarca o estado inicial da transição marcando por F e desmarca toda memoria → `q18`
5. Símbolo lido `b` → `q32` 
6. Símbolo lido `s` → `q62`
7. Deslocamento para a direita → `q55`
8. Desloca para a esquerda → `q92`

## Entradas Testadas

| Status | Entrada | Saída |
| --- | --- | --- |
| ✅ | q1ba11Rqf$a11 | q1ba11Rqf$a11#R |
| ✅ | q1ssRqf$a11 | Invalido |
| ➡️ | q1a1a1Rqf$a1 | q1a1a1Rqf$a1#A |
| ➡️ | q1a1a11Rqf$a1 | q1a1a11Rqf$a11#A |
| ➡️ | q1a1a11Rq11#q11a11a1Lqf$a1 | q1a1a11Rq11#q11a11a1Lqf$a1#A |
| ➡️ | q1a1a1Rq11#q11ba1Lqf$a1 | q1a1a1Rq11#q11ba1Lqf$a1a1#A |
| ➡️ | q1a1a1Rq1$a11 | q1a1a1Rq1$a11#R |
| ➡️ | q1a1a1Lq11#q11a1a1Rqf$a1 | q1a1a1Lq11#q11a1a1Rqf$a1#R |
| ➡️ | q1a11a1Rq11#q1a111a1Rq11#q1a1a11Rqf$a1 | q1a11a1Rq11#q1a111a1Rq11#q1a1a11Rqf$a11#A |
| ➡️ | q1a1a1Rq1$a1 | A máquina não para |
| ➡️ | q1a1a1Xq1$a1 | A fita trava |
| ➡️ | q1a1a1Lq11#q11ssRqf$a1 | q1a1a1Lq11#q11ssRqf$a1#A |
