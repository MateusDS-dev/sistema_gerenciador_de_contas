# sistema_gerenciador_de_contas


Nesse exercício eu desenvolvi um sistema simples de manutenção de contas utilizando a linguagem C. A ideia principal foi trabalhar com arquivos binários e aprender melhor como salvar informações diretamente em um arquivo usando registros de tamanho fixo.

O programa possui um menu com opções para cadastrar clientes, consultar uma conta pelo número, atualizar saldo, encerrar contas e listar todos os clientes cadastrados. Também utilizei a função rewind() para reiniciar a leitura do arquivo e permitir uma nova listagem dos dados.

Para fazer o acesso direto às posições do arquivo usei fseek(), enquanto a leitura e gravação dos registros foram feitas com fread() e fwrite().

Tentei deixar o código mais organizado e simples de entender, usando funções separadas para cada parte do sistema. Os dados dos clientes ficam armazenados em um arquivo binário, então as informações continuam salvas mesmo após fechar o programa.
