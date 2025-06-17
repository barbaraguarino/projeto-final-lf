## Máquina de Turing v5.0

## Anotações

- 

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
|  | q2 ; a ; a ; R ; q3 | Verifica se a entrada começa com `q1`  |
|  | q3 ; !$ ; ~ ; R ; q3 | Procura `$` para marcar o cabeçote na palavra |
|  | q3 ; $ ; $ ; R ; q4 |  |
|  | q4 ; a ; A ; L ; q5 | Marca o cabeçote na palavra |
|  | q4 ; □ ; □ ; L ; q5 | A palavra esta vazia |
|  | q5 ; !□ ; ~ ; L ; q5 | Volta para o inicio da fita para criar a memoria |
|  | q5 ; □ ; @ ; L ; q6 | Escreve pela primeira vez o `@` |
|  | q6 ; □ ; t ; L ; q7 | Escreve pela primeira vez o `t` |
|  | q7 ; □ ; 1 ; R ; q8 | Inicia a memoria com estado `q1` |
|  | q8 ; !@ ; ~ ; R ; q8 | Inicio da execução da entrada |
|  | q8 ; @ ; @ ; L ; q9 | Inicia a leitura do estado gravado na memoria |
|  | q9 ; t ; T ; L ; q10 |  |
|  | q9 ; T ; T ; L ; q10 |  |
|  | q10 ; 3 ; 3 ; L ; q10 | Passa por todos os `1` já lidos |
|  | q10 ; 1 ; 3 ; R ; q11 | Marca `1` como lida na memoria |
|  | q10 ; □ ; □ ; R ; q15 | Memoria completamente lida |
|  | q11 ; !@ ; ~ ; R ; q11 | Começa a procura pela transição |
|  | q11 ; @ ; @ ; R ; q12 |  |
|  | q12 ; q ; Q ; R ; q13 | Começa pela primeira vez a ler estado inicial da transição |
|  | q12 ; Q ; Q ; R ; q14 | Transição lida e ainda em verificação |
|  | q12 ; F ; F ; R ; q20 | Transição já lida e invalidada |
|  | q13 ; 1 ; 1 ; S ; q14 | Verifica se possui `1` depois do `q` |
|  | q14 ; 2 ; 2 ; R ; q14 | Passa por todos os `1`s já verificados |
|  | q14 ; 1 ; 2 ; L ; q8 | Marca um `1` como verificado do lado da transição |
|  | q14 ; a ; a ; L ; q17 | Não possuí mais `1` para marcar, o estado NÂO COMPATÍVEL com a memoria, ou seja, `t > q` |
|  | q15 ; !Q ; ~ ; R ; q15 | Volta para a transição para verificar completamente a compatibilidade |
|  | q15 ; Q ; Q ; R ; q16 |  |
|  | q16 ; 2 ; 2 ; R ; q16 | Passa por todos os 2 já marcados |
|  | q16 ; a ; A ; R ; q21 | Encontrou `a`, portanto `q` é compatível com o estado na memoria, ou seja, `q = t` |
| (2) | 16 ; b ; B ; R ; q* | Encontrou `b`, portanto `q` é compatível com o estado na memoria, ou seja, `q = t` |
| (3) | 16 ; s ; S ; R ; q* | Encontrou `s`, portanto `q` é compatível com o estado na memoria, ou seja, `q = t` |
|  | q16 ; 1 ; 1 ; R ; q17 | Encontrou mais `1`, portanto `q` não é compatível com o estado na memória, ou seja, `q > t` |
|  | q17 ; 2 ; 2 ; L ; q17 | Desmarca todos os `1`s verificados |
|  | q17 ; Q ; F ; L ; q18 | Troca `Q` por `F` para marcar a transição já verificada |
|  | q18 ; !T ; ~ ; L ; q18 | Procura o inicio da memoria para de começar a desmarcar |
|  | q18 ; T ; T ; L ; q19 |  |
|  | q19 ; 3 ; 1 ; L ; q19 | Desmarcar todos os `1`s lidos na memoria |
|  | q19 ; 1 ; 1 ; R ; q8 | Se encontrar `1` significa que desmarcou toda a memoria |
|  | q19 ; □ ; □ ; R ; q8 | Se encontrar `□`, significa que desmarcou toda a memoria |
|  | q20 ; {1, a, s, b, R, L, q, f}w ; w ; R ; q20 | Procura pela próxima transição na fita |
| (1) | q20 ; $ ; $ ; L ; q* | Se encontrar `$` indica que leu todas as transições e nenhum delas é compatível |
| (1) | q20 ; ! ; ! ; S ; q* | Se encontrar `!` indica que leu todas as transições e nenhuma delas é compatível |
|  | q20 ; # ; # ; R ; q12 | Se encontrar `#` indica que achou a próxima transição da fita |
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
