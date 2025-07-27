### Como Usar o Yosys (Fluxo e Comandos)

O processo de síntese no Yosys é executado através de um console interativo onde comandos específicos são inseridos em sequência.

1.  **Iniciar o Yosys:** O processo começa com a inicialização do Yosys no terminal.

2.  **Carregar a Biblioteca de Células (`read_liberty`):** O primeiro passo é informar ao Yosys quais "peças" ele pode usar. Isso é feito carregando a biblioteca de células padrão (`.lib`), que contém a definição de todas as portas lógicas disponíveis.
    * **Comando:** `read_liberty -lib <caminho_para_o_arquivo.lib>`

3.  **Carregar o Design RTL (`read_verilog`):** Em seguida, você carrega o seu arquivo de design Verilog, que descreve o comportamento do circuito que você deseja sintetizar.
    * **Comando:** `read_verilog <caminho_para_o_arquivo.v>`

4.  **Executar a Síntese (`synth`):** Este é o comando principal que realiza a mágica. Ele executa um script de síntese padrão que otimiza o design e o mapeia para as células da biblioteca carregada.
    * **Comando:** `synth -top <nome_do_modulo_principal>`
    * O argumento `-top` especifica qual módulo do seu design é o principal.

5.  **Mapear para as Células (`abc`):** O comando `abc` é uma ferramenta integrada ao Yosys, que realiza o mapeamento tecnológico final, garantindo que o design seja implementado usando apenas as células da biblioteca especificada.
    * **Comando:** `abc -liberty <caminho_para_o_arquivo.lib>`

6.  **Gerar a Netlist (`write_verilog`):** Finalmente, você exporta o resultado da síntese para um novo arquivo Verilog, que é a netlist.
    * **Comando:** `write_verilog -noattr <nome_do_arquivo_de_saida.v>`
    * **Flag `-noattr`:** É altamente recomendável usar a flag `-noattr`. Sem ela, o Yosys inclui muitos atributos e metadados no arquivo de saída, tornando-o verboso e difícil de ler. A flag `-noattr` gera uma netlist limpa, focada apenas na estrutura do circuito.

### Como o Yosys Utiliza as Células (Exemplo do MUX)

A netlist gerada revela exatamente como o Yosys traduziu a lógica comportamental do MUX em uma estrutura de portas lógicas.

* **Lógica do MUX:** Um MUX 2 para 1 tem a equação `y = (a & ~sel) | (b & sel)`.
* **Implementação do Yosys:** Em vez de usar diretamente portas AND, OR e NOT, o Yosys (através do `abc`) otimiza a lógica para usar as células disponíveis da forma mais eficiente. No exemplo da aula, ele implementa a lógica do MUX usando:
    1.  Um **inversor (`INV`)** para criar o sinal `sel_n` a partir da entrada `sel`.
    2.  Duas portas **NAND**.
    3.  Uma célula complexa chamada **OAI (OR-AND-INVERT)**, que implementa a função `NOT ( (A OR B) AND C )` em uma única célula, sendo mais eficiente que usar portas separadas.

Isso demonstra que a síntese não é uma tradução literal, mas um processo de otimização que seleciona a melhor combinação de células da biblioteca para implementar a função desejada.

### Como Ler e Entender uma Netlist Gerada

A netlist é um arquivo Verilog que descreve o circuito em nível estrutural. Aqui estão as chaves para entendê-la:

* **Declaração do Módulo:** O arquivo começa com a declaração do módulo, que terá as mesmas portas de entrada e saída do seu RTL original (ex: `module good_mux(i0, i1, sel, y);`).
* **Declaração de Fios (`wire`):** A netlist declara vários `wires` (fios) internos. Estes são os nós que conectam as saídas de umas portas às entradas de outras. O Yosys geralmente lhes dá nomes numéricos ou baseados nos nomes das células.
* **Instanciação de Células:** O corpo da netlist é uma lista de instanciações de células. Cada linha representa uma porta lógica do circuito. A sintaxe é:
    `nome_da_celula_da_biblioteca nome_da_instancia (.PORTA_A(fio_1), .PORTA_B(fio_2), .SAIDA(fio_3));`
    * **`nome_da_celula_da_biblioteca`:** O nome exato da célula conforme definido no arquivo `.lib` (ex: `sky130_fd_sc_hd__inv_2`).
    * **`nome_da_instancia`:** Um nome único para esta instância específica da célula (ex: `_001_`).
    * **`.PORTA(fio)`:** A conexão das portas da célula. Mostra qual `wire` (ou porta de entrada/saída do módulo) está conectado a cada pino da célula.

Ao seguir os fios de uma saída de uma célula para a entrada de outra, você pode traçar o caminho lógico completo e entender como a função do seu RTL foi fisicamente implementada pela ferramenta de síntese.
