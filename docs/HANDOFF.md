# HANDOFF - Creative Intel (spy-cs)

> Cole este documento inteiro na primeira mensagem do novo Claude.
> Ultima atualizacao: 2026-09-03

## QUEM

Tiago Felipe de Souza. Copywriter Senior DR / Trafego Meta / Creative Strategist.
Santa Fe do Sul, SP. PT-BR, conciso, direto.

## O QUE E

**Creative Intel** e um dashboard de espionagem criativa para marcas DTC (Direct-to-Consumer) dos mercados US/EU. Cobre 5 nichos (health, skincare, cookware, apparel, accessories) com 17 marcas detalhadas, 57 ads catalogados, e um diretorio de 129 marcas de referencia.

O proposito e mapear, dissecar e comparar as estrategias criativas dos maiores anunciantes DTC, identificando controles (ads vencedores), angulos, formatos, gaps de cobertura e oportunidades.

Construido como arquivo HTML unico, tudo inline (CSS + JS + dados). Sem dependencias externas alem do Google Fonts (Inter + JetBrains Mono).

## ONDE

- **Repo:** https://github.com/Drape7/spy-cs
- **Live (GitHub Pages):** https://drape7.github.io/spy-cs/
- **Local:** `C:\Users\Usuario\spy-cs\index.html`
- **Arquivo:** `index.html` (unico, ~161 KB, ~2033 linhas, tudo inline)
- **Dev server:** porta 8099, config em `.claude/launch.json` (Python http.server)
- **Supabase:** project id `iqwzrejpoqkmbaybiney`, credentials via localStorage (`spy-cs-sb-url`, `spy-cs-sb-key`)

## ESTADO ATUAL (2026-09-03)

### Features implementadas (14 commits, tudo em 2026-09-02/03)
- Dashboard completo com 15 tabs na sidebar
- 5 nichos com cores: health=#22c55e, skincare=#ec4899, cookware=#f97316, apparel=#3b82f6, accessories=#f59e0b
- 17 marcas detalhadas (BRANDS array) com posicionamento, total de ads, controle
- 57 ads catalogados (ADS_CATALOG) com formato, angulo, awareness level, hook text
- 129 marcas no Brand Directory (BRAND_DIRECTORY) com nicho, site, pageId, tier
- Filtro global por nicho (pills no topbar)
- Graficos canvas (Format Distribution, Top Angles)
- Keywords multilang (EN/PT/ES) com link pra Meta Ad Library
- Auto-Discovery tab com localStorage (adicionar marcas, monitorar)
- Spy Reports tab com 3 relatorios de intel por nicho
- Video modal pra preview de ads
- Ad Library links clicaveis nos Ad IDs
- Brand Directory com filtro por nicho/tier e busca
- Longwell & Co. como CLIENT com badge especial e links diretos
- Scroll fix: `.main` com `height:100vh;overflow-y:auto`

### Bugs conhecidos
- Nenhum bug aberto no momento.

### O que falta fazer
- 8 marcas no BRANDS sem pageId (Tatcha, The Ordinary, Paula's Choice, Vuori, Quince, Mejuri, Ana Luisa, Gorjana) - precisam ser preenchidos via Meta Ad Library
- Tabs Timeline, Compare, Teardowns estao vazias (placeholder)
- VSL Extractor (tab "Extract VSL") e funcional mas basico
- Spy Reports tem apenas 3 relatorios; pode crescer
- Integracao Supabase esta configurada mas sem uso ativo (apenas armazena URL/key)

### Tarefas de sessoes anteriores ainda pendentes
- P0.3: Atualizar EXISTING_MEDIA com bucket Supabase (spy-renda-extra, nao spy-cs)
- P0.4: Completar EVO_DATA para 12 paginas (spy-renda-extra, nao spy-cs)

## COMO CONTINUAR

### Para rodar localmente
```bash
cd C:\Users\Usuario\spy-cs
python -m http.server 8099
```
Abrir `http://localhost:8099` no browser.

### Padrao do codigo
- Tudo num unico `index.html` (CSS no `<style>`, JS no `<script>`)
- CSS usa custom properties (`:root` vars) com suporte dark/light theme
- Dados em arrays/objects JS constantes no topo do script
- Cada tab tem uma funcao `render[TabName]()` que monta o HTML
- Navegacao: click em `.nav-item[data-section]` chama `showSection()` + `renderTab()`
- Filtro global: variavel `_nichoF` (null = todos, ou string do nicho)
- Sort em tabelas: `_sortCol` / `_sortDir` / `_brandDirSortCol` / `_brandDirSortDir`
- Discovery e Keywords status salvos em localStorage

### Para adicionar uma nova marca ao BRANDS
Adicionar objeto ao array `BRANDS` (linha ~622) seguindo o schema:
```js
{ id:'slug', name:'Nome', nicho:'health', url:'site.com', pageId:'META_PAGE_ID',
  posicionamento:'Descricao curta', totalAds:100, videoPercent:50,
  controle:'Hook do ad controle', dataUltimoScan:'YYYY-MM-DD', status:'active' }
```
E adicionar ads no `ADS_CATALOG` (linha ~776) no key correspondente ao `id`.

### Para adicionar uma marca ao Brand Directory
Adicionar objeto ao array `BRAND_DIRECTORY` (linha ~643):
```js
{ brand:'Nome', nicho:'health', site:'site.com', pageId:'META_PAGE_ID',
  tier:'watchlist', what:'Descricao curta', creatives:[] }
```

### Para commit e deploy
```bash
cd C:\Users\Usuario\spy-cs
git add index.html
git commit -m "feat: descricao"
git push origin main
```
GitHub Pages atualiza automaticamente em ~1 min.

## REGRAS

- **REGRA ZERO:** O projeto original (renda-extra) NAO e tocado. Este e um repo SEPARADO.
- **R1:** NUNCA modificar `/raw` (material bruto imutavel na base_conhecimento).
- **R7:** Nunca apagar arquivos. Arquivar em `/ARCHIVE/` com timestamp.
- **gh CLI NAO esta instalado** nesta maquina. Usar `git push` direto.

## ARQUIVOS RELACIONADOS

- `C:\Users\Usuario\spy-cs\index.html` - o dashboard completo
- `C:\Users\Usuario\spy-cs\.claude\launch.json` - config do dev server (port 8099)
- `C:\Users\Usuario\spy-cs\docs\` - esta documentacao
- `C:\base_conhecimento\wiki\candidaturas\creative-strategist-usa\swipe-dr-dtc\swipe-dr-dtc.json` - fonte dos dados do Brand Directory (129 marcas extraidas daqui)

## HISTORICO DE SESSOES

O dashboard foi construido inteiramente em 2 sessoes (2026-09-02 e 2026-09-03). Ver `docs/BUILD-HISTORY.md` para o historico completo commit a commit.
