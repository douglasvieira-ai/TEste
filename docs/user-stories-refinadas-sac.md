# User Stories Refinadas - Agente IA/SAC

> Documento complementar ao backlog original. Contém as **novas User Stories** e os **refinamentos** das histórias existentes, prontos para serem adicionados ao board de desenvolvimento no Azure DevOps.
>
> **Como usar este documento:**
> - As **Novas USs** (Seção A) devem ser criadas como novos itens no board.
> - Os **Refinamentos** (Seção B) devem ser adicionados aos critérios de aceite das USs já existentes.
> - Os **Critérios Transversais** (Seção C) aplicam-se a todas as USs e podem virar uma Definition of Done (DoD) do time.

---

# SEÇÃO A — Novas User Stories

---

## US 18 (Mobile): [Agente IA/SAC] Comportamento Offline e Reconexão

**Como** cliente utilizando o Agente IA em ambiente com conexão instável,
**Gostaria** de ter feedback visual sobre o status da conexão e a garantia de que minhas mensagens não serão perdidas,
**Para que** eu saiba quando minhas solicitações foram enviadas com sucesso.

> **Contexto:** O cliente pode perder a conexão durante a interação com o Agente. O sistema deve detectar a perda, informar o usuário, armazenar mensagens pendentes localmente e reenviá-las automaticamente ao reconectar.

### Cenários

**Cenário 01: Perda de conexão durante o envio**
Dado que o cliente está interagindo com o Agente,
Quando a conexão for perdida durante o envio de uma mensagem,
Então a mensagem deve ser marcada como "pendente" (ícone de relógio),
E deve ser exibido um banner no topo da drawer: "Sem conexão. Tentando reconectar...".

**Cenário 02: Reconexão automática**
Dado que a conexão foi perdida,
Quando a conexão for restabelecida,
Então as mensagens pendentes devem ser reenviadas automaticamente na ordem original,
E o status de cada mensagem deve mudar para "enviado" (ícone de check).

**Cenário 03: Falha após múltiplas tentativas**
Dado que a conexão não pôde ser restabelecida após 3 tentativas,
Quando o limite de tentativas for atingido,
Então deve ser exibida a mensagem: "Não foi possível enviar. Toque para tentar novamente.",
E o cliente deve poder reenviar manualmente ao tocar na mensagem.

### Critérios de Aceite
1. Detectar perda de conexão em até 5 segundos.
2. Tentar reconexão automática a cada 10 segundos (máximo de 3 tentativas).
3. Mensagens pendentes devem ser armazenadas localmente até o reenvio bem-sucedido.
4. O banner de status de conexão deve ser exibido no topo da drawer, sem cobrir a conversa.
5. Após a reconexão, sincronizar mensagens recebidas enquanto o cliente esteve offline.
6. O estado "pendente/enviado/falha" deve ter indicadores visuais distintos por mensagem.

### Tasks
- **Task 1** — Detecção de status de conexão: monitorar eventos online/offline do dispositivo.
- **Task 2** — Fila de mensagens pendentes: armazenar localmente e reenviar na ordem correta.
- **Task 3** — UI de status: banner de conexão e ícones de status (pendente/enviado/falha).
- **Task 4** — Reenvio manual: permitir toque na mensagem com falha para nova tentativa.
- **Task 5** — Validação e QA: simular perda de conexão em diferentes momentos do fluxo.

---

## US 19 (Mobile): [Agente IA/SAC] Avaliação do Atendimento

**Como** cliente que finalizou uma interação com o Agente IA,
**Gostaria** de avaliar a qualidade do atendimento,
**Para que** a Drogaria Araujo possa melhorar continuamente o serviço.

> **Contexto:** Após a resolução de uma solicitação ou encerramento da conversa, o cliente é convidado a avaliar o atendimento com uma nota simples e comentário opcional. Avaliações negativas devem oferecer caminho para o atendimento humano.

### Cenários

**Cenário 01: Exibição da avaliação após resolução**
Dado que o Agente IA identificou a resolução de uma solicitação,
Quando a resposta final for enviada,
Então deve ser exibido um card de avaliação contendo a pergunta "O atendimento foi útil?", as opções 👍 Sim / 👎 Não e um campo opcional de comentário.

**Cenário 02: Avaliação positiva**
Dado que o cliente selecionou "Sim",
Quando a avaliação for registrada,
Então deve ser exibida a mensagem "Obrigado pelo feedback!",
E a avaliação deve ser enviada ao backend.

**Cenário 03: Avaliação negativa**
Dado que o cliente selecionou "Não",
Quando a avaliação for registrada,
Então deve ser exibido um campo para comentário e a opção "Falar com atendente humano".

### Critérios de Aceite
1. A avaliação deve aparecer apenas uma vez por conversa resolvida.
2. O cliente pode ignorar a avaliação e continuar a conversa normalmente.
3. A avaliação negativa deve acionar um alerta para o time de CX.
4. Os dados registrados devem conter: nota, comentário, ID da conversa e timestamp.
5. O card de avaliação não deve bloquear a interface nem impedir novas mensagens.

### Tasks
- **Task 1** — Card de avaliação: implementar UI conforme design.
- **Task 2** — Integração com backend: enviar avaliação via API.
- **Task 3** — Fluxo de avaliação negativa: exibir comentário e opção de escalar para humano.
- **Task 4** — Validação e QA: testar fluxos de avaliação positiva, negativa e ignorada.

---

## US 20 (Mobile/Desktop): [Agente IA/SAC] Persistência e Gerenciamento do Histórico

**Como** cliente autenticado,
**Gostaria** de acessar o histórico das minhas conversas anteriores com o Agente,
**Para que** eu possa retomar assuntos pendentes sem repetir informações.

> **Contexto:** O histórico da conversa deve ser mantido entre sessões para dar continuidade ao atendimento. É necessário definir tempo de retenção, retomada de conversa e opção de limpeza.

### Cenários

**Cenário 01: Retomada do histórico ao reabrir**
Dado que o cliente teve uma conversa anterior com o Agente,
Quando ele reabrir o Agente dentro do período de retenção,
Então o histórico da conversa anterior deve ser carregado,
E o cliente deve poder rolar para visualizar as mensagens anteriores.

**Cenário 02: Nova conversa**
Dado que o cliente possui histórico anterior,
Quando ele optar por iniciar uma nova conversa,
Então o histórico anterior deve ser arquivado,
E uma nova sessão deve iniciar com a saudação personalizada.

**Cenário 03: Limpeza do histórico**
Dado que o cliente está visualizando o histórico,
Quando ele acionar a opção "Limpar conversa",
Então deve ser solicitada uma confirmação,
E, após confirmar, o histórico visível deve ser removido.

### Critérios de Aceite
1. O histórico deve ser mantido por um período definido (ex.: 30 dias) após a última interação.
2. Ao reabrir o Agente dentro do período, o histórico deve ser restaurado.
3. Deve existir opção para iniciar nova conversa, preservando o histórico anterior arquivado.
4. Deve existir opção de limpar o histórico com confirmação prévia.
5. Dados sensíveis no histórico (CPF, cartão, dados de pedido) devem ser mascarados.

### Tasks
- **Task 1** — Persistência do histórico: armazenar e recuperar conversas por cliente.
- **Task 2** — Retomada ao reabrir: carregar histórico dentro do período de retenção.
- **Task 3** — Nova conversa e arquivamento: separar sessões.
- **Task 4** — Limpeza com confirmação: implementar opção de limpar histórico.
- **Task 5** — Validação e QA: testar retenção, retomada, nova conversa e limpeza.

---

## US 21 (Mobile/Desktop): [Agente IA/SAC] Notificações de Novas Mensagens

**Como** cliente que minimizou ou fechou a drawer do Agente,
**Gostaria** de ser notificado quando receber uma resposta,
**Para que** eu não perca informações importantes do atendimento.

> **Contexto:** Quando o cliente minimiza a drawer (ou mantém o balão flutuante) e o Agente envia uma resposta, deve haver indicação visual de mensagem não lida.

### Cenários

**Cenário 01: Badge de nova mensagem**
Dado que o cliente minimizou a drawer do Agente,
Quando o Agente enviar uma nova resposta,
Então deve ser exibido um badge numérico no balão flutuante indicando mensagens não lidas.

**Cenário 02: Visualização e limpeza do badge**
Dado que há mensagens não lidas,
Quando o cliente reabrir a drawer,
Então o scroll deve posicionar na primeira mensagem não lida,
E o badge deve ser removido após a visualização.

### Critérios de Aceite
1. O badge deve exibir o número de mensagens não lidas no balão flutuante.
2. Ao reabrir, o scroll deve ir automaticamente à primeira mensagem não lida.
3. O badge deve ser removido após a visualização das mensagens.
4. (Opcional) Som de notificação configurável, respeitando as preferências do dispositivo.

### Tasks
- **Task 1** — Contador de não lidas: controlar o estado de mensagens não visualizadas.
- **Task 2** — Badge no balão: renderizar o indicador numérico.
- **Task 3** — Scroll automático: posicionar na primeira mensagem não lida ao reabrir.
- **Task 4** — Validação e QA: testar badge, contagem e limpeza.

---

## US 22 (Desktop): [Agente IA/SAC] Atalhos de Teclado

**Como** cliente utilizando o Agente IA no desktop,
**Gostaria** de usar atalhos de teclado para ações frequentes,
**Para que** eu possa interagir de forma mais rápida e eficiente.

> **Contexto:** No desktop, a entrada ocorre por teclado físico. Atalhos padronizados aumentam a produtividade e alinham a experiência às convenções de chat.

### Cenários

**Cenário 01: Enviar mensagem com Enter**
Dado que o cliente digitou uma mensagem,
Quando ele pressionar Enter,
Então a mensagem deve ser enviada.

**Cenário 02: Quebra de linha com Shift+Enter**
Dado que o cliente está digitando,
Quando ele pressionar Shift+Enter,
Então deve ser inserida uma nova linha, sem enviar a mensagem.

**Cenário 03: Fechar drawer com Escape**
Dado que a drawer do Agente está aberta,
Quando o cliente pressionar Escape,
Então a drawer deve ser fechada, mantendo o balão flutuante.

### Critérios de Aceite
1. Enter envia a mensagem; Shift+Enter quebra linha.
2. Escape fecha a drawer (mantendo o balão).
3. Ctrl+V permite colar texto e imagem da área de transferência.
4. Os atalhos só devem atuar quando o foco estiver dentro do Agente.
5. Os atalhos não devem conflitar com atalhos nativos do navegador.

### Tasks
- **Task 1** — Listeners de teclado para cada atalho.
- **Task 2** — Colagem de imagem via Ctrl+V tratada como anexo.
- **Task 3** — Escopo do foco: garantir atuação apenas no componente do Agente.
- **Task 4** — Validação e QA: testar atalhos nos principais navegadores.

---

## US 23 (Mobile/Desktop): [Agente IA/SAC] Tratamento de Erros de Anexos e Áudio

**Como** cliente enviando anexos ou áudio ao Agente IA,
**Gostaria** de receber feedback claro quando algo falhar,
**Para que** eu possa corrigir e reenviar sem perder a solicitação.

> **Contexto:** Uploads de imagem/arquivo e a transcrição de áudio podem falhar por rede, formato ou tamanho. O sistema deve tratar essas falhas de forma amigável.

### Cenários

**Cenário 01: Falha no upload de anexo**
Dado que o cliente anexou uma imagem ou arquivo,
Quando o upload falhar,
Então deve ser exibida uma mensagem de erro no anexo,
E deve haver a opção "Tentar novamente".

**Cenário 02: Formato ou tamanho inválido**
Dado que o cliente selecionou um arquivo,
Quando o formato não for suportado ou o tamanho exceder o limite,
Então o sistema deve impedir o envio,
E informar o motivo (formato/tamanho) de forma clara.

**Cenário 03: Falha na transcrição de áudio**
Dado que o cliente gravou um áudio para transcrição,
Quando a transcrição falhar,
Então deve ser exibida a opção de enviar o áudio diretamente ou regravar.

### Critérios de Aceite
1. Falhas de upload devem exibir mensagem de erro e opção de reenvio por anexo.
2. Uploads acima de 1 MB devem exibir barra de progresso.
3. Formato e tamanho devem ser validados antes do upload (imagens .png/.jpg/.jpeg; documentos conforme limites definidos).
4. Falha na transcrição deve oferecer alternativas (enviar áudio direto ou regravar).
5. Mensagens de erro devem ser objetivas e orientadas à ação.

### Tasks
- **Task 1** — Tratamento de falha de upload: erro por anexo e reenvio.
- **Task 2** — Validação prévia de formato/tamanho com mensagens claras.
- **Task 3** — Barra de progresso para uploads grandes.
- **Task 4** — Fallback de transcrição de áudio.
- **Task 5** — Validação e QA: simular falhas de rede, formato e transcrição.

---

## US 24 (Mobile/Desktop): [Agente IA/SAC] Tracking de Eventos e Analytics

**Como** Product Owner do Agente IA/SAC,
**Gostaria** de rastrear as interações dos clientes com o Agente,
**Para que** eu possa analisar métricas de uso e priorizar melhorias.

> **Contexto:** Para evoluir o produto, é necessário instrumentar eventos-chave da jornada, respeitando a privacidade dos dados.

### Cenários

**Cenário 01: Registro de abertura do Agente**
Dado que o cliente aciona o Agente,
Quando a drawer/painel for aberto,
Então deve ser registrado o evento de abertura com a origem (balão, menu, detalhes do pedido) e o estado de autenticação.

**Cenário 02: Registro de transbordo**
Dado que ocorreu um transbordo para atendimento humano,
Quando o protocolo for aberto,
Então deve ser registrado o evento com o motivo do transbordo.

### Critérios de Aceite
1. Eventos mínimos: abertura do Agente, envio de mensagem (por tipo), seleção de opção rápida, transbordo e fechamento.
2. Os eventos devem conter metadados relevantes (origem, tipo, timestamp).
3. Nenhum dado pessoal sensível deve ser enviado ao analytics em texto claro.
4. Os eventos devem respeitar o consentimento de cookies/tracking do cliente.

### Tasks
- **Task 1** — Mapeamento de eventos e metadados.
- **Task 2** — Instrumentação no frontend (mobile e desktop).
- **Task 3** — Garantia de anonimização de dados sensíveis.
- **Task 4** — Validação e QA: conferir disparo e payload dos eventos.

---

# SEÇÃO B — Refinamentos das User Stories Existentes

> Adicionar os critérios abaixo às USs correspondentes do backlog original.

## Mobile

### US 01 — Reestruturação do Menu Lateral
**Novo cenário:**
> Cenário 06: Erro ao carregar o menu SAC
> Dado que o cliente está autenticado e ocorre erro na API ao carregar o menu SAC,
> Quando o cliente clicar no menu SAC,
> Então deve ser exibida uma mensagem de erro amigável com o botão "Tentar novamente".

### US 03 — Balão Flutuante
**Novos critérios de aceite:**
- O balão flutuante deve permanecer fixo durante o scroll da página.
- O balão não deve sobrepor elementos críticos (ex.: botões de compra, CTAs principais).

### US 07 — Sugestões de Funções
**Novos critérios de aceite:**
- Caso as sugestões não caibam em 2 linhas, aplicar scroll horizontal.
- A ordem de prioridade das sugestões deve ser definida pelo backend.

### US 09 — Interação via Áudio
**Novos critérios de aceite:**
- A gravação deve ter tempo máximo definido (ex.: 120 segundos).
- Ao atingir o limite, a gravação deve ser interrompida automaticamente.
- Exibir contador de tempo durante a gravação.

### US 11 — Limite de Requisições Intermitentes
**Definição do placeholder:**
- `{Solicitação identificada}` deve ser extraída da intenção da mensagem do usuário.
- Exemplos: "Status do pedido #12345", "Estorno", "Cancelamento".
- Se não for possível identificar, usar o texto genérico "sua solicitação".

### US 15 — Galeria
**Novos critérios de aceite:**
- Imagens acima do limite de tamanho devem ser comprimidas automaticamente antes do envio.
- Exibir preview da imagem selecionada antes do envio.

## Desktop

### US 02 — Layout e Controles de Janela
**Novos critérios de aceite:**
- Definir as dimensões da janela expandida conforme o design (largura x altura).
- A janela expandida deve ser centralizada com overlay de escurecimento.
- Clicar fora da janela expandida não deve fechá-la (apenas o botão fechar).

### US 11 — Anexos via Explorador de Arquivos
**Novos critérios de aceite:**
- Se o cliente tentar anexar o mesmo arquivo duas vezes, exibir aviso de duplicidade.
- Permitir cancelar um upload em andamento.

---

# SEÇÃO C — Critérios Transversais (Definition of Done)

> Aplicáveis a todas as USs. Recomenda-se transformá-los na DoD do time.

## Acessibilidade (WCAG 2.1 AA)
- Todos os elementos interativos acessíveis via teclado (desktop) e com áreas de toque mínimas de 44x44px (mobile).
- Elementos com labels descritivos para leitores de tela (ARIA).
- Contraste de cores mínimo de 4.5:1 para textos.
- Estados de foco visíveis em todos os controles.

## Segurança e Privacidade
- Toda a comunicação deve ocorrer via HTTPS.
- Dados sensíveis não devem ser persistidos sem criptografia.
- Sessão deve expirar após período de inatividade definido; ao expirar, redirecionar ao login preservando o contexto.
- Dados sensíveis (CPF, cartão, dados de pedido) mascarados na interface e nos logs.

## Performance
- Abertura da drawer/painel em até 300ms.
- Indicador de loading para operações acima de 1 segundo.
- Definição de SLA de resposta do Agente IA (alinhar com backend).

## Qualidade e Testes
- Cenários E2E críticos automatizados por US.
- Testes de integração com mocks das APIs do Agente.
- Validação cross-browser (desktop) e em dispositivos representativos (mobile).

---

*Documento de refinamento — Versão 1.0 — Outubro/2026*
