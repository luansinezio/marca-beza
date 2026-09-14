# Marca Beza

Identidade visual da Beza Media e da marca pessoal do Luan (@luansinezio).

## Fonte de verdade

- `design-guide.md` — guia da Beza. É o que as skills leem.
- `design-guide-luan.md` — guia da marca pessoal do Luan.

Os dois sistemas **não se misturam na mesma peça**. Material da Beza usa o
guia principal. Material pessoal do Luan usa o dele: cinza neutro sem
saturação, saturação só dentro de foto, lilás só em collab.

## Versão visual

`manual-de-marca.html` e `manual-de-marca-luan.html` são os guias renderizados,
publicados na Vercel. Cada bloco tem um código (`07.3`, `12.4`) que o Luan cita
ao pedir ajuste.

**Toda alteração aprovada no manual volta pro `design-guide.md`**, que é a
fonte em texto e o que as skills leem. O HTML nunca é a fonte.

## Como isso é consumido

Este repo entra como submodule em `marca/` no workspace do Beza OS. O caminho
`marca/design-guide.md` é lido por 9 skills (`proposta`, `funil`, `criativo-ads`,
`post4me`, `link`, `agenda`, `setup`, `mapear`, `atualizar`) e pelo `AGENTS.md`.
Mudar esse caminho quebra todas elas.

Mudança aqui pede dois commits: um neste repo, outro no workspace movendo o ponteiro.
