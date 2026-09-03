# BUILD HISTORY - Creative Intel (spy-cs)

> Historico completo de construcao. Cada commit expandido com contexto.

## Timeline

Construido em 2 sessoes (2026-09-02 e 2026-09-03), 14 commits.

---

### Commit 1: `fe7bfeb` (2026-09-02)
**feat: Creative Intel dashboard v1.0**

Primeiro commit. Dashboard criado do zero como arquivo HTML unico.
- Estrutura base: sidebar, topbar, content area
- 4 nichos iniciais: health, skincare, apparel, accessories
- 13 marcas no BRANDS array (Seed, AG1, Bioma, ARMRA, Tatcha, The Ordinary, Paula's Choice, Vuori, True Classic, Quince, Mejuri, Ana Luisa, Gorjana)
- 47 ads no ADS_CATALOG
- 15 tabs na sidebar (Overview, Brands, Catalog, Controls, Coverage, Gaps, Angles, Spy Reports, Timeline, Compare, Teardowns, Extract VSL, Config, Keywords, Discovery)
- CSS dark theme com custom properties
- KPIs no overview (Total Ads, Brands, Controls, Avg Variacoes)
- Canvas charts (Format Distribution, Top Angles)
- Config tab com campos pra Supabase URL/key

Decisao: arquivo unico pra facilitar deploy via GitHub Pages (zero build step).

---

### Commit 2: `d1bd69b` (2026-09-02)
**feat: add cookware niche + clickable Ad Library keywords**

- Adicionado 5o nicho: **cookware** (cor #f97316)
- 4 marcas cookware: Longwell, HexClad, Our Place, Caraway
- 10 ads cookware no ADS_CATALOG
- Keywords clicaveis que abrem busca na Meta Ad Library
- NICHE_CONFIG atualizado com keywords por nicho
- Total: 5 nichos, 17 marcas, 57 ads

Decisao: cookware adicionado porque Longwell e cliente.

---

### Commit 3: `126ab88` (2026-09-02)
**feat: add 6 features from spy-renda-extra**

Portado funcionalidades do spy-renda-extra (dashboard original BR):
1. **Insights banner** no overview (analise automatica do catalogo)
2. **Charts canvas** melhorados (bar charts com labels, valores, grid)
3. **Rich catalog** com acoes por ad (menu dropdown: Ad Library, save, notes)
4. **Video modal** pra preview de ads
5. **Discovery tab** com formulario de adicao + localStorage
6. **Keywords DB** com chips clicaveis + link Meta Ad Library

---

### Commit 4: `3793777` (2026-09-02)
**feat: add clickable Ad ID column to Ad Catalog tab**

- Coluna "Ad ID" na tabela do catalogo agora e clicavel
- Link vai pra pagina do anunciante na Meta Ad Library
- Monta URL: `https://www.facebook.com/ads/library/?active_status=active&ad_type=all&country=US&view_all_page_id={pageId}`

---

### Commit 5: `fbb942a` (2026-09-03)
**fix: Ad ID links now open advertiser page in Meta Ad Library**

Bug: links do Ad ID apontavam pra URL incorreta.
Fix: corrigido o padrao da URL da Meta Ad Library.

---

### Commit 6: `681fdbb` (2026-09-03)
**feat: add full Auto-Discovery and Keywords tabs adapted from spy-renda-extra**

- Discovery tab completa: formulario, lista, monitoramento toggle, edit ad count, quick keywords
- Keywords tab: cards por nicho com chips clicaveis, multilang (EN/PT/ES), link Ad Library
- NICHE_KW_LANG com keywords em 3 idiomas por nicho
- localStorage pra persistir discovery data e keyword status

---

### Commit 7: `c321f0a` (2026-09-03)
**fix: hide video modal overlay by default**

Bug: modal de video aparecia como overlay escuro na carga inicial.
Fix: `display:none` no `.video-modal-overlay`, so aparece quando aberto via JS.

---

### Commit 8: `4a6eaf2` (2026-09-03)
**feat: populate Spy Reports tab with 3 niche intel reports**

- 3 relatorios de espionagem adicionados ao SPY_REPORTS array:
  1. Health & Wellness: GLP-1 Supplement Wars (health)
  2. Cookware: PFAS Fear Funnel Analysis (cookware)
  3. Skincare: Colostrum vs Collagen Battle (skincare)
- Cada relatorio com analise detalhada, marcas envolvidas, tendencias

---

### Commit 9: `64a2e46` (2026-09-03)
**feat: add 50+ keywords extracted from ads catalog angles/benefits**

- Keywords expandidas significativamente em todas as categorias
- Cookware ganhou 35 keywords (EN/PT/ES) incluindo fear-based, comparison, warranty
- Apparel e Accessories ganharam keywords mais especificas
- Total: 50+ keywords novas

---

### Commit 10: `a2d84c7` (2026-09-03)
**feat: Longwell client integration -- 10 ads, 17 cookware keywords, CLIENT badge**

- Longwell marcado como `status:'client'` (vs 'active' das outras)
- Badge visual "CLIENT" na Brand Card
- 10 ads da Longwell no catalogo com angulos reais (fear-pfas, titanium-pure, warranty-anchor, etc.)
- 17 keywords cookware adicionais

---

### Commit 11: `545240d` (2026-09-03)
**feat: AD ID column links to specific ad when link available**

- Se o ad tem campo `link` preenchido, o Ad ID aponta direto pro ad
- Se nao tem link, aponta pra pagina do anunciante (comportamento anterior)

---

### Commit 12: `12f93d0` (2026-09-03)
**feat: add direct Ad Library links for 5 Longwell ads**

- Links diretos da Meta Ad Library adicionados a 5 ads Longwell (lw-005 a lw-010)
- Links reais extraidos manualmente da biblioteca de anuncios

---

### Commit 13: `369d140` (2026-09-03)
**feat: add Brand Directory tab with 129 DTC brands**

Feature grande. Nova tab "Brand Directory" com 129 marcas DTC.
- Dados extraidos de `swipe-dr-dtc.json` via script Python (`gen_brand_dir.py`)
- Classificacao por nicho via sets manuais no script
- Tabela com colunas: Brand, Niche, Site, Page ID, Tier, Description
- Filtro por nicho (chips) e por tier (select)
- Busca por texto
- Sort clicavel nas colunas
- Page IDs como links pra Meta Ad Library
- Contagem no badge da sidebar

Pipeline de geracao:
1. `gen_brand_dir.py` leu `swipe-dr-dtc.json` → gerou `brand_dir_array.js` (129 entries)
2. `merge_brand_dir.py` inseriu o array no `index.html` antes do `ADS_CATALOG`
3. Funcoes `renderBrandDirectory()`, `renderBrandDirTable()`, `sortBrandDir()` adicionadas

---

### Commit 14: `28a40df` (2026-09-03)
**fix: add scroll to main content area**

Bug: Brand Directory com 129 linhas (~5200px de conteudo) nao scrollava corretamente. A sidebar era `position:fixed` mas o conteudo crescia sem limite, mostrando espaco vazio abaixo do viewport.

Diagnostico: inspecao do DOM via JS, checando `overflow`, `position`, `clientHeight`, `scrollHeight` de cada container.

Fix: adicionado `height:100vh;overflow-y:auto` ao `.main`, fazendo dele o scroll container.

Linha alterada (75): `.main{margin-left:var(--sidebar-w);flex:1;min-width:0;height:100vh;overflow-y:auto}`

---

## Decisoes de design

1. **Arquivo unico**: facilita deploy GitHub Pages, sem build step
2. **Dados inline**: BRANDS/ADS_CATALOG como constantes JS no arquivo, nao fetch externo
3. **Canvas pra charts**: sem dependencia de chart library (Chart.js etc.)
4. **localStorage pra state**: Discovery e Keywords status persistem no browser
5. **Supabase opcional**: config salva em localStorage, integracao futura
6. **Dark theme default**: padrao pra ferramentas de trabalho
7. **Longwell como CLIENT**: marca do cliente tem tratamento visual diferenciado
