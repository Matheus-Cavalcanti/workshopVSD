### Conceitos Fundamentais

#### O que é um Sintetizador?
* **Definição:** Um sintetizador é uma ferramenta de software que traduz o código RTL (Register-Transfer Level), que descreve o comportamento de um circuito, em uma "netlist".
* **Netlist:** A netlist é uma descrição do circuito em termos de portas lógicas e células padrão (como AND, OR, NOT, flip-flops) que estão disponíveis em uma biblioteca de tecnologia específica.

#### Yosys
* **Ferramenta utilizada:** A aula utiliza o Yosys, um popular sintetizador de código aberto, para realizar essa conversão.

### O Fluxo de Síntese com Yosys

O processo de síntese com o Yosys envolve os seguintes passos:

1.  **Entradas:** O Yosys requer duas entradas principais:
    * **O Design (RTL):** O código Verilog que descreve o seu circuito.
    * **A Biblioteca de Células Padrão (arquivo .lib):** Um arquivo que descreve as características (função, tempo, etc.) das células lógicas básicas que podem ser usadas para construir o circuito.

2.  **Processo de Síntese:** O Yosys processa o design RTL e o mapeia para as células disponíveis na biblioteca.

3.  **Saída:** A saída do Yosys é um arquivo de **netlist**, também em formato Verilog. Esta netlist é funcionalmente equivalente ao RTL original, mas agora é descrita em termos de portas lógicas concretas da biblioteca.

### Comandos Essenciais do Yosys

A aula destaca três comandos principais para executar a síntese:

* `read_verilog`: Para carregar os arquivos de design Verilog no Yosys.
* `read_liberty`: Para carregar o arquivo da biblioteca de células padrão.
* `write_verilog`: Para gerar o arquivo de netlist de saída.

### Verificação Pós-Síntese (Verificação de Equivalência Lógica)

Depois de gerar a netlist, é crucial verificar se ela se comporta exatamente como o RTL original. Este processo é chamado de verificação de equivalência lógica:

* **Reutilização do Test Bench:** Como a netlist sintetizada mantém as mesmas entradas e saídas primárias que o design RTL, o mesmo test bench usado para simular o RTL pode ser reutilizado para simular a netlist.
* **Fluxo de Verificação:**
    1.  A **netlist** e o **test bench** são fornecidos ao simulador Icarus Verilog (`iverilog`).
    2.  O `iverilog` executa a simulação e gera um arquivo **VCD (Value Change Dump)**.
    3.  Este arquivo VCD é visualizado no **GTKWave**.
* **Critério de Sucesso:** A síntese é considerada correta se as formas de onda observadas no GTKWave para a simulação da netlist forem idênticas às formas de onda da simulação do RTL original, para o mesmo conjunto de estímulos.
