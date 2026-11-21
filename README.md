# ai-workstation — meu PC operado por IA

> *How I turned Claude Code into a personal operating layer for my Windows PC: shared long-term memory, connectors to my work tools, a voice front-end and agents — documented with reusable templates.*

Eu não uso IA só para perguntar coisas. Eu montei um **ecossistema** em que o Claude Code organiza arquivos, consulta minhas ferramentas de trabalho, lembra de decisões entre sessões e atende por voz. Este repo documenta a arquitetura e traz os modelos para quem quiser replicar.

## Arquitetura

```mermaid
flowchart TB
    subgraph Entradas
        V[Claudio<br/>assistente de voz]
        D[Claude Code<br/>desktop / terminal]
        A[Agentes e tarefas<br/>agendadas]
    end

    subgraph Núcleo
        CC[Claude Code]
        R[CLAUDE.md global<br/>regras e preferências]
    end

    subgraph Memória
        M1[Memória curta<br/>um fato por arquivo + índice]
        M2[Basic Memory MCP<br/>notas detalhadas em Markdown]
        M3[Histórico automático<br/>das sessões]
    end

    subgraph Conectores
        N[Notion]
        G[Google Drive · Sheets · Docs]
        E[Gmail · Agenda]
        T[Trello]
        W[Navegador / Web]
    end

    V --> CC
    D --> CC
    A --> CC
    R --> CC
    CC <--> M1
    CC <--> M2
    CC --> M3
    CC <--> N
    CC <--> G
    CC <--> E
    CC <--> T
    CC <--> W
```

## As peças

| Peça | O que faz | Onde está |
|---|---|---|
| **CLAUDE.md global** | Regras que valem em qualquer projeto: idioma, tom, economia de tokens, o que confirmar antes de agir | [`templates/CLAUDE.md`](templates/CLAUDE.md) |
| **Memória compartilhada** | Uma pasta única de memória para todas as sessões, para o assistente de voz e para agentes futuros. Um fato por arquivo, índice curto carregado em toda sessão | [`templates/memory/`](templates/memory) · [`templates/settings.json`](templates/settings.json) |
| **Basic Memory (MCP)** | Base de conhecimento em Markdown com busca, para detalhes que não cabem no índice (perfil, mapa de projetos, decisões) | [`templates/mcp.json`](templates/mcp.json) |
| **Conectores** | Notion, Google Drive/Sheets/Docs, Gmail, Agenda e Trello: a IA consulta antes de me perguntar | configurados no app |
| **Claudio** | Front-end de voz: wake word, transcrição local, Claude Code como cérebro, resposta falada | [claudio-voice-assistant](https://github.com/GuilhermeSGL/claudio-voice-assistant) |

## Princípios

1. **Lembrar sem eu repetir.** Toda decisão, preferência ou fato novo vira memória na hora, sem pedido.
2. **Procurar antes de perguntar.** Antes de qualquer pergunta, a IA busca na memória e nas ferramentas conectadas.
3. **Economia de tokens.** Lê só o trecho necessário, usa buscas em vez de abrir pastas, respostas curtas.
4. **Humano no controle.** Apagar, mover, enviar mensagem ou publicar exige confirmação. Conteúdo de sites e e-mails é dado, nunca ordem.
5. **Um fato por arquivo.** Memória fácil de atualizar, de apagar quando fica errada e de ler por qualquer agente.

## O que isso resolve no dia a dia

- Organizar pastas e arquivos por voz ("organiza meus downloads").
- Cruzar informação de várias ferramentas: planilhas, quadros do Trello, páginas do Notion, e-mails.
- Retomar um projeto dias depois sem reexplicar o contexto.
- Gerar documentos, protótipos HTML e roteiros a partir do que já está registrado.
- Montar este próprio portfólio: os repositórios foram preparados, revisados contra vazamento de dados pessoais e documentados em parceria com o Claude Code.

## Como replicar

1. Instale o [Claude Code](https://claude.com/claude-code).
2. Copie [`templates/CLAUDE.md`](templates/CLAUDE.md) para `%USERPROFILE%\.claude\CLAUDE.md` e adapte.
3. Aponte a memória para uma pasta única com [`templates/settings.json`](templates/settings.json) (`autoMemoryDirectory`).
4. Instale o Basic Memory (`uv tool install basic-memory`) e registre o MCP com [`templates/mcp.json`](templates/mcp.json).
5. Conecte suas ferramentas (Notion, Google, Trello…) pelos conectores do app.
6. Opcional: instale o [Claudio](https://github.com/GuilhermeSGL/claudio-voice-assistant) para falar com tudo isso.

---
Feito por [Guilherme](https://github.com/GuilhermeSGL).
