# Horta_Inteligente
Automação e otimização de plantio utilizando IOT e IA.

# 🌱 PlantiA: Automação com ESP32 + Node-Red + MQTT (Mosquitto)

Automação de plantio utilizando Node-RED, MQTT (Mosquitto), ESP32, DHT11 e Sensor de Umidade do Solo Capacitivo.

---

## 🧠 Node-RED (Interface UI e Plataforma de Desenvolvimento)

### 🔎 O que é?

O Node-RED permite criar aplicações que coletam, transformam e visualizam dados por meio de fluxos. É conhecido como a "máquina virtual do JavaScript". Desenvolvido em C++, é open-source e otimizado para acelerar aplicações JavaScript.

---

### 💻 Instalação (Node.js + Node-RED)

1. Baixe o **Node.js LTS (versão recomendada)** em:  
   👉 https://nodejs.org/

2. Após a instalação, abra o **Prompt de Comando (CMD)** e execute:

   ```bash
   npm install -g --unsafe-perm node-red
   ```

> O `--unsafe-perm` evita problemas de permissão em alguns sistemas operacionais.

---

### ▶️ Como Iniciar o Node-RED

1. No **CMD**, digite:

   ```bash
   node-red
   ```

2. Acesse no navegador:

   ```
   http://127.0.0.1:1880/
   ```

> 💡 *Recomenda-se configurar o Node-RED como um serviço do sistema para iniciar automaticamente com o Windows.*

---

### 📊 Acessar a Dashboard

```
http://localhost:1880/ui
```

---

## 🛰️ Eclipse Mosquitto MQTT Broker

### 🔎 O que é?

Mosquitto é um broker MQTT de código aberto, leve e ideal para aplicações IoT. Ele gerencia mensagens entre clientes, filtrando e distribuindo-as conforme as assinaturas dos tópicos.

---

### 💻 Instalação (Windows)

1. Acesse o site oficial:  
   👉 https://mosquitto.org/download/

2. Baixe o instalador para Windows e execute-o.

3. Durante a instalação, marque a opção **"Service installation"** para que o Mosquitto seja instalado como serviço.

4. Adicione as pastas `mosquitto` e `mosquitto\src` ao **Path** do sistema (variáveis de ambiente), se necessário.

---

### ▶️ Como Iniciar

1. Para executar manualmente, abra o CMD e digite:

   ```bash
   mosquitto -v
   ```

2. Se instalado como serviço, o Mosquitto será iniciado automaticamente com o Windows.

---

### 🧪 Teste de Envio e Recebimento

**1. Em um terminal (assinando tópico):**

```bash
mosquitto_sub -h localhost -p 1883 -t "temperatura"
```

**2. Em outro terminal (publicando mensagem):**

```bash
mosquitto_pub -h localhost -p 1883 -t "temperatura" -m "25.6°C"
```

---

### 📚 Parâmetros Comuns

| Parâmetro | Significado               |
|-----------|---------------------------|
| `-v`      | Verbose (modo detalhado)  |
| `-h`      | Host                      |
| `-p`      | Porta                     |
| `-t`      | Tópico                    |
| `-m`      | Mensagem                  |
| `-q`      | QoS (Qualidade de Serviço)|
| `sub`     | Assinatura (subscribe)    |
| `pub`     | Publicação (publish)      |

---

### 🔐 Autenticação com `mosquitto_passwd`

Crie um usuário e senha:

```bash
mosquitto_passwd -c senha.txt nome_de_usuario
```

Edite o arquivo `mosquitto.conf` para incluir:

```conf
allow_anonymous false
password_file senha.txt
```

Referência:  
[Secure Mosquitto com Certificados - Medium](https://medium.com/gravio-edge-iot-platform/how-to-set-up-a-mosquitto-mqtt-broker-securely-using-client-certificates-82b2aaaef9c8)

---

### 📡 QoS (Quality of Service)

- **QoS 0** – Entrega no máximo uma vez (sem confirmação)
- **QoS 1** – Entrega pelo menos uma vez (confirmação simples)
- **QoS 2** – Entrega exatamente uma vez (confirmações de ida e volta)

---

## 📚 Bibliografia e Referências

### 📁 Repositório Base

- https://github.com/joaoalexarruda/esp32_dht11_mosquitto_node-red
- https://github.com/joaoalexarruda/esp32_dht11_mosquitto_node-red?tab=readme-ov-file

### 🎥 Tutoriais

- [Monitoramento com ESP32 + DHT11 + Node-RED](https://www.youtube.com/watch?v=OM4QDv2lu24)
- [Automate Your Life With Node-RED + MQTT](https://hackaday.com/2020/01/15/automate-your-life-with-node-red-plus-a-dash-of-mqtt/)
- [Como rodar o ChatGPT em projetos Arduino](https://www.youtube.com/watch?v=EAwh4ul-K0g)
