### Conceitos Fundamentais

#### O Simulador (Iverilog)
* **O que é?** Um simulador é um programa de software que imita o comportamento de um circuito digital descrito em uma linguagem como Verilog. Sua principal função é verificar se o design (o código do circuito) atende às especificações funcionais.
* **Como funciona?** O simulador monitora constantemente os sinais de entrada do seu design. Sempre que um sinal de entrada muda de valor (por exemplo, de 0 para 1), o simulador recalcula as saídas correspondentes com base na lógica descrita no seu código. Se as entradas não mudam, as saídas permanecem estáveis.
* **Ferramenta utilizada:** A aula especifica o uso do Iverilog, que é um simulador de Verilog de código aberto e amplamente utilizado.

#### O Design (RTL - Register-Transfer Level)
* **O que é?** O "design" é o próprio código Verilog que descreve a funcionalidade do circuito que você está a criar. Ele pode consistir em um ou vários arquivos Verilog que, juntos, implementam a lógica desejada (por exemplo, um somador, um processador, etc.).
* **Nível de Abstração:** O código é escrito no nível de transferência de registros (RTL), que descreve como os dados se movem entre os registros de armazenamento (como flip-flops) e através da lógica combinacional.

#### O Test Bench
* **O que é?** Um "test bench" é um ambiente de teste, também escrito em Verilog, criado especificamente para verificar o seu design. Ele não faz parte do circuito final, mas é crucial para a simulação.
* **Qual a sua função?** A principal função do test bench é gerar e aplicar um conjunto de estímulos (chamados de "vetores de teste") às entradas do seu design. Depois, ele pode (opcionalmente) verificar if as saídas produzidas pelo design correspondem aos resultados esperados.
* **Estrutura:** Um test bench instancia (cria uma cópia) do design que se quer testar dentro dele. É importante notar que, ao contrário do design, o test bench em si não possui portas de entrada ou saída externas; ele é um ambiente de simulação autocontido.

### O Fluxo de Simulação com Iverilog

O processo prático de simulação, conforme descrito na aula, segue estes passos:

1.  **Entrada:** Você fornece dois conjuntos de arquivos para o simulador Iverilog:
    * Os arquivos do Design (o seu circuito em Verilog).
    * O arquivo do Test Bench (o código que testará o seu design).
2.  **Simulação:** O Iverilog compila e executa o test bench. O test bench, por sua vez, aplica os estímulos ao design.
3.  **Saída:** O resultado da simulação é um arquivo chamado VCD (Value Change Dump). Este arquivo é um registro detalhado de todas as mudanças de valor (0 para 1, 1 para 0, etc.) em todos os sinais (entradas, saídas e sinais internos) do seu design ao longo do tempo de simulação.
4.  **Visualização e Verificação:** Para analisar os resultados, você utiliza uma ferramenta de visualização de formas de onda, como o GTKWave. Ao abrir o arquivo VCD no GTKWave, você pode ver graficamente as formas de onda de todos os sinais, permitindo verificar visualmente se o seu design se comportou como esperado em resposta aos estímulos do test bench.
