# Estrutura de Trabalho — Épicos, Features e User Stories (Agente IA/SAC)

> Organização hierárquica para o board do Azure DevOps: **Épico → Feature → User Story**.
>
> **Épicos (por plataforma/camada):**
> 1. **EPIC 1 — Front Site (Desktop/Web)**
> 2. **EPIC 2 — Front APP (Mobile)**
> 3. **EPIC 3 — Middleware (Integrações, IA e Serviços)**
>
> **Nota sobre USs transversais:** Histórias que exigem trabalho em mais de uma camada foram alocadas no épico onde está o maior esforço de entrega, com as dependências sinalizadas em "Dependências". Itens de backend/serviço (orquestração da IA, protocolos, transcrição, analytics) foram consolidados no EPIC 3 (Middleware).

---

## Mapa Geral (visão rápida)

| Épico | Feature | USs associadas |
|-------|---------|----------------|
| **1 — Front Site** | F1.1 Navegação e Entrada do Agente (Site) | D-US01, D-US12 |
| **1 — Front Site** | F1.2 Layout e Controles de Janela (Site) | D-US02, D-US03 |
| **1 — Front Site** | F1.3 Interação e Conteúdo (Site) | D-US04, D-US05, D-US06, D-US07, D-US08, D-US22 |
| **1 — Front Site** | F1.4 Anexos e Mídia (Site) | D-US11 |
| **1 — Front Site** | F1.5 Área do Cliente (Site) | D-US13 |
| **2 — Front APP** | F2.1 Navegação e Menu SAC (App) | M-US01, M-US02 |
| **2 — Front APP** | F2.2 Entrada do Agente (App) | M-US03, M-US04, M-US05, M-US06 |
| **2 — Front APP** | F2.3 Interação e Conteúdo (App) | M-US07, M-US08, M-US09, M-US10 |
| **2 — Front APP** | F2.4 Anexos e Mídia (App) | M-US13, M-US14, M-US15 |
| **2 — Front APP** | F2.5 Área do Cliente (App) | M-US16, M-US17 |
| **2 — Front APP** | F2.6 Resiliência e Feedback (App) | US18, US19, US21-app |
| **3 — Middleware** | F3.1 Orquestração da IA e Processamento | M-US11, D-US09 |
| **3 — Middleware** | F3.2 Transbordo e Protocolos | M-US12, D-US10 |
| **3 — Middleware** | F3.3 Transcrição e Mídia (serviços) | (suporte a M-US09/D-US06, M-US08/D-US07) |
| **3 — Middleware** | F3.4 Persistência de Histórico e Contexto | US20 |
| **3 — Middleware** | F3.5 Notificações e Eventos | US21-svc, US24 |
| **3 — Middleware** | F3.6 Tratamento de Erros e Validações | US23 |

> Legenda de prefixos: **M-US** = US Mobile original · **D-US** = US Desktop original · **US18–24** = novas USs refinadas.

---

# EPIC 1 — Front Site (Desktop/Web)

**Objetivo:** Entregar toda a experiência do Agente IA/SAC na versão web/desktop do site, incluindo balão de entrada, controles de janela, interação por texto/áudio/anexos e integração com a Área do Cliente.

---

## Feature F1.1 — Navegação e Entrada do Agente (Site)

**Descrição:** Pontos de entrada e navegação do Agente no site: balão na Home e menu SAC no header.

- **D-US01** — Comportamento do Agente na Home Page (Logado e Deslogado)
  - *Dependências:* EPIC 3 / F3.1 (escopo de respostas logado vs deslogado), F3.4 (retoma de contexto pós-login).
- **D-US12** — Reestruturação do Header do Site e Menu SAC

---

## Feature F1.2 — Layout e Controles de Janela (Site)

**Descrição:** Comportamento visual do Agente no desktop: drawer, janela expandida, controles e abertura sem teclado virtual.

- **D-US02** — Layout do Agente e Controles de Janela (Drawer e Expandida)
- **D-US03** — Abertura da Drawer do Agente sem Teclado Virtual (Desktop)

---

## Feature F1.3 — Interação e Conteúdo (Site)

**Descrição:** Interações de conversa no desktop: texto, sugestões, áudio, imagens e atalhos de teclado.

- **D-US04** — Interação via Texto com o Agente IA
- **D-US05** — Exibição de Sugestões de Funções da IA Araujo
- **D-US06** — Interação via Áudio com o Agente IA
  - *Dependências:* EPIC 3 / F3.3 (transcrição de áudio).
- **D-US07** — Envio de Imagens pelo Agente IA Araujo
  - *Dependências:* EPIC 3 / F3.3 (referência/armazenamento de mídia).
- **D-US08** — Ampliação da Imagem na Conversa
- **US22** — Atalhos de Teclado (Desktop) *(nova)*

---

## Feature F1.4 — Anexos e Mídia (Site)

**Descrição:** Envio de anexos e câmera via explorador de arquivos do SO no desktop.

- **D-US11** — Inserção de Anexos e Câmera via Explorador de Arquivos do SO
  - *Inclui refinamentos:* aviso de arquivo duplicado; cancelar upload em andamento.
  - *Dependências:* EPIC 3 / F3.6 (validação de formato/tamanho).

---

## Feature F1.5 — Área do Cliente (Site)

**Descrição:** Comportamento do Agente dentro da Área do Cliente no desktop (Início, Meus Pedidos, Meus Chamados).

- **D-US13** — Comportamento do Agente na Área do Cliente (Início, Meus Pedidos e Meus Chamados)

---

# EPIC 2 — Front APP (Mobile)

**Objetivo:** Entregar toda a experiência do Agente IA/SAC na versão mobile do app, incluindo menu lateral SAC, entrada do Agente, interação, anexos e Área do Cliente.

---

## Feature F2.1 — Navegação e Menu SAC (App)

**Descrição:** Reestruturação do menu lateral e comportamento para cliente logado e deslogado.

- **M-US01** — Reestruturação do Menu Lateral (Cliente Logado)
  - *Inclui refinamento:* cenário de erro ao carregar o menu SAC.
- **M-US02** — Comportamento do Menu Lateral (SAC) para Cliente Deslogado
  - *Dependências:* EPIC 3 / F3.4 (retoma de contexto pós-login).

---

## Feature F2.2 — Entrada do Agente (App)

**Descrição:** Pontos de acionamento do Agente no app: balão flutuante, apresentações (drawer/tela cheia), acesso pelos detalhes do pedido e teclado nativo.

- **M-US03** — Balão Flutuante do Agente e Apresentações da Janela
  - *Inclui refinamentos:* balão fixo no scroll; não sobrepor CTAs críticos.
- **M-US04** — Experiência com o Agente na Home Page (Balão Flutuante)
- **M-US05** — Acesso ao Agente pelo Histórico de Pedidos (Detalhes do Pedido)
  - *Dependências:* EPIC 3 / F3.1 (carregar contexto do pedido).
- **M-US06** — Abertura da Drawer do Agente com Teclado Nativo (Mobile)

---

## Feature F2.3 — Interação e Conteúdo (App)

**Descrição:** Interações de conversa no mobile: sugestões, imagens, áudio e ampliação de imagem.

- **M-US07** — Exibição de Sugestões de Funções da IA Araujo
  - *Inclui refinamentos:* scroll horizontal quando exceder 2 linhas; ordem por prioridade do backend.
- **M-US08** — Envio de Imagens pelo Agente IA Araujo
  - *Dependências:* EPIC 3 / F3.3 (referência/armazenamento de mídia).
- **M-US09** — Interação via Áudio com o Agente IA
  - *Inclui refinamentos:* tempo máximo de gravação; contador; interrupção automática.
  - *Dependências:* EPIC 3 / F3.3 (transcrição de áudio).
- **M-US10** — Ampliação da Imagem na Conversa

---

## Feature F2.4 — Anexos e Mídia (App)

**Descrição:** Submenu de anexos (botão "+"), câmera e galeria no mobile.

- **M-US13** — Opções de Submenu de Anexos (Botão "+")
- **M-US14** — Acionar Opção "Câmera" no Submenu
- **M-US15** — Acionar Opção "Galeria" no Submenu
  - *Inclui refinamentos:* compressão de imagens grandes; preview antes do envio.
  - *Dependências:* EPIC 3 / F3.6 (validação de formato/tamanho).

---

## Feature F2.5 — Área do Cliente (App)

**Descrição:** Comportamento do Agente dentro da Área do Cliente no mobile.

- **M-US16** — Comportamento do Agente na Área do Cliente (Início, Meus Pedidos e Meus Chamados)
- **M-US17** — Agente em Tela Cheia na Aba "Fale Conosco"

---

## Feature F2.6 — Resiliência e Feedback (App)

**Descrição:** Experiências de robustez e feedback no app: offline/reconexão, avaliação e notificações (camada de UI).

- **US18** — Comportamento Offline e Reconexão (Mobile) *(nova)*
  - *Dependências:* EPIC 3 / F3.5 (sincronização de mensagens).
- **US19** — Avaliação do Atendimento (Mobile) *(nova)*
  - *Dependências:* EPIC 3 / F3.5 (registro de avaliação).
- **US21-app** — Notificações de Novas Mensagens — UI (badge no balão, scroll para não lidas) *(nova)*
  - *Dependências:* EPIC 3 / F3.5 (push/evento de nova mensagem).

---

# EPIC 3 — Middleware (Integrações, IA e Serviços)

**Objetivo:** Entregar as capacidades de backend/serviço que sustentam ambos os fronts: orquestração da IA, transbordo e protocolos, transcrição e mídia, persistência de histórico/contexto, notificações, analytics e validações.

---

## Feature F3.1 — Orquestração da IA e Processamento

**Descrição:** Processamento das solicitações, escopo logado/deslogado, contexto do pedido e feedback de processamento (bloqueio de controles).

- **M-US11** — Limite de Requisições Intermitentes
  - *Inclui definição:* placeholder `{Solicitação identificada}` derivado da intenção da mensagem.
  - *Dependências:* EPIC 2 (bloqueio visual dos controles).
- **D-US09** — Limite de Requisições Intermitentes (Desktop)
  - *Dependências:* EPIC 1 (bloqueio visual dos controles).

> Observação: o escopo de respostas (logado × deslogado) de D-US01 e o contexto do pedido de M-US05 são suportados por serviços desta feature.

---

## Feature F3.2 — Transbordo e Protocolos

**Descrição:** Detecção de timeout/erro, abertura de protocolo, tratamento fora do horário e direcionamento a "Meus Chamados".

- **M-US12** — Processamento da IA e Transbordo para Atendimento Humano
- **D-US10** — Processamento da IA e Transbordo para Atendimento Humano (Desktop)

---

## Feature F3.3 — Transcrição e Mídia (serviços)

**Descrição:** Serviços de transcrição de áudio e gestão/armazenamento de mídia e suas referências. Suporta as USs de áudio e imagem de ambos os fronts.

- *Serviço de transcrição de áudio* — suporta **M-US09** e **D-US06**.
- *Serviço de mídia e referências* — suporta **M-US08** e **D-US07**.

> Recomenda-se registrar estas como USs técnicas (enabler) nesta feature, caso o time trabalhe com histórias de backend separadas.

---

## Feature F3.4 — Persistência de Histórico e Contexto

**Descrição:** Armazenamento de conversas, retomada de contexto pós-login e gestão do histórico.

- **US20** — Persistência e Gerenciamento do Histórico *(nova)*
  - *Dependências:* EPIC 1 e EPIC 2 (UI de histórico e nova conversa).

> Observação: a "retoma de contexto pós-login" citada em M-US02 e D-US01 é sustentada por esta feature.

---

## Feature F3.5 — Notificações e Eventos

**Descrição:** Serviços de evento de nova mensagem, sincronização offline, registro de avaliação e analytics.

- **US21-svc** — Notificações de Novas Mensagens — Serviço (evento/push de nova mensagem) *(nova)*
- **US24** — Tracking de Eventos e Analytics *(nova)*
  - *Dependências:* EPIC 1 e EPIC 2 (instrumentação no front).

> As partes de UI de US18 (offline) e US19 (avaliação) ficam no EPIC 2; a sincronização e o registro correspondentes são suportados por esta feature.

---

## Feature F3.6 — Tratamento de Erros e Validações

**Descrição:** Validações de formato/tamanho de anexos, falhas de upload e fallback de transcrição.

- **US23** — Tratamento de Erros de Anexos e Áudio *(nova)*
  - *Dependências:* EPIC 1 / F1.4 e EPIC 2 / F2.4 (feedback de erro na UI).

---

# Critérios Transversais (Definition of Done)

> Aplicáveis a todas as USs dos três épicos (ver documento `user-stories-refinadas-sac.md`, Seção C):
> - **Acessibilidade** (WCAG 2.1 AA)
> - **Segurança e Privacidade** (HTTPS, mascaramento, expiração de sessão)
> - **Performance** (abertura ≤ 300ms, loading > 1s, SLA da IA)
> - **Qualidade e Testes** (E2E críticos, integração com mocks, cross-browser/dispositivos)

---

*Estrutura de Épicos e Features — Versão 1.0 — Outubro/2026*
