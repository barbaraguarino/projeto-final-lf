# Máquina de Turing Universal (JFLAP)

Projeto final da disciplina de Linguagens Formais e Teoria da Computação.

## Sobre o Projeto

Este repositório contém a implementação de uma **Máquina de Turing Universal (MTU) Determinística**, desenvolvida inteiramente na ferramenta JFLAP. [cite_start]O objetivo do projeto é criar uma Máquina de Turing capaz de simular qualquer outra Máquina de Turing Determinística (`M`) a partir de uma descrição de suas regras e uma palavra de entrada (`w`).

A fita da máquina é finita à esquerda e infinita à direita.

## Especificação Técnica

A entrada para a MTU é fornecida em uma única fita, que contém a codificação da máquina `M` a ser simulada, seguida por um separador `$` e a palavra de entrada `w` para `M`.

### 1. Codificação da Máquina `M`

A máquina `M` é descrita por uma sequência de suas transições, separadas pelo símbolo `#`. Cada transição é uma 5-tupla no seguinte formato:

`(estado origem; símbolo lido; símbolo escrito; movimento; estado destino)`

Os componentes são codificados da seguinte forma:

* **Estados:** Representados por `q` seguido por uma notação unária de `1`s.
    * O estado inicial é sempre `q1`.
    * O estado final (único) é `qf`, de onde nenhuma transição se origina.
* **Símbolos:** Representados por `a` seguido por uma notação unária de `1`s.
    * O símbolo **branco** é representado por `b`.
    * O símbolo de **início da fita** é `s`.
* **Movimento:** Um símbolo do conjunto `{R, L}`, denotando movimento para a direita ou para a esquerda, respectivamente.

### 2. Simulação e Resultado

A MTU simula a execução de `M` sobre a palavra `w`. A simulação para sob duas condições, produzindo os seguintes resultados:

* **Aceitação:** Se `M` atinge seu estado final `qf`, a MTU escreve **`#A`** no final da fita.
* **Rejeição:** Se `M` atinge um estado não-final a partir do qual não há transição válida para o símbolo lido, a MTU escreve **`#R`** no final da fita.

## Como Usar

1.  **Abra o JFLAP:** Carregue o arquivo `.jff` da última versão da MTU contido neste repositório.
2.  **Configure a Entrada:** No menu "Input", selecione "Multiple Run".
3.  **Insira os Testes:** Adicione as fitas de teste. Uma fita de exemplo é:

    ```
    q1a1a11Rq11#q11a11a111Lqf#q11a1a11Rq1$a1a11
    ```
    [cite_start][cite: 16]

4.  **Execute:** Inicie a simulação para observar o comportamento da MTU.

##  deliverables Estrutura do Repositório

Conforme os entregáveis solicitados:

* `/`: Contém os arquivos `.jff` com as versões das implementações da Máquina de Turing Universal.
* `/`: Contém o arquivo `mtu.md` onde contém a Máquina de Turing Universal na forma de tabela e as entradas para teste.
* `/README.md`: Esta documentação.

