# Novas User Stories — Agente IA/SAC

> Avaliação crítica do documento `avaliacao-user-stories-sac.md` confrontada com o
> **protótipo funcional real** (`index.html`) e com o backlog de refinamento
> (`user-stories-refinadas-sac.md`).
>
> **Data:** 26/10/2026 · **Versão:** 1.0
>
> **Objetivo:** validar se a avaliação continua coerente com o que já está
> implementado, apontar divergências, refinar o que estiver defasado e criar as
> User Stories adicionais necessárias para cobrir o comportamento real do
> protótipo.

---

## 1. Como esta avaliação foi feita

Confrontei três fontes:

| Fonte | Papel |
|-------|-------|
| `avaliacao-user-stories-sac.md` | Crítica original das 30 USs (gaps e sugestões) |
| `user-stories-refinadas-sac.md` | USs novas (18–24) + refinamentos + DoD transversal |
| `index.html` | **Protótipo funcional real** do widget do Agente SAC |

O ponto central: a avaliação original listou vários gaps como "ausentes", mas
**parte deles já está implementada** no protótipo. Outros continuam válidos.
Há ainda comportamentos **no código que nenhuma US descreve** — e esses são a
maior fonte de novas histórias.

---

## 2. O que o protótipo (`index.html`) realmente faz

Mapeamento do comportamento implementado, para servir de base da comparação.

### 2.1 Já implementado e funcionando

- **Balão flutuante gatilho** com expansão no hover revelando o rótulo "Fale Conosco" (animação de borda/brilho).
- **Abertura/fechamento da drawer** com `openAgent()` / `closeAgent()`.
- **Fechar ao clicar fora** no modo reduzido (lateral), incluindo clique dentro do iframe de fundo (detecção via `blur` da window).
- **Modo expandido ("Full")** centralizado com overlay escurecido; alterna ícone expandir/recolher.
- **Layout mobile em sheet** (ocupa 100% da largura e 90% da altura, entra de baixo, alça no topo, sem botões de ação no header).
- **Tela de boas-vindas** com saudação + 4 pílulas de atalho: Status do pedido, Estorno, Dúvidas, Cancelamento.
- **Regra de autenticação nas pílulas**: Status/Estorno/Cancelamento redirecionam para `html/portal-login.html`; Dúvidas segue no chat.
- **Entrada por texto** com envio via botão e via **Enter**.
- **Gravação de áudio** (simulada): microfone inicia, botão quadrado interrompe e **transcreve** para o campo, seta envia o texto transcrito.
- **Menu de anexo** (Câmera / Galeria / Arquivos) — todos abrem o seletor de arquivo.
- **Preview de imagem anexada** com botão remover; layout empilhado com cantos ajustados.
- **Drag & drop** e **colar (paste)** de imagem, arquivo ou texto no input.
- **Indicador "digitando"** (dot-wave) + processamento simulado (~2,2s).
- **Resposta com card de produto** (imagem + nome + preço) quando há imagem anexada.
- **Modal de imagem ampliada** com **copiar** e **salvar**, com feedback visual; fecha no backdrop, botão e **Escape**.
- **Escape de HTML** nas mensagens do usuário (`escapeHtml`) — proteção básica contra injeção.

### 2.2 Parcialmente implementado

- **Acessibilidade**: há `aria-label`, `role="dialog"`, `aria-hidden` e `alt` em ícones — mas **não há gerência de foco (focus trap), ordem de tabulação garantida nem foco visível customizado**.
- **Atalhos de teclado**: **Enter** envia; **Escape** fecha apenas o **modal de imagem** (não a drawer). Não há Shift+Enter (campo é `input type="text"`, linha única) nem Ctrl+V tratado como anexo explícito (o paste funciona, mas não está descrito como atalho).
- **Tratamento de anexos**: aceita e pré-visualiza, mas **não valida formato/tamanho, não mostra progresso e não trata falha de upload**.

### 2.3 Não implementado (continuam gaps reais)

- Comportamento offline / reconexão / fila de mensagens pendentes.
- Persistência de histórico entre sessões (`agentBody.innerHTML=''` ao fechar — **o histórico é descartado**).
- Badge de notificação de mensagens não lidas no balão.
- Card de avaliação do atendimento.
- Limite de caracteres no campo de texto.
- Analytics / tracking de eventos.
- Timeout de inatividade de sessão / mascaramento de dados sensíveis.

---

## 3. Crítica ao documento `avaliacao-user-stories-sac.md`

### 3.1 Pontos em que a avaliação acertou

- A lista de gaps de **alta prioridade** (offline, erros de anexo/áudio, notificações, limite de caracteres) é coerente: **nenhum deles está no protótipo**.
- A recomendação de **acessibilidade transversal** é pertinente — o protótipo tem apenas a camada básica.
- A sugestão de **Analytics** é válida: não há nenhuma instrumentação no código.

### 3.2 Pontos a corrigir na avaliação (defasagem vs protótipo)

| Item da avaliação | Status real no `index.html` | Correção sugerida |
|-------------------|-----------------------------|-------------------|
| "US 02 Desktop — janela expandida não definida" | **Já implementada**: `min(960px,82vw) × min(840px,calc(100vh-48px))`, centralizada, overlay, clicar fora **não** fecha no modo expandido | Mudar de "gap" para "documentar os valores já existentes" |
| "Preview da imagem antes do envio" (US 15) | **Já implementado** (`attach-preview` + remover) | Remover do gap; manter só compressão de imagens grandes |
| "Enviar com Enter / fechar com Escape" (US 22) | Enter **já envia**; Escape fecha o **modal** (não a drawer) | Refinar a US 22 para o estado real, não como se fosse tudo ausente |
| "Colar imagem (Ctrl+V)" | **Já funciona** via handler `paste` | Reclassificar como implementado, faltando apenas tratamento de erro |
| "Transbordo/estados de erro" tratados como ausentes em várias USs | O protótipo **não tem backend**, então não há como validar erro 4xx/5xx hoje | Separar claramente "gap de UX no protótipo" de "gap de integração" |

### 3.3 Lacuna metodológica da avaliação

A avaliação **não confronta as USs com o protótipo**. Ela critica o texto das
histórias no vácuo. Resultado: alguns "gaps" já estavam resolvidos no front e
vários comportamentos **que existem no código não têm US nenhuma** — por exemplo:

- Redirecionamento das pílulas para login (regra de negócio real no JS).
- Fechar a drawer ao clicar fora **e ao clicar dentro do iframe de fundo**.
- Resposta condicional com **card de produto** quando há imagem.
- Transcrição de áudio para o campo de texto antes do envio.

Esses comportamentos precisam virar USs para que o protótipo seja rastreável.

---

## 4. Novas User Stories (derivadas do comportamento real do protótipo)

> Numeração continuando a partir das existentes (18–24 já estão em
> `user-stories-refinadas-sac.md`). Começamos em **US 25**.

---

### US 25 (Mobile/Desktop): Redirecionamento de Atalhos que Exigem Autenticação

**Como** cliente não autenticado que aciona uma pílula de atalho sensível,
**Gostaria** de ser levado ao login do Portal do Cliente,
**Para que** eu consiga acessar dados protegidos (pedido, estorno, cancelamento) com segurança.

> **Contexto:** No protótipo, as pílulas *Status do pedido*, *Estorno* e *Cancelamento* redirecionam para `html/portal-login.html`; *Dúvidas* segue no chat. A decisão usa o atributo `data-text` da pílula.

**Cenário 01: Atalho sensível redireciona**
Dado que o cliente está na tela de boas-vindas do Agente,
Quando ele clicar em "Status do pedido", "Estorno" ou "Cancelamento",
Então ele deve ser redirecionado para a tela de login do Portal do Cliente.

**Cenário 02: Atalho não sensível segue no chat**
Dado que o cliente está na tela de boas-vindas,
Quando ele clicar em "Dúvidas",
Então a mensagem correspondente deve ser enviada no próprio chat, sem redirecionamento.

**Critérios de Aceite**
1. A classificação sensível/não-sensível deve usar um identificador estável (ex.: `data-text`), não o texto visível.
2. O redirecionamento deve funcionar mesmo quando o widget estiver embutido via iframe (usar o contexto de janela mais externo disponível).
3. Após o login, o cliente deve retornar ao contexto da solicitação (ponto de melhoria: preservar intenção).

**Tasks**
- Task 1 — Mapear quais atalhos exigem autenticação.
- Task 2 — Implementar redirecionamento resiliente a iframe (`window.top` com fallback).
- Task 3 — (Evolução) Preservar a intenção do cliente para retomar após o login.
- Task 4 — Validação e QA: testar cada pílula autenticada e não autenticada.

---

### US 26 (Mobile/Desktop): Fechamento Inteligente da Drawer

**Como** cliente que abriu o Agente no modo reduzido,
**Gostaria** que a drawer fechasse ao clicar fora dela,
**Para que** eu volte à navegação do site sem precisar procurar o botão fechar.

> **Contexto:** O protótipo fecha a drawer no modo reduzido ao clicar em qualquer área fora dela, **inclusive dentro do iframe da página de fundo** (detectado pelo `blur` da window). No modo expandido e no mobile, o fechamento por clique externo **não** se aplica.

**Cenário 01: Clique fora no modo reduzido**
Dado que a drawer está aberta no modo reduzido (lateral),
Quando o cliente clicar em qualquer ponto fora da drawer,
Então a drawer deve fechar e o balão flutuante deve reaparecer.

**Cenário 02: Clique dentro do site de fundo (iframe)**
Dado que a drawer está aberta no modo reduzido,
Quando o cliente clicar em uma área da página de fundo renderizada em iframe,
Então a drawer deve fechar da mesma forma.

**Cenário 03: Modo expandido não fecha por clique externo**
Dado que a drawer está no modo expandido (Full) com overlay,
Quando o cliente clicar fora da janela (no overlay),
Então a janela deve apenas **recolher** para o modo reduzido (não fechar).

**Critérios de Aceite**
1. O clique dentro de qualquer região da drawer, no balão ou no modal de imagem **não** fecha a drawer.
2. O comportamento de clique externo não se aplica ao mobile (lá o overlay controla o fechamento).
3. Fechar deve limpar o estado (conteúdo, anexo pendente, gravação) para a próxima abertura.

**Tasks**
- Task 1 — Listener global de `pointerdown` com verificação de origem do clique.
- Task 2 — Detecção de clique no iframe via perda de foco da window.
- Task 3 — Diferenciar modos reduzido/expandido/mobile.
- Task 4 — Validação e QA.

---

### US 27 (Mobile/Desktop): Resposta Visual com Card de Produto

**Como** cliente que enviou a imagem de um produto ao Agente,
**Gostaria** de receber uma resposta com um card visual do produto,
**Para que** eu identifique rapidamente o item, suas informações e o preço.

> **Contexto:** No protótipo, quando o cliente envia uma imagem, o Agente responde com um card contendo imagem, nome e preço do produto (ex.: *Aptamil Pepti*). Sem imagem, a resposta é textual simples.

**Cenário 01: Resposta com card ao enviar imagem**
Dado que o cliente anexou e enviou uma imagem,
Quando o Agente concluir o processamento,
Então a resposta deve incluir um card de produto com imagem, nome e preço.

**Cenário 02: Resposta textual sem imagem**
Dado que o cliente enviou apenas texto,
Quando o Agente concluir o processamento,
Então a resposta deve ser textual, sem card de produto.

**Cenário 03: Imagem do card ampliável**
Dado que uma resposta contém card de produto,
Quando o cliente clicar na imagem do card,
Então deve abrir o modal de imagem ampliada.

**Critérios de Aceite**
1. O card deve conter imagem, nome e preço (com parcelamento quando aplicável).
2. No modo expandido, o card deve ter largura máxima definida e ficar centralizado.
3. A imagem do card deve abrir no modal de zoom ao clique.
4. (Integração) Os dados do card devem vir do backend/catálogo, não fixos.

**Tasks**
- Task 1 — Componente de card de produto responsivo.
- Task 2 — Lógica condicional de resposta (com/sem imagem).
- Task 3 — Integração do clique do card com o modal de zoom.
- Task 4 — (Evolução) Buscar dados reais do produto.
- Task 5 — Validação e QA.

---

### US 28 (Mobile/Desktop): Ampliação, Cópia e Download de Imagens

**Como** cliente que visualiza uma imagem no chat (enviada ou recebida),
**Gostaria** de ampliá-la, copiá-la ou salvá-la,
**Para que** eu possa examinar detalhes e reutilizar a imagem fora do chat.

> **Contexto:** O protótipo abre um modal em tela cheia ao clicar em qualquer imagem (`sent-img` ou `js-zoom`), com ações de copiar (clipboard) e salvar (download) e feedback visual.

**Cenário 01: Abrir imagem ampliada**
Dado que há uma imagem no chat,
Quando o cliente clicar nela,
Então deve abrir um modal sobre todo o conteúdo, exibindo a imagem centralizada.

**Cenário 02: Copiar imagem**
Dado que o modal de imagem está aberto,
Quando o cliente clicar em "Copiar",
Então a imagem deve ir para a área de transferência e um feedback deve ser exibido.

**Cenário 03: Salvar imagem**
Dado que o modal está aberto,
Quando o cliente clicar em "Salvar",
Então a imagem deve ser baixada e um feedback deve ser exibido.

**Cenário 04: Fechar o modal**
Dado que o modal está aberto,
Quando o cliente clicar no backdrop, no botão fechar ou pressionar Escape,
Então o modal deve fechar.

**Critérios de Aceite**
1. O modal deve sobrepor **todo** o conteúdo, inclusive a própria drawer.
2. Copiar e salvar devem exibir feedback visual temporário.
3. Caso o navegador bloqueie a cópia de imagem, a ação não deve quebrar a interface.
4. Escape, backdrop e botão fechar devem encerrar o modal.

**Tasks**
- Task 1 — Modal de imagem em camada superior (fora do contêiner com transform).
- Task 2 — Ações de copiar (Clipboard API) e salvar (download) com tratamento de erro.
- Task 3 — Feedback visual das ações.
- Task 4 — Validação e QA (incl. navegadores que bloqueiam clipboard de imagem).

---

### US 29 (Mobile/Desktop): Transcrição de Áudio Antes do Envio

**Como** cliente que prefere falar em vez de digitar,
**Gostaria** que meu áudio fosse transcrito para o campo de texto antes do envio,
**Para que** eu possa revisar e corrigir a mensagem antes de enviá-la.

> **Contexto:** No protótipo, o microfone inicia a gravação; o botão de interromper **transcreve** o áudio para o campo de texto (permitindo edição); a seta envia o texto transcrito.

**Cenário 01: Interromper e transcrever**
Dado que o cliente está gravando um áudio,
Quando ele acionar o botão de interromper,
Então o texto transcrito deve aparecer no campo de digitação para revisão.

**Cenário 02: Editar e enviar**
Dado que o texto foi transcrito para o campo,
Quando o cliente ajustar o texto e enviar,
Então a mensagem enviada deve ser o texto final do campo.

**Cenário 03: Enviar direto após gravar**
Dado que o cliente gravou um áudio,
Quando ele acionar a seta de envio na barra de gravação,
Então o conteúdo transcrito deve ser enviado diretamente.

**Critérios de Aceite**
1. Durante a gravação, exibir um indicador visual (waveform animada).
2. A transcrição deve ir para o campo editável, não ser enviada automaticamente ao interromper.
3. (Refinamento herdado da US 09) Definir tempo máximo de gravação e exibir contador.
4. (Integração) Substituir a transcrição simulada por serviço real de speech-to-text.

**Tasks**
- Task 1 — Estados de gravação/transcrição no input.
- Task 2 — Fluxo interromper → transcrever → editar → enviar.
- Task 3 — (Integração) Conectar a serviço de transcrição real.
- Task 4 — Validação e QA.

---

### US 30 (Mobile/Desktop): Anexos por Câmera, Galeria, Arquivos, Drag-and-Drop e Colar

**Como** cliente que quer anexar conteúdo ao Agente,
**Gostaria** de poder anexar por câmera, galeria, arquivos, arrastar-e-soltar ou colar,
**Para que** eu use o meio mais conveniente no momento.

> **Contexto:** O protótipo oferece um menu de anexo (Câmera/Galeria/Arquivos), além de drag-and-drop e colar (paste) de imagem, arquivo ou texto no input, com preview e remoção.

**Cenário 01: Menu de anexo**
Dado que o cliente abriu o menu de anexo,
Quando ele escolher Câmera, Galeria ou Arquivos,
Então o seletor de arquivo deve abrir para a origem escolhida.

**Cenário 02: Arrastar e soltar**
Dado que o cliente arrasta um arquivo sobre o input,
Quando ele soltar o arquivo,
Então o anexo deve ser processado e pré-visualizado (imagem) ou referenciado (outros tipos).

**Cenário 03: Colar da área de transferência**
Dado que o cliente copiou uma imagem/arquivo/texto,
Quando ele colar no input,
Então o conteúdo deve ser anexado (imagem/arquivo) ou inserido (texto).

**Cenário 04: Remover anexo**
Dado que há um anexo pré-visualizado,
Quando o cliente clicar em remover,
Então o input deve voltar ao estado original.

**Critérios de Aceite**
1. As três origens do menu devem acionar a seleção de arquivo.
2. Drag-and-drop deve ter estado visual de "soltar aqui".
3. Imagens geram preview; outros tipos exibem referência por nome.
4. (Refinamento herdado da US 23) Validar formato/tamanho, mostrar progresso e tratar falhas.

**Tasks**
- Task 1 — Menu de anexo e integração com seletor de arquivo.
- Task 2 — Drag-and-drop com estado visual.
- Task 3 — Paste de imagem/arquivo/texto.
- Task 4 — Preview e remoção de anexo.
- Task 5 — Validação e QA.

---

### US 31 (Desktop): Modos de Exibição da Janela (Reduzido ↔ Expandido)

**Como** cliente no desktop,
**Gostaria** de alternar entre a janela lateral reduzida e a janela expandida (Full),
**Para que** eu ajuste o espaço de leitura conforme a complexidade do atendimento.

> **Contexto:** O protótipo implementa dois modos: reduzido (lateral, canto inferior direito) e expandido/Full (centralizado, `min(960px,82vw) × min(840px,calc(100vh-48px))`) com overlay. O ícone alterna entre expandir e recolher.

**Cenário 01: Expandir**
Dado que a drawer está no modo reduzido,
Quando o cliente clicar no botão expandir,
Então a janela deve ir ao modo Full centralizado, com overlay escurecido.

**Cenário 02: Recolher**
Dado que a drawer está no modo expandido,
Quando o cliente clicar no botão recolher (ou clicar no overlay),
Então a janela deve voltar ao modo reduzido.

**Critérios de Aceite**
1. As dimensões do modo expandido devem seguir o design (largura/altura máximas definidas).
2. O modo expandido deve ser centralizado com overlay.
3. No modo expandido, clicar fora **recolhe** (não fecha).
4. O ícone deve refletir o estado atual (expandir vs recolher).

**Tasks**
- Task 1 — Alternância de modos com transições.
- Task 2 — Overlay e centralização no modo Full.
- Task 3 — Ajuste de paddings/largura de conteúdo (ex.: pills centralizadas, card limitado).
- Task 4 — Validação e QA.

---

### US 32 (Mobile): Apresentação em Sheet (Bottom Sheet)

**Como** cliente no mobile,
**Gostaria** que o Agente aparecesse como um painel inferior (sheet),
**Para que** a interação seja confortável em telas pequenas.

> **Contexto:** No mobile (`max-width: 640px`), o protótipo exibe o Agente como sheet: 100% de largura, 90% de altura, entrando de baixo, com alça no topo, cantos superiores arredondados, header centralizado e **sem** botões de expandir/fechar.

**Cenário 01: Abertura em sheet**
Dado que o cliente está no mobile,
Quando ele abrir o Agente,
Então o painel deve subir a partir da base ocupando 100% da largura e 90% da altura.

**Cenário 02: Overlay com blur**
Dado que o sheet está aberto,
Quando ele estiver visível,
Então deve haver um overlay escurecido com leve desfoque atrás do painel.

**Critérios de Aceite**
1. O sheet deve ter alça visual no topo e cantos superiores arredondados.
2. O header no mobile deve ocultar os botões de ação e centralizar a marca.
3. Tocar no overlay deve fechar o sheet.
4. Áreas de toque mínimas de 44x44px (acessibilidade móvel).

**Tasks**
- Task 1 — Layout responsivo do sheet.
- Task 2 — Overlay com blur e fechamento por toque.
- Task 3 — Ajustes de header no mobile.
- Task 4 — Validação e QA em dispositivos representativos.

---

## 5. Novas USs ainda necessárias (gaps reais, não cobertos pelo protótipo)

Estas confirmam e consolidam gaps da avaliação original que **de fato** não
existem no `index.html`. Onde já há US equivalente em
`user-stories-refinadas-sac.md`, aponto a referência para evitar duplicidade.

| US | Tema | Situação | Ação |
|----|------|----------|------|
| US 18 | Offline/reconexão | Não implementado | Já descrita em `user-stories-refinadas-sac.md` — manter |
| US 19 | Avaliação do atendimento | Não implementado | Já descrita — manter |
| US 20 | Persistência de histórico | Não implementado (histórico é descartado no fechar) | Já descrita — **elevar prioridade** |
| US 21 | Notificações / badge | Não implementado | Já descrita — manter |
| US 22 | Atalhos de teclado | **Parcial** (Enter sim; Escape só no modal) | Refinar conforme item 6 |
| US 23 | Erros de anexo/áudio | Não implementado | Já descrita — manter |
| US 24 | Analytics | Não implementado | Já descrita — manter |
| **US 33** | **Limite de caracteres no input** | Não implementado | **Criar (abaixo)** |
| **US 34** | **Acessibilidade: foco e teclado** | Parcial (só aria/alt) | **Criar (abaixo)** |

---

### US 33 (Mobile/Desktop): Limite de Caracteres da Mensagem

**Como** cliente digitando uma mensagem longa,
**Gostaria** de saber quando estou próximo do limite de caracteres,
**Para que** eu ajuste o texto antes de enviar.

**Cenário 01: Aproximação do limite**
Dado que o cliente está digitando,
Quando restarem menos de 50 caracteres para o limite,
Então deve ser exibido um contador visível.

**Cenário 02: Limite atingido**
Dado que o cliente atingiu o limite,
Quando ele tentar digitar mais,
Então a digitação deve ser bloqueada com feedback visual.

**Critérios de Aceite**
1. Definir limite máximo de caracteres por mensagem (alinhar com backend).
2. Exibir contador quando faltarem menos de 50 caracteres.
3. Bloquear entrada além do limite, sem travar a interface.

**Tasks**
- Task 1 — Contador e bloqueio no input.
- Task 2 — Feedback visual de limite.
- Task 3 — Validação e QA.

---

### US 34 (Mobile/Desktop): Acessibilidade de Foco e Navegação por Teclado

**Como** cliente que usa teclado ou leitor de tela,
**Gostaria** de navegar e operar o Agente sem mouse,
**Para que** eu tenha uma experiência acessível (WCAG 2.1 AA).

> **Contexto:** O protótipo já tem `aria-label`, `role="dialog"` e `alt`, mas falta gerência de foco (focus trap na drawer/modal), ordem de tabulação e foco visível customizado.

**Cenário 01: Foco ao abrir**
Dado que o cliente abre a drawer,
Quando ela abrir,
Então o foco deve ir para o campo de mensagem, e o Tab deve circular apenas dentro da drawer (focus trap).

**Cenário 02: Foco no modal de imagem**
Dado que o modal de imagem abre,
Quando ele estiver visível,
Então o foco deve ficar preso ao modal até o fechamento, retornando ao elemento anterior ao fechar.

**Critérios de Aceite**
1. Focus trap na drawer e no modal de imagem.
2. Todos os controles alcançáveis e operáveis via teclado.
3. Estados de foco visíveis em todos os controles.
4. Contraste mínimo de 4.5:1 para textos; áreas de toque 44x44px no mobile.

**Tasks**
- Task 1 — Implementar focus trap e retorno de foco.
- Task 2 — Revisar ordem de tabulação e estados de foco.
- Task 3 — Auditoria de contraste e touch targets.
- Task 4 — Validação e QA com leitor de tela e teclado.

---

## 6. Refinamento da US 22 (Atalhos de Teclado) — ajuste ao estado real

A US 22 em `user-stories-refinadas-sac.md` trata Enter/Shift+Enter/Escape/Ctrl+V
como se nada existisse. Estado real no protótipo:

- **Enter** → **já envia** a mensagem. ✔️
- **Escape** → fecha apenas o **modal de imagem**; **não** fecha a drawer. ⚠️
- **Shift+Enter** → **não aplicável** hoje (campo é `input` de linha única). Para suportar quebra de linha é preciso migrar para `textarea`.
- **Ctrl+V** → o **paste já funciona** (imagem/arquivo/texto), mas não está formalizado como atalho.

**Refinamento proposto para a US 22:**
1. Manter Enter como envio (já atendido).
2. Adicionar Escape para **fechar a drawer** no desktop (hoje só fecha o modal).
3. Decidir sobre Shift+Enter: se desejado, migrar o input para `textarea` com auto-resize.
4. Formalizar Ctrl+V como anexo (já suportado via handler `paste`).

---

## 7. Priorização consolidada (revisada)

### Alta prioridade
1. **US 20 — Persistência de histórico** (hoje o histórico é perdido ao fechar — impacto direto na continuidade do atendimento).
2. US 23 — Tratamento de erros de anexo/áudio.
3. US 18 — Offline/reconexão.
4. US 33 — Limite de caracteres.
5. US 21 — Notificações de novas mensagens.

### Média prioridade
1. US 34 — Acessibilidade de foco/teclado.
2. US 22 (refinada) — Atalhos de teclado.
3. US 24 — Analytics.

### Rastreabilidade (documentar o que já existe)
1. US 25 a US 32 — formalizam comportamentos **já implementados** no protótipo, garantindo que o backlog reflita o produto real.

### Baixa prioridade
1. US 19 — Avaliação do atendimento.
2. Internacionalização (i18n).

---

## 8. Conclusão

A avaliação original é sólida na identificação de gaps, mas foi escrita **sem
confrontar o protótipo**. Isso gerou dois efeitos:

1. **Falsos gaps** — itens já implementados (janela expandida, preview de imagem,
   Enter para enviar, colar imagem) foram listados como ausentes.
2. **Comportamentos órfãos** — regras reais do `index.html` (redirecionamento de
   pílulas, fechar ao clicar fora/iframe, card de produto, transcrição de áudio,
   anexos múltiplos, modos de janela, sheet mobile, modal de imagem) **não tinham
   nenhuma US**.

Este documento corrige isso: refina a avaliação, ajusta a US 22 ao estado real e
cria as **US 25–US 34**, separando claramente o que já existe (rastreabilidade)
do que ainda é gap (desenvolvimento). Recomendo iniciar pela **persistência de
histórico (US 20)**, por ser o gap de maior impacto na experiência atual.

---

*Documento gerado em 26/10/2026 · Versão 1.0*
