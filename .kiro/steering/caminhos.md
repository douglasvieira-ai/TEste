---
inclusion: always
---

# Convenção de caminhos do projeto

A pasta `icon` foi movida para dentro de `src`. O caminho atual e correto é:

```
Componentes agente/src/icon/
```

## Regras

- Sempre que o usuário mencionar um arquivo que está na pasta `src` ou `icon`, considere o caminho novo:
  - `src/icon/<arquivo>` para ícones.
  - `src/<arquivo>` para demais recursos dentro de `src`.
- Em referências dentro de arquivos (ex.: `src="..."` em HTML), ícones devem apontar para `src/icon/<arquivo>.svg`.
- Não use mais o caminho antigo `icon/...` (sem o prefixo `src/`).
