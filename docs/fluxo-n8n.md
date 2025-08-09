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
WhatsApp Cliente
↓
Evolution API
↓
Redis (Fila)
↓
N8N (Processamento)
↓
Evolution API (Envio)
↓
WhatsApp Cliente
![Texto descritivo da imagem](images/NOME-DA-SUA-IMAGEM.png)
---

## 📌 Observações
- Fluxo único com diferenciação por cliente reduz custo e complexidade.
- Redis garante que mensagens sejam processadas na ordem correta.
- Possível criar fluxos separados apenas para clientes de maior volume.

