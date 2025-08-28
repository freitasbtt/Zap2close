# 🔄 Fluxo de Mensagens no N8N

Este documento descreve o fluxo básico de mensagens do sistema WhatsApp SDR usando **N8N**, **Evolution API** e **Redis**.

---

## 📌 Visão Geral
O N8N é responsável por orquestrar o processamento das mensagens recebidas e enviadas.  
Ele se conecta à Evolution API para receber eventos e processa os dados de acordo com as regras de cada cliente.

---

## 📥 Entrada de Mensagens
1. O cliente envia mensagem via WhatsApp.
2. A Evolution API captura o evento e envia para o N8N.
3. O Redis armazena temporariamente a mensagem na fila.

---

## 🛠 Processamento no N8N
- Verifica **qual cliente** enviou a mensagem (identificação via token/sessão).
- Consulta **regras de qualificação** (ex.: se a mensagem contém palavra-chave X, seguir para fluxo Y).
- Integra com APIs externas se necessário (CRM, planilhas, e-mail).
- Gera a resposta automática ou encaminha para atendimento humano.

---

## 📤 Saída de Mensagens
- O N8N envia a resposta para a Evolution API.
- A Evolution API transmite a resposta para o WhatsApp do cliente.

---

## 🗂 Estrutura de Fluxos
- **Fluxo principal**: Controla mensagens de todos os clientes (multi-tenant).
- **Subfluxos**: Regras específicas de cada cliente.
- **Webhook Node**: Recebe mensagens da Evolution API.
- **Function Node**: Processa dados.
- **HTTP Request Node**: Envia mensagens de volta.

---

## 🖼 Diagrama Simplificado
1. Mensagem chega no WhatsApp

O cliente final envia uma mensagem para o número conectado na Evolution API.

A Evolution dispara um Webhook para o n8n com os dados crus da mensagem (remoteJid, conversation, instance, apikey etc).

2. Webhook Node (n8n)

O Webhook Node recebe o evento HTTP da Evolution.

Esse é o gatilho do fluxo (start).

3. Code Node (normalização)

O node Code transforma os dados crus em variáveis mais limpas:

chatInput → texto da mensagem recebida.

sessionId → identificador completo do contato (553171155225@s.whatsapp.net).

number → número puro (553171155225).

instance → nome da instância na Evolution (clienteteste).

apikey → chave de autenticação da instância.

sessionKey → chave formatada para o Redis (clienteteste_553171155225).

👉 Essa é a base para garantir que cada mensagem seja única e não se misture.

4. AI Agent (IA + Memória)

O AI Agent recebe o chatInput.

Ele está conectado a:

Google Gemini Chat Model → gera a resposta.

Redis Chat Memory → guarda o histórico daquela conversa (sessionKey).

👉 Isso faz com que cada usuário tenha seu próprio contexto de conversa isolado.

5. AI Output

O AI Agent devolve um campo output com a resposta gerada pela IA.
Exemplo: "Olá, tudo bem? Posso te ajudar com nossos produtos 🚗".

6. HTTP Request (envio da resposta)

O fluxo monta um POST para a Evolution API:

URL: http://evolution_api:8080/message/sendText/{{instance}}

Headers: apikey (da instância)

Body:

{
  "number": "553171155225",
  "text": "Olá, tudo bem? Posso te ajudar com nossos produtos 🚗"
}


A Evolution envia essa mensagem de volta para o WhatsApp do cliente final.
![FLuxo n8n](images/fluxo_n8n_whatsapp_sdr.png)
---

## 📌 Observações
- Fluxo único com diferenciação por cliente reduz custo e complexidade.
- Redis garante que mensagens sejam processadas na ordem correta.
- Possível criar fluxos separados apenas para clientes de maior volume.
- Postgree

