# Avaliação e Refinamento das User Stories - Agente IA/SAC

## Resumo Executivo

O documento apresenta **30 User Stories** bem estruturadas (17 para Mobile e 13 para Desktop), cobrindo a maior parte dos fluxos do Agente IA/SAC. A estrutura geral segue boas práticas com cenários BDD, critérios de aceite e tasks bem definidas. No entanto, identifiquei alguns pontos de melhoria e gaps que precisam ser endereçados.

---

## ✅ Pontos Positivos

1. **Estrutura consistente**: Todas as USs seguem o formato "Como/Gostaria/Para que"
2. **Cenários BDD bem definidos**: Dado/Quando/Então claramente especificados
3. **Critérios de aceite objetivos**: Facilita a validação pelo QA
4. **Tasks granulares**: Permitem acompanhamento detalhado do progresso
5. **Links para Figma**: Referências visuais facilitam o desenvolvimento
6. **Cobertura Mobile e Desktop**: Ambas as plataformas contempladas

---

## 🔴 Gaps Identificados e Recomendações

### 1. **Acessibilidade (WCAG)**

**Gap**: Nenhuma US menciona requisitos de acessibilidade.

**Recomendação**: Adicionar critérios de aceite relacionados a:
- Navegação por teclado
- Compatibilidade com leitores de tela (ARIA labels)
- Contraste de cores
- Tamanhos mínimos de touch targets (44x44px para mobile)
- Focus states visíveis

**Sugestão de Critério de Aceite a adicionar em todas as USs relevantes**:
```
- Todos os elementos interativos devem ser acessíveis via teclado
- Elementos devem possuir labels descritivos para leitores de tela
- Contraste de cores deve atender WCAG 2.1 AA (mínimo 4.5:1 para texto)
```

---

### 2. **Performance e SLAs**

**Gap**: Apenas o timeout de 15s para transbordo é mencionado. Faltam métricas de performance.

**Recomendação**: Adicionar critérios como:
- Tempo máximo de abertura da drawer (ex: < 300ms)
- Tempo de resposta do Agente IA (SLA esperado)
- Tamanho máximo de payload para imagens/anexos
- Comportamento com conexão lenta/offline

**Nova US sugerida**:
```
US XX: [Agente IA/SAC] Comportamento Offline e Performance

Como cliente da Drogaria Araujo,
Gostaria de ter feedback visual quando houver problemas de conexão,
Para que eu saiba que minha solicitação não foi perdida.

Cenários:
- Perda de conexão durante envio de mensagem
- Reconexão automática e reenvio
- Indicadores de loading com tempo estimado
```

---

### 3. **Persistência de Histórico de Conversas**

**Gap**: Não há menção sobre como o histórico é persistido entre sessões.

**Recomendação**: Criar uma US específica ou adicionar critérios:
- O histórico da conversa deve ser mantido ao fechar e reabrir o chat?
- Por quanto tempo o histórico é armazenado?
- O cliente pode limpar o histórico?

**Nova US sugerida**:
```
US XX: [Agente IA/SAC] Persistência e Gerenciamento do Histórico

Como cliente autenticado,
Gostaria de acessar o histórico das minhas conversas anteriores,
Para que eu possa retomar assuntos pendentes sem repetir informações.

Critérios:
- Histórico mantido por X dias após a última interação
- Opção de limpar histórico disponível
- Ao iniciar nova conversa, oferecer opção de continuar conversa anterior
```

---

### 4. **Tratamento de Erros Específicos**

**Gap**: A US 12 (transbordo) cobre timeout e erros 4xx/5xx, mas falta cobertura para outros cenários.

**Recomendação**: Adicionar tratamento para:
- Falha no upload de imagem (rede, tamanho, formato inválido)
- Erro na transcrição de áudio
- Sessão expirada durante a conversa
- Limite de caracteres excedido

**Critérios a adicionar na US 08 (Envio de Imagens)**:
```
- Caso o upload falhe, exibir mensagem de erro e permitir reenvio
- Exibir barra de progresso durante upload de arquivos > 1MB
- Validar formato do arquivo antes do upload
```

---

### 5. **Limites de Caracteres**

**Gap**: Não há menção sobre limites de caracteres nas mensagens de texto.

**Recomendação**: Definir:
- Limite máximo de caracteres por mensagem
- Feedback visual quando se aproxima do limite
- Comportamento quando o limite é atingido

**Critério a adicionar na US de interação via texto**:
```
- O campo de digitação deve suportar até X caracteres
- Exibir contador quando restarem menos de 50 caracteres
- Bloquear digitação ao atingir o limite, com feedback visual
```

---

### 6. **Notificações e Indicadores**

**Gap**: Não há menção sobre notificações quando o Agente responde (se o cliente minimizou a drawer).

**Recomendação**: Adicionar:
- Badge de notificação no balão flutuante quando há resposta não lida
- Som opcional de notificação
- Indicador visual de "nova mensagem" na drawer minimizada

**Nova US sugerida**:
```
US XX: [Agente IA/SAC] Notificações de Novas Mensagens

Como cliente que minimizou a drawer do Agente,
Gostaria de ser notificado quando receber uma resposta,
Para que eu não perca informações importantes.

Critérios:
- Badge numérico no balão flutuante indicando mensagens não lidas
- Ao abrir a drawer, scroll automático para a última mensagem não lida
- Badge removido após visualização das mensagens
```

---

### 7. **Segurança e Privacidade**

**Gap**: Não há menção sobre segurança dos dados transmitidos.

**Recomendação**: Adicionar critérios de segurança:
- Todas as comunicações devem ser via HTTPS
- Dados sensíveis não devem ser armazenados em localStorage sem criptografia
- Timeout de sessão por inatividade
- Mascaramento de dados sensíveis (CPF, cartão)

**Critérios a adicionar**:
```
- Dados de pedido e informações pessoais devem ser mascarados no histórico
- Sessão deve expirar após X minutos de inatividade
- Ao detectar sessão expirada, redirecionar para login preservando contexto
```

---

### 8. **Internacionalização (i18n)**

**Gap**: Todas as mensagens estão hardcoded em português.

**Recomendação**: Considerar se há necessidade de suporte a outros idiomas futuramente. Se sim, garantir que:
- Textos estejam em arquivos de tradução
- Datas e números sejam formatados conforme locale

---

### 9. **Analytics e Métricas**

**Gap**: Não há menção sobre tracking de eventos.

**Recomendação**: Definir eventos a serem rastreados:
- Abertura do Agente (por origem: balão, menu, detalhes do pedido)
- Uso de opções rápidas vs digitação livre
- Taxa de transbordo para atendimento humano
- Tempo médio de resolução
- Abandono da conversa

**Nova US sugerida**:
```
US XX: [Agente IA/SAC] Tracking de Eventos e Analytics

Como product owner,
Gostaria de rastrear as interações dos clientes com o Agente IA,
Para que eu possa analisar métricas de uso e melhorar a experiência.

Eventos a rastrear:
- agent_opened (origem, cliente_logado)
- message_sent (tipo: texto/áudio/anexo)
- quick_option_selected (opção)
- escalation_triggered (motivo)
- agent_closed (tempo_sessão, mensagens_trocadas)
```

---

### 10. **Testes de Regressão**

**Gap**: Não há menção sobre cenários de teste automatizado.

**Recomendação**: Para cada US, definir:
- Cenários de teste E2E críticos
- Mocks necessários para testes de integração
- Critérios de cobertura mínima

---

## 🟡 Refinamentos Sugeridos em USs Existentes

### US 01 (Mobile) - Reestruturação do Menu Lateral

**Refinamento**: Adicionar cenário para comportamento quando há erro ao carregar os sub-itens.

```
--> Cenário 06: Erro ao carregar o menu SAC
Dado que o cliente está autenticado,
E ocorre um erro na API ao carregar o menu SAC,
Quando o cliente clicar no menu SAC,
Então deve ser exibida uma mensagem de erro amigável,
E deve haver um botão "Tentar novamente".
```

---

### US 03 (Mobile) - Balão Flutuante

**Refinamento**: Falta definir comportamento do balão durante scroll.

```
Adicionar Critério de Aceite:
10. O balão flutuante deve permanecer fixo durante o scroll da página.
11. O balão não deve sobrepor elementos críticos como botões de compra.
```

---

### US 07 (Mobile) - Sugestões de Funções

**Refinamento**: Falta definir comportamento quando as sugestões não cabem em 2 linhas.

```
Adicionar Critério de Aceite:
5. Caso as sugestões não caibam em 2 linhas, implementar scroll horizontal.
6. As sugestões devem ter ordem de prioridade definida pelo backend.
```

---

### US 09 (Mobile) - Interação via Áudio

**Refinamento**: Falta definir tempo máximo de gravação.

```
Adicionar Critério de Aceite:
3. A gravação deve ter tempo máximo de X segundos.
4. Ao atingir o limite, a gravação deve ser interrompida automaticamente.
5. Exibir contador de tempo durante a gravação.
```

---

### US 11 (Mobile) - Limite de Requisições

**Refinamento**: O placeholder "{Solicitação identificada}" precisa de definição.

```
Adicionar definição:
- A "{Solicitação identificada}" deve ser extraída da mensagem do usuário
- Exemplos: "Status do pedido #12345", "Estorno", "Cancelamento"
- Se não identificar, usar texto genérico: "sua solicitação"
```

---

### US 15 (Mobile) - Galeria

**Refinamento**: Falta definir comportamento para imagens muito grandes.

```
Adicionar Critério de Aceite:
8. Imagens maiores que 10MB devem ser comprimidas automaticamente.
9. Exibir preview da imagem antes do envio com opção de recorte.
```

---

### US 02 (Desktop) - Layout e Controles de Janela

**Refinamento**: Falta definir dimensões da janela expandida.

```
Adicionar Critério de Aceite:
7. Na visão expandida, a janela deve ter dimensões de X x Y pixels.
8. A janela expandida deve ser centralizada na tela com overlay escurecido.
9. Clicar fora da janela expandida não deve fechá-la (apenas o botão fechar).
```

---

### US 11 (Desktop) - Anexos via Explorador

**Refinamento**: Falta definir comportamento para arquivos duplicados.

```
Adicionar Critério de Aceite:
9. Se o cliente tentar anexar o mesmo arquivo duas vezes, exibir aviso.
10. Permitir cancelar upload em andamento.
```

---

## 📋 Checklist de Completude por US

| US | Cenários | Critérios | Tasks | Figma | Acessib. | Erros | Analytics |
|----|----------|-----------|-------|-------|----------|-------|-----------|
| Mobile US01 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Mobile US02 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Mobile US03 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Mobile US04 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Mobile US05 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Mobile US06 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Mobile US07 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Mobile US08 | ✅ | ✅ | ✅ | ✅ | ❌ | ⚠️ | ❌ |
| Mobile US09 | ✅ | ✅ | ✅ | ✅ | ❌ | ⚠️ | ❌ |
| Mobile US10 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Mobile US11 | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Mobile US12 | ✅ | ✅ | ✅ | ⚠️ | ❌ | ✅ | ❌ |
| Mobile US13 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Mobile US14 | ✅ | ✅ | ✅ | ✅ | ❌ | ⚠️ | ❌ |
| Mobile US15 | ✅ | ✅ | ✅ | ✅ | ❌ | ⚠️ | ❌ |
| Mobile US16 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Mobile US17 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Desktop US01 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Desktop US02 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Desktop US03 | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Desktop US04 | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Desktop US05 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Desktop US06 | ✅ | ✅ | ✅ | ✅ | ❌ | ⚠️ | ❌ |
| Desktop US07 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Desktop US08 | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Desktop US09 | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Desktop US10 | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ |
| Desktop US11 | ✅ | ✅ | ✅ | ❌ | ❌ | ⚠️ | ❌ |
| Desktop US12 | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ | ❌ |
| Desktop US13 | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |

**Legenda**: ✅ Completo | ⚠️ Parcial | ❌ Ausente

---

## 🆕 Novas User Stories Sugeridas

### US Mobile 18: Comportamento Offline e Reconexão

```
US 18: [Agente IA/SAC] Comportamento Offline e Reconexão (Mobile)

Como cliente utilizando o Agente IA em ambiente com conexão instável,
Gostaria de ter feedback visual sobre o status da conexão,
Para que eu saiba quando minhas mensagens foram enviadas com sucesso.

> Contexto: O cliente pode perder a conexão durante a interação com o Agente.
O sistema deve detectar a perda de conexão, informar o usuário e tentar 
reconectar automaticamente, reenviando mensagens pendentes.

--> Cenário 01: Perda de conexão durante envio
Dado que o cliente está interagindo com o Agente,
Quando a conexão for perdida durante o envio de uma mensagem,
Então a mensagem deve ser marcada como "pendente" com ícone de relógio,
E deve ser exibido um banner "Sem conexão. Tentando reconectar...".

--> Cenário 02: Reconexão automática
Dado que a conexão foi perdida,
Quando a conexão for restabelecida,
Então as mensagens pendentes devem ser reenviadas automaticamente,
E o status deve mudar para "enviado" (ícone de check).

--> Cenário 03: Falha após múltiplas tentativas
Dado que a conexão não pode ser restabelecida após 3 tentativas,
Então deve ser exibida a mensagem "Não foi possível enviar. Toque para tentar novamente.",
E o cliente deve poder reenviar manualmente.

--> Critérios de Aceite:
1. Detectar perda de conexão em até 5 segundos.
2. Tentar reconexão automática a cada 10 segundos (máximo 3 tentativas).
3. Mensagens pendentes devem ser armazenadas localmente.
4. Banner de status de conexão deve ser exibido no topo da drawer.
5. Após reconexão, sincronizar possíveis mensagens recebidas enquanto offline.

--> Tasks:
- Task 1 — Detecção de status de conexão: monitorar online/offline.
- Task 2 — Fila de mensagens pendentes: armazenar localmente e reenviar.
- Task 3 — UI de status de conexão: banner e ícones de status das mensagens.
- Task 4 — Validação e QA: simular perda de conexão em diferentes cenários.
```

---

### US Mobile 19: Avaliação do Atendimento

```
US 19: [Agente IA/SAC] Avaliação do Atendimento (Mobile)

Como cliente que finalizou uma interação com o Agente IA,
Gostaria de avaliar a qualidade do atendimento,
Para que a Drogaria Araujo possa melhorar o serviço.

> Contexto: Após a resolução de uma solicitação ou encerramento da conversa,
o cliente deve ser convidado a avaliar o atendimento com uma nota e comentário opcional.

--> Cenário 01: Exibição da avaliação após resolução
Dado que o Agente IA identificou a resolução de uma solicitação,
Quando a resposta final for enviada,
Então deve ser exibido um card de avaliação com:
- Pergunta: "O atendimento foi útil?"
- Opções: 👍 Sim / 👎 Não
- Campo opcional para comentário

--> Cenário 02: Avaliação positiva
Dado que o cliente selecionou "Sim" na avaliação,
Então deve ser exibida a mensagem "Obrigado pelo feedback! 😊",
E a avaliação deve ser registrada no backend.

--> Cenário 03: Avaliação negativa
Dado que o cliente selecionou "Não" na avaliação,
Então deve ser exibido um campo para comentário,
E a opção de "Falar com atendente humano".

--> Critérios de Aceite:
1. A avaliação deve aparecer apenas uma vez por conversa.
2. O cliente pode ignorar a avaliação e continuar a conversa.
3. A avaliação negativa deve acionar alerta para o time de CX.
4. Dados da avaliação: nota, comentário, ID da conversa, timestamp.

--> Tasks:
- Task 1 — Card de avaliação: implementar UI conforme design.
- Task 2 — Integração com backend: enviar avaliação via API.
- Task 3 — Fluxo de avaliação negativa: opção de escalar para humano.
- Task 4 — Validação e QA: testar fluxos de avaliação positiva e negativa.
```

---

### US Desktop 14: Atalhos de Teclado

```
US 14: [Agente IA/SAC] Atalhos de Teclado (Desktop)

Como cliente utilizando o Agente IA no desktop,
Gostaria de usar atalhos de teclado para ações frequentes,
Para que eu possa interagir de forma mais rápida e eficiente.

> Contexto: No desktop, atalhos de teclado melhoram a produtividade.

--> Cenário 01: Enviar mensagem com Enter
Dado que o cliente digitou uma mensagem no campo de texto,
Quando ele pressionar Enter,
Então a mensagem deve ser enviada.

--> Cenário 02: Nova linha com Shift+Enter
Dado que o cliente está digitando uma mensagem,
Quando ele pressionar Shift+Enter,
Então deve ser adicionada uma nova linha no texto.

--> Cenário 03: Fechar drawer com Escape
Dado que a drawer do Agente está aberta,
Quando o cliente pressionar Escape,
Então a drawer deve ser fechada.

--> Critérios de Aceite:
1. Enter: envia mensagem
2. Shift+Enter: quebra de linha
3. Escape: fecha a drawer
4. Ctrl+V: cola texto/imagem da área de transferência
5. Os atalhos devem funcionar apenas quando o foco está no Agente.

--> Tasks:
- Task 1 — Implementar listeners de teclado para cada atalho.
- Task 2 — Garantir que atalhos não conflitem com atalhos do navegador.
- Task 3 — Validação e QA: testar todos os atalhos em diferentes navegadores.
```

---

## 📊 Priorização Sugerida para os Refinamentos

### Alta Prioridade (Impacto na experiência do usuário)
1. Tratamento de erros específicos (upload, transcrição)
2. Comportamento offline/reconexão
3. Limite de caracteres no campo de texto
4. Notificações de novas mensagens

### Média Prioridade (Compliance e qualidade)
1. Acessibilidade (WCAG)
2. Segurança e privacidade
3. Analytics e métricas

### Baixa Prioridade (Nice to have)
1. Avaliação do atendimento
2. Atalhos de teclado (desktop)
3. Internacionalização

---

## 📝 Conclusão

O documento está **bem estruturado** e cobre a maioria dos fluxos principais. Os principais gaps identificados são:

1. **Acessibilidade**: Ausente em todas as USs
2. **Tratamento de erros**: Parcial - precisa ser expandido
3. **Performance/SLAs**: Não definidos
4. **Comportamento offline**: Não coberto
5. **Analytics**: Não mencionado

Recomendo priorizar os refinamentos de **alta prioridade** antes de iniciar o desenvolvimento, pois impactam diretamente a experiência do usuário e podem gerar retrabalho se implementados posteriormente.

---

*Documento gerado em: Outubro/2026*
*Versão: 1.0*
