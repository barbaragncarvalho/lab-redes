# Respostas

## Parte A

1. Se iniciar o cliente antes do servidor, ocorre uma falha e é lançado uma exceção de recusa de conexão. Isso ocorre, porque o protocolo TCP é orientado a conexão. Então, antes de qualquer troca de dados, o cliente precisa realizar o processo de estabelecimento de conexão, que é o Three-Way Handshake. Porém, se o servidor não estiver em execução, não haverá nenhum processo escutando naquela porta específica, fazendo com que o sistema operacional de destino rejeite a tentativa de conexão, impedindo, assim, o início da comunicação.

2. O mecanismo do TCP que garante que as mensagens cheguem na ordem em que foram enviadas é os números de sequência junto aos números de confirmação (ACK). Nisso, cada byte de dados transmitido em um segmento recebe um número de sequência único, para que o receptor utilize esses números para remontar o dado na ordem exata de envio.

3. Se dois clientes tentassem se conectar ao mesmo tempo, o segundo cliente ficaria em espera na fila de conexões pendentes do sistema operacional, só conseguindo se conectar após o primeiro cliente encerrar sua sessão. O código atual não suporta conexões simultâneas, pois no código do servidor (tanto em Java quanto em Python), a chamada accept() funciona apenas para um cliente por vez, só voltando a aceitar novas conexões após a conexão atual ser finalizada (digitar "sair").

## Parte B

1. Quando foi enviado uma mensagem com o servidor desligado o cliente conseguiu enviar a mensagem com sucesso (já que o método de envio não lançou erro), mas ficou travado na chamada de recepção (receive() em Java e recvfrom() em Python), aguardando uma resposta que nunca chegou.

No TCP, como a conexão precisa estar previamente estabelecida e mantida viva, tentar enviar dados para um destino desconectado geraria um erro, pois não haveria mensagens de confirmações (ACK) vindas do destino.

Como o UDP envia os dados e não confirma recebimento, o cliente não sabe o estado do servidor no momento do envio. Assim, ele só fica travado, porque a aplicação cliente fica esperando por um pacote de volta.

1. Dois exemplos de aplicações que usam UDP são:
   - Discord: como é uma plataforma de streamming de vídeo/áudio, é mais importante a baixa latência e a entrega contínua dos dados do que a confiabilidade de entrega de todos eles, já que perder um ou outro dado não afeta drasticamente a experiência do usuário. Assim, se fosse usado o TCP, caso ocorresse perda de pacotes, os atrasos na reprodução do vídeo/áudio para reenviar pacotes perdidos seria perceptível.
   - Jogos em tempo real online: como o estado dos objetos que compõem o jogo são atualizados frequentemente a cada movimento do jogador, é mais relevante mostrar o estado atual do que retransmitir um pacote de dados perdido lá atrás na conexão. Neste caso, a confiabilidade do TCP também impactaria negativamente a experiência do usuário, já que o tempo gasto tentando recuperar pacotes defasados causaria travamentos e atrasos na sincronização do jogo, prejudicando diretamente a jogabilidade. O que realmente importa nesses cenários é a sincronia imediata com o estado presente.

3. É possível implementar um registro de "quem está conectado", mas, para isso, teria que se ter uma lista com o IP e porta dos dispositivos que enviaram mensagens e implementar uma forma do cliente, a cada determinado tempo, enviar uma mensagem informando que ainda está ativo na conexão. Caso extrapole o tempo sem enviar nada, a conexão deve ser derrubada e esse dispositivo seria excluído da lista.

