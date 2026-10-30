# Agente IA / SAC — Protótipo

Protótipo estático (HTML/CSS/JS) do agente de atendimento SAC da Araujo.

## Como executar

Abra `index.html` na raiz. Ele carrega a página da Araujo como fundo (iframe) e exibe o widget do agente de chat no canto inferior direito.

## Estrutura (Layer-based)

Os arquivos são organizados por camada / responsabilidade técnica:

```
.
├── index.html            # Ponto de entrada (composição da aplicação)
│
├── pages/                # Camada de apresentação (telas HTML)
│   ├── portal/           # Páginas do Portal do Cliente (app)
│   │   ├── portal-inicio.html
│   │   ├── portal-login.html
│   │   ├── portal-fale-conosco.html
│   │   ├── portal-meus-pedidos.html
│   │   └── portal-meus-chamados.html
│   └── external/         # Snapshots de páginas externas (site da Araujo)
│
├── assets/               # Camada de recursos estáticos
│   ├── icons/            # Ícones de UI (SVG)
│   ├── images/           # Imagens: marca, botões, feedback, barra de menu
│   └── mobile/           # Arte específica para mobile
│
└── docs/                 # Camada de documentação (user stories, regras, épicos)
```

## Convenção de caminhos

- A partir de `index.html` (raiz): `assets/...` e `pages/...`.
- A partir de uma página em `pages/portal/` ou `pages/external/`: suba dois níveis — `../../assets/...` e `../../index.html`.
- Links entre páginas do portal são irmãos (mesmo diretório).
- As páginas em `pages/external/` referenciam o portal via `../portal/`.
