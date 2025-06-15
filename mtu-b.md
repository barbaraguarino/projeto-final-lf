## Guia de Identificação para Possíveis Caminhos a Seguir

1. 

## Atenção!

- Quando a transição que não é verificada esta com a escrita incorreta, a máquina aceita mesmo assim, pois não lê a transição completamente.
- Verificar os estados `q75`, `q66` e `q49` com o modelo no no JFLAP
- Reverificar os estados com loop de transições escritas um por um.

## O que falta?

- [x]  **Movimento para a Esquerda (`L`)**: `q54` cobre o movimento para a direita (`R`). Falta o movimento à esquerda (`L`).
- [ ]  **Tratamento do Símbolo Branco (`b`)**: tratar os casos em que não exista `a` ou `1` e sim `b`
- [ ]  **Se encontrar vazio**: Se o cabeçote estiver no vazio, o que fazer?
- [ ]  **Movimento para a Esquerda no Início da Fita**: Tratar os casos onde o cabeçote deve se mover para o início da palavra
- [x]  **Quando for Escrever o Símbolo ele Diminui de Tamanho**: fazer a solução para quando ele for escrever um símbolo a11 no lugar de a111 ele deslocar para a esquerda o resto da palavra.

## Máquina de Turing Universal

| ‼️ | Transição | Anotação |
| --- | --- | --- |
|  | q0    q       q         R      q1 | **INICIO DA FITA** |
|  | q1    1        1        R        q9 |  |
|  | q9      a       a       R      q2 |  |
|  | q2    !$       ~       R        q2 |  |
|  | q2     $       $       R       q3 |  |
|  | q3      a       A       L       q4 |  |
|  | q4      !□      ~       L     q4 |  |
| vi | q4      □      @     L       q5 | **INICIO DA SIMULAÇÃO** |
|  | q5      □       t        L      q6 |  |
|  | q6      □       1       R      q7 |  |
|  | q7     !@      ~      R      q7 |  |
|  | q7      @      @     L       q8 |  |
|  | q8       t        T       L      q10 |  |
|  | q10      1       3      R     q11 |  |
|  | q11     !@      ~     R    q11 |  |
|  | q11     @       @    R    q12 |  |
| ii | q12      q       Q     R     q13 | **INÍCIO DA VERIFICAÇÃO DE UMA REGRA** |
|  | q12      Q        Q     R    q13 |  |
|  | q12      F         F       R     q36 |  |
|  | q13      2       2       R      q13 |  |
|  | q13      1        2      L     q14 |  |
| xvi | q13      a         a      L     q30 | **FALHA NA COMPARAÇÃO DE ESTADO** |
|  | q14      !T      ~      L      q14 |  |
|  | q14       T       T       L      q15 |  |
|  | q15       3       3      L     q15 |  |
|  | q15       1       3      R      q11 |  |
| ix | q15       □        □     R      q16 | **SUCESSO NA COMPARAÇÃO DE ESTADO** |
|  | q16     !Q       ~       R      q16 |  |
|  | q16      Q        Q      R       q17 |  |
| v | q17      2         2       R      q17 | **INÍCIO DA COMPARAÇÃO DE SÍMBOLO** |
|  | q17      b         B       R       q95 | se encontrar um b para ler |
| x | q17      a         A       R       q18 | **VERIFICA INÍCIO DO SÍMBOLO 'a...’ marco A** |
|  | q17       1        1      L        q38 |  |
|  | q18      1         2        R       q19 |  |
|  | q19     !$        ~        R        q19 |  |
|  | q19      $       $        R       q20 |  |
|  | q20     a       a       R        q20 |  |
|  | q20     1       1       R        q20 |  |
|  | q20      A        A       R        q21 |   |
|  | q21      2        2       R         q21 |  |
| xi | q21      1        2       L          q22 | **PASSO 1 DO PING-PONG (PALAVRA -> REGRA)** |
|  | q21       a       a      L          q25 |  |
|  | q22     !$        ~      L          q22 |  |
|  | q22      $         $      L         q23 |  |
|  | q23      !2       ~       L         q23 |  |
|  | q23        2       2       R        q24 |  |
|  | q24       2        2       R        q24 |  |
| xii | q24        1        2      R         q19 | **PASSO 2 DO PING-PONG (REGRA -> PALAVRA)** |
|  | q24        a        a     R         q107 |  |
|  | q107      !$       ~     R         q107 |  |
|  | q107       $        $      R       q108 |  |
|  | q108      a         a       R       q108 |  |
|  | q108      1         1       R       q108 |  |
|  | q108       A        A      R       q109 |  |
|  | q109        2        2       R       q109 |  |
|  | q109        1       1       L        q25 |  |
|  | q109        a       a       L       q110 |  |
|  | q109        □       □       L       q110 |  |
|  | q110       !$     ~     L       q110 |  |
|  | q110        $         $     L      q111 |  |
|  | q111       !2        ~      L      q111 |  |
|  | q111       2         2       R     q112 |  |
|  | q112       a         A         R     q39 |  |
|  | q25         2       1      L         q25 |  |
|  | q25         A       A     L         q26 |  |
|  | q26         !$       ~       L       q26 |  |
|  | q26         $        $        L       q28 |  |
|  | q28         !2       ~        L      q28 |  |
|  | q28        2         1        L       q29 |  |
|  | q29        2          1         L     q29 |  |
|  | q29        A         a         L      q30 |  |
|  | q30       2        1       L      q30 |  |
| iii | q30       Q        F       L       q31 | **GATILHO DE FALHA DE COMBINAÇÃO** |
|  | q31      !T         ~      L       q31 |  |
|  | q31      T         T        L        q32 |  |
|  | q32      3         1        L      q32 |  |
|  | q32      1        1       L          q32 |  |
|  | q32      □        □        R       q33 |  |
|  | q33       !T      ~        R       q33 |  |
|  | q33       T        T        L        q34 |  |
|  | q34       1       3      R         q35 |  |
|  | q35       !F       ~        R      q35 |  |
|  | q35      F        F       R      q36      |  |
|  | q36     1        1       R      q36 |  |
|  | q36     a        a       R      q36 |  |
|  | q36     b        b       R      q36 |  |
|  | q36     s         s       R       q36 |  |
|  | q36     q        q       R      q36 |  |
|  | q36     S        S      R      q36 |  |
|  | q36     R        R       R      q36 |  |
|  | q36     L        L       R      q36 |  |
|  | q36     f        f       R      q36 |  |
|  | q36     #        #       R      q37 |  |
| vii | q36      $        $      R        q72 | **REJEIÇÃO - FIM DAS REGRAS** |
| ii | q37      q        Q       R       q13 | **INÍCIO DA VERIFICAÇÃO DE UMA REGRA** |
|  | q37      Q       Q        R       q13 |  |
|  | q37      F         F      R         q36 |  |
|  | q38      2         1        L        q38 |  |
|  | q38      Q       F         L       q31 |  |
|  | q39      1        2          R      q40   |  |
|  | q40     !$       ~         R       q40 |  |
|  | q40       $       $        R       q41 |  |
|  | q41       a      a       R       q41 |  |
|  | q41       1      1       R       q41 |  |
|  | q41        A       A       R       q42 |  |
|  | q42       4        4        R       q42 |  |
| xiii | q42       2        4        L        q43 | **ESCREVENDO UM DÍGITO DO NOVO SÍMBOLO** |
|  | q42       a        a        S         q46 |  |
|  | q42       1        1        S        q83 |  |
|  | q42      □         4        S        q43 |  |
|  | q43      !$       ~        L        q43 |  |
|  | q43      $         $        L        q44 |  |
|  | q44      !2         ~         L      q44 |  |
|  | q44       2        2        R       q45 |  |
|  | q45        1       2        R      q40 |  |
|  | q45        R       R        R      q118 |  |
|  | q45        S       S        R      q118 |  |
|  | q45        L       L        R      q118 |  |
|  | q118      !$       ~       R     q118 |  |
|  | q118      $        $       R      q119 |  |
|  | q119      !4      ~       R      q119 |  |
|  | q119       4      4       R       q120 |  |
|  | q120      4        4      R     q120 |  |
|  | q120      2        2        S     q83 |  |
|  | q120     a         a       L      q121 |  |
|  | q46       !□       ~       R       q46 |  |
|  | q46       □         □       L        q47 |  |
|  | q47       1          □      R        q48 |  |
|  | q48        □       1         L        q49 |  |
|  | q49       !□       ~        L       q49 |  |
|  | q49        1        □        R       q48 |  |
|  | q49         a       □        R       q50 |  |
|  | q49       4         4        R       q51 |  |
|  | q50        □         a       L        q49 |  |
|  | q51       □          4        L        q43 |  |
|  | q52       !$         ~       R       q52 |  |
|  | q52       $           $       R      q53 |  |
|  | q53      a          a       R       q53 |  |
|  | q53      1          1       R       q53 |  |
|  | q53       A         a        R        q54 |  |
|  | q54       4          1        R       q54 |  |
| xvii | q54       a           A       L       q56 | **FINALIZA MOVIMENTO DO CABEÇOTE** |
| xviii | q54       □          □        L      q56 | **TRATA MOVIMENTO (DIREITA) PARA CÉLULA VAZIA** |
|  | q56        !$        ~       L        q56 |  |
|  | q56       $         $       L        q57 |  |
|  | q57       !2        ~      L       q57 |  |
|  | q57       2         2      R        q58 |  |
|  | q58       R        R       R        q59 |  |
|  | q58       L         L       R        q59 |  |
|  | q58        S         S       R       q59 |  |
| xix | q59      q          Q      R        q60 | **INICIA LEITURA DO ESTADO DE DESTINO** |
|  | q60      4          4        R       q60 |  |
| xiv | q60      1          4       L        q61 | **ATUALIZANDO ESTADO NA MEMÓRIA PRINCIPAL** |
| viii | q60      f           f         L       q75 | **DETECTOR DE ESTADO FINAL** |
| xx | q60       $          $       L        q64 | **FINALIZA A CÓPIA DO ESTADO DE DESTINO** |
| xx | q60        #          #      L        q64 | **FINALIZA A CÓPIA DO ESTADO DE DESTINO** |
|  | q61     !@       ~        L         q61 |  |
|  | q61      @      @        L        q66 |  |
|  | q66      T          t        L       q62 |  |
|  | q66     t           t        L        q62 |  |
|  | q62      1        1        L        q62 |  |
|  | q62     3         1        R       q63 |  |
|  | q62     □         1        R       q63 |  |
|  | q63     !4        ~        R       q63 |  |
|  | q63     4         4       R       q60 |  |
|  | q64      4         1       L       q64 |  |
|  | q64      Q        q        L       q64 |  |
|  | q64      R          R       L       q64 |  |
|  | q64       L         L        L        q64 |  |
|  | q64       S          S       L       q64 |  |
|  | q64        2      1        L         q64 |  |
|  | q64        A       a       L          q64 |  |
|  | q64       □       □      L         q66 |  |
| xv | q64        #       #       L         q65 | **CONTINUA LIMPEZA DE MARCADORES 'F’** |
| i | q64        @       @     L        q8 | **PONTO DE PARTIDA DO CICLO DE SIMULAÇÃO** |
|  | q65       !F        ~      L        q65 |  |
|  | q65       F          q      L       q64 |  |
|  | q66       !$        ~       R       q66 |  |
|  | q66       $         $        R      q67 |  |
|  | q67       !□       ~        R      q67 |  |
|  | q67       □         #        R      q68 |  |
|  | q68      □          A         S     **`q69`** |  |
|  | q70     !$         ~        R      q70 |  |
|  | q70      $         $        R       q71 |  |
|  | q71       a       a        R      q71 |  |
|  | q71       1       1        R      q71 |  |
|  | q71        A        A        R     q56 |  |
|  | q72      !□       ~        R      q72 |  |
|  | q72      □         #        R      q74 |  |
|  | q74       □        R         L      q113 |  |
|  | q113    #         #      L        q113 |  |
|  | q113    2         1      L        q113 |  |
|  | q113    5         1      L        q113 |  |
|  | q113     a          a      L        q113 |  |
|  | q113     A        A      L        q113 |  |
|  | q113     1        1        L        q113 |  |
|  | q113      $        $       L       q114 |  |
|  | q114      A         a       L       q114 |  |
|  | q114      2        1        L       q114 |  |
|  | q114      1        1        L       q114 |  |
|  | q114      a        a        L       q114 |  |
|  | q114      Q         q       L       q114 |  |
|  | q114      q         q       L       q114 |  |
|  | q114      F         q       L       q114 |  |
|  | q114      #         #       L       q114 |  |
|  | q114      f         f       L       q114 |  |
|  | q114      b         b       L       q114 |  |
|  | q114      B         b       L       q114 |  |
|  | q114      R         R       L       q114 |  |
|  | q114      L         L       L       q114 |  |
|  | q114      S         S       L       q114 |  |
|  | q114      s         s       L       q114 |  |
|  | q114      @       □      L        q115 |  |
|  | q115      t          □     L       q115 |  |
|  | q115      T          □     L       q115 |  |
|  | q115      3          □     L       q115 |  |
|  | q115      1         □     L       q115 |  |
|  | q115      □          □     R       q116 |  |
|  | q116     !$         ~     R       q116 |  |
|  | q116     $         $       R       `q69` |  |
|  | q75      4         1       L       q75 |  |
|  | q75      Q        q        L       q75 |  |
|  | q75      R          R       L       q75 |  |
|  | q75       q         q        L        q75 |  |
|  | q75       1         1        L        q75 |  |
|  | q75       L         L        L        q75 |  |
|  | q75       S          S       L       q75 |  |
|  | q75        2      1        L         q75 |  |
|  | q75        #       #       L         q75 |  |
|  | q75        A       a       L          q75 |  |
|  | q75        @      □      L          q76 |  |
|  | q76       T       □      L          q76 |  |
|  | q76       t       □      L          q76 |  |
|  | q76       3       □      L          q76 |  |
|  | q76       1       □      L          q76 |  |
|  | q76      □         □      R          q77 |  |
|  | q77     !q       ~       R         q77 |  |
|  | q77     q       q       R         q66 |  |
|  | q78    !$        ~       R        q78 |  |
|  | q78     $        $         R       q79 |  |
|  | q79     a         a        R       q79 |  |
|  | q79     1        1         R       q79 |  |
|  | q79      A       N         R       q80 |  |
|  | q80      4        1        R        q80 |  |
|  | q80      a        a        L        q81      |  |
|  | q80     □         □       L         q81 |  |
|  | q81     !N       ~       L        q81 |  |
|  | q81     N        a        L        q82 |  |
|  | q82      1        1       L         q82 |  |
|  | q82       a        A       L        q56 |  |
|  | q83       2        2        R         q83 |  |
|  | q83        □         □        L        q92 |  |
|  | q83       a         2       L          q84 |  |
|  | q84        2         2       L          q84 |  |
|  | q84         4         4       R        q85 |  |
|  | q85         2         a        R       q86 |  |
|  | q86        !2        ~        R       q86 |  |
|  | q86        2         2        R        q87 |  |
|  | q87       2         2        R       q87 |  |
|  | q87        1          2        L         q88 |  |
|  | q87       a           2        L         q90 |  |
|  | q87       □           □       L         q92 |  |
|  | q88        !a         ~        L       q88 |  |
|  | q88         a          a       R      q89 |  |
|  | q89        5           5       R      q89 |  |
|  | q89         1          5       R        q86 |  |
|  | q89         2         5        R        q86 |  |
|  | q90         !5         ~      L        q90 |  |
|  | q90        5           5       R       q91 |  |
|  | q91       5          5        R        q91 |  |
|  | q91       1          a        R        q86 |  |
|  | q91       2          a         R        q86 |  |
|  | q92       2        □         L         q92 |  |
|  | q92        5         1         L        q92 |  |
|  | q92        a          a         L        q92 |  |
|  | q92        4          4       L         q121 |  |
|  | q121      !$      ~       L         q121 |  |
|  | q121      $        $      L         q122 |  |
|  | q122     !2      ~        L       q122 |  |
|  | q122      2       2     R       q123 |  |
| iv | q123        R        R       R       q52 | **DECISÃO DE MOVIMENTO (R)** |
| iv | q123        S         S       R       q70 | **DECISÃO DE MOVIMENTO (S)** |
| iv | q123      L         L       R      q78 | **DECISÃO DE MOVIMENTO (L)** |
|  |  |  |
|  | q95     !$          ~        R       q95 |  |
|  | q95      $        $          R       q96 |  |
|  | q96      a         a          R       q96 |  |
|  | q96      1         1          R       q96 |  |
|  | q96        A        A        L       q97 |  |
|  | q97        !B       ~       L       q97 |  |
|  | q97        B        b        R       q28 |  |
|  | q96        □         □        L       q98 |  |
|  | q98      !$         $         L       q98 |  |
|  | q98       $         $        L       q99 |  |
|  | q99        !B        ~       L      q100 |  |
|  | q100     B         B       R       q101 |  |
|  | q101    a         A       R        q102 | se encontrar a, tem que escrever a no vazio ao final da palavra |
|  | q101     b        B       R      | se encontrar b, quer dizer que não precisa fazer nada |
|  | q102      !$      ~       R      q102 |  |
|  | q102      $      $        R      q103 |  |
|  | q103       a         a      R     q103 |  |
|  | q103       1         1      R    q103 |  |
|  | q103       □         A      L     q104 |  |
|  | q104      !$        ~        L     q104 |  |
|  | q104       $        $      L        q105 |  |
|  | q105      !A     ~       L        q105 |  |
|  | q105       A        A     R      q106 |  |
|  | q106       2        2      R    q106 |  |
|  | q106        1        2       R    q42 |  |
|  |  |  |

## Pontos Importantes

1. **PONTO DE PARTIDA DO CICLO DE SIMULAÇÃO:** Depois de terminar um passo (ou fazer uma limpeza), este estado te manda de volta para o começo da fita. A partir dali, ele pula para `q8` para recomeçar o ciclo: ler o estado atual da máquina, procurar uma regra que sirva, executá-la, e assim por diante.
2. **INÍCIO DA VERIFICAÇÃO DE UMA REGRA:** É aqui que a máquina começa a testar se uma regra pode ser usada. O `q` da regra é marcado com um `Q` para mostrar que está sendo verificado. Se algo der errado no meio do caminho, a lógica em `q30` vai trocar esse `Q` por um `F`, indicando falha.
3. **GATILHO DE FALHA DE COMBINAÇÃO:** Se o estado atual da máquina ou o símbolo na fita não batem com o que a regra exige, a máquina vem para este ponto. Aqui, ela marca a regra que falhou com um `F` e já começa a se preparar para buscar a próxima, limpando a bagunça e pulando para `q36`.
4. **DECISÃO DE MOVIMENTO:** Este estado (`q45`) simplesmente lê qual é a direção do movimento (esquerda, direita ou parado) que a regra manda fazer.
5. **INÍCIO DA COMPARAÇÃO DE SÍMBOLO:** O estado `q17` dá o pontapé inicial para comparar o símbolo que a regra espera com o símbolo que está de fato na fita.
6. **INICIO DA SIMULAÇÃO:** O bloco (`q3`-`q7`) é executado apenas uma vez, bem no comecinho. Ela faz duas coisas simples: primeiro, marca onde a cabeça de leitura está na palavra usando a letra `A`. Depois, volta lá para o início de tudo e escreve o estado inicial da máquina (`@t1`).
7. **REJEIÇÃO - FIM DAS REGRAS:** O estado `q36` fica procurando a próxima regra (o próximo `#`). Se em vez disso ele achar um `$`, é porque não há mais regras para testar e nenhuma funcionou. A máquina então desiste e vai para `q72` escrever `#R`.
8. **DETECTOR DE ESTADO FINAL:** Ao ler o estado de destino de uma regra, o estado `q60` fica de olho. Se ele encontrar a letra `f`, ele já sabe: se essa transição for até o fim, a simulação será um sucesso e o resultado será **ACEITO** (`#A`)
9. **SUCESSO NA COMPARAÇÃO DE ESTADO:** Quando a máquina chega aqui e encontra um espaço em branco (`□`), é um bom sinal. Significa que o estado da regra e o estado atual da máquina combinaram perfeitamente. Com o estado confirmado, a máquina segue para `q16` para começar a comparar o símbolo na fita.
10. **VERIFICA INÍCIO DO SÍMBOLO 'a...':** Essa transição checa se o símbolo na regra começa com `a`. Se sim, ela o marca com um `A` temporário e segue para `q18`, pronta para comparar o resto do símbolo (a parte dos `1`s).
11. **PASSO 1 DO PING-PONG (PALAVRA -> REGRA):** Aqui começa um vai e vem para validar o símbolo. A máquina acha um `1` na palavra, marca que já o viu (trocando por `2`) e viaja de volta para a área das regras para achar o `1` correspondente por lá.
12. **PASSO 2 DO PING-PONG (REGRA -> PALAVRA):** Agora a volta do "ping-pong". A máquina encontra o `1` correspondente na regra, o marca como verificado (`2`) e inicia a viagem de volta para a palavra, para continuar a comparação.
13. **ESCREVENDO UM DÍGITO DO NOVO SÍMBOLO:** A máquina está no processo de escrita. Ela está basicamente copiando um `1` do novo símbolo (que está na regra) para a posição atual da cabeça de leitura (`A`), usando um `4` como marcador temporário.
14. **ATUALIZANDO ESTADO NA MEMÓRIA PRINCIPAL:** A máquina está atualizando seu próprio estado. Ela lê um `1` do estado de destino da regra e usa o marcador `4` para não se perder no processo de cópia para a sua área de memória (`@t...`).
15. **CONTINUA LIMPEZA DE MARCADORES 'F':** Na fase de faxina (`q64`), se a máquina encontra um `#`, ela sabe que ainda não voltou ao começo da lista de regras. Então, ela continua indo para a esquerda (`L`) através de `q65`, procurando mais marcadores `F` para apagar.
16. **FALHA NA COMPARAÇÃO DE ESTADO:** 
Se no meio da comparação de estados (em `q13`) a máquina encontra um `a`, significa que o estado da regra é mais curto que o estado atual da máquina. Essa diferença causa uma falha, forçando um desvio para a rotina de erro em `q30`.
17. **FINALIZA MOVIMENTO DO CABEÇOTE:** Depois de se mover, a máquina encontra a célula de destino, a define como a nova posição da cabeça de leitura e pula para `q56`. De lá, ela começa o processo de atualizar sua memória de estado (`@t...`).
18. **TRATA MOVIMENTO (DIREITA) PARA CÉLULA VAZIA:** Este é um caso especial. Se a ordem é mover para a direita (`R`) e a máquina cai numa célula vazia (`□`), ela nem se dá ao trabalho de marcar uma nova posição. Simplesmente vai direto para `q56` para atualizar a memória de estado.
19. **INICIA LEITURA DO ESTADO DE DESTINO:** A máquina se posiciona bem no comecinho do estado de destino da regra que acabou de combinar. Ela marca o `q` com um `Q` e entra em `q60`, se preparando para copiar esse novo estado para sua memória principal (`@t...`).
20. **FINALIZA A CÓPIA DO ESTADO DE DESTINO.** Enquanto está copiando o estado em `q60`, encontrar um `$` ou um `#` avisa que os `1`s do estado acabaram. A máquina então pula para `q64` para limpar todos os marcadores temporários e se preparar para o próximo ciclo da simulação.

## Entradas Testas

|  | Entrada | Saída |
| --- | --- | --- |
| ✅ | q1a11a111Rq11#q11a11a111Sqf#q1a1a11Rq11$a1a11 | q1a11a111Rq11#q11a11a111Sqf#q1a1a11Rq11$a11A111 |
| ✅ | q1a11a111Rq11#q11a11a111#q1a1a11Rq11$a1a11 | Trava |
|  | q1a1a1Rqf$a1 | q1a1a1Rqf$a1#A |
|  | q1a1a1Sqf$a1 | q1a1a1Sqf$A1#A |
|  | q1a1a11Rqf$a1 | q1a1a11Rqf$a11#A |
|  | q1a1a11Sqf$a1 | q1a1a11Sqf$A11#A |
|  | q1a1a1Rq11#q11ba1Lqf$a1 | q1a1a1Rq11#q11ba1Lqf$A1a1#A |
|  | q1a1bLqf$a1a1 | q1a1bLqf$a1#A |
|  | q1a1a1Xqf$a1 | Deve Travar |
|  | q1a1a1Rqf#a1 | Deve Travar |
|  | q1a1a1Rqf$a1c11 | Deve Travar |
|  | q1a1a1Rq11$a11 | q1a1a1Rq11$a11#R |
|  | q1a1a1Rq1$a1 | Não Para |
