# SecureOne
## Problema Abordado

Em situações de emergência, pode ser difícil ou inviável acessar rapidamente meios de comunicação para pedir ajuda. Isso é especialmente crítico em cenários que exigem uma resposta imediata, como incidentes de segurança ou problemas de saúde.

## Solução Proposta
 
O projeto propõe a criação de um sistema de botão de pânico utilizando o ESP32, capaz de enviar uma mensagem de alerta automaticamente via WhatsApp para contatos pré-definidos. Ao pressionar o botão, o dispositivo se conecta à internet, dispara a mensagem e garante que o pedido de ajuda seja recebido rapidamente.

## Ferramentas e Tecnologias Utilizadas

- ESP32: Microcontrolador responsável pelo processamento e comunicação Wi-Fi/Bluetooth.

- Botão Físico: Dispositivo de entrada para acionar o sistema de alerta.

- API de WhatsApp (como Twilio ou Meta): Interface para o envio de mensagens.

- Linguagem de Programação: C/C++ utilizando o framework Arduino.

- Sensor de Estado: Opcional para verificar condições adicionais, como movimento ou queda.

- Alimentação: Bateria ou fonte de energia para funcionamento autônomo.

## Funcionalidades

- Envio de mensagens automáticas via WhatsApp ao pressionar o botão.
  
- Possibilidade de configurar contatos destinatários através do código.

- Conexão Wi-Fi para acesso à internet.

- Opção de personalização da mensagem enviada.

## Como Usar

1. Configure a rede Wi-Fi e os contatos destinatários no código.

2. Carregue o código no ESP32 usando o Arduino IDE.

3. Instale o dispositivo em um local acessível.

4. Pressione o botão em caso de emergência para disparar o alerta.

5. Este projeto visa oferecer uma solução simples, acessível e eficaz para emergências, utilizando tecnologias modernas e de baixo custo.
