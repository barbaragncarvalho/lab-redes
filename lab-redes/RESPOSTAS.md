# Respostas

## Parte A

1. Se iniciar o cliente antes do servidor, ocorre uma falha e é lançado uma exceção de recusa de conexão. Isso ocorre, porque o protocolo TCP é orientado a conexão. Então, antes de qualquer troca de dados, o cliente precisa realizar o processo de estabelecimento de conexão, que é o Three-Way Handshake. Porém, se o servidor não estiver em execução, não haverá nenhum processo escutando naquela porta específica, fazendo com que o sistema operacional de destino rejeite a tentativa de conexão, impedindo, assim, o início da comunicação.

2. O mecanismo do TCP que garante que as mensagens cheguem na ordem em que foram enviadas é os números de sequência junto aos números de confirmação (ACK). Nisso, cada byte de dados transmitido em um segmento recebe um número de sequência único, para que o receptor utilize esses números para remontar o dado na ordem exata de envio.

3. Se dois clientes tentassem se conectar ao mesmo tempo, o segundo cliente ficaria em espera na fila de conexões pendentes do sistema operacional, só conseguindo se conectar após o primeiro cliente encerrar sua sessão. O código atual não suporta conexões simultâneas, pois no código do servidor (tanto em Java quanto em Python), a chamada accept() funciona apenas para um cliente por vez, só voltando a aceitar novas conexões após a conexão atual ser finalizada (digitar "sair").
