# Projeto Kai – Agente Pessoal Multilingue

## Visão Geral

O **Kai** é um agente pessoal que integra captura de comandos, classificação de intenções e execução automática de tarefas via voz ou texto, com foco multilingue (PT/EN/DE) e baixo custo usando ferramentas no-code/low-code.

## Objetivos

- Entrada mínima: **texto (Telegram/Signal)** ou **voz (Shortcuts iOS → Whisper)**
- Decidir entre **Tarefa** ou **Lista de Compras** e adicionar no Todoist
- Criar **eventos no Google Calendar e enviar convites** (v0.2+)
- Responder a e-mails com rascunho e confirmação humana (v0.3+)
- Implementar logs básicos e fallback manual

## Fluxograma MVP (v0.1)

Cole este diagrama no [Mermaid Live Editor](https://mermaid.live/) para visualizar:

flowchart TD
A["Entrada: Texto (Telegram/Signal)"]
A2["Entrada: Voz (Shortcuts iOS)"]
B["Whisper: transcrição PT/EN/DE"]
C{"Classificador GPT"}
D["Todoist: criar tarefa"]
E["Todoist: lista de compras (subtarefas)"]
Z["Fallback: pedir confirmação curta"]
A --> C
A2 --> B --> C
C -->|Tarefa| D
C -->|Lista de compras| E
C -->|Baixa confiança| Z
