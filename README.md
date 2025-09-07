# 💡 IoT Lâmpada Inteligente com ESP32, MQTT e FIWARE

## 📌 Descrição do Projeto
Este projeto implementa um **sistema IoT** utilizando **ESP32** integrado com um **broker MQTT** e a plataforma **FIWARE**.  
O dispositivo é capaz de:
- Conectar-se a uma rede Wi-Fi.  
- Assinar tópicos MQTT para receber comandos de ligar/desligar um LED (simulando uma lâmpada inteligente).  
- Publicar seu estado atual (`on`/`off`) para o broker.  
- Ler valores de luminosidade de um sensor analógico (simulado com potenciômetro).  
- Enviar periodicamente a luminosidade para o broker MQTT.  
- Integrar-se ao **FIWARE Orion Context Broker**, que coleta e armazena os dados enviados pelo ESP32, permitindo monitoramento e análise.  

---

## 👥 Integrantes do Grupo
- Alexandre Wesley – 561622  
- Gabriel Ferreira Machado – 562330  
- João Paulo Santana Basta – 565383  
- João Stellare – 565813  
- Kauê de Almeida Pena – 564211 

**Youtube:** https://youtu.be/_4C-xnb_rjM
**Wokwi:** https://wokwi.com/projects/441468148965036033
**Grupo:** Data-Vine  
**Turma:** 1ESPF  

---

## 🛠️ Tecnologias e Bibliotecas Utilizadas
- **Placa:** ESP32  
- **Linguagem:** C++ (Arduino IDE / PlatformIO)  
- **Bibliotecas:**  
  - `WiFi.h` → conexão com rede Wi-Fi  
  - `PubSubClient.h` → comunicação MQTT  
- **Plataforma IoT:** [FIWARE](https://www.fiware.org/)  
  - **Orion Context Broker** → recebe e gerencia o contexto (estado do LED e luminosidade).  
  - Comunicação via **MQTT IoT Agent**.  

---

## ⚙️ Configurações Principais
- **SSID e Senha Wi-Fi:** configurados no código (`default_SSID` e `default_PASSWORD`).  
- **Broker MQTT:**  
  - IP: `20.49.4.108`  
  - Porta: `1883`  
- **Tópicos MQTT:**  
  - **Subscribe (escuta):** `/TEF/lamp001/cmd`  
  - **Publish (estado do LED):** `/TEF/lamp001/attrs`  
  - **Publish (luminosidade):** `/TEF/lamp001/attrs/l`  
- **ID MQTT:** `fiware_001`  
- **Pino do LED:** `D4` (GPIO 2 do ESP32)  
- **Pino do Sensor:** GPIO 34 (entrada analógica para potenciômetro/luminosidade).  

---

## 📡 Fluxo de Funcionamento
1. O ESP32 conecta-se à rede Wi-Fi.  
2. Em seguida, conecta-se ao broker MQTT.  
3. O dispositivo **escuta comandos** via tópico `/TEF/lamp001/cmd`:  
   - `lamp001@on|` → Liga LED  
   - `lamp001@off|` → Desliga LED  
4. O estado atual do LED é **publicado** no tópico `/TEF/lamp001/attrs`.  
5. O valor de luminosidade lido do potenciômetro é **publicado** no tópico `/TEF/lamp001/attrs/l`.  
6. O **FIWARE IoT Agent MQTT** recebe esses dados e os encaminha para o **Orion Context Broker**, armazenando o estado e atributos da entidade "lamp001".  

---

## 📋 Estrutura do Código
- `initWiFi()` → Inicializa e mantém a conexão Wi-Fi.  
- `initMQTT()` → Configura o broker MQTT e callback de mensagens.  
- `mqtt_callback()` → Processa mensagens recebidas no tópico de comando.  
- `InitOutput()` → Inicializa o LED com piscadas de teste.  
- `EnviaEstadoOutputMQTT()` → Envia estado atual do LED ao broker.  
- `handleLuminosity()` → Lê o valor analógico do sensor (potenciômetro) e publica luminosidade.  
- `VerificaConexoesWiFIEMQTT()` → Garante reconexão ao Wi-Fi e broker em caso de falha.  

---

## 🚀 Como Executar
1. Configure o ESP32 no **Arduino IDE** ou **PlatformIO**.  
2. Instale as bibliotecas:  
   - `WiFi`  
   - `PubSubClient`  
3. Altere, se necessário, as credenciais de rede Wi-Fi no código:  
   ```cpp
   const char* default_SSID = "Seu-WiFi";
   const char* default_PASSWORD = "Sua-Senha";
