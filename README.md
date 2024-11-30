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

## Código Atualizado

#include <WiFi.h> // Biblioteca para conectar o ESP32 ao WiFi
#include <HTTPClient.h> // Biblioteca para fazer requisições HTTP //
#include <UrlEncode.h>  // Biblioteca para codificar texto em formato de URL, 
                        // disponível na lista de bibliotecas do Arduino: "UrlEncode" de Masayuki Sugahara

// Definição das credenciais do WiFi
const char* ssid = "CIT_Alunos"; // Nome da rede WiFi para o ESP fazer a conexão com a internet. Exemplo: "Wificasa", sempre entre aspas duplas
const char* password = "alunos@2024"; // Senha do WiFi. Exemplo: "12345678"

// Definição dos pinos do botão e LEDs
#define botao 21 // Pino de conexão no ESP para o botão
#define led1 23  // Pino conexão no ESP para o LED externo PROTOBOARD (acende ao enviar mensagem)
#define led2 2   // Pino para o LED inteno do ESP (indica o status de conexão)

bool flag = 1; // Variável de controle para evitar múltiplos envios da mensagem

// Número de telefone e chave da API para CallMeBot
String phoneNumber1 = "559591402835"; // Número de telefone para enviar mensagem (Brasil 55) Exemplo: 5595991441665
String apiKey1 = "7059560"; // Chave de API para autenticação no CallMeBot, é dada pelo whats CallMeBot após enviar a mensagem: "I allow callmebot to send me messages"

String phoneNumber2 = "559581042496"; // Número de telefone para enviar mensagem (Brasil 55) Exemplo: 5595991441665
String apiKey2 = "8962654";

// Função para enviar a mensagem via CallMeBot API
void sendMessage(String phoneNumber, String apiKey, String message) {
 // Monta a URL com os parâmetros: número, chave da API e mensagem
String url = "https://api.callmebot.com/whatsapp.php?phone=" + phoneNumber + "&apikey=" + apiKey + "&text=" + urlEncode(message);
 HTTPClient http; // Cria um objeto HTTPClient
 http.begin(url); // Inicia a conexão com a URL
 // Adiciona o cabeçalho indicando que a requisição está no formato x-www-form-urlencoded
 http.addHeader("Content-Type", "application/x-www-form-urlencoded");
 // Envia a requisição POST e armazena o código de resposta HTTP
 int httpResponseCode = http.POST(url);
 if (httpResponseCode == 200) { // Código 200 indica sucesso
 Serial.println("Mensagem enviada com sucesso");
 } else { // Qualquer outro código indica erro
 Serial.println("Erro no envio da mensagem");
 Serial.print("HTTP response code: ");
 Serial.println(httpResponseCode);
 }
 http.end(); // Libera e fecha os recursos usados pela requisição HTTP
}

void setup() {
 Serial.begin(115200); // Inicia a comunicação serial com a velocidade de 115200
 pinMode(botao, INPUT_PULLUP); // Configura o pino do botão como entrada com pull-up interno (retorna 0 ou 1 de acordo com o valor de tensão lido)
 pinMode(led1, OUTPUT); // Configura o pino do LED1 como saída
 pinMode(led2, OUTPUT); // Configura o pino do LED2 como saída
 digitalWrite(led1, LOW); // Inicialmente, os LEDs ficam apagados
 digitalWrite(led2, LOW);
 WiFi.begin(ssid, password); // Inicia a conexão com o WiFi
 Serial.println("Conectando");

 // Enquanto não estiver conectado ao WiFi, pisca o LED2
 while (WiFi.status() != WL_CONNECTED) {
 delay(500); // Aguarda 500 ms
 Serial.print("."); // Imprime um ponto para indicar tentativa de conexão
 digitalWrite(led2, !digitalRead(led2)); // Inverte o estado do LED2 (pisca)
 }

 // Quando conectado ao WiFi
 Serial.println("");
 Serial.print("Conectado ao WiFi neste IP");
 digitalWrite(led2, HIGH); // Mantém o LED2 aceso indicando conexão bem-sucedida
 Serial.println(WiFi.localIP()); // Exibe o IP local do ESP32 na rede
}

void loop() {
 int estado_botao = digitalRead(botao); // Lê o estado do botão
 if (estado_botao == 0) { // Se o botão esƟver pressionado (estado LOW)
 Serial.println("Botão Pressionado, enviando mensagem");
 digitalWrite(led1, HIGH); // Acende o LED1 enquanto a mensagem está sendo enviada
 if (flag) { // Verifica se a flag está ativada (evita envio repetido)
 sendMessage(phoneNumber1, apiKey1, "SOCORRO!!!!, ME AJUDE");
 sendMessage(phoneNumber2, apiKey2, "SOCORRO!!!!, Help me"); // Envia a mensagem de socorro Ajustável
 flag = 0; // Desativa a flag após o envio
 }
 } else {
 flag = 1; // Se o botão não estiver pressionado, reseta a flag
 digitalWrite(led1, LOW); // Apaga o LED1
 }
}
