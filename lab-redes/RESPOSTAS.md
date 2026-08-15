# Respostas

## Parte A

1. Se iniciar o cliente antes do servidor, ocorre uma falha e é lançado uma exceção de recusa de conexão. Isso ocorre, porque o protocolo TCP é orientado a conexão. Então, antes de qualquer troca de dados, o cliente precisa realizar o processo de estabelecimento de conexão, que é o Three-Way Handshake. Porém, se o servidor não estiver em execução, não haverá nenhum processo escutando naquela porta específica, fazendo com que o sistema operacional de destino rejeite a tentativa de conexão, impedindo, assim, o início da comunicação.

2. O mecanismo do TCP que garante que as mensagens cheguem na ordem em que foram enviadas é os números de sequência junto aos números de confirmação (ACK). Nisso, cada byte de dados transmitido em um segmento recebe um número de sequência único, para que o receptor utilize esses números para remontar o dado na ordem exata de envio.

3. Se dois clientes tentassem se conectar ao mesmo tempo, o segundo cliente ficaria em espera na fila de conexões pendentes do sistema operacional, só conseguindo se conectar após o primeiro cliente encerrar sua sessão. O código atual não suporta conexões simultâneas, pois no código do servidor (tanto em Java quanto em Python), a chamada accept() funciona apenas para um cliente por vez, só voltando a aceitar novas conexões após a conexão atual ser finalizada (digitar "sair").

## Parte B

1. Quando foi enviado uma mensagem com o servidor desligado o cliente conseguiu enviar a mensagem com sucesso (já que o método de envio não lançou erro), mas ficou travado na chamada de recepção (receive() em Java e recvfrom() em Python), aguardando uma resposta que nunca chegou.

No TCP, como a conexão precisa estar previamente estabelecida e mantida viva, tentar enviar dados para um destino desconectado geraria um erro, pois não haveria mensagens de confirmações (ACK) vindas do destino.

Como o UDP envia os dados e não confirma recebimento, o cliente não sabe o estado do servidor no momento do envio. Assim, ele só fica travado, porque a aplicação cliente fica esperando por um pacote de volta.

2. Dois exemplos de aplicações que usam UDP são:
   - Discord: como é uma plataforma de streamming de vídeo/áudio, é mais importante a baixa latência e a entrega contínua dos dados do que a confiabilidade de entrega de todos eles, já que perder um ou outro dado não afeta drasticamente a experiência do usuário. Assim, se fosse usado o TCP, caso ocorresse perda de pacotes, os atrasos na reprodução do vídeo/áudio para reenviar pacotes perdidos seria perceptível.
   - Jogos em tempo real online: como o estado dos objetos que compõem o jogo são atualizados frequentemente a cada movimento do jogador, é mais relevante mostrar o estado atual do que retransmitir um pacote de dados perdido lá atrás na conexão. Neste caso, a confiabilidade do TCP também impactaria negativamente a experiência do usuário, já que o tempo gasto tentando recuperar pacotes defasados causaria travamentos e atrasos na sincronização do jogo, prejudicando diretamente a jogabilidade. O que realmente importa nesses cenários é a sincronia imediata com o estado presente.

3. É possível implementar um registro de "quem está conectado", mas, para isso, teria que se ter uma lista com o IP e porta dos dispositivos que enviaram mensagens e implementar uma forma do cliente, a cada determinado tempo, enviar uma mensagem informando que ainda está ativo na conexão. Caso extrapole o tempo sem enviar nada, a conexão deve ser derrubada e esse dispositivo seria excluído da lista.

## Parte C

1. A diferença entre enviar a mesma mensagem para 3 clientes usando unicast repetido 3 vezes e enviar uma única vez via multicast é que, no unicast, o remetente precisa enviar três cópias separadas da mesma mensagem, uma para cada cliente, o que significa que o computador gasta três vezes mais processamento e mais largura de banda. Já no multicast, o remetente envia a mensagem apenas uma única vez para o endereço do grupo, o que economiza muito tráfego de saída no servidor e evita sobrecarregar a rede.

2. O TTL (Time-To-Live) é um contador de saltos por roteadores que indica até onde o pacote tem permissão de ir antes de ser descartado. Assim, cada vez que a mensagem atravessa um roteador, esse valor diminui em 1 e, quando chega a zero, o pacote é descartado. Ele é fundamental para controlar o alcance da rede, pois garante que um pacote vá somente até onde precisa, sem ficar em loop de existência, já que terá um tempo limite até esgotar.

3. Se um dos clientes ficar temporariamente offline e voltar depois ele não recebe os avisos perdidos, porque, no multicast, os dados são transmitido em tempo real e de maneira não orientada a conexão (baseado em UDP). Além disso, ele não guarda cópias das mensagens, não sabe quem estava ouvindo na hora do envio e não possui mecanismo de retransmissão. 

## Parte D

1. O que muda na conexão depois que o handshake HTTP é concluído é que a conexão deixa de seguir o modelo tradicional do HTTP (onde o cliente faz uma pergunta e o servidor responde) e passa a funcionar como um canal bidirecional (full-duplex). Assim, tanto o cliente quanto o servidor podem enviar dados em tempo real a qualquer momento sem a necessidade de abrir novas conexões ou reenviar cabeçalhos.

2. A diferença entre WebSocket e aviso via Multicast é que, no primeiro, a comunicação é centralizada no servidor, em que cada cliente precisa se conectar individualmente à ele, além de manter uma lista desses clientes conectados. Então, quando alguém manda uma mensagem, o servidor itera sobre a lista e envia uma cópia separada para cada conexão (um broadcast). Já no Multicast, o servidor não conhece quem são os clientes, ele apenas dispara um único pacote para um endereço IP de grupo e são os roteadores e switches da rede que se encarregam de duplicar e encaminhar o pacote para todas as máquinas que se inscreveram no grupo.

3. O WebSocket é mais adequado do que o TCP cru, porque este só entrega um fluxo contínuo de bytes sem nenhuma estrutura pré-definida, impossibilitando saber onde cada mensagem começa e termina. Assim, se dois alunos mandarem avisos ao mesmo tempo, a mensagem de um aluno vai se misturar com a do outro na tela. Além de que, para ver atualizações na tela, seria necessário atualizá-la constantemente. Já o WebSocket divide os dados em mensagens bem definidas, entregando cada postagem como uma mensagem individual e organizade, bem como mantém a atualização da tela de forma instantânea.
