# ARCHITECTURE - Creative Intel (spy-cs)

> Estrutura tecnica do dashboard. Referencia para quem vai modificar o codigo.

## Visao geral

Arquivo unico `index.html` (~161 KB, ~2033 linhas). Tudo inline: CSS (linhas 7-235), HTML (linhas 237-470), JS (linhas 471-2030). Sem build step, sem bundler, sem framework.

Dependencias externas: Google Fonts (Inter + JetBrains Mono) via CSS import.

## CSS (linhas 7-235)

### Custom Properties (`:root`)
Dark theme por default. Light theme via `@media(prefers-color-scheme:light)` e `[data-theme="dark"]` override.

Variaveis principais:
- `--bg-0` a `--bg-4`: backgrounds (do mais escuro ao mais claro)
- `--text-1` a `--text-4`: textos (do mais claro ao mais opaco)
- `--accent`, `--accent-2`, `--accent-bg`: azul primario
- `--green/yellow/red/orange/purple/cyan/pink` + `-bg`: cores semanticas
- `--sidebar-w: 240px`: largura da sidebar
- `--header-h: 60px`: altura do topbar

### Layout
- `.app`: flex container, min-height 100vh
- `.sidebar`: `position:fixed`, top/left/bottom 0, largura `--sidebar-w`, z-index 100
- `.main`: `margin-left: var(--sidebar-w)`, `height:100vh`, `overflow-y:auto` (scroll container)
- `.topbar`: `position:sticky`, top 0, z-index 50, dentro de `.main`
- `.content`: padding 24px, contem as sections

### Responsividade (max-width 768px)
- Sidebar esconde com `translateX(-100%)`, aparece com classe `.open`
- `.main` perde `margin-left`
- `.burger` aparece no topbar
- Grids se adaptam (KPIs 2col, brands 1col)

### Classes de componentes
- `.kpi` / `.kpi-grid`: cards de metricas no overview
- `.card`: container generico com borda e padding
- `.brand-card` / `.brand-grid`: cards de marca
- `.table-card`: tabela com header sticky e hover
- `.tag-*`: badges de formato (video/static/carousel/ugc)
- `.pill` / `.pill-niche`: filtros de nicho no topbar
- `.kw-chip` / `.kw-card`: keywords chips e cards
- `.nav-item` / `.nav-badge`: items e badges da sidebar

## HTML (linhas 237-470)

### Estrutura
```
div.app
  aside.sidebar#sidebar
    div.sidebar-brand (logo "CI" + titulo)
    nav.sidebar-nav
      [nav-sections: Core, Strategy, Analysis, Tools]
      [nav-items com data-section="..."]
    div.sidebar-footer
  div.main
    header.topbar
      button.burger#burger
      span.topbar-title
      div.topbar-right#niche-filters (pills de nicho)
    div.content
      [15 sections com id="sec-{nome}"]
```

### Tabs (sections)
| data-section | id | Funcao render |
|---|---|---|
| overview | sec-overview | renderOverview() |
| brands | sec-brands | renderBrands() |
| brand-dir | sec-brand-dir | renderBrandDirectory() |
| catalog | sec-catalog | renderCatalog() |
| controls | sec-controls | renderControls() |
| coverage | sec-coverage | renderCoverage() |
| gaps | sec-gaps | renderGaps() |
| angles | sec-angles | renderAngles() |
| spy-reports | sec-spy-reports | renderSpyReports() |
| timeline | sec-timeline | renderTimeline() |
| compare | sec-compare | renderCompare() |
| teardowns | sec-teardowns | renderTeardowns() |
| vsl-extract | sec-vsl-extract | extractVturb() |
| config | sec-config | renderConfig() |
| keywords | sec-keywords | renderKeywordsTab() |
| discovery | sec-discovery | renderDiscovery() |

## JS - Arrays de dados (linhas 471-880)

### SPY_REPORTS (linha 472)
Array de objetos. 3 relatorios de intel.
```js
{ id, title, nicho, date, type, brands:[], content:'HTML string' }
```

### NICHE_CONFIG (linha 583)
Objeto com 5 keys (health, skincare, cookware, apparel, accessories).
```js
{ label:'Health & Wellness', cor:'#22c55e', keywords:['...'] }
```

### NICHE_KW_LANG (linha 592)
Keywords multilang por nicho.
```js
{ health: { en:['...'], pt:['...'], es:['...'] }, skincare: {...}, ... }
```

### BRANDS (linha 622)
Array de 17 objetos. Marcas com dados detalhados.
```js
{ id:'seed', name:'Seed', nicho:'health', url:'seed.com', pageId:'178024832922517',
  posicionamento:'...', totalAds:210, videoPercent:45, controle:'Hook text',
  dataUltimoScan:'2026-09-02', status:'active'|'client' }
```
`status:'client'` = Longwell (marca do cliente, badge especial).

### BRAND_DIRECTORY (linha 643)
Array de 129 objetos. Catalogo amplo de marcas DTC.
```js
{ brand:'Nome', nicho:'health', site:'site.com', pageId:'META_PAGE_ID',
  tier:'watchlist'|'tier1'|'tier2'|'emerging', what:'Descricao curta', creatives:[] }
```
Fonte: `swipe-dr-dtc.json` (base_conhecimento), processado por script Python.

### ADS_CATALOG (linha 776)
Objeto com keys = brand id, cada uma array de ads.
```js
{ seed: [
    { adId:'seed-001', formato:'video'|'static'|'carousel', duracao:'30s',
      angulo:'science-authority', produto:'DS-01', beneficios:['gut health'],
      awareness:'solution-aware', hookText:'Most probiotics die before...',
      variacoes:12, dataInicio:'2026-05-10', isControle:true, link:'' }
  ], ag1: [...], ... }
```
17 brands, 57 ads total.

### AWARENESS_LEVELS / AWARENESS_COLORS / AWARENESS_SHORT (linha 871)
Constantes pra mapear os 5 niveis de awareness (Schwartz).

### ALL_FORMATS / ALL_ANGLES (linhas 876-877)
Arrays com todos os formatos e angulos possiveis.

### SEED_DISCOVERY (linha 881)
Discovery entries pre-populados. localStorage key: `spy_cs_discovery`.

## JS - Funcoes (linhas 890-2030)

### State
```js
let _nichoF = null;      // filtro de nicho ativo (null = todos)
let _sortCol = 'brand';  // sort column do Ad Catalog
let _sortDir = 1;        // sort direction (1=asc, -1=desc)
let _brandDirSortCol = 'brand';  // sort do Brand Directory
let _brandDirSortDir = 1;
```

### Computed (funcoes puras)
- `getAllAds(nichoFilter)` - retorna flat array de todos os ads, filtrado por nicho
- `getFilteredBrands()` - retorna BRANDS filtrado pelo nicho ativo
- `computeControls(nichoFilter)` - ads onde isControle=true
- `computeCoverage(brandId)` - % de cobertura de formatos e awareness por marca
- `computeAwarenessDist(brandId)` - distribuicao de awareness levels
- `computeGaps(nichoFilter)` - gaps de cobertura (formatos/angulos nao cobertos)
- `computeAngleDist(nichoFilter)` - distribuicao de angulos

### Helpers
- `escHtml(s)` - escape HTML
- `showToast(msg, type)` - toast notification
- `nicheColor(nicho)` - retorna cor hex do nicho
- `nicheLabel(nicho)` - retorna label legivel do nicho

### Navegacao
- `TAB_TITLES` - mapa de data-section para titulo do topbar
- Event listener nos `.nav-item` que chama `showSection()` + `renderTab()`
- `showSection(sectionId)` - toggle `.active` nas sections
- `renderTab(tab)` - dispatcher que chama a funcao render correta
- `updateBadges()` - atualiza contadores nos nav-badges

### Niche Filter
- `renderNicheFilters()` - monta as pills no topbar
- `setNiche(n)` - seta `_nichoF` e re-renderiza a tab ativa

### Render functions (uma por tab)
- `renderOverview()` - KPIs + insights + ranking + charts
- `renderBrands()` - grid de brand cards
- `renderBrandDirectory()` + `renderBrandDirTable()` + `sortBrandDir()` - tabela com filtro/sort
- `renderCatalog()` + `renderCatalogTable()` - tabela de ads com sort
- `renderControls()` - lista de ads controle
- `renderCoverage()` - matriz de cobertura formato x marca
- `renderGaps()` - lista de gaps
- `renderAngles()` - distribuicao de angulos
- `renderSpyReports()` - relatorios de intel
- `renderTimeline()` / `renderCompare()` / `renderTeardowns()` - placeholders
- `renderConfig()` - config do Supabase + keywords
- `renderKeywordsTab()` - keywords por nicho com chips clicaveis
- `renderDiscovery()` - auto-discovery com localStorage

### Charts (Canvas)
- `drawBar(id, labels, data, colors, maxVal)` - desenha bar chart em canvas
- `renderCharts(ads)` - monta os 2 charts do overview (formatos + angulos)

### Video Modal
- `openVideoModal(vidUrl, thumbUrl, adId)` - abre modal de video
- `closeVideoModal()` - fecha modal
- `toggleModalView()` - toggle entre video e thumbnail

### Discovery
- `loadDiscoveryData()` / `saveDiscoveryData()` - localStorage CRUD
- `salvarDiscovery()` - adiciona novo item
- `toggleMonitorar(id)` / `removeDiscovery(id)` / `editarAdCount(id)` - acoes

### Config / Supabase
- `saveSbConfig()` - salva URL e key do Supabase em localStorage
- Init block (linha 2022): carrega config do Supabase do localStorage

### VSL Extractor
- `extractVturb()` - extrai VTurb/ConverteAI player config de URL colada

## Fluxo de navegacao

```
1. Pagina carrega → INIT (linha 2017)
   ├── loadDiscoveryData() (localStorage)
   ├── renderNicheFilters() (pills no topbar)
   ├── renderOverview() (tab default)
   └── Load Supabase config (localStorage)

2. Click em nav-item
   ├── showSection() → toggle .active
   ├── renderTab() → chama render function da tab
   └── updateBadges()

3. Click em pill de nicho
   ├── setNiche() → seta _nichoF
   └── Re-renderiza tab ativa com filtro
```
