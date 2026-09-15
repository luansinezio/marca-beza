# Guia de design de sistema — Beza Media

> Guia visual dos sistemas proprietários da Beza (área logada, painel, CRM, produto interno). A estrutura, os componentes e os padrões de tela vêm do CRM Tenda (repo `luansinezio/crm`, extraído em 15/set/2026). Os tokens de cor, a fonte e as regras de marca vêm do `marca/design-guide.md`, que é o guia oficial da Beza. Este guia é a junção dos dois: a marca Beza pensada pra sistema.
> Qual guia usar: este vale pra sistema (área logada, painel, CRM, produto interno). Conteúdo, site, landing, proposta, deck e página de venda usam o `marca/design-guide.md`. A regra completa está em `marca/qual-guia-usar.md`.
> Versão renderizada: `marca/manual-de-marca-sistemas.html`, publicada em https://manual-marca-beza-v2.vercel.app/sistemas (e no Artifact https://claude.ai/artifact/MqeRpEbNjigjXNArRhHuST)
> Alinhado à marca em 15/set/2026. Onde o valor do CRM original mudou, está dito na seção "O que mudou em relação ao CRM".

---

## Princípios

O sistema é dark-first com tema claro oficial, igual à marca. A identidade é a mesma do site e das propostas: fundo preto azulado, lilás como único acento, cinzas com temperatura lilás, IBM Plex. O que muda em relação ao site são três decisões que valem pra qualquer sistema da Beza:

1. **Elevação por tom, não por borda.** Fundo `#0A0A10`, card um tom acima (`#14141C`), bloco interno mais um tom (`#1B1B26`). A borda existe a 8% e delimita, nunca desenha.
2. **Um acento só.** Lilás em botão primário, item ativo da navegação, foco e linha principal de gráfico. Nada de gradient em botão da área logada. Magenta só como segunda série de gráfico, e é a única cor fora da marca (ver "Gráficos").
3. **Luz ambiente em vez de decoração.** Glow radial lilás difuso, fixo no viewport e no topo de card herói, sem borda perceptível: se dá pra apontar onde a luz termina, está forte demais. No tema claro o glow ambiente some. Grain, glass e gradient animado ficam na superfície pública.

Sistema é o lado sóbrio da marca. Tudo que dá personalidade no site fica fora da área logada, porque ali quem manda é o dado.

---

## Cores

Tokens da marca (`marca/design-guide.md`) mapeados nos papéis que o sistema usa. Componente nunca escreve hex, escreve o token. O tema troca pelo atributo `data-theme` no `<html>`, resolvido antes do primeiro paint por um script inline que lê `localStorage` ou `prefers-color-scheme`.

### Tema escuro (padrão e fallback de SSR)

```css
:root, [data-theme="dark"] {
  color-scheme: dark;

  /* Superfícies, do fundo pra cima */
  --bg:       #0A0A10;   /* --bg da marca: preto azulado */
  --surface:  #111119;   /* --bg-soft: sidebar, container de nav */
  --card:     #14141C;   /* --bg-card */
  --raised:   #1B1B26;   /* hover de linha, tooltip, popover, bloco interno (derivado, entre card e gray-800) */

  /* Texto */
  --fg:       #ECEDF6;   /* --text: nunca branco puro */
  --muted:    #8B8DA1;   /* --text-muted: label, texto secundário */
  --subtle:   #606282;   /* --text-dim: metadata, ícone inativo, placeholder */

  /* Bordas (cinza lilás, nunca branco) */
  --border:        rgba(199,201,214,0.08);
  --border-strong: rgba(199,201,214,0.16);

  /* Acento */
  --accent:       #9A9BE5;   /* --beza-roxo */
  --accent-hover: #ADAEEC;   /* no escuro o hover CLAREIA */
  --accent-fg:    #0D0D0D;   /* --beza-dark: texto sobre lilás chapado */
  --accent-deep:  #7B79C9;   /* --beza-roxo-deep: par do gradient, arco de gauge */
  --accent-2:     #E85CC4;   /* magenta: segunda série de gráfico. Única cor fora da marca. */

  /* Gráficos */
  --chart-primary: #9A9BE5;
  --chart-2:       #E85CC4;
  --chart-grid:    rgba(199,201,214,0.06);
  --chart-cursor:  rgba(199,201,214,0.25);
  --chart-cursor-fill: rgba(199,201,214,0.04);
  --chart-prev:    rgba(199,201,214,0.35);   /* série do período anterior */
  --chart-glow:    rgba(154,155,229,0.45);   /* drop-shadow do gauge */

  /* Estado (cores da marca, tons pra fundo escuro) */
  --success: #6FC878;
  --danger:  #F34D4D;
  --warning: #F3AE4D;
  --info:    #77B8F1;

  /* Scrim de modal e drawer (sem preto absoluto) */
  --scrim: rgba(10,10,16,0.65);

  /* Alvo do color-mix dos badges de status */
  --status-tint: #ECEDF6;

  /* Sombra e luz */
  --shadow-card: inset 0 1px 0 rgba(255,255,255,0.03), 0 8px 24px rgba(0,0,0,0.35);
  --shadow-lift: inset 0 1px 0 rgba(255,255,255,0.04), 0 12px 40px rgba(0,0,0,0.4);   /* nível 03 da marca */
  /* Luz difusa: elipse larga, nasce fora da tela e se esvai ao longo de 3/4 dela. Sentida, não vista. */
  --glow-purple: radial-gradient(ellipse 130% 90% at 50% -40%, rgba(154,155,229,0.13), transparent 72%);
  --glow-purple-strong: radial-gradient(ellipse 120% 100% at 50% -40%, rgba(154,155,229,0.20), transparent 74%);
  --glow-bg:
    radial-gradient(ellipse 95% 75% at 8% -25%, rgba(154,155,229,0.09), transparent 78%),
    radial-gradient(ellipse 70% 60% at 92% 118%, rgba(154,155,229,0.045), transparent 78%),
    var(--bg);
}
```

### Tema claro

É o tema claro oficial da marca, não um branco neutro. Cinza com temperatura lilás, lilás profundo como acento.

```css
[data-theme="light"] {
  color-scheme: light;

  --bg:       #FAF9FC;   /* --bg do tema claro da marca */
  --surface:  #FFFFFF;   /* input, container de nav */
  --card:     #FFFFFF;   /* --bg-card */
  --raised:   #F1F0F7;   /* --bg-soft: hover de linha, popover, bloco interno */

  --fg:       #23222E;   /* --text: nunca preto absoluto */
  --muted:    #61607A;
  --subtle:   #9A99B0;

  --border:        rgba(35,34,46,0.10);
  --border-strong: rgba(35,34,46,0.18);

  --accent:       #6E6CBE;   /* o #9A9BE5 some em fundo claro */
  --accent-hover: #55539E;   /* no claro o hover ESCURECE */
  --accent-fg:    #FFFFFF;
  --accent-deep:  #55539E;
  --accent-2:     #D1379E;

  --chart-primary: #6E6CBE;
  --chart-2:       #D1379E;
  --chart-grid:    rgba(35,34,46,0.06);
  --chart-cursor:  rgba(35,34,46,0.2);
  --chart-cursor-fill: rgba(35,34,46,0.04);
  --chart-prev:    rgba(35,34,46,0.3);
  --chart-glow:    rgba(110,108,190,0.22);

  --success: #3F8F49;
  --danger:  #C9342F;
  --warning: #B57A18;
  --info:    #2E7BB8;

  --scrim: rgba(35,34,46,0.4);
  --status-tint: #23222E;

  /* a sombra esfria: azulada, curta, fraca; a linha de luz interna sobe pra 70% */
  --shadow-card: 0 1px 2px rgba(35,34,60,0.06), 0 8px 24px rgba(35,34,60,0.06);
  --shadow-lift: inset 0 1px 0 rgba(255,255,255,0.7), 0 10px 30px rgba(35,34,60,0.08);
  --glow-purple: radial-gradient(ellipse 130% 90% at 50% -40%, rgba(110,108,190,0.06), transparent 72%);
  --glow-purple-strong: radial-gradient(ellipse 120% 100% at 50% -40%, rgba(110,108,190,0.09), transparent 74%);
  --glow-bg: var(--bg);   /* claro fica limpo, sem glow ambiente */
}
```

### As regras da tradução pro claro (da marca)

1. O lilás desce: `#9A9BE5` vira `#6E6CBE`. O `#9A9BE5` nunca aparece em texto sobre fundo claro.
2. Glow vira sombra: o halo do card herói cai pra 8% e a sombra colorida assume.
3. A sombra esfria: preto a 45% vira `rgba(35,34,60,0.10)`.
4. Hover do acento inverte: no escuro clareia, no claro escurece.
5. Sem glow ambiente. O body é `#FAF9FC` chapado.

### Contraste (meta WCAG AA)

| Fundo | Texto mínimo | Nunca |
|---|---|---|
| Escuro `#0A0A10` a `#14141C` | `#ECEDF6` corpo, `#8B8DA1` apoio | `#606282` em body, branco puro |
| Claro `#FAF9FC` a `#FFFFFF` | `#23222E` corpo, `#61607A` apoio | `#9A99B0` em texto pequeno, `#9A9BE5` em texto |
| Lilás chapado `#9A9BE5` | `#0D0D0D` | branco puro, texto lilás |
| Lilás profundo `#6E6CBE` | `#FFFFFF` | cinza claro |

### Cor de status dinâmica

Cores vindas do banco (etapa do pipeline, tag) foram escolhidas pra fundo branco. Nunca usar cruas. O helper `statusTone(hex)` normaliza pros dois temas com `color-mix()` e o alvo `--status-tint` (`#ECEDF6` no escuro, `#23222E` no claro):

```css
/* texto */  color-mix(in srgb, <hex> 55%, var(--status-tint))
/* fundo */  color-mix(in srgb, <hex> 16%, transparent)
/* borda */  color-mix(in srgb, <hex> 32%, transparent)
/* sólido */ color-mix(in srgb, <hex> 72%, var(--status-tint))   /* barra, dot, donut */
```

Default quando não há cor: `#9A9BE5`.

### Cor de estado fixa

Em fundo escuro a marca usa cor de estado como texto colorido, sem box. No sistema o badge segue isso com um fundo de 10% da própria cor, que é tinta e não box: `text-success bg-success/10`. Neutro é `text-muted bg-foreground/5`. Box preenchido só pra erro destrutivo (botão `danger`).

| Estado | Classe |
|---|---|
| positivo (ativo, pago) | `text-success bg-success/10` |
| em andamento (onboarding, pago parcial) | `text-info bg-info/10` |
| atenção (renovação, a receber) | `text-warning bg-warning/10` |
| problema (em atraso) | `text-danger bg-danger/10` |
| encerrado (inativo, cancelado, reembolsado) | `text-muted bg-foreground/5` |

---

## Tipografia

**Fonte da marca em todo o sistema:** IBM Plex Sans no texto e IBM Plex Mono em rótulo de grupo, código e número tabular. Pesos carregados: 400, 500, 600. Geist sai.

A escala e os tamanhos são os do CRM, porque foram calibrados pra tabela, número e densidade. O que muda é a fonte e a regra de peso da marca: heading em Medium 500, Semibold 600 só em número, valor e botão primário.

| Uso | Tamanho e peso | Origem |
|---|---|---|
| Valor herói (MRR ativo) | 34px 600, tracking -0.02em, tabular | balance-card |
| Título de página | 28px 500, tracking -0.02em | page-header |
| Valor de stat | 20px 600, tracking -0.01em, tabular | stat |
| Título de drawer | 18px 500, tracking -0.01em | lead-drawer |
| Título de modal | 16px 500, tracking -0.01em | modal |
| Título de card | 14px 500, tracking -0.01em | card-title |
| Body | 14px 400, linha 1.6 | base |
| Botão | 14px 500; 600 no `accent` | button |
| Label de campo | 13px 500, sentence case (regra do checkout da marca) | field |
| Label de stat, hint | 12px 500 e 12px 400 | stat |
| Rótulo de grupo (overline) | 11px 500 mono, caixa alta, tracking 0.18em | cabeçalho de seção em popover, drawer, busca |
| Tick de gráfico | 11px 400, `--muted` | chart-theme |

Regras da marca que valem aqui:

- **Sentence case sempre.** Título, botão, título de card e coluna de tabela. Nunca Title Case.
- **Caixa alta só em overline mono.** Rótulo que encima algo (cabeçalho de grupo, categoria acima de lista). Label de campo, hint e legenda vão em sentence case. Teste: se dá pra apagar e o bloco continua se explicando, é caption, não overline.
- **Dose de mono:** no máximo três rótulos mono visíveis ao mesmo tempo na tela.
- **Número comparado em coluna leva `tabular-nums`.**
- **`text-wrap: pretty`** em título e parágrafo.
- **Seleção de texto:** escuro `rgba(154,155,229,.32)` com texto `#ECEDF6`; claro `rgba(110,108,190,.20)` com texto `#413F86`.
- Seta em botão é SVG de linha fina, nunca o caractere `→`. Triângulos de delta (▲ ▼) continuam em texto porque são glifo de dado, não seta.

O que fica de fora de propósito: gradient em texto, hierarquia inline com lilás, escala de hero com `clamp` e eyebrow acima de todo heading. Isso é linguagem de página, não de painel. Só aparecem na superfície pública.

---

## Raio, borda, sombra e glow

### Raio

| Valor | Onde |
|---|---|
| 8px | segmento ativo dentro do segmented, botão de toggle |
| 12px | botão, input, select, textarea, segmented, tooltip |
| 14px | item de navegação e marca na sidebar |
| 16px | card, modal, drawer |
| 20px | container da nav lateral |
| pill | badge, chip, dot, contador |

Raio cresce com o tamanho do bloco. Botão de sistema fica em 12px, e essa é uma divergência declarada da marca (que pede pill em botão): num painel denso o botão pill briga com input e select de 12px na mesma linha. Pill vale pra badge, chip e pro botão `brand` da superfície pública.

### Elevação (níveis da marca aplicados ao sistema)

| Nível | Escuro | Claro | Onde |
|---|---|---|---|
| 00 plano | sem sombra, borda 8% | idem | tabela, linha, bloco interno, stat em faixa |
| 01 repouso | `--shadow-card` | `--shadow-card` | card, sidebar, lead card |
| 02 ativo | 01 mais `0 0 0 1px #9A9BE5` | 01 mais `0 0 0 1px #6E6CBE` | card clicável em hover, célula em edição, drop zone ativa |
| 03 flutuante | `--shadow-lift` | `--shadow-lift` | modal, drawer, popover, busca global, toast |

Densidade: no máximo um elemento em nível 02 por tela. Elemento parado no fluxo leva borda, não sombra.

### Glow

`--glow-purple` no topo de card herói (MRR ativo, win rate, caixa acumulado, login, onboarding). Não é padrão do `Card`, entra à mão. `--glow-bg` é a luz ambiente da cena, fixa no viewport, compartilhada por sidebar e conteúdo. As duas são elipses largas (mais de 100% do bloco) que nascem fora dele e se esvaem devagar, a 9% e 13% de opacidade. No claro os dois quase somem. Halo do login: `960x600`, `bg-accent/10`, `blur(160px)`.

---

## App shell

Origem: `app/(app)/layout.tsx`, `app-shell/sidebar.tsx`, `app-shell/page-header.tsx`. Mantido como está no CRM.

```
┌──────┬──────────────────────────────────────┐
│ [A]  │  Título da página        [ação] [ação]│
│ nav  │                                       │
│ rail │  conteúdo (scroll próprio)            │
│ 🔔   │                                       │
│ (eu) │                                       │
└──────┴──────────────────────────────────────┘
```

Shell `flex h-screen overflow-hidden`: sidebar fixa, `main` com scroll próprio e scrollbar escondida. Só o conteúdo rola.

**Rail.** Marca no topo: quadrado de 44px, raio 14, `bg-accent`, iso oficial da Beza (`marca/logo-beza-iso.svg`) com 24px de largura em `#1A1830` (cor do logo sobre lilás chapado, regra da marca). O "A" do favicon do CRM não é a marca e sai. Nav em container `bg-surface shadow-card rounded-[20px] p-2`, item de 44px com ícone Lucide de 18px: inativo `text-subtle`, hover `bg-foreground/5 text-foreground`, ativo `bg-accent text-accent-foreground`. Rodapé com sino e perfil. Some abaixo de 768px. Itens filtrados por papel.

**Cabeçalho de página.** `px-6 pt-6 pb-4`, título 28px medium à esquerda, ações à direita com gap de 12px. Só título e ação.

**Gutter.** 24px em todo conteúdo interno. Card `p-5`, stat `p-4`, gap entre controles 12px.

Regra: o item ativo é o mesmo quadrado lilás da marca, único acento chapado fora de botão. Tudo que nasce da rail abre à direita dela, alinhado embaixo.

---

## Primitivas

### Botão

Base `rounded-xl text-sm font-medium h-9 px-4 gap-2`, ícone de 16px, foco por teclado com anel lilás. Um `accent` por tela.

| Variante | Escuro | Claro | Uso |
|---|---|---|---|
| `accent` | `#9A9BE5` com texto `#0D0D0D`, hover `#ADAEEC`, 600 | `#6E6CBE` com texto branco, hover `#55539E` | ação primária |
| `outline` | borda 16%, fundo `surface`, hover borda lilás e texto lilás com fundo `accent/4` (ghost da marca) | borda 18%, fundo branco, mesmo hover | secundária |
| `ghost` | `text-muted`, hover `bg-foreground/5 text-foreground` | idem | terciária, ícone |
| `premium` | `bg-raised`, hover 80% | idem | secundária de destaque |
| `danger` | borda `danger/30`, fundo `danger/15`, texto danger | idem | destrutiva |
| `brand` | gradient `#A2A3EA → #7B79C9`, texto `#0A0A10`, glow no hover, pill | no claro o glow vira sombra colorida `0 10px 30px rgba(110,108,190,.35)` | só superfície pública |

Tamanhos: `sm` 32px, `md` 36px, `lg` 44px, `icon` 36 x 36.

### Card

`bg-card border border-border rounded-2xl shadow-card`. Header `p-5 pb-3` com título 14px medium, content `p-5 pt-0`. Card clicável (nicho, widget da galeria) ganha o hover da marca: borda lilás e sombra nível 02, sem lift de 4px (em grid denso o lift pula).

### Stat

Card compacto `p-4`: label 12px 500 em `--muted` com ícone opcional em `--subtle`, valor 20px 600 tabular, hint 12px em `--subtle`. Variante `accent` (`border-accent/25 bg-accent/10`) destaca um número por faixa.

### Badge

Pill 12px 500, `px-2 py-0.5`. Estado fixo (tabela acima), temperatura com dot de 6px, status dinâmico com borda via `statusTone`.

### Delta chip

`rounded-md px-1.5 py-0.5 text-[11px] font-semibold tabular-nums`. Cor por semântica (`goodWhen`): bom `success/10`, ruim `danger/10`, zero `foreground/5`. Sem base vira chip cinza "sem base", nunca `+∞`.

### Segmented

Container `bg-card border rounded-xl h-9 p-1`, opção `rounded-lg px-3 text-sm font-medium`, ativa `bg-accent text-accent-foreground`. Troca visão ou filtro.

### Abas

`border-b-2 px-3.5 py-2.5 text-sm font-medium`, ativa `border-accent text-foreground`. Troca conteúdo de uma entidade. Aba é underline, segmented é pílula. Não se confundem.

### Campo

Label 13px 500 sentence case, controle, erro 12px em danger. Input segue a marca no escuro: fundo translúcido `rgba(255,255,255,0.04)`, borda 12%, hover borda 20%, foco borda lilás com anel `0 0 0 3px rgba(154,155,229,0.18)` e fundo 6%. No claro: fundo branco, borda 10%, foco anel `rgba(110,108,190,0.18)`. Altura 36px, raio 12px, texto 14px, placeholder em `--subtle`. Textarea `min-h-[72px]`. Select nativo com chevron SVG de 16px. Foco por mouse não mostra outline, por teclado mostra.

### Modal e drawer

Scrim `--scrim` com `backdrop-blur-sm`, fecham com Esc e clique fora, travam o scroll. Modal `max-w-lg rounded-2xl shadow-lift`, header `p-5 border-b` com título 16px medium. Drawer à direita, `max-w-[620px] border-l shadow-lift`, entra em 200ms. Detalhe abre em drawer, ação abre em modal.

---

## Motion

Da marca: easing dominante `cubic-bezier(.2,.7,.2,1)` pra hover, cor e estado; `cubic-bezier(0.16,1,0.3,1)` pra drawer, etapa e entrada de conteúdo. Durações: 150ms hover de link e ícone, 250ms hover de botão e estado, 350ms card, 400ms drawer e nav.

| Nome | Receita | Uso |
|---|---|---|
| `animate-rise` | 14px pra cima mais fade, 550ms, curva suave, `both` | entrada de conteúdo |
| `animate-step` | mesma curva, 260ms, `backwards`, direção via `--step-from` | etapa do formulário público |
| `step-leaving` | fade mais meio deslocamento contrário, 130ms | saída da etapa |
| `animate-drawer` | translateX 100% → 0, 200ms | drawer |
| `animate-overlay` | fade, 150ms | scrim |
| gauge | `stroke-dashoffset`, 1s, curva suave | win rate na montagem |
| `transition-colors` | 250ms, easing dominante | botão, nav, segmented |

Reduced motion é global, como na marca:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: .01ms !important; transition-duration: .01ms !important; scroll-behavior: auto !important; }
}
```

---

## Gráficos

Recharts lê CSS vars, então re-tematiza sozinho. Tick 11px em `--muted`, grid a 6% de cinza lilás só horizontal, eixos sem linha, tooltip `rounded-xl border-strong bg-raised text-12 shadow-card`. Nunca caixa de foco.

Série principal `#9A9BE5` (claro `#6E6CBE`). Período anterior em `--chart-prev`. Meta e comparação entram como marca (linha de referência tracejada, bullet tick, série tracejada), nunca como segundo eixo.

**Segunda série é magenta `#E85CC4`, e é a única cor fora da marca.** Dois lilases não se separam num gráfico e as cores de estado não podem virar cor de dado. Fica restrita a gráfico: segunda série de barra ou linha e fatia de donut quando não há cor de etapa. Não entra em botão, badge, texto nem ícone. Se um dia a marca ganhar uma cor de dado, ela substitui a magenta aqui.

Gauge: arco em gradiente do lilás profundo ao lilás (`#7B79C9 → #9A9BE5`), trilha em `--border-strong`, traço de 14px, `drop-shadow(0 0 6px var(--chart-glow))`, preenche em 1s.

Gráfico se desenha na largura real do container (Recharts faz isso com `ResponsiveContainer`). Nunca um SVG com `preserveAspectRatio="none"` esticado: texto, tracejado e ponto deformam em tela larga. Receitas por tipo (área acumulada, barras com meta, donut, funil CSS, barra de ranking, barra de progresso) seguem o CRM sem mudança: ver `dashboard/hero-chart.tsx`, `closing-trend.tsx`, `distribution-donut.tsx`, `funnel-chart.tsx`, `hbar-row.tsx`. O manual renderizado mostra cada uma.

---

## Ícones e logo

Lucide, stroke 2, cor por `currentColor`. 18px na nav, 16px em botão e stat, 14px em ordenação e timeline, 12px dentro do lead card.

Logo entra inline como SVG com `fill="currentColor"`, regra da marca. Os três lockups oficiais moram em `marca/`: `logo-beza-completo.svg` (wordmark, viewBox 760 x 198), `logo-beza-vertical.svg` (compacto, pro celular) e `logo-beza-iso.svg` (só o iso, viewBox 271 x 166). Cor por contexto: `#ECEDF6` no escuro, `#23222E` no claro, `#1A1830` sobre lilás chapado. Nunca `filter: brightness(0)`.

| Onde | Lockup | Tamanho | Cor |
|---|---|---|---|
| Rail | iso | 24px de largura | `#1A1830` sobre `bg-accent` |
| Login, onboarding | completo | 24px de altura | `--fg` |
| Formulário público | completo (vertical no celular) | 20px de altura | `--muted` |
| Rodapé, documento | completo | 32px | `--fg` |
| Favicon, ícone de app | iso | | `#9A9BE5` sobre `#0A0A10` |

O favicon do CRM (`app/icon.svg`) é um "A" em `#8E8FDD` que não é o iso da marca. Sai e entra o iso.

---

## Padrões de página

Esqueleto: `PageHeader` + toolbar opcional + tabela em card + drawer ou modal. Gutter 24px.

- **Toolbar canônica:** `border-b border-border px-6 py-3 flex flex-wrap gap-2`. Segmented, `Select h-9`, busca com ícone de 16px à esquerda, toggle pílula, "Limpar" em 12px muted, contador `ml-auto`.
- **Tabela em card:** `bg-card border rounded-2xl shadow-card overflow-hidden`, cabeçalho 12px 500 em `--muted` (sentence case), coluna ordenável com chevron de 14px, linha `hover:bg-foreground/5` quando abre detalhe, célula título 500 com subtítulo 12px, valor à direita tabular.
- **Tabela de baixa densidade (widget):** cabeçalho overline mono 11px, corpo `divide-y`, categoria em chip pill.
- **Stats no topo:** `grid grid-cols-2 xl:grid-cols-4 gap-3`, um `accent` por faixa.
- **Rótulo de grupo** em drawer, popover e busca: overline mono 11px caixa alta 0.18em em `--subtle`.
- **Empty state:** texto 14px em `--subtle` centralizado, `py-10`. Painel vazio: `Card p-10` com botão outline.
- **Loading:** skeleton `animate-pulse rounded-2xl bg-foreground/[0.04]` só no dashboard e no pipeline. Página interna mostra "Carregando…" em `--subtle`.
- **Erro:** `Card p-6`, título 14px 500 em danger, explicação em `--muted`.
- **Toast:** sonner `richColors`, canto superior direito, nível 03 de sombra, tema acompanha. Todo feedback de ação sai por toast.

### Dashboard

Grade `grid-cols-12 gap-5 px-6`. Widget `STAT` é um terço, `HALF` é metade. Reordena e reflui, nunca redimensiona. Layout por usuário é lista de ids em `profiles.dashboard_layout`. Modo edição: célula com `ring-1 ring-accent/40` (nível 02), item arrastando a 40%, remover em círculo `danger` de 24px, slot tracejado, galeria em modal agrupada por categoria. Heróis com glow: MRR ativo (34px), win rate (gauge), caixa acumulado (área). Feed de atividade com ícone em quadrado de 36px e cor por tipo.

### Pipeline

Board `overflow-x-auto p-6 cursor-grab select-none`, colunas `w-72 gap-4`, fades de 32px nas bordas. Coluna: dot de 8px na cor da etapa, nome 14px 500, contagem em pill. Drop zone `rounded-2xl p-2 bg-foreground/[0.02]`, ativa `bg-accent/10`. Lead card `rounded-xl p-3 shadow-card`, hover nível 03, arrastando a 40% com `rotate-1`. Perdido abre modal de motivo, ganho abre registro de venda, qualificar sem valor abre modal de valor com chips de preço. Drawer com seis abas underline.

### Busca global, notificações, perfil

`Cmd+K`: overlay `pt-[12vh]`, caixa `bg-raised rounded-2xl shadow-lift max-w-xl`, campo de 48px, `kbd` ESC, grupos com overline mono, debounce 250ms. Sino: badge `danger` de 16px, popover `w-[380px] bg-raised rounded-xl shadow-lift` à direita da rail. Perfil: avatar lilás de 44px, menu `w-52` com toggle de tema (três ícones, segmented compacto) e "Sair".

---

## Superfície pública

Login, onboarding e formulário conversacional são escuros mesmo pra quem usa o sistema no claro. É onde o sistema encosta na marca por inteiro.

| Superfície | Fonte | Botão primário | Grain | Eyebrow | Tema |
|---|---|---|---|---|---|
| Área logada | IBM Plex | `accent` chapado | não | não | claro ou escuro |
| Login e onboarding | IBM Plex | `accent` chapado | não | não | escuro fixo |
| Formulário conversacional | IBM Plex | `brand` (gradient, pill) | sim, 4% overlay | sim | escuro fixo |

**Login e onboarding:** fundo `--bg`, halo `720x480 bg-accent/15 blur-[120px]` no topo, card `max-w-sm rounded-2xl p-6 shadow-lift` com `glow-purple`, wordmark de 24px em `currentColor`, título 18px 500, campos em `space-y-3`, botão `accent` largura total. Login tem separador "ou" e botão Google outline.

**Formulário conversacional:** uma pergunta por vez, sem opcional, três telas num formulário só. Layout `max-w-xl px-5 min-h-svh`, glow ambiente, grain. Fio de progresso de 2px no topo, setas de 36px no canto inferior direito ao lado do wordmark. Enunciado `clamp(1.5rem, 5.5vw, 2rem)` 500 tracking -0.02em. Opção `min-h-14 rounded-xl border bg-surface`, letra de atalho em mono num quadrado de 24px, marcada via `has-[:checked]` com borda `accent/50`, fundo `accent/12`, letra `bg-accent`, check lilás. Escolha única avança em 420ms. Campo `h-12 rounded-xl text-base`, Enter avança. Botão `brand` 56px pill, hint "ou aperte Enter". Intro com eyebrow (dot pulsante, mono 11px 0.18em) e título `clamp(2rem, 7vw, 2.75rem)`. Final com círculo de check lilás e CTA pro WhatsApp. Transição: sai em 130ms, entra em 260ms, direção inverte ao voltar.

---

## Calendar (Pauta)

Desenhado em 15/set/2026 pro Agency OS, a partir da regra 2 do backlog: Pauta é pessoas mais blocos de tempo. A unidade é o meio-dia, não a hora. Cada dia tem dois slots. Um bloco ocupa um, dois ou vários. Manual: blocos 19.1 a 19.4.

**Mês por colaborador (19.1, 19.2).** Card do sistema com cabeçalho em duas linhas (`p-5 gap-3.5`): avatar de 36px, nome 16px 500, segmented Mês / Semana / Time e o seletor de mês à direita; embaixo, a barra de concluído contra planejado (trilha 6px, fill lilás, "12 de 20 meios-dias") e o filtro: select de cliente mais chips de status com dot na cor. Cabeçalho dos dias em overline mono. Grade de sete colunas.

- Célula do dia: `min-h-[148px] p-2.5 flex flex-col gap-1.5`, borda 8% à direita e embaixo. Fim de semana `bg-foreground/2`. Dia de outro mês com número a 60%. Hoje: número em pill `bg-accent text-accent-foreground rounded-md px-1.5`.
- Contador de capacidade no canto direito, `n/2` em 11px `--subtle`. Cheio vira `--fg`. Acima de 2 vira `--danger` 600 e a célula ganha `ring-1 ring-danger/50` interno. É o conflito de alocação (PAU-12): visível e permitido, quem decide é o Head.
- Slot de meio dia: dois por dia, `flex-1 min-h-[56px] rounded-[10px]`, invisível quando vazio. Recebendo um arraste: borda tracejada `accent/40` no slot e `bg-accent/10` no dia, igual à drop zone do kanban.
- Bloco: o cartão do kanban em miniatura. `bg-card border border-border rounded-[10px] p-2 shadow-card cursor-grab`, hover nível 03. Linha 1: cliente 12px 500 e duração em mono 10px à direita (½ dia, 1 dia, 2 dias). Linha 2: atividade 11px `--muted`. Linha 3: dot de 6px e o status em 10.5px na cor do status. Sem barra lateral, sem fundo tingido.
- **Cor é status da demanda, nunca cliente.** Planejado lilás `#8b8ce0`, em produção azul `#0ea5e9`, em aprovação âmbar `#f59e0b`, em alteração laranja `#f97316`, aprovado verde `#10b981`, atrasado vermelho `#ef4444`. Tudo via `statusTone`, nos dois temas. Cliente se filtra pelo select e se lê no nome.
- Bloco de 1 dia ocupa os dois slots (`min-h-[118px]`). Bloco maior ocupa os dias seguintes com fantasma tracejado e "continua" no lugar da duração.
- Estados de execução: concluído (check verde, nome riscado, 60%), cancelado (sem fundo, borda tracejada, 70%), arrastando (40%).
- Rodapé "A distribuir": faixa `bg-foreground/2` com os blocos sem data. Arrastar pra um dia agenda. Contagem à direita.

**Semana do time (19.3).** Linhas são pessoas (coluna de 180px com avatar, nome e cadeira), colunas são os dias úteis, coluna de hoje em lilás no cabeçalho. O bloco vira uma linha: dot do status e cliente. Célula em conflito com o mesmo ring do mês. Mesmos filtros.

**Seletor de data (19.4).** Popover `w-[280px] bg-raised rounded-xl shadow-lift p-3`, nível 03, alinhado à esquerda do campo. Célula `h-[34px] rounded-lg text-[13px]` tabular, número centrado; hover `bg-foreground/5`; hoje com anel interno lilás e número lilás; selecionado `bg-accent text-accent-foreground` 600; outro mês a 50%. Dia com bloco de Pauta ganha ponto de 4px lilás posicionado embaixo (absoluto, não desloca o número). Rodapé "Hoje" e "Limpar". Setas movem o dia, PageUp e PageDown o mês, Enter escolhe. O campo aceita `dd/mm/aaaa` digitado.

Regras: sem hora. Concluído se deriva do estado da peça quando dá (regra 15). Arrastar reprograma só aquele bloco (PAU-14).

---

## MediaPreview (peça na aprovação)

Desenhado em 15/set/2026 pro Agency OS, em cima da referência do Radar de setembro. O arquivo mora no Drive (regra 13), o sistema mostra o preview quando existe (DRV-08). Um componente em três lugares: Radar (CS confere), Revisão (Head aprova), Portal e link público (cliente decide). O visual é o mesmo, muda o par de ações. Manual: blocos 20.1 a 20.8.

**Moldura (20.1).** `rounded-xl overflow-hidden bg-raised border border-border` com `aspect-ratio` pelo formato: 4:5 feed (1080 x 1350), 9:16 vertical, 1:1, 16:9. Nasce no tamanho final, a página não pula. Mídia em `object-cover`; vídeo mostra o frame de capa e só carrega o player no clique.

- Etiqueta de formato: overline mono 10px em pill escuro translúcido `rgba(10,10,16,.55)` com blur, canto superior esquerdo, sempre `#ECEDF6` porque fica sobre a mídia, não sobre o tema.
- Play: círculo de 48px no centro, mesmo pill escuro; hover vira lilás com ícone escuro e cresce 6%. Duração no canto inferior direito, mono tabular.
- Carrossel: contador "1/5" no canto superior direito, dots de 6px (ativo vira barra de 14px), setas de 32px que aparecem no hover. Setas do teclado trocam.
- Sem preview: ícone de arquivo num quadrado de 44px, nome 13px 500, tipo, tamanho e origem em 12px `--subtle`, botão outline "Abrir no Drive". Nunca miniatura genérica.
- Estado da peça: badge no canto inferior esquerdo. Aprovada: badge success e anel de 1px success. Alteração pedida: badge warning. Pendente: sem badge.
- Carregando: moldura `bg-foreground/4 animate-pulse`. É a exceção declarada à regra de skeleton.

**Thumb (20.2).** 48px, raio 8, ícone do tipo num quadrado de 16px no canto. Sem mídia, ícone de arquivo em `--muted`. Primeira coluna da tabela; à esquerda do título no cartão do kanban. Versões usam thumb de 32px com anel lilás de 2px na atual.

**Cartão na fila do Radar (20.3).** Raio 16, moldura no topo sem raio próprio, título 14px 500, meta (cliente, data, funil) em 12px `--muted` separada por ponto, última linha com status da peça e conferência do CS (RAD-05). Hover do card clicável. Grid `grid-cols-2 lg:grid-cols-4 gap-4`.

**Palco de aprovação (20.4 a 20.8).** Portal e link público são **claros por padrão**, como o checkout da marca, e seguem a superfície pública: botão pill e o "Aprovar" no botão `brand` (gradient lilás, brilho interno, glow no hover, **texto branco**). O Radar interno mostra os mesmos blocos no tema do usuário com o accent chapado de 12px.

- Cabeçalho: título 28px 500 com cliente e Radar; à direita overline "Status" e pill mono do mês (verde preenchida "Aprovado", âmbar tingida "Aguardando você"). Fio de progresso de 3px com overline "67% revisado · 8 de 12 peças".
- Corpo: duas colunas `1fr / 1.15fr`, 40px de gap, mídia até 420px, empilha no celular. À direita: data 28px 500 tabular com chip mono do formato; bloco de legenda com fio à esquerda, overline "Legenda", parágrafos 15px, hashtags `--muted`, botão outline pill "Comentar sobre a legenda"; faixa de comentários entre dois fios.
- Rodapé: "Anterior" outline, contador mono "conteúdo 3 de 12", "Ajustar" premium, "Aprovar" brand, "Próximo" outline (desabilitado na última).
- Reels (20.5): capa e vídeo lado a lado em 9:16, etiquetas mono, tempo e barra de progresso na base do vídeo.
- Carrossel (20.6): coverflow. Slide atual em 1080 x 1350 no centro (altura manda, largura deriva), anterior e próximo a 35% atrás na escala de 80%, inteiros dentro do palco, cantos de 12px. Setas redondas de 40px com "slide 2 de 9". Botão "Ver em tela cheia" em accent mono no canto inferior esquerdo do slide.
- Tela cheia com comentários (20.7): scrim com blur de 14px, slide grande, painel `w-[360px] bg-raised rounded-2xl p-5 shadow-lift`. Comentário em cartão: avatar 24px, nome 14px 500, pill mono lilás com a origem ("Slide 2 · área marcada", "Legenda", "Geral"), texto 14px, ações "Responder" e "Resolver" em mono. Resposta do time aninhada com overline "Beza Media". Textarea "Comentário geral do conteúdo" e "Comentar" brand. Cliente vê só comentários `client` e respostas.
- Pedido de ajuste (20.8): "Ajustar" entra no modo de marcação. Retângulo tracejado de 1.5px lilás, raio 6, fundo lilás a 6%, salvo em porcentagem do slide. Popover `w-[290px] bg-card rounded-xl shadow-lift p-3.5` com avatar, nome, input "Comente sobre esta área", "Cancelar" mono e "Comentar" brand pequeno. Comentário obrigatório. O primeiro pedido leva a peça pra "Em alteração" e devolve pro time. Em vídeo guarda o tempo junto com a área.

O que o cliente nunca vê: responsável, cadeira, Pauta, comentários internos, arquivos internos e de backup. O componente recebe só o que pode mostrar.

---

## O que mudou em relação ao CRM

| Item | CRM (Geist, zinc) | Sistema Beza (este guia) |
|---|---|---|
| Fonte | Geist Sans e Mono | IBM Plex Sans e Mono |
| Peso de heading | 600 | 500 (600 só em número e botão primário) |
| Fundo escuro | `#0a0a0f`, `#131318`, `#1c1c24` | `#0A0A10`, `#14141C`, `#1B1B26` |
| Texto escuro | `#f4f4f6`, `#a1a1aa`, `#6b6b76` (neutro) | `#ECEDF6`, `#8B8DA1`, `#606282` (temperatura lilás) |
| Borda | branco a 8% e 14% | cinza lilás `199,201,214` a 8% e 16% |
| Lilás escuro | `#b4b5ec` | `#9A9BE5` |
| Lilás claro | `#6d5ae6` | `#6E6CBE` |
| Fundo claro | `#ffffff` chapado, zinc | `#FAF9FC`, card branco, `#F1F0F7` raised |
| Texto claro | `#0b0b0c`, `#52525b`, `#71717a` | `#23222E`, `#61607A`, `#9A99B0` |
| Estado escuro | `#34d399`, `#f87171`, `#fbbf24`, `#7dd3fc` | `#6FC878`, `#F34D4D`, `#F3AE4D`, `#77B8F1` |
| Estado claro | `#059669`, `#dc2626`, `#d97706`, `#0284c7` | `#3F8F49`, `#C9342F`, `#B57A18`, `#2E7BB8` |
| Gauge | lilás → magenta | lilás profundo → lilás |
| Input escuro | fundo `surface` sólido, anel 2px a 30% | fundo translúcido 4%, anel 3px a 18% |
| Outline hover | fundo 5% | borda e texto lilás (ghost da marca) |
| Label de campo | 12px | 13px 500 sentence case |
| Rótulo de grupo | sans uppercase | mono uppercase 0.18em |
| Sombra flutuante | `0 16px 40px` preto 50% | nível 03 da marca com linha de luz |
| Scrim | preto puro 60% | `#0A0A10` a 65% |
| Símbolo sobre lilás | `#0b0b0c` | `#1A1830` |
| Easing de hover | padrão do Tailwind | `cubic-bezier(.2,.7,.2,1)` da marca |

## O que ficou de fora da marca, de propósito

- **Botão pill** na área logada. Botão de sistema é 12px pra alinhar com input e select. Pill vale pra badge, chip e pro `brand` público.
- **Ponteiro lilás** (cursor SVG). Ferramenta de trabalho usa cursor nativo. Volta se o Luan pedir.
- **Grain noise** e **glass** na área logada. Competem com dado.
- **Gradient em texto, hierarquia inline, eyebrow em todo heading, escala hero com clamp.** Linguagem de página. Só na superfície pública.
- **Card com lift de 4px no hover.** Em grid denso o lift pula. Fica só a borda lilás e a sombra.
- **Reveal on scroll.** Painel não revela nada; o dado tem que estar lá quando a tela abre.

## O que nunca fazer

- Hex direto em componente. Sempre token.
- Branco puro em texto. Preto absoluto em qualquer lugar, inclusive scrim e `filter: brightness(0)` no logo.
- Cinza neutro (`gray-*`, `zinc-*`). Sempre o cinza com temperatura lilás dos tokens.
- Roxo antigo `#C82AEF`. `#9A9BE5` em texto sobre fundo claro.
- Dois acentos na mesma tela. Magenta fora de gráfico.
- Acento chapado fora de: botão `accent`, item ativo de nav, segmented e toggle, avatar, dot de etapa, fill de ranking.
- Botão `brand` fora do formulário conversacional.
- Grain, glass ou gradient animado na área logada.
- Borda como hierarquia. Bold (700). Title Case.
- Mono em caixa alta embaixo de alguma coisa (não é overline).
- Cor de status vinda do banco usada crua.
- Segundo eixo em gráfico. Skeleton fora do dashboard e do pipeline.
- Detalhe de entidade em página nova. Scrollbar visível no conteúdo.
- Misturar os dois temas no mesmo material. Animação sem `prefers-reduced-motion`.

## O que o sistema faz sempre

- Número comparado em coluna leva `tabular-nums`.
- Estado é texto na cor mais 10% dela no fundo. Neutro é `text-muted bg-foreground/5`.
- Delta por semântica, não por sinal.
- Barra de progresso capa em 100%, o texto mostra a superação.
- Feedback de ação sai por toast no canto superior direito.
- Popover que nasce da rail abre à direita dela, alinhado embaixo.
- `select-none` em cabeçalho, kanban e filtro.
- Um `accent` por tela. Um overline por bloco, três mono por tela no máximo.
- Hover do acento clareia no escuro e escurece no claro.

---

## Sobre o TendaOS v2

O repo `luansinezio/beza-tendaos` não é evolução visual do CRM. É outro produto (operação de conteúdo), em CSS cru sem tokens, tema claro creme, Inter, botão azul em bold. Não serve como referência de design. A base visual de sistema novo é este guia.
