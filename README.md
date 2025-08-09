# Zap2close
Sistema de agente SDR para whatsapp com N8N, Evolution API e Docker

# WhatsApp SDR

Este projeto é um sistema de agente SDR que usa **N8N**, **Evolution API** e **Docker** para automação e atendimento via WhatsApp.

---

## 📌 Arquitetura
- **Servidor**: Hostinger VPS
- **Containerização**: Docker
- **Orquestração de fluxos**: N8N
- **Integração WhatsApp**: Evolution API
- **Banco de dados**: PostgreSQL + Redis (armazenamento de logs e mensagens)
- **Gerenciamento de múltiplos clientes**: multi-instância Evolution API

---

## 🔄 Fluxo básico
1. Cliente envia mensagem via WhatsApp.
2. Evolution API recebe a mensagem.
3. N8N processa o fluxo (qualificação, respostas, integrações).
4. Resposta enviada de volta pelo WhatsApp.

---

## 📂 Estrutura de pastas
/docs # Documentação
/src # Código fonte
/docker # Configurações de containers

---

## 🛠 Tecnologias
- [N8N](https://n8n.io/)
- [Docker](https://www.docker.com/)
- [Evolution API](https://github.com/...)
- PostgreSQL
- Redis
---

## ✏️ Autor
Pedro Bittencourt
