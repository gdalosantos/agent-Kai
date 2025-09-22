# agent-Kai
assistente pessoal inteligente
# Agent-Kai – Agente Pessoal Multilingue inteligente

## Visão Geral
O Kai é um agente pessoal que integra captura de comandos, classificação de intenção e execução de tarefas do dia a dia (listas, calendários, mensagens).  
O projeto busca ser **multilingue (PT/EN/DE)** e de baixo custo, usando ferramentas no-code/low-code.

## Objetivos
- Automatizar criação de listas de compras e tarefas (Todoist).
- Integrar com GPT para classificação e enriquecimento dos comandos.
- Permitir envio de mensagens para marido (via Signal/Telegram).
- Manter arquitetura flexível para evoluir em versões (v0.1, v0.2…).

## Stack Atual
- **Make (ex-Integromat)** – orquestração de cenários.
- **Todoist** – gestão de tarefas.
- **Telegram Bot** – canal inicial de comunicação (substituível por Signal).
- **OpenAI GPT API** – processamento e classificação de intenções.
- **Atalhos iOS** – ponto de entrada para comandos manuais.
- **Google Calendar** – integração planejada para eventos.

## Estrutura de Arquivos
- `README.md` – visão geral.
- `ROADMAP.md` – versões e milestones.
- `DECISIONS.md` – registro de decisões e alternativas.
- `CHANGELOG.md` – histórico de alterações.

## Status Atual
- ✅ Captura de comandos via Telegram Bot e Atalho iOS.  
- ✅ Integração com Make + Todoist.  
- 🚧 Classificação automática de comandos via GPT.  
- 🚧 Integração com Google Calendar.  
- ⏳ Implementação no Signal.
