
# 🌱 Configuração do Ambiente para Horta Inteligente em VPS Ubuntu

Este guia descreve os passos para instalar e configurar o Node-RED e o broker MQTT Mosquitto em uma VPS (Virtual Private Server) rodando Ubuntu. Este ambiente será utilizado para receber dados de um ESP32 e monitorar parâmetros de uma horta inteligente.

## 🔧 Especificações da VPS (Exemplo)
- **OS:** Ubuntu (ex: 20.04 LTS, 22.04 LTS)
- **RAM:** 4GB
- **ROM:** 80GB

---

## ⚙️ 1. Atualização Inicial do Sistema

É sempre uma boa prática começar atualizando a lista de pacotes e os pacotes já instalados.

```bash
sudo apt update
sudo apt upgrade -y
```

---

## 📡 2. Instalação do Mosquitto MQTT Broker

Mosquitto é um broker de mensagens open source que implementa o protocolo MQTT.

### 2.1. Instalar Mosquitto

```bash
sudo apt install mosquitto mosquitto-clients -y
```

O pacote `mosquitto-clients` é útil para testes, pois fornece utilitários de linha de comando para publicar e subscrever mensagens MQTT.

### 2.2. Habilitar e Iniciar o Serviço Mosquitto

```bash
sudo systemctl enable mosquitto
sudo systemctl start mosquitto
```

### 2.3. Verificar o Status do Serviço

```bash
sudo systemctl status mosquitto
```

Você deve ver uma saída indicando que o serviço está "**active (running)**". Pressione `q` para sair.

### 2.4. Configurar o Firewall (se aplicável)

Se você estiver usando o `ufw` (Uncomplicated Firewall), precisará permitir o tráfego nas portas padrão do MQTT:

```bash
sudo ufw allow 1883/tcp
sudo ufw allow 9001/tcp
sudo ufw reload
sudo ufw status
```

---

## 💻 3. Instalação do Node.js e NPM

Node-RED é construído sobre Node.js, então precisamos instalá-lo primeiro. Recomenda-se usar uma versão LTS (Long Term Support).

### 3.1. Instalar Node.js (usando NodeSource)

Para Node.js v18.x (LTS atual na data de criação deste guia):

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Se preferir outra versão LTS (ex.: 20.x), substitua `setup_18.x` por `setup_20.x` no comando acima.

### 3.2. Verificar as Versões

```bash
node -v
npm -v
```

---

## 🧠 4. Instalação do Node-RED

Com Node.js e NPM instalados, podemos instalar o Node-RED globalmente.

### 4.1. Instalar Node-RED

```bash
sudo npm install -g --unsafe-perm node-red
```

### 4.2. Executando o Node-RED (Teste Inicial)

Para testar, você pode executar o Node-RED diretamente no terminal:

```bash
node-red
```

Você verá logs no terminal, incluindo a URL para acessar o editor (normalmente `http://SEU_IP_DA_VPS:1880`). Pressione `Ctrl + C` para parar.

### 4.3. Configurar Node-RED para Executar como um Serviço (usando PM2)

Para manter o Node-RED rodando em background e reiniciá-lo automaticamente, usaremos o PM2.

#### 4.3.1. Instalar PM2

```bash
sudo npm install -g pm2
```

#### 4.3.2. Iniciar Node-RED com PM2

```bash
pm2 start $(which node-red) -- -v
```

> **Nota:**  
Se for rodar o PM2 com `sudo` para gerenciar o Node-RED como um serviço de sistema (recomendado), use:

```bash
sudo pm2 start $(which node-red) --name node-red -- -v --userDir /root/.node-red
```

E ajuste os comandos subsequentes do PM2 para também usar `sudo`.

#### 4.3.3. Configurar PM2 para Iniciar na Inicialização do Sistema

```bash
pm2 startup
```

Este comando irá gerar outro comando que você precisa executar, algo como:

```bash
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u seu_usuario --hp /home/seu_usuario
```

Se estiver usando `sudo pm2` para tudo, pode usar diretamente:

```bash
sudo pm2 startup systemd -u root --hp /root
```

#### 4.3.4. Salvar a Configuração do PM2

```bash
pm2 save
```

Ou, se estiver usando `sudo`:

```bash
sudo pm2 save
```

#### 4.3.5. Comandos Úteis do PM2

- Verificar status dos processos:

```bash
pm2 status
```

- Monitorar logs:

```bash
pm2 logs node-red
```

- Parar Node-RED:

```bash
pm2 stop node-red
```

- Reiniciar Node-RED:

```bash
pm2 restart node-red
```

- Remover Node-RED do PM2:

```bash
pm2 delete node-red
```

> Lembre-se de usar `sudo pm2 ...` se estiver rodando como root.

---

### 4.4. Configurar o Firewall para Node-RED

Se estiver utilizando `ufw`, libere a porta padrão do Node-RED (1880):

```bash
sudo ufw allow 1880/tcp
sudo ufw reload
sudo ufw status
```

---

## 🔗 5. Acessando o Node-RED

Após a configuração, você poderá acessar o editor do Node-RED em seu navegador através do endereço:

```
http://<IP_DA_SUA_VPS>:1880
```

Substitua `<IP_DA_SUA_VPS>` pelo endereço IP público da sua VPS.

---
