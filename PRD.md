# PRD — Cópia fiel do index.html com paleta de cores da Evolux

## 1. Escopo (reduzido, conforme instrução do usuário)

Duas coisas, só isso:

1. **Cópia fiel** do `index.html` atual (mesma estrutura, mesmo texto, mesmas
   seções, e — crítico — **nenhuma animação pode faltar**).
2. **Trocar a paleta de cores** de acento (hoje âmbar/laranja/rosa, um tema
   "fogo") pelo **verde extraído do `icone.jpeg`** da Evolux.

Nada de reescrever texto, nada de inserir `logo.jpeg`/`icone.jpeg` como
imagem em nenhum lugar da página, nada de mexer em layout/estrutura. Só copiar
fielmente + recolorir.

## 2. Contexto técnico (por que a cópia não pode ser um `cp` simples)

O `index.html` atual (717KB) é um "Save Page As" de um site chamado "Vectra
Digital Agency", construído na plataforma **aura.build**. Ele contém:

- O HTML/CSS reais do site (CSS do Tailwind já **compilado e injetado como
  `<style>` estático**, capturado no momento do save).
- Um monte de código que **não é o site**, é bagagem do editor da
  aura.build: um `<script data-offline-runtime="1">` com ~490KB de snapshot
  interno do editor (referenciando ~250 arquivos JS/JSON que **não são
  carregados pela página real**), scripts de fallback de imagem/offline do
  editor, um firewall de token do Supabase da conta da Vectra, e tracking do
  Google Ads/Analytics da Vectra.

Confirmado por inspeção: das ~260 referências a `assets/*` dentro do
`index.html`, **apenas 13 arquivos aparecem em tags reais** (`src=`/`href=`
fora dos blobs de script) — o resto é usado só dentro daquele blob de
snapshot do editor, que não afeta o site renderizado.

**Isso importa para "cópia fiel" porque**: manter esse código não muda nada
visualmente, mas um dos itens é tecnicamente incompatível com a troca de
cores — ver seção 4.

## 3. O que precisa ser preservado 100% (não pode faltar)

### 3.1 Animações CSS (`@keyframes`, no `<style>` principal)
`navDrop`, `logoFade`, `lineReveal`, `ctaRise`, `softIn`, `shimmer`,
`cms-shimmer`, `scrollMarquee`, `rotateCubeY`, `pulse`, `ping` — e as classes
que as disparam (`.nav-animate`, `.eyebrow-animate`, `.line-animate-*`, etc.).
Copiar exatamente como estão.

### 3.2 Interações/animações via JavaScript (scripts inline, mantidos como estão)
- Menu mobile (`#mobile-menu-btn` / `#mobile-menu`)
- Reveal palavra-por-palavra em `#lyric-container` (`.lyric-word`)
- Cena 3D em **Three.js** no `#webgl-canvas-wow` (importado via ESM do
  jsdelivr — permanece como dependência externa, não vendorizar)
- Dois efeitos de "typewriter" (`#typewriter-text-aura-local` e
  `#typewriter-text-aura`)
- Reveal de depoimentos via `IntersectionObserver` (`#testimonials` /
  `#scroll-container`)
- Animação de cards em `#pricing .pricing-stage`
- Autoplay controlado de vídeo (`data-aura-video-controller`)

Nenhum desses 7 blocos de script deve ser removido, resumido ou alterado.

### 3.3 Texto e estrutura
Todo o texto (headlines, menu, depoimentos, preços, rodapé, "VECTRA" na
navbar etc.), toda a estrutura de seções, classes de layout, espaçamento e
breakpoints responsivos permanecem **exatamente iguais** ao original. Só a
cor muda.

## 4. Código a remover na cópia (não afeta fidelidade visual)

Remover apenas estes `<script>` (identificar por atributo, não por
posição/linha):

- `<script data-offline-runtime="1">` — blob de ~490KB do editor, não usado
  pela página renderizada.
- `<script data-offline-resolve="1">`, `<script data-offline-fix="1">`,
  `<script data-img-fallback-handler="">` — mecanismos de fallback do editor.
- `<script id="aura-supabase-token-firewall">` — protege um projeto Supabase
  que pertence à Vectra.
- `<script async src="https://www.googletagmanager.com/gtag/js?...">` + o
  `gtag('config', 'G-2M6V79H761')` inline — Analytics da Vectra.
- `<script async src="https://googleads.g.doubleclick.net/...">` — Ads da
  Vectra.
- **`<script src="assets/176e894661aa9cdc_3.4.17">`** — este arquivo é o
  runtime do **Tailwind CDN** (`cdn.tailwindcss.com/3.4.17`), que recompila
  as classes Tailwind no navegador a partir dos nomes das classes. Como o
  `<style>` já tem o CSS **pré-compilado e estático**, deixar esse script
  ativo re-executaria a compilação e **desfaria a troca de cores da seção
  5**, devolvendo o âmbar/laranja original. Por isso precisa sair — não é
  só limpeza, é requisito técnico para a recoloração funcionar.

Nenhuma dessas remoções altera texto, layout ou qualquer animação da seção 3.

## 5. Assets: copiar só o que é usado

Copiar para a pasta da cópia (`evolux/assets/` ou equivalente) **somente**
estes 13 arquivos:

| Arquivo atual | Uso |
|---|---|
| `f32c4a47e91d6996_css2.css` | Fonte Inter (Google Fonts, localizada) |
| `851bed7af266f96a_iconify-icon.min.js` | Lib de ícones (`<iconify-icon>`) |
| `0414cc4e2e5b7224_1600w.jpg` | Imagem de seção |
| `24d5d7c955266d47_b202409f-816e-4451-8ac9-bd0b04.webp` | Imagem |
| `96d1a7a807e001b5_ff19765f-d4f3-444a-989f-5936bb.webp` | Imagem |
| `986de0a12ccb7dc2_ce5a380c-785b-4ec0-9cd9-486d09.webp` | Imagem |
| `a3ae47881a6bd12c_a4b4a91c-eb34-493f-93c5-9b21e5.webp` | Imagem |
| `b5758c60a86b52de_9fc26ef9-ae15-4f56-ac63-077c76.png` | Imagem |
| `bbf5b5aedbc9dd1b_30ff8562-5a2d-46c8-8aa2-ab717e.jpg` | Imagem |
| `c6465b2a9a6dda4f_72adc0f8-ad1f-4732-a5bf-c000b4.webp` | Imagem |
| `ec96e7c3eb50c147_cca1b731-0621-4994-a36c-b2f5d5.webp` | Imagem |
| `5a2632adcabf5338_1779983177549-0b0303f7-4287-44.mp4` | Vídeo de fundo/seção |

Não copiar mais nada de `assets/` — o resto (JS de ícones Lucide, JSONs
`shared_code`/`solar`, HTMLs de outras rotas do editor, bundles como
`SandpackContainer-*.js`) pertence ao editor da aura.build, não ao site.
`logo.jpeg`/`icone.jpeg` **não entram** na cópia — são só a fonte de cor
(seção 6), não vão para dentro da página.

## 6. Paleta de cores: extraída do `icone.jpeg`

Analisei os pixels do `icone.jpeg` (ignorando o fundo preto): a cor
dominante do "E" isométrico da Evolux é um verde-jade saturado, em torno de
**`#009B73`** (HSL ≈ 163°, 100%, 30%).

O site atual usa a escala de cores do Tailwind (amber/orange/rose) tanto em
nomes de classe (`text-amber-400`, `from-orange-500`, `to-rose-600`) quanto
em valores `rgb()/rgba()` já expandidos no `<style>` compilado e em classes
de valor arbitrário (ex.:
`hover:shadow-[0_0_28px_rgba(251,191,36,0.22)]`). Como essas classes de
valor arbitrário guardam a cor dentro do próprio nome da classe — e esse
nome é idêntico no HTML e no CSS compilado — a troca pode ser feita com
**uma única substituição de texto global por par hex/rgb**, sem precisar
caçar seletor por seletor nem tocar nos nomes das classes.

Tabela de substituição (aplicar global, hex e o rgb triplet equivalente,
preservando o alpha/opacidade que já estiver em cada ocorrência):

| Papel na escala Tailwind | Hex original | RGB original | Novo hex (verde) |
|---|---|---|---|
| amber-100 | `#fef3c7` | 254,243,199 | `#c9fbe9` |
| amber-200 | `#fde68a` | 253,230,138 | `#8ff3d2` |
| amber-300 | `#fcd34d` | 252,211,77 | `#4fe3b9` |
| amber-400 | `#fbbf24` | 251,191,36 | `#16c98e` |
| amber-500 | `#f59e0b` | 245,158,11 | `#00b47f` |
| amber-600 | `#d97706` | 217,119,6 | `#00875f` |
| orange-200 | `#fed7aa` | 254,215,170 | `#a3f2da` |
| orange-300 | `#fdba74` | 253,186,116 | `#3fdcb2` |
| orange-400 | `#fb923c` | 251,146,60 | `#0fbd8c` |
| orange-500 | `#f97316` | 249,115,22 | `#009b73` (cor-base do ícone) |
| rose-200 | `#fecdd3` | 254,205,211 | `#bdf3e6` |
| rose-300 | `#fda4af` | 253,164,175 | `#5be6c4` |
| rose-400 | `#fb7185` | 251,113,133 | `#1ccf9e` |
| rose-500 | `#f43f5e` | 244,63,94 | `#009b73` |
| rose-600 | `#e11d48` | 225,29,72 | `#017a5c` |
| fuchsia-400/500 (uso pontual) | `#e879f9` / `#d946ef` | 232,121,249 / 217,70,239 | `#2fe0b8` / `#00c896` |

Cantos de vinheta com tom quente embutido no fundo (gradientes radiais de
seção, ex. `from-[#120a05]`), trocar por versão com tom frio neutro (mesmo
nível de escurecido, só sem o viés laranja/rosa):

| Original | Novo |
|---|---|
| `#120a05` | `#03110c` |
| `#120805` | `#03110c` |
| `#120509` | `#03110d` |
| `#100512` | `#02110e` |

**Não mexer** em: `#000`, `#050505`, `#0b0b0d`, `#0a0a0c`, `#fff`/`#ffffff`,
cinzas neutros (`#e5e7eb`, `#9ca3af` etc.) — fundo preto e textos neutros
continuam iguais, só o acento muda de quente pra verde.

Se aparecerem tons intermediários da escala amber/orange/rose não listados
na tabela (variações de opacidade tipo `/10`, `/20`, `/45` já são cobertas
pela troca do RGB base — a opacidade em si não muda), interpolar seguindo a
mesma lógica de matiz (~163°) e luminosidade equivalente à cor original.

## 7. Passo a passo

1. Criar pasta nova para a cópia (ex.: `evolux/`).
2. Copiar `index.html` → `evolux/index.html`.
3. Remover os `<script>` listados na seção 4 (por atributo, não por linha).
4. Copiar só os 13 assets da seção 5 para `evolux/assets/`.
5. Ajustar `src`/`href` em `evolux/index.html` se algum caminho mudar.
6. Aplicar a tabela de cores da seção 6 como substituição de texto global
   (hex e rgb triplet, preservando alpha de cada ocorrência).
7. Não tocar em mais nada: texto, estrutura, classes de layout, espaçamento
   e breakpoints ficam idênticos ao original.

## 8. Critérios de aceite

- [ ] As 11 `@keyframes` continuam presentes e idênticas (nomes e regras).
- [ ] Os 7 comportamentos JS da seção 3.2 continuam funcionando.
- [ ] Nenhuma requisição para Google Analytics/Ads, Supabase da Vectra, ou
      `cdn.tailwindcss.com` acontece ao carregar a página.
- [ ] Todo texto e estrutura idênticos ao `index.html` original.
- [ ] Nenhum tom âmbar/laranja/rosa/fúcsia visível sobra na página — todo
      acento "quente" virou verde.
- [ ] Fundo continua preto; textos neutros (branco/cinza) inalterados.
- [ ] `evolux/assets/` não contém nenhum arquivo fora da lista da seção 5.

## 9. Fora de escopo

- Reescrever qualquer texto do site.
- Inserir `logo.jpeg`/`icone.jpeg` como imagem em qualquer lugar da página.
- Favicon, og:image, splash/loading screen.
- Vendorizar o Three.js localmente (continua vindo do jsdelivr).
- Alterar estrutura, seções ou layout do site.
