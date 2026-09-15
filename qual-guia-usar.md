# Qual guia usar

A Beza tem duas versões do design guide. Mesma marca, mesma cor, mesma fonte. O que muda é o que a peça precisa fazer: convencer ou operar.

| Você está fazendo | Guia | Renderizado |
|---|---|---|
| Conteúdo (carrossel, reel, stories, post), site, landing, página de venda, proposta, deck, aula, email, checkout | `design-guide.md` | `manual-de-marca.html` (https://manual-marca-beza-v2.vercel.app) |
| Sistema: área logada, painel, dashboard, CRM, admin, produto interno, tela de configuração, qualquer coisa com tabela, formulário de operação e dado | `design-guide-sistemas.md` | `manual-de-marca-sistemas.html` (https://manual-marca-beza-v2.vercel.app/sistemas) |
| Material pessoal do Luan (@luansinezio) | `design-guide-luan.md` | `manual-de-marca-luan.html` (https://manual-marca-beza-v2.vercel.app/luan) |

## O teste

Se a tela existe pra alguém tomar uma decisão de compra ou consumir uma ideia, é marca. Se a tela existe pra alguém trabalhar dentro dela todo dia, é sistema. Na dúvida, pergunta: essa pessoa vai olhar isso uma vez ou cinquenta vezes por semana? Uma vez é marca. Cinquenta é sistema.

## Por que existem dois

O guia da marca foi feito pra convencer: grain, glass, gradient em texto, eyebrow em todo heading, botão pill com glow, reveal on scroll, escala hero. Tudo isso chama atenção pra peça. Dentro de um painel, chama atenção pra si mesmo e compete com o dado.

O guia de sistemas é a mesma marca com esse volume abaixado. Mantém o preto azulado, o lilás como único acento, os cinzas com temperatura lilás, a IBM Plex, o tema claro oficial e as cores de estado. Tira o que decora e acrescenta o que uma ferramenta precisa: tokens de superfície em três tons, rail de navegação, tabela, kanban, drawer, modal, toast, gráfico, badge de status, skeleton, empty state, regra de densidade.

## O que cada um decide

| Decisão | Marca | Sistemas |
|---|---|---|
| Botão primário | gradient lilás com glow, pill | lilás chapado, raio 12px |
| Heading | Medium 500, gradient em destaque, eyebrow acima | Medium 500, sem gradient, sem eyebrow |
| Fundo | grain noise, luz ambiente | luz ambiente difusa, sem grain |
| Card | lift de 4px no hover, sombra forte | borda lilás no hover, sombra leve |
| Cursor | ponteiro lilás | nativo |
| Motion | reveal on scroll, aurora no claro | só entrada de conteúdo e transição de estado |
| Mono em caixa alta | eyebrow e label | só overline de grupo, três por tela no máximo |

## Onde os dois se encontram

Login, onboarding e formulário público de um sistema são a fronteira: seguem o guia de sistemas na estrutura e o guia da marca no acabamento (IBM Plex, botão brand no formulário conversacional, grain, eyebrow na intro). A tabela exata está na seção "Superfície pública" do guia de sistemas.

## Regras que valem pros dois

- Não misturar os dois guias na mesma tela. Escolhe um e vai até o fim.
- O arquivo `.md` é a fonte de verdade. O `.html` é a versão renderizada pra revisar por código de bloco (`06.4`, `11.3`). Ajuste aprovado no HTML volta pro `.md`.
- Mudança de token de cor ou fonte nasce no `design-guide.md` da marca e desce pro de sistemas. Nunca o contrário.
- Skill que gera tela de sistema lê `design-guide-sistemas.md`. Skill que gera conteúdo, proposta ou site lê `design-guide.md`.

## Origem

O guia de sistemas nasceu em 15/set/2026 da extração visual do CRM Tenda (`github.com/luansinezio/crm`), alinhado depois aos tokens da marca. A estrutura e os componentes vêm do CRM. A cor, a fonte e as regras vêm da marca.
