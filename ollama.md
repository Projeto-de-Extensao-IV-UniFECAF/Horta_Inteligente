
# 🦦 Instalando e Usando Ollama na VPS Ubuntu

Este guia descreve os passos para instalar o Ollama em uma VPS (Virtual Private Server) rodando Ubuntu, e como baixar e gerenciar modelos de linguagem. Ollama permite que você execute modelos de linguagem grandes (LLMs) localmente.

## 💡 Considerações Iniciais para VPS

- **Recursos da VPS:** Modelos de linguagem podem ser exigentes.
  - **RAM:** Para uma VPS com 4GB de RAM, você estará limitado a modelos menores (ex.: ~3 bilhões de parâmetros como Phi-3 Mini, Gemma:2b). Modelos maiores exigirão significativamente mais RAM.
  - **Espaço em Disco:** Cada modelo ocupa alguns gigabytes de espaço. Certifique-se de ter espaço suficiente.
  - **CPU:** Um bom processador ajudará na velocidade de inferência.
- **Acesso Root/Sudo:** Você precisará de privilégios `sudo` para a instalação.

---

## ⚙️ 1. Instalação do Ollama

O Ollama fornece um script de instalação que simplifica o processo.

### 1.1. Baixar e Executar o Script de Instalação

O comando a seguir baixa e executa o script de instalação oficial do Ollama:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Este script detectará automaticamente o seu sistema operacional e arquitetura. Ele também tentará configurar o Ollama para usar uma GPU NVIDIA, se detectada e corretamente configurada (veja a seção sobre Suporte a GPU).

### 1.2. Verificar a Instalação

Após a conclusão do script, verifique se o Ollama foi instalado corretamente:

```bash
ollama --version
```

Isso deve exibir a versão instalada do Ollama.

---

## ⚙️ 2. Gerenciando o Serviço Ollama

O script de instalação geralmente configura o Ollama para rodar como um serviço `systemd`.

### 2.1. Verificar o Status do Serviço

```bash
sudo systemctl status ollama
```

Você deve ver que o serviço está **"active (running)"**.

### 2.2. Comandos Comuns do Serviço (se necessário)

- **Iniciar o serviço Ollama:**

```bash
sudo systemctl start ollama
```

- **Parar o serviço Ollama:**

```bash
sudo systemctl stop ollama
```

- **Habilitar o Ollama para iniciar no boot:**

```bash
sudo systemctl enable ollama
```

- **Desabilitar o Ollama de iniciar no boot:**

```bash
sudo systemctl disable ollama
```

- **Reiniciar o serviço (após alguma configuração, por exemplo):**

```bash
sudo systemctl restart ollama
```

---

## 🚀 3. Baixando e Gerenciando Modelos

Com o Ollama instalado e o serviço rodando, você pode baixar e executar modelos.

### 3.1. Baixar um Modelo (Pull)

Para baixar um modelo sem executá-lo imediatamente, use `ollama pull`. Você pode encontrar uma lista de modelos disponíveis na biblioteca Ollama.

- Por exemplo, para baixar o modelo **phi3:mini** (um modelo menor, adequado para sistemas com menos RAM):

```bash
ollama pull phi3:mini
```

- Ou um modelo um pouco maior, como **gemma:2b**:

```bash
ollama pull gemma:2b
```

### 3.2. Executar um Modelo Interativamente (Run)

Para baixar (se ainda não baixado) e executar um modelo interativamente no terminal:

```bash
ollama run phi3:mini
```

Isso abrirá um prompt onde você pode conversar com o modelo. Para sair, digite `/bye`.

### 3.3. Listar Modelos Baixados Localmente

Para ver todos os modelos que você já baixou:

```bash
ollama list
```

### 3.4. Remover um Modelo Local

Se precisar liberar espaço, você pode remover um modelo:

```bash
ollama rm <nome_do_modelo>
```

Exemplo:

```bash
ollama rm phi3:mini
```

---

## 🔥 4. Acessando a API do Ollama Remotamente (Opcional)

Por padrão, o Ollama serve sua API na porta **11434** e escuta em **localhost**. Se você deseja que a API seja acessível de outras máquinas (ex.: da sua máquina local para a VPS), você precisará:

### 4.1. Configurar o Ollama para Escutar em Todos os IPs

Edite o serviço systemd do Ollama para definir a variável de ambiente `OLLAMA_HOST`.

- Crie um arquivo de override para o serviço:

```bash
sudo systemctl edit ollama.service
```

- Adicione o seguinte conteúdo no editor que abrir, salve e feche:

```ini
[Service]
Environment="OLLAMA_HOST=0.0.0.0"
```

- Recarregue o systemd e reinicie o Ollama:

```bash
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

### 4.2. Abrir a Porta no Firewall da VPS (se estiver usando ufw)

```bash
sudo ufw allow 11434/tcp
sudo ufw reload
sudo ufw status
```

Agora você deve conseguir acessar a API do Ollama de outra máquina usando:

```
http://<IP_DA_SUA_VPS>:11434
```

⚠️ **Atenção:** Expor a API publicamente sem medidas de segurança adicionais pode ser um risco. Considere usar um firewall para restringir o acesso apenas aos seus IPs, ou configurar um proxy reverso com autenticação.

---

## 🔌 5. (Opcional) Suporte a GPU NVIDIA

Se sua VPS possui uma GPU NVIDIA compatível, o Ollama pode utilizá-la para uma inferência muito mais rápida.

### 5.1. Pré-requisitos

- **Drivers NVIDIA:** Os drivers NVIDIA apropriados devem estar instalados.
- **NVIDIA Container Toolkit:** Embora o Ollama não rode em contêineres por padrão, suas dependências de GPU são similares.

O script de instalação:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

tentará detectar automaticamente uma GPU NVIDIA e seus drivers. Se bem-sucedido, o Ollama usará a GPU.

### 5.2. Verificação

Ao executar um modelo, o Ollama geralmente indica se está usando a GPU. Você também pode monitorar o uso da GPU com:

```bash
nvidia-smi
```

em outro terminal enquanto um modelo está carregado ou processando.

Se você instalar os drivers NVIDIA **após** ter instalado o Ollama, pode ser necessário **reinstalar o Ollama** ou garantir que as bibliotecas CUDA estejam acessíveis para ele.

---

Se quiser, posso gerar esse guia em PDF ou HTML pra você. Quer?
