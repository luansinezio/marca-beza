# Guia de Design — Beza Media

> Você pode editar esse arquivo a qualquer momento.
> As skills de carrossel, proposta e slide leem este arquivo antes de criar qualquer visual.
> Fonte de verdade visual: tema WP em `projetos/site-beza/wp-theme/beza-theme/` (no ar em https://www.beza.media).

---

## Princípios

A identidade Beza é **dark-first**. O fundo padrão é preto azulado profundo (`#0A0A10`), e tudo cresce dali: textos em branco lilás, lilás (`#9A9BE5`) como cor de marca aparecendo em accents, gradients e estados de hover, e cinzas com temperatura lilás (nunca cinza neutro).

O sistema tem cinco marcas visuais que aparecem em todos os materiais:

1. **Tipografia Medium 500** em headings, sempre com `letter-spacing: -0.02em` — peso firme sem ser bold
2. **IBM Plex Mono** em eyebrows, labels, anos e categorias — cria o "ar editorial/técnico"
3. **Glassmorphism** em elementos flutuantes (nav, dropdowns, drawer) — `backdrop-filter: blur(20px) saturate(140%)`
4. **Grain noise** sutil em todo fundo (SVG turbulence, opacity 4%, mix-blend overlay)
5. **Eyebrow pill** com dot lilás pulsante antes de cada heading principal — mono uppercase

Nunca usar branco puro `#FFFFFF` em texto. Nunca usar cinza neutro (tipo gray do Tailwind). Nunca usar o roxo antigo `#C82AEF`.

---

## Cores

### Tokens base

```css
:root {
  /* Lilás Beza */
  --beza-roxo:        #9A9BE5;  /* lilás de marca, accent principal */
  --beza-roxo-deep:   #7B79C9;  /* lilás profundo, par do gradient */
  --beza-accent:      #C7C9D6;  /* cinza lilás claro, contraponto */
  --beza-light:       #ECEDF6;  /* off-white lilás, texto principal */
  --beza-dark:        #0D0D0D;  /* preto puro, raro */

  /* Fundos (dark-first) */
  --bg:               #0A0A10;  /* fundo base de página */
  --bg-soft:          #111119;  /* seção alternativa, ligeiramente acima */
  --bg-card:          #14141C;  /* fundo de card */

  /* Texto */
  --text:             #ECEDF6;  /* texto principal */
  --text-muted:       #8B8DA1;  /* texto secundário, captions */
  --text-dim:         #606282;  /* texto terciário, metadata */

  /* Bordas */
  --border:           rgba(199,201,214,0.08);   /* borda sutil padrão */
  --border-strong:    rgba(199,201,214,0.16);   /* borda de botão/destaque */

  /* Escala de cinzas (temperatura lilás) */
  --gray-50:          #F4F3F9;
  --gray-100:         #E8E7F0;
  --gray-200:         #D5D6E1;
  --gray-300:         #C4C2D6;
  --gray-400:         #A8A9BE;
  --gray-500:         #8B8DA1;
  --gray-600:         #606282;
  --gray-700:         #4A4C5F;
  --gray-800:         #25253D;
  --gray-900:         #15151A;

  /* Glow do lilás (componível) */
  --glow: 154,155,229;
}
```

### Regras de aplicação

- **Fundo padrão**: sempre `--bg` (`#0A0A10`). Não usar fundos claros em web — exceção rara em material impresso ou docs internos.
- **Seções alternativas**: usar `--bg-soft` (`#111119`) pra criar contraste sutil entre blocos. Nunca contraste forte.
- **Cards**: usar `--bg-card` (`#14141C`) com border `rgba(255,255,255,0.08)`.
- **Texto principal**: `--text` (`#ECEDF6`). Nunca `#FFFFFF`.
- **Texto secundário**: `--text-muted` (`#8B8DA1`). Captions e metadata.
- **Texto terciário/metadata**: `--text-dim` (`#606282`).
- **Lilás é accent**, não fundo de bloco. Aparece em: eyebrows, gradients de destaque, hover states, ícones, tags, números, scroll progress.

### Gradients oficiais

```css
/* Texto hero — corpo (branco esmaecendo) */
background: linear-gradient(180deg, #ECEDF6 0%, #8B8DA1 100%);

/* Texto hero — destaque (lilás profundo) */
background: linear-gradient(180deg, #A2A3EA, #7B79C9);

/* Botão primary */
background: linear-gradient(180deg, #A2A3EA, #7B79C9);

/* Scroll progress bar */
background: linear-gradient(90deg, #9A9BE5, #7B79C9);

/* Grid bg utility (linhas finas) */
background-image:
  linear-gradient(rgba(199,201,214,0.04) 1px, transparent 1px),
  linear-gradient(90deg, rgba(199,201,214,0.04) 1px, transparent 1px);
background-size: 80px 80px;
```

Pra usar gradient em texto, sempre: `-webkit-background-clip: text; background-clip: text; -webkit-text-fill-color: transparent;`

### Cores de estado

| Estado | Cor | Uso |
|--------|-----|-----|
| Success | `#6FC878` | Texto colorido em fundo escuro (sem box) |
| Error | `#F34D4D` | Texto ou box preenchido + texto branco |
| Warning | `#F3AE4D` | Texto colorido |
| Info | `#77B8F1` | Texto colorido |

Em fundo escuro, cores de estado aparecem **como texto colorido** — o contraste com o fundo já basta, sem precisar de box. Boxes preenchidos só pra erro destrutivo ou alerta crítico.

---

## Tipografia

**Fontes:**
- `'IBM Plex Sans', system-ui, sans-serif` — corpo, headings, navegação
- `'IBM Plex Mono', monospace` — eyebrows, labels, anos, categorias, copyright

Pesos carregados: 300 (Light), 400 (Regular), 500 (**Medium — padrão**), 600 (Semi-bold).

### Regra de peso (importante)

- **Medium 500** é o peso padrão de headings (h1, h2, h3, h4). Não Regular.
- **Regular 400** só pra body text e parágrafos longos.
- **Semi-bold 600** pra CTAs em fundo branco (botão CTA da nav) e palavra-chave isolada.
- **Light 300** raríssimo, só em display editorial muito grande.

### Letter-spacing

Headings têm letter-spacing negativo:
- Display/hero (60px+): `-0.025em`
- H1-H2 (32-48px): `-0.02em`
- H3 (22-28px): `-0.01em`
- Body e abaixo: 0 ou `-0.005em` (sutil)

Eyebrows e labels mono têm letter-spacing **positivo** alto:
- Eyebrow padrão: `0.18em`
- Year/category: `0.22em`
- Footer copyright: `0.15em`

### Escala (web — clamp responsivo)

| Nível | Tamanho | Peso | Line-height | Uso |
|-------|---------|------|-------------|-----|
| Hero | `clamp(40px, 5.5vw, 80px)` | 500 | 1.05 | Capa de página, título principal |
| H1 | `clamp(32px, 4vw, 48px)` | 500 | 1.1 | Início de seção forte |
| H2 | `clamp(26px, 3vw, 36px)` | 500 | 1.15 | Subseção |
| H3 | `clamp(22px, 2.2vw, 30px)` | 500 | 1.25 | Título de card |
| H4 | `18-20px` | 500 | 1.3 | Item destacado |
| Body | `16-17px` | 400 | 1.6 | Texto corrido |
| Body small | `14-15px` | 400 | 1.5 | Body de card, descrições |
| Mono caption | `11-13px` | 500 mono · uppercase · letter-spacing 0.18-0.22em | 1.3 | Eyebrow, year, category, label |
| Caption | `12px` | 400 | 1.4 | Metadata, opacity 0.7 |

### text-wrap

Em todos os elementos de texto longo (`h1, h2, h3, h4, h5, p, blockquote`), usar:

```css
text-wrap: pretty;
```

Evita viúvas e arranja quebras melhores automaticamente.

### Hierarquia inline (destaque em frases)

Em headings com 6+ palavras, destacar a parte-chave em **gradient lilás** (não com peso). O resto fica em **gradient branco esmaecendo**. Não usar negrito inline — o destaque vem da cor.

```html
<h1 class="hero-title">
  <span class="line">Marcas que crescem</span>
  <span class="line line--em">com a operação Beza.</span>
</h1>
```

Onde `.line` recebe o gradient branco→cinza e `.line--em` recebe o gradient lilás.

Exceção: em material impresso ou apresentação Figma sem gradient text, usar Medium pro destaque e Light pro restante.

### Selection

```css
::selection { background: rgba(154,155,229,0.35); color: #fff; }
```

---

## Eyebrow (componente padrão)

Pill que aparece **acima de todo heading principal** (hero de página, início de seção, capa de proposta, slide de abertura).

**Anatomia:** dot lilás pulsante (6×6px, glow) + label mono uppercase letter-spacing 0.18em.

```css
.eyebrow {
  display: inline-flex; align-items: center; gap: 10px;
  width: fit-content; max-width: max-content; align-self: flex-start;
  padding: 6px 14px;
  border-radius: 999px;
  border: 1px solid rgba(154,155,229,0.25);
  background: rgba(154,155,229,0.04);
  font-family: 'IBM Plex Mono', monospace;
  font-size: 11px;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.18em;
  color: var(--beza-roxo);
}
.eyebrow .dot {
  width: 6px; height: 6px;
  border-radius: 50%;
  background: var(--beza-roxo);
  box-shadow: 0 0 10px var(--beza-roxo);
  animation: pulse 2s ease-in-out infinite;
}
@keyframes pulse {
  0%, 100% { opacity: 1; transform: scale(1); }
  50%      { opacity: 0.4; transform: scale(0.85); }
}
```

**Quando usar:**
- Capa de página/landing (acima do hero)
- Início de seção temática (Expertise, Cases, Processo)
- Capa de proposta (acima do título)
- Capa de slide / carrossel

**Quando não usar:** dentro de cards pequenos, em headings de listas, em body text.

---

## Botões

### Sistema base

```css
.btn {
  display: inline-flex; align-items: center; gap: 12px;
  padding: 14px 22px;
  border-radius: 999px;            /* pill, sempre */
  font-family: 'IBM Plex Sans', sans-serif;
  font-weight: 500; font-size: 14px;
  border: 1px solid var(--border-strong);
  background: transparent;
  color: var(--text);
  text-decoration: none;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(.2,.7,.2,1);
  position: relative; overflow: hidden;
}
```

Todos os botões são **pill** (`border-radius: 999px`). Não usar `8px` ou `12px` — esse padrão foi descontinuado.

### Variantes

**Primary** — gradient lilás com glow no hover (CTA principal de seção):

```css
.btn-primary {
  background: linear-gradient(180deg, #A2A3EA, #7B79C9);
  color: #0A0A10;
  border: 1px solid rgba(255,255,255,0.18);
  box-shadow: inset 0 1px 0 rgba(255,255,255,0.25);
  font-weight: 600;
}
.btn-primary:hover {
  box-shadow:
    0 0 60px 0 rgba(154,155,229,0.5),
    inset 0 1px 0 rgba(255,255,255,0.4);
  transform: translateY(-1px);
}
```

**Ghost** — outline transparente com hover lilás (secundário, navegação interna):

```css
.btn-ghost {
  background: transparent;
  border: 1px solid var(--border-strong);
  color: var(--text);
}
.btn-ghost:hover {
  border-color: var(--beza-roxo);
  color: var(--beza-roxo);
  background: rgba(154,155,229,0.04);
}
```

**CTA branco** — botão sólido branco (CTA da nav, ação principal de página):

```css
.btn-cta {
  background: #FFFFFF;
  color: #0A0A10;
  border: 1px solid rgba(255,255,255,0.9);
  box-shadow:
    inset 0 1px 0 rgba(255,255,255,0.4),
    0 4px 14px rgba(0,0,0,0.3);
  font-weight: 600;
  padding: 11px 18px;
  border-radius: 10px;             /* exceção: CTA da nav usa 10px, não pill */
}
.btn-cta:hover { background: #F4F3F9; transform: translateY(-1px); }
```

### Seta animada

Botões com seta SVG usam `.arrow` que translada no hover:

```css
.btn .arrow { transition: transform 0.3s ease; }
.btn:hover .arrow { transform: translateX(4px); }
```

Nunca usar caractere `→`. Sempre SVG linha fina:

```html
<svg width="12" height="12" viewBox="0 0 14 14" fill="none" aria-hidden="true">
  <path d="M3 7h8m0 0L7.5 3.5M11 7L7.5 10.5" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
</svg>
```

### Tamanhos

| Size | Padding | Font | Uso |
|------|---------|------|-----|
| sm | `8px 16px` | 12px | Tags, filtros, controles densos |
| md | `14px 22px` | 14px | **Default** — 90% dos casos |
| lg | `16px 28px` | 15-16px | Hero CTA, ação principal de página |

---

## Container e espaçamento

### Container

```css
.container {
  max-width: 1320px;
  margin: 0 auto;
  padding: 0 32px;
}
```

Em mobile (≤700px), reduzir padding pra `20px`.

### Padding lateral

| Breakpoint | Padding |
|------------|---------|
| Mobile (≤700px) | `20px` |
| Tablet | `24-28px` |
| Desktop (1320+) | `32px` |

### Padding vertical entre seções

| Tipo | Padding |
|------|---------|
| Seção padrão | `80-120px` desktop · `64-80px` mobile |
| Hero | `140px 0 120px` desktop · `120px 0 80px` mobile |
| Card body | `28px 32px 32px` |

### Border-radius

| Elemento | Raio |
|----------|------|
| Botão / pill / tag | `999px` |
| Card padrão | `14-16px` |
| Card grande (case, hero) | `24px` |
| CTA branco da nav | `10px` |
| Nav glass | `14px` |
| Imagem em card | inherit do card |

### Sombras (em fundo escuro)

Em fundo escuro, sombras são **escuras + cor de accent**, não cinzas. Padrão:

```css
/* Card */
box-shadow: 0 28px 60px rgba(0,0,0,0.45);

/* Card com hover (accent + glow) */
box-shadow:
  0 28px 60px rgba(0,0,0,0.45),
  0 0 0 1px var(--accent-color);   /* color-mix do accent ou roxo */

/* Botão primary hover (glow lilás) */
box-shadow: 0 0 60px 0 rgba(154,155,229,0.5);

/* Nav glass */
box-shadow:
  inset 0 1px 0 rgba(255,255,255,0.04),
  0 12px 40px rgba(0,0,0,0.4),
  0 0 0 1px rgba(0,0,0,0.2);
```

---

## Cards

### Card padrão (case, conteúdo)

```css
.card {
  background: var(--bg-card);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 24px;
  overflow: hidden;
  transition: transform 0.35s cubic-bezier(.2,.7,.2,1),
              border-color 0.35s ease,
              box-shadow 0.35s ease;
  isolation: isolate;
}
.card:hover {
  transform: translateY(-4px);
  border-color: var(--accent, var(--beza-roxo));
  box-shadow: 0 28px 60px rgba(0,0,0,0.45),
              0 0 0 1px var(--accent, var(--beza-roxo));
}
```

**Hover lift de 4px** + accent color invadindo a borda + sombra forte. Cards sem hover (estáticos) ficam com border sutil e sem sombra.

### Card de case (anatomia)

1. **Thumb** topo, aspect-ratio 16:10, overlay gradient escuro 40%→100%, scale 1.04 no hover
2. **Body** padding 28-32px, gap 12px:
   - Row: name (left, Plex Sans Medium 22-30px) + year (right, Plex Mono 11px letter-spacing 0.22em uppercase)
   - Sector (Plex Sans 14px gray-500)
   - Tags (Plex Mono 10px uppercase, pill com `color-mix` do accent: bg 8%, border 28%)
   - CTA (Plex Mono 11px uppercase letter-spacing 0.22em + seta SVG, gap aumenta 8→14px no hover)
3. **Accent color** vem como CSS variable `--accent` no card (cor da marca do cliente)

### Card com accent dinâmico

Cards de cliente recebem a cor do cliente como `--accent`:

```html
<a class="card" style="--accent: #FF5500;">...</a>
```

Tags e CTA usam `color-mix(in srgb, var(--accent) 8%, transparent)` pra criar tinta suave.

---

## Navegação

### Header — floating glass nav (padrão)

Nav fixo no topo (`top: 20px`), centralizado, com glassmorphism. Não é "header full-width" — é uma **pill flutuante** com `max-width: 1320px`:

```css
.nav {
  position: fixed; top: 20px; left: 0; right: 0;
  z-index: 100;
  display: flex; justify-content: center;
  padding: 0 32px;
  pointer-events: none;
  transition: transform 0.4s cubic-bezier(.2,.7,.2,1), opacity 0.3s ease;
}
.nav-glass {
  pointer-events: auto;
  width: 100%; max-width: 1320px;
  display: flex; align-items: center;
  padding: 14px 14px 14px 26px;
  border-radius: 14px;
  background: rgba(16,16,22,0.72);
  backdrop-filter: blur(20px) saturate(140%);
  -webkit-backdrop-filter: blur(20px) saturate(140%);
  border: 1px solid rgba(255,255,255,0.07);
  box-shadow:
    inset 0 1px 0 rgba(255,255,255,0.04),
    0 12px 40px rgba(0,0,0,0.4);
}
.nav.is-hidden {
  transform: translateY(calc(-100% - 30px));
  opacity: 0;
}
```

**Auto-hide:** em páginas de case, esconder a nav fora do hero e da seção CTA final. Usa IntersectionObserver no hero e no CTA — visible quando algum dos dois tá visível.

**Estrutura:**
- Logo à esquerda (22px altura, `filter: brightness(0) invert(1)` pra ficar branco)
- Menu central absoluto (Plex Sans 13px Medium, gap 4px, hover bg `rgba(255,255,255,0.04)`)
- Spacer flexível
- Lang switcher pill mono (BR/EN, 11px letter-spacing 0.12em)
- CTA branco com seta

### Dropdown desktop

Submenus (ex: "Cases") abrem no hover, vidro fosco ainda mais escuro (`rgba(16,16,22,0.92)`), radius 14px, padding 10px. Items com logo do case + nome + setor. CSS hover + JS fallback pra touch.

### Drawer mobile

Aparece a partir de 900px de largura. Slide da direita, 360px de largura, `cubic-bezier(0.16, 1, 0.3, 1)` em 400ms. Overlay backdrop blur 8px. Cases viram accordion. Foot com © Beza em mono.

---

## Footer

Fundo segue o `--bg` da página (não vira ilha escura como em outros sistemas), só com `border-top: 1px solid var(--border)` pra separar.

**Estrutura:**

```
[Brand: logo 32px + tagline 14px gray-300 max 360px]   |   [Cols: Serviços | Empresa | Contato]

──────────────────────────────────────────────────────
© 2026 Beza Media · Todos os direitos reservados        Instagram · LinkedIn
```

- Grid `1.2fr 2fr` desktop · 1 coluna mobile
- Cols: title em Plex Mono 11px letter-spacing 0.18em uppercase, gap 18px abaixo + 10px entre items
- Links em gray-300, hover lilás
- Bottom: mono 11px gray-muted, social como texto mono (não ícones)

---

## Motion

### Easing dominante

```css
--ease-beza: cubic-bezier(.2, .7, .2, 1);   /* ease-out forte, o easing padrão da Beza */
```

Quase tudo na interface usa esse easing. Pra drawer e modais, alternativa mais suave: `cubic-bezier(0.16, 1, 0.3, 1)`.

### Durações

```css
--duration-fast:      150ms;   /* hover de link, chev de dropdown */
--duration-base:      250ms;   /* hover de botão, troca de estado simples */
--duration-card:      350ms;   /* hover de card (transform + shadow) */
--duration-drawer:    400ms;   /* drawer mobile, nav auto-hide */
--duration-reveal:    900ms;   /* reveal on scroll */
```

### Reveal on scroll

Padrão de animação de entrada quando elemento aparece no viewport:

```css
.reveal {
  opacity: 0;
  transform: translateY(24px);
  transition:
    opacity 0.9s ease var(--reveal-delay, 0ms),
    transform 0.9s cubic-bezier(.2,.7,.2,1) var(--reveal-delay, 0ms);
}
.reveal.in {
  opacity: 1;
  transform: translateY(0);
}
```

Usar IntersectionObserver pra adicionar `.in` quando o elemento entra. `--reveal-delay` permite staggered entrada em grids.

### Reduced motion

Sempre incluir:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

---

## Efeitos característicos

### Grain noise overlay

SVG turbulence aplicado em todo `body::before`, opacity 4%, mix-blend overlay. Cria textura sutil que dá "ar fotográfico" pra interface. É marca visual da Beza:

```css
body::before {
  content: "";
  position: fixed; inset: 0;
  pointer-events: none; z-index: 1;
  background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='200' height='200'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2' stitchTiles='stitch'/></filter><rect width='100%25' height='100%25' filter='url(%23n)' opacity='0.55'/></svg>");
  opacity: 0.04;
  mix-blend-mode: overlay;
}
```

Sempre incluir em landings, propostas e qualquer material HTML da Beza.

### Cursor glow

Glow lilás radial 480×480px que segue o mouse, com `mix-blend-mode: screen`. Sutil mas inconfundível:

```css
.cursor-glow {
  position: fixed; top: 0; left: 0;
  width: 480px; height: 480px;
  border-radius: 50%;
  pointer-events: none; z-index: 2;
  background: radial-gradient(circle, rgba(154,155,229,0.18), transparent 60%);
  transform: translate(-50%, -50%);
  transition: opacity 0.3s ease;
  mix-blend-mode: screen;
}
```

```js
document.addEventListener('mousemove', (e) => {
  const el = document.querySelector('.cursor-glow');
  el.style.left = e.clientX + 'px';
  el.style.top = e.clientY + 'px';
});
```

Usar em landings principais e capas de proposta. Não usar em interfaces densas/dashboards (distrai).

### Grid bg utility

Linhas finas formando grid 80×80px, opacity baixíssima. Usar como fundo de hero ou seção de destaque:

```css
.grid-bg {
  background-image:
    linear-gradient(rgba(199,201,214,0.04) 1px, transparent 1px),
    linear-gradient(90deg, rgba(199,201,214,0.04) 1px, transparent 1px);
  background-size: 80px 80px;
}
```

### Scroll progress bar

Em páginas longas (case, proposta, ebook web), barra horizontal fina no topo mostrando progresso de leitura:

```css
.scroll-progress {
  position: fixed; top: 0; left: 0; right: 0;
  height: 2px;
  background: linear-gradient(90deg, #9A9BE5, #7B79C9);
  width: 0%;
  z-index: 1000;
  transition: width 0.1s linear;
  pointer-events: none;
}
```

---

## Logo

### Arquivos

- `marca/Logo SVG.svg` — fonte vetorial (paths originais com `fill="#C7C6D7"`)
- `marca/logo-beza-completo.svg` — logo completo (assinado)
- `marca/logo-beza-iso.svg` — apenas o iso (favicon, ícone)
- `marca/Logo Beza Claro.png` — PNG branco/lilás (raster, Figma)
- `marca/Logo Beza Escuro.png` — PNG preto (raster, contextos light)
- Logo atualizado em 31/jul/2026 (versão final "Ativo 1"): wordmark script beza + MEDIA, viewBox 760x198. Todos os arquivos acima regravados; PNGs re-renderizados do vetor (claro #ECEDF6, escuro #0D0D0D). Versão líquida interativa em `marca/logo-liquid-beza.html` e capa de proposta em `marca/capa-proposta-liquid.html`.

### Em HTML (regra)

Em interfaces dark-first (que é o padrão), o logo aparece branco. Como o SVG fonte tem fill cinza, **forçar branco com filter**:

```css
.nav-logo img {
  height: 22px;
  width: auto;
  filter: brightness(0) invert(1);
}
```

Esse filter funciona em qualquer SVG monocromático e mantém o vetor leve. Para logos coloridos (marcas de clientes), inline o SVG e use `fill="currentColor"` + `color: inherit` no wrapper.

### Tamanhos sugeridos

- Nav desktop: `22px` altura
- Nav mobile drawer: `22px`
- Footer: `32px`
- Hero/capa de proposta: `40-56px`
- Capa de carrossel/slide: proporção variável conforme layout

---

## Inputs e formulários

Em fundo escuro, inputs ficam translúcidos com hover/focus em lilás:

```css
.input {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.12);
  border-radius: 10px;
  color: var(--text);
  padding: 12px 14px;
  font-family: inherit; font-size: 15px;
  transition: border-color 0.2s ease, background 0.2s ease;
}
.input::placeholder { color: var(--text-dim); }
.input:hover { border-color: rgba(255,255,255,0.2); }
.input:focus {
  border-color: var(--beza-roxo);
  outline: none;
  box-shadow: 0 0 0 3px rgba(154,155,229,0.18);
  background: rgba(255,255,255,0.06);
}
```

Tamanho default: 44px altura, padding 12-14px, radius 10px. Para checkbox/radio em fundo escuro, usar bg `rgba(255,255,255,0.04)` + border `0.12` + check SVG branco.

---

## Slides e apresentações (Figma 1920×1080)

Slides seguem o **mesmo sistema dark-first do web**. O sistema antigo de três fundos alternantes (escuro/lilás/claro) foi descontinuado. Agora:

### Fundo padrão

- **Fundo escuro** (`#0A0A10` ou gradient `#0A0A10 → #14141C`) — usar em **todos os slides**
- **Lilás chapado** (`#9A9BE5`) — só em slide de pergunta/clímax pontual (1-2 por apresentação, no máximo)
- **Fundo claro** — descontinuado. Não usar mais

### Texto em slide

- Texto principal: `#ECEDF6` (branco lilás)
- Destaque inline (palavras-chave): `#9A9BE5` (lilás) — não usar peso, usar cor
- Texto secundário/breadcrumbs: `#8B8DA1` ou opacity 0.5 do texto principal
- Em slide lilás (raro), texto em `#ECEDF6` com 90% opacity

### Tipografia em slide

Tudo em IBM Plex Sans **Medium 500** com letter-spacing -0.02em (consistente com web).

| Nível | Tamanho | Peso | Uso |
|-------|---------|------|-----|
| Display (capa) | 180-200px | Medium | Título de capa, frase de impacto |
| Headline | ~116px | Medium | Citação, pergunta clímax |
| Title | ~74px | Medium | Subtítulo de slide |
| Subtitle | ~53px | Medium | Explicação |
| Body | ~38px | Regular ou Medium | Texto corrido, dados |
| Caption | ~22px | Mono uppercase letter-spacing 0.18em | Breadcrumb, label |

### Display dinâmico (capa)

- 2 palavras curtas → 200px
- 2 longas ou 3 palavras → 180px
- 4+ palavras → 160px (ou quebrar)
- Não usar Auto Layout — posicionar direto no frame centralizado

### Eyebrow em slide

Mesmo padrão do web. Posicionar acima do título da capa e de slides de seção. Mono uppercase letter-spacing 0.18em, cor lilás `#9A9BE5`, dot lilás à esquerda (sem animação no Figma — fica estático).

### Rag shaping (regras de quebra)

Mantidas — são regras de tipografia válidas em qualquer mídia.

**Texto centralizado (headline/pergunta):** formato **sanduíche**
- Linha 1 ≈ linha 3 (menores), linha 2 (maior)
- Quando 6+ palavras, quebrar em 3 linhas
- Destaque inline cai na linha 2
- Exemplo: `Por que a gente\nprecisa falar sobre\nresultados?`

**Texto alinhado à esquerda (subtitle/contexto):** formato **diagonal/cascata**
- Primeira linha maior, próximas decrescendo
- Cria arco descendente
- Evitar viúvas (1-2 palavras soltas na última linha)
- Nunca duas linhas consecutivas do mesmo comprimento

### Breadcrumbs

Topo e rodapé do slide, mono uppercase letter-spacing 0.18em em `#8B8DA1`:

```
Beza Media        [Tema]        Apresentação
```

O texto central é contextual (nome da apresentação ou cliente). Auto Layout space-between.

### Centralização

- Texto centralizado → centralizar vertical + horizontalmente na área útil
- Texto à esquerda → centralizar verticalmente
- Área útil: entre breadcrumbs (top ~80px, bottom ~1000px = 920px de altura útil)

### Checklist obrigatório antes de entregar

1. **Fundo escuro** em todos os slides — exceção lilás chapado só em 1-2 slides de clímax
2. **Texto branco lilás** (`#ECEDF6`), nunca branco puro
3. **Destaque inline em lilás** (`#9A9BE5`) — não usar peso, usar cor
4. **Eyebrow** em capa e início de seção (mono lilás + dot)
5. **Rag shaping** aplicado na montagem (não depois): centralizado = sanduíche, esquerda = cascata, sem viúvas
6. **Breadcrumbs contextuais** com tema da apresentação no centro
7. **Logo** em branco (claro PNG) em fundo escuro
8. **Canvas do arquivo** `#1E1E1E` em todas as páginas

---

## Estilo geral / direção visual

- **Dark-first sempre.** Materiais da Beza são escuros — landings, propostas, slides, ebooks, posts. Material claro é exceção pontual.
- **Lilás é o protagonista,** mas como accent — não como bloco de fundo. Aparece em headings com gradient, eyebrows, hovers, CTAs, scroll progress, palavras-chave.
- **Texto editorial.** Letter-spacing negativo em headings, line-height comprimido (1.05), mono pra labels. Sente como editorial de revista, não dashboard.
- **Glassmorphism em elementos flutuantes.** Nav, dropdowns, drawer. Nunca em bloco grande de conteúdo.
- **Grain noise sempre.** É a "pele" da interface — sutil mas presente.
- **Motion suave e curto.** Easing `cubic-bezier(.2,.7,.2,1)`, durações 250-400ms. Sem bounce, sem spring exagerado.

Para materiais de clientes, a identidade da Beza fica em background — o branding do cliente assume o protagonismo, e a Beza aparece só na assinatura final.

---

## Tagline

"Construindo estruturas para o extraordinário."

---

## O que NUNCA fazer

- Branco puro `#FFFFFF` em texto (sempre `#ECEDF6`)
- Roxo antigo `#C82AEF` em qualquer lugar
- Cinza neutro (tipo gray-500 do Tailwind) — sempre cinza com temperatura lilás
- Border-radius `8px` ou `12px` em botões — botões são sempre pill (`999px`)
- Headings em Regular 400 — sempre Medium 500
- Caractere `→` como seta — sempre SVG linha fina
- Fundo claro como bloco principal de seção
- Misturar mais de 3 cores no mesmo material
- Material da Beza sem grain noise, sem eyebrow, sem hierarquia de gradient

---

## Arquivos de referência

- `projetos/site-beza/wp-theme/beza-theme/` — fonte de verdade visual (tema WP no ar)
- `projetos/site-beza/wp-theme/beza-theme/header.php` — tokens, nav, drawer (fonte primária)
- `projetos/site-beza/wp-theme/beza-theme/footer.php` — footer estático
- `projetos/site-beza/wp-theme/beza-theme/template-cases-index.php` — cards de case
- `marca/Logo SVG.svg` — logo vetorial
- `marca/manual-de-marca.html` — manual visual interno
- `marca/design-guide-test.html` — preview dos tokens em HTML
- `marca/design-guide-test-tipografia.html` — escala tipográfica
- `marca/design-guide-test-botoes.html` — sistema de botões
- `marca/design-guide-test-cards.html` — anatomia de card
- `marca/design-guide-test-inputs.html` — inputs e formulários
- `marca/design-guide-test-nav.html` — header, drawer, tabs, footer
- `marca/design-guide-test-overlays.html` — modais, toasts, tooltips

> Os arquivos `design-guide-test-*.html` foram criados pro sistema antigo. Quando for revisar/atualizar componentes web, partir do tema WP como fonte e adaptar esses testes em paralelo.

---

## Observações adicionais

- Para materiais de clientes, sempre consultar o branding específico do cliente antes de criar — a estética Beza fica como suporte
- Referências visuais usadas em alguns projetos: storytelling em quadrinhos, blueprint style
- Geração de imagens via IA: estilo cinematic concept art, photorealistic quando aplicável
- Quando criar um material novo, abrir um arquivo do tema WP em paralelo pra calibrar o "tom" visual

---

## Marca pessoal do Luan (founder)

A marca pessoal do Luan usa o mesmo sistema da Beza, numa leitura ainda mais escura e monocromática. Serve pros materiais de founder-led growth: posts pessoais, apresentações e aulas onde ele assina como Luan, não como Beza.

Diferenças em relação ao guia da Beza:

- Fundo puxa pro preto (`#0A0A10` a `#0D0D0D`), mais chapado, com menos variação entre `bg`, `bg-soft` e `bg-card`.
- Lilás entra com muito mais parcimônia. Aqui ele é pontual (um dot, um accent, uma palavra-chave), e o material pode ser quase preto e branco. Na Beza o lilás aparece com mais frequência; na marca do Luan ele é assinatura mínima.
- Tipografia idêntica: IBM Plex Sans Medium 500 nos headings, IBM Plex Mono nos labels e eyebrows.
- Grain noise, eyebrow com dot e glassmorphism continuam valendo.
- Sensação alvo: mais sóbria e "stealth". A Beza é dark premium com lilás. A marca do Luan é dark premium quase monocromática, com o lilás só na assinatura.

Ponto de partida. Refinar quando o Luan produzir o primeiro material pessoal.

Estilo de tirinha doodle (line art preto à mão, personagem = Luan): guia completo e templates de prompt em `conteudo/luan/tirinhas/estilo.md`.
