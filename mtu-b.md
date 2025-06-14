# Construção da Máquina de Turing (Barbara)

## Guia de Identificação para Possíveis Caminhos a Seguir

1. Se não encontrar vazio e sim outro 1
2. Se não encontrar a e sim mais 1, quer dizer que não condiz com número de 1 em T
3. Se não tiver mais 1, significa que o símbolo lido não condiz com o símbolo na palavra 
4. Se não tiver mais 1 no símbolo lido na transição, significa que acabou de ler o símbolo
5. Se não encontrar um q e sim um F, significa que já verificou essa transição e deve ir para a próxima
6. Se encontrar um a quer dizer que não tem espaço para escrever a nova palavra. 
7. Se encontrar L no lugar de R, deve seguir para a esquerda na palavra
8. Se achar f no lugar de 1 indica que o próximo estado é um estado final
9. Se a transição for a primeira da fita não precisa remover os F, pois não deve ter F na fita

## O que falta?

- **Movimento para a Esquerda (`L`)**: `q54` cobre o movimento para a direita (`R`). Falta o movimento à esquerda (`L`).
- **Tratamento do Símbolo Branco (`b`)**: tratar os casos em que não exista a ou 1 e sim `b`
- **Movimento para a Esquerda no Início da Fita**: Tratar os casos onde o cabeçote deve se mover para o início da palavra
- **Quando for Escrever o Símbolo ele Diminui de Tamanho**: fazer a solução para quando ele for escrever um símbolo a11 no lugar de a111 ele deslocar para a esquerda o resto da palavra.

## Entradas Testas:

- q1a11a111Rq11#q11a11a111Sqf#q1a1a11Rq11$a1a11: ok

## Máquina de Turing

| ‼️ | Transição | Anotação |
| --- | --- | --- |
|  | q0    q       q         R      q1 |  |
|  | q1    1        1        R        q2 |  |
|  | q2    !$       ~       R        q2 |  |
|  | q2     $       $       R       q3 |  |
|  | q3      a       A       L       q4 |  |
|  | q4      !□      ~       L     q4 |  |
|  | q4      □      @     L       q5 |  |
|  | q5      □       t        L      q6 |  |
|  | q6      □       1       R      q7 |  |
|  | q7     !@      ~      R      q7 |  |
|  | q7      @      @     L       q8 |  |
|  | q8       t        T       L      q10 |  |
|  | q10      1       3      R     q11 |  |
|  | q11     !@      ~     R    q11 |  |
|  | q11     @       @    R    q12 |  |
|  | q12      q       Q     R     q13 |  |
|  | q12      Q        Q     R    q13 |  |
|  | q13      F         F       R     q36 |  |
|  | q13      2       2       R      q13 |  |
|  | q13      1        2      L     q14 |  |
|  | q13      a         a      L     q30 | O que leu não condiz com o estado procurado |
|  | q14      !T      ~      L      q14 |  |
|  | q14       T       T       L      q15 |  |
|  | q15       3       3      L     q15 |  |
|  | q15       1       3      R      q11 |  |
| 1 | q15       □        □     R      q16 | Se achar vazio depois dos 3, já leu todo o estado que deve procurar |
|  | q16     !Q       ~       R      q16 |  |
|  | q16      Q        Q      R       q17 |  |
|  | q17      2         2       R      q17 |  |
| 2 | q17      a         A       R       q18 | Significa que o número de 2 bate com o numero de 3 |
|  | q17       1        1      L        q38 |  |
|  | q18      1         2        R       q19 |  |
|  | q19     !$        ~        R        q19 |  |
|  | q19      $       $        R       q20 |  |
|  | q20     !A       ~       R        q20 |  |
|  | q20      A        A       R        q21 |  |
|  | q21      2        2       R         q21 |  |
| 3 | q21      1        2       L          q22 | Significa que até o momento condiz com a letra lida na transição |
|  | q21       a       a      L          q25 |  |
|  | q22     !$        ~      L          q22 |  |
|  | q22      $         $      L         q23 |  |
|  | q23      !2       ~       L         q23 |  |
|  | q23        2       2       R        q24 |  |
|  | q24       2        2       R        q24 |  |
| 4 | q24        1        2      R         q19 | Marcou mais um 1 no símbolo lido e volta a procurar a palavra |
|  | q24        a        A     R         q39 |  |
|  | q25         2       1      L         q25 |  |
|  | q25         A       A     L         q26 |  |
|  | q26         !$       ~       L       q27 |  |
|  | q27         $        $        L       q28 |  |
|  | q28         !2       ~        L      q28 |  |
|  | q28        2         1        L       q29 |  |
|  | q29        2          1         L     q29 |  |
|  | q29        A         a         L      q30 |  |
|  | q30       2        1       L      q30 |  |
|  | q30       Q        F       L       q31 |  |
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
|  | q36     q        q       R      q36 |  |
|  | q36     a        q       R      q36 |  |
|  | q36     S        S      R      q36 |  |
|  | q36     R        R       R      q36 |  |
|  | q36     L        L       R      q36 |  |
|  | q36     f        f       R      q36 |  |
|  | q36     b        b       R      q36 |  |
|  | q36     #        #       R      q37 |  |
|  | q36      $        $      R        q72 |  |
| 5 | q37      q        Q       R       q13 | Encontrou uma transição que ainda não verificou |
|  | q37      Q       Q        R       q13 |  |
|  | q37      F         F      R         q36 |  |
|  | q38      2         1        L        q38 |  |
|  | q38      Q       F         L       q31 |  |
|  | q39      1        2          R      q40   |  |
|  | q40     !$       ~         R       q40 |  |
|  | q40       $       $        R       q41 |  |
|  | q41       !A      ~       R       q41 |  |
|  | q41        A       A       R       q42 |  |
|  | q42       4        4        R       q42 |  |
| 6 | q42       2        4        L        q43 | 2 substitui por 4, mostra que esta escrevendo sem problema |
|  | q42       a        a        S         q46 |  |
|  | q42      □         4        S        q43 |  |
|  | q43      !$       ~        L        q43 |  |
|  | q43      $         $        L        q44 |  |
|  | q44      !2         $         L      q44 |  |
|  | q44       2        2        R       q45 |  |
|  | q45        1       2        R      q40 |  |
| 7 | q45        R        R       R      q52 | Escreveu o símbolo e encontrou a direção que deve seguir na palavra |
|  | q45        S         S       R     q70 |  |
|  | q46       !□       ~       R       q46 |  |
|  | q46       □         □       L        q47 |  |
|  | q47       1          □      R        q48 |  |
|  | q48        □       1         L        q49 |  |
|  | q49       !□       ~        L       q49 |  |
|  | q49        1        □        R       q48 |  |
|  | q49         a       □        R       q50 |  |
|  | q49       4         4        R       51 |  |
|  | q50        □         a       L        49 |  |
|  | q51       □          4        L        43 |  |
|  | q52       !$         ~       R       q52 |  |
|  | q52       $           $       R      q53 |  |
|  | q53      !A          ~       R       q53 |  |
|  | q53       A         a        R        q54 |  |
|  | q54       4          1        R       q54 |  |
|  | q54       a           A       L       q56 | Marcou o novo cabeçote, agora deve escrever para qual estado deve ir |
|  | q54       □          □        L      q56 |  |
|  | q56        !$        ~       L        q56 |  |
|  | q56       $         $       L        q57 |  |
|  | q57       !2        ~      L       q57 |  |
|  | q57       2         2      R        q58 |  |
|  | q58       R        R       R        q59 |  |
|  | q58       L         L       R        q59 |  |
|  | q58        S         S       R       q59 |  |
|  | q59      q          Q      R        q60 | Lidando com o estado que deve ir |
|  | q60      4          4        R       q60 |  |
| 8 | q60      1          4       L        q61 | Copiando para o inicio da fita e indica que não é final |
|  | q60      f           f         L       q75 |  |
|  | q60       $          $       L        q64 | Indica que terminou de ler o estado que deve ir |
|  | q60        #          #      L        q64 | Indica que terminou de ler o estado que deve ir |
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
| 9 | q64        #       #       L         q65 | Indica que a transição não é a primeira transição da fita |
|  | q64        @       @     L        q8 | Indica que posso começar a procurar o novo estado |
|  | q65       !F        ~      L        q65 |  |
|  | q65       F          q      L       q64 |  |
|  | q66       !$        ~       R       q66 |  |
|  | q66       $         $        R      q67 |  |
|  | q67       !□       ~        R      q67 |  |
|  | q67       □         #        R      q68 |  |
|  | q68      □          A         S     **`q69`** |  |
|  | q70     !$         ~        R      q70 |  |
|  | q70      $         $        R       q71 |  |
|  | q71       !A       ~        R      q71 |  |
|  | q71        A        A        R     q54 |  |
|  | q72      !□       ~        R      q73 |  |
|  | q73       □       #         R      q74 |  |
|  | q74       □        R         S      `q69` |  |
|  | q75      4         1       L       q75 |  |
|  | q75      Q        q        L       q75 |  |
|  | q75      R          R       L       q75 |  |
|  | q75       L         L        L        q75 |  |
|  | q75       S          S       L       q75 |  |
|  | q75        2      1        L         q75 |  |
|  | q75        #       #       L         q75 |  |
|  | q75        A       a       L          q75 |  |
|  | q75        @      □      L          q76 |  |
|  | q76       !□       □      L          q76 |  |
|  | q76      □         □      R          q77 |  |
|  | q77     !q       ~       R         q66 |  |

