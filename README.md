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

%% ========= LEGENDA =========
%% [MVP v0.1]  -> mínimo: decidir Tarefa vs Lista de Compras -> Todoist
%% [v0.2]      -> eventos no Calendar + convites p/ contatos
%% [v0.3+]     -> responder e-mails
%% Logs/erros em todos os estágios

%% ========= ENTRADAS =========
subgraph Entrada
    A1[Texto no Telegram/Signal\nou formulário simples]:::entry
    A2[Voz no iPhone Shortcuts\n(PT/EN/DE misturados)]:::entry
end

%% ========= TRANSCRIÇÃO & NL =========
subgraph Transcricao & Linguagem
    B1[Shortcuts envia áudio\n-> Make webhook]:::proc
    B2[Whisper API\nTranscrição + detecção de idioma]:::ai
    B3[Normalização de texto\n(limpeza, pontuação, timezone)]:::proc
end

%% ========= CLASSIFICAÇÃO =========
subgraph NLU & Roteamento
    C1[Classificador GPT\nTarefa vs Lista de Compras vs Evento vs Email]:::ai
    C2[Extração de entidades\n(título, data/hora, local, pessoas,\nlista de itens, prioridade, labels)]:::ai
    C3{Roteador}:::decision
end

%% ========= SAÍDAS =========
subgraph Saídas (Execução)
    D1[Todoist API\nCriar Tarefa]:::action
    D2[Todoist API\nAtualizar Lista de Compras\n(com subtarefas/labels)]:::action

    D3[Google Calendar API\nCriar evento]:::action
    D4[Contatos (Google Contacts)\nResolver e-mails dos convidados]:::proc
    D5[Enviar convite (Calendar)]:::action

    D6[Gmail API\nRascunho/Responder e-mail]:::action
end

%% ========= SUPORTE =========
subgraph Suporte & Observabilidade
    E1[Validação\n(regras: data futura,\nlista não-vazia, etc.)]:::guard
    E2[Logs & Auditoria (Make + Sheets/DB)]:::log
    E3[Tratamento de erros & Retentativas]:::guard
    E4[Fallback manual\n(mensagem para usuário)]:::fallback
end

%% ========= FLUXO PRINCIPAL =========
A1 --> C0[Pré-processamento leve\n(normaliza texto e remetente)]:::proc --> C1
A2 --> B1 --> B2 --> B3 --> C1
C1 --> C2 --> C3

%% ---------- Rotas ----------
C3 -->|Tarefa| D1
C3 -->|Lista de Compras| D2
C3 -->|Evento| E1
E1 -->|ok| D3
D3 --> D4 --> D5
E1 -->|falha| E4

C3 -->|Email (v0.3+)| D6

%% ========= OBSERVABILIDADE =========
A1 -.-> E2
A2 -.-> E2
B2 -.-> E2
C1 -.-> E2
C2 -.-> E2
C3 -.-> E2
D1 -.-> E2
D2 -.-> E2
D3 -.-> E2
D5 -.-> E2
D6 -.-> E2

%% ========= ERROS =========
B2 -->|timeout/erro| E3 --> E4
C1 -->|baixa confiança| E3 --> E4
D3 -->|data inválida| E3 --> E4
D5 -->|e-mail faltando| E3 --> E4

%% ========= ESTILOS =========
classDef entry fill:#eef,stroke:#669,stroke-width:1px
classDef proc fill:#f7f7f7,stroke:#999,stroke-width:1px
classDef ai fill:#eaffea,stroke:#6a6,stroke-width:1px
classDef decision fill:#fff3cd,stroke:#d8a800,stroke-width:1px
classDef action fill:#e8f4ff,stroke:#3a7bd5,stroke-width:1px
classDef guard fill:#ffecec,stroke:#d9534f,stroke-width:1px
classDef log fill:#f0e9ff,stroke:#7d55c7,stroke-width:1px
classDef fallback fill:#fff,stroke:#333,stroke-dasharray: 4 2
