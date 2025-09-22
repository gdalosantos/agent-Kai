# Projeto Kai – Agente Pessoal Multilingue

## Visão Geral

O **Kai** é um agente pessoal capaz de receber comandos via texto ou voz, processá-los automaticamente com classificação inteligente e executar ações práticas: criar tarefas, listas de compras, eventos, enviar convites, realizar buscas externas com resposta mastigada e até responder e-mails com confirmação humana. Fluxo multicanal, multilíngue (PT/EN/DE), seguro e extensível.

## Objetivos

- Captura de comandos por texto (Telegram/Signal) ou voz (Shortcuts iOS → Whisper)
- Decisão inteligente: tarefa, lista de compras, evento, e-mail ou busca externa
- Execução automática no serviço adequado: Todoist, Google Calendar, Gmail, pesquisa via Perplexity
- Logs/análise do pipeline e fallback para confirmação manual quando necessário

## Fluxo Geral do Kai (Mermaid)

Cole no [Mermaid Live Editor](https://mermaid.live/) para visualizar:

flowchart TD
A["Entrada: Texto (Telegram/Signal)"] --> C{"Classificador GPT"}
A2["Entrada: Voz (Shortcuts iOS)"] --> B["Whisper: transcrição PT/EN/DE"] --> C

C -->|Tarefa| D["Todoist: criar tarefa"]
C -->|Lista de compras| E["Todoist: lista de compras (subtarefas)"]
C -->|Evento| F["Google Calendar: criar evento"]
F --> G["Convidar contatos via Google Contacts"]
C -->|Busca| H["Perplexity: busca externa"]
H --> I["Resumo mastigado/links"]
I --> J["Gmail: enviar resultado por e-mail"]

C -->|E-mail| K["Gmail: criar rascunho de resposta"]
K --> L{"Confirmação humana"}
L -->|Confirma| M["Gmail: enviar e-mail"]

C -->|Baixa confiança| N["Fallback: pedir confirmação curta"]


## Funcionalidades

- **MVP (v0.1):** Comandos de voz/texto, classificação, criação de tarefas/listas no Todoist, logs e fallback
- **v0.2:** Criação de eventos no Calendar, convidados, validação de dados
- **v0.3:** Busca externa (Perplexity), entregando respostas mastigadas/links, envio por e-mail
- **v0.4:** Rascunho automático de e-mails/respostas, confirmação obrigatória antes do envio
- **v1.0:** Robustez, auditoria, multilíngue PT/EN/DE, multicanal Telegram/Signal, painel de analytics

## Recuperação Rápida & Auditoria

- Exportar frequentemente blueprint JSON dos cenários do Make para GitHub
- Log/auditoria completo de cada request/response
- Retentativas automáticas em módulos falhando
- Estrutura clara de projetos/labels no Todoist, eventos e convites padronizados no Calendar

---


