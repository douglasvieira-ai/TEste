# Regras do Projeto — Agente IA / SAC

Estas regras valem sempre. Leia antes de qualquer alteração.

## Arquivo oficial do protótipo

- O ÚNICO HTML do Agente SAC é:
  `input_interactions/agente_sac_chatopen.html`
- Sempre edite esse arquivo. NÃO crie novos arquivos HTML, nem cópias, nem variações (ex.: `_v2`, `_novo`, `_final`). Se precisar mudar algo, altere o arquivo oficial no lugar.

## Componentes SVG (preservar fidelidade)

Os SVGs do design são a fonte da verdade visual e devem ser conservados exatamente como estão:

- Balão flutuante: `Agente de IA/SITE-Balão fale conosco/Botão balão.svg` (embutido inline no gatilho).
- Janela do chat (drawer): `input_interactions/Chatopen.svg`.
- Janela do chat (expandida / tela cheia): `input_interactions/chatopen Size=Full.svg`.

Não substitua esses SVGs por desenhos recriados. Se um SVG necessário não estiver na pasta, peça o arquivo em vez de inventar um equivalente.

## Base de requisitos

O comportamento do Agente segue as histórias de usuário em:
`US_Agente_SAC_Desktop_Consolidado.md`

## Preferências de trabalho

- Responder em português (pt-BR).
- Não criar arquivos de teste nem Markdown extra sem pedido explícito.
- Ao mudar algo no HTML oficial, validar a integridade (tags balanceadas, referências de SVG resolvendo) antes de concluir.
