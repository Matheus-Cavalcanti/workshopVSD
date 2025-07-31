# Como Usar o Yosys (Fluxo e Comandos)

O processo de síntese no Yosys é executado através de um console interativo onde comandos específicos são inseridos em sequência.

1. **Iniciar o Yosys:** O processo começa com a inicialização do Yosys no terminal.
    * **Comando:** `yosys`

2. **Carregar a Biblioteca de Células (`read_liberty`):** O primeiro passo é informar ao Yosys quais "peças" ele pode usar. Isso é feito carregando a biblioteca de células padrão (`.lib`), que contém a definição de todas as portas lógicas disponíveis.
    * **Comando:** `read_liberty -lib <caminho_para_o_arquivo.lib>`

3. **Carregar o Design RTL (`read_verilog`):** Em seguida, você carrega o seu arquivo de design Verilog, que descreve o comportamento do circuito que você deseja sintetizar.
    * **Comando:** `read_verilog <caminho_para_o_arquivo.v>`

4. **Executar a Síntese (`synth`):** Este é o comando principal, ele executa um script de síntese padrão que otimiza o design e o mapeia para as células da biblioteca carregada.
    * **Comando:** `synth -top <nome_do_modulo_principal>`
    * O argumento `-top` especifica qual módulo do seu design é o principal.

5. **Mapear para as Células (`abc`):** O comando `abc` é uma ferramenta integrada ao Yosys, que realiza o mapeamento tecnológico final, garantindo que o design seja implementado usando apenas as células da biblioteca especificada.
    * **Comando:** `abc -liberty <caminho_para_o_arquivo.lib>`

6. **Gerar a Netlist (`write_verilog`):** Finalmente, você exporta o resultado da síntese para um novo arquivo Verilog, que é a netlist.
    * **Comando:** `write_verilog -noattr <nome_do_arquivo_de_saida.v>`
    * **Flag `-noattr`:** É altamente recomendável usar a flag `-noattr`. Sem ela, o Yosys inclui muitos atributos e metadados no arquivo de saída, tornando-o verboso e difícil de ler. A flag `-noattr` gera uma netlist limpa, focada apenas na estrutura do circuito.
