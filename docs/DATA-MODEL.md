# DATA MODEL - Creative Intel (spy-cs)

> Schema completo de todos os arrays de dados, fontes, e o que falta.

## NICHE_CONFIG (linha 583)

Objeto com 5 keys. Define os nichos do dashboard.

| Campo | Tipo | Descricao |
|---|---|---|
| label | string | Nome legivel (ex: "Health & Wellness") |
| cor | string | Hex color (ex: "#22c55e") |
| keywords | string[] | Keywords pra Meta Ad Library search |

**Valores:**
| Key | Label | Cor | Keywords |
|---|---|---|---|
| health | Health & Wellness | #22c55e | 33 keywords |
| skincare | Skin Care | #ec4899 | 30 keywords |
| cookware | Cookware | #f97316 | 21 keywords |
| apparel | Apparel | #3b82f6 | 14 keywords |
| accessories | Accessories/Jewelry | #f59e0b | 13 keywords |

---

## BRANDS (linha 622) - 17 marcas

Array de objetos. Marcas com dados detalhados de espionagem.

| Campo | Tipo | Descricao |
|---|---|---|
| id | string | Slug unico (ex: "seed") |
| name | string | Nome da marca (ex: "Seed") |
| nicho | string | Key do NICHE_CONFIG |
| url | string | Dominio (sem https://) |
| pageId | string | Meta Ad Library page ID (15-16 digitos, ou vazio) |
| posicionamento | string | Descricao do posicionamento/estrategia |
| totalAds | number | Volume estimado de ads ativos |
| videoPercent | number | % de ads que sao video |
| controle | string | Hook text do ad controle (mais rodado) |
| dataUltimoScan | string | Data do ultimo scan (YYYY-MM-DD) |
| status | string | "active" ou "client" (Longwell) |

**Marcas por nicho:**
- health (4): Seed, AG1, Bioma, ARMRA
- skincare (3): Tatcha, The Ordinary, Paula's Choice
- cookware (4): Longwell (CLIENT), HexClad, Our Place, Caraway
- apparel (3): Vuori, True Classic, Quince
- accessories (3): Mejuri, Ana Luisa, Gorjana

**Page IDs faltando (8 marcas):**
- Tatcha, The Ordinary, Paula's Choice (skincare)
- Vuori, Quince (apparel)
- Mejuri, Ana Luisa, Gorjana (accessories)

Para preencher: buscar na Meta Ad Library (`facebook.com/ads/library/`) pelo nome da marca, copiar o page ID da URL.

---

## BRAND_DIRECTORY (linha 643) - 129 marcas

Array de objetos. Catalogo amplo de marcas DTC de referencia.

| Campo | Tipo | Descricao |
|---|---|---|
| brand | string | Nome da marca |
| nicho | string | Key do NICHE_CONFIG |
| site | string | Dominio (sem https://) |
| pageId | string | Meta Ad Library page ID (ou vazio) |
| tier | string | "watchlist", "tier1", "tier2", "emerging" |
| what | string | Descricao curta do que a marca faz/vende |
| creatives | string[] | IDs de criativos salvos (geralmente vazio) |

**Distribuicao por nicho:**
- health: 79 marcas
- skincare: 28 marcas
- apparel: 12 marcas
- accessories: 2 marcas (Ridge, Beis)
- cookware: 8 marcas (mesmas do BRANDS + mais)

**Fonte dos dados:**
Arquivo `C:\base_conhecimento\wiki\candidaturas\creative-strategist-usa\swipe-dr-dtc\swipe-dr-dtc.json`

Processado por 2 scripts Python (salvos no scratchpad da sessao):
1. `gen_brand_dir.py` - leu o JSON, classificou por nicho via sets manuais, gerou array JS
2. `merge_brand_dir.py` - inseriu o array no index.html

**Classificacao de nicho:** feita via sets manuais no script Python (health_set, skincare_set, apparel_set, accessories_set). Marcas nao classificadas foram excluidas. O mapeamento site-domain tambem esta hardcoded no script (site_map dict).

---

## ADS_CATALOG (linha 776) - 57 ads

Objeto onde cada key e o `id` de uma marca do BRANDS, e o valor e um array de ads.

| Campo | Tipo | Descricao |
|---|---|---|
| adId | string | ID unico do ad (ex: "seed-001") |
| formato | string | "video", "static", "carousel" |
| duracao | string | Duracao (ex: "30s", "90s", ou vazio pra static) |
| angulo | string | Angulo criativo (ex: "science-authority", "quiz-funnel") |
| produto | string | Nome do produto anunciado |
| beneficios | string[] | Lista de beneficios mencionados |
| awareness | string | Nivel de awareness (Schwartz): "unaware", "problem-aware", "solution-aware", "product-aware", "most-aware" |
| hookText | string | Texto do hook do ad |
| variacoes | number | Numero de variacoes do ad |
| dataInicio | string | Data de inicio (YYYY-MM-DD) |
| isControle | boolean | Se e o ad controle da marca |
| link | string | Link direto pro ad na Meta Ad Library (ou vazio) |

**Ads por marca:**
| Marca | Qtd ads | Controles |
|---|---|---|
| seed | 4 | 1 |
| ag1 | 4 | 1 |
| bioma | 4 | 1 |
| armra | 3 | 1 |
| tatcha | 3 | 1 |
| ordinary | 3 | 1 |
| paulas | 3 | 1 |
| vuori | 3 | 1 |
| trueclassic | 4 | 1 |
| quince | 3 | 1 |
| mejuri | 3 | 1 |
| analuisa | 3 | 1 |
| gorjana | 3 | 1 |
| longwell | 10 | 1 |
| hexclad | 3 | 1 |
| ourplace | 3 | 1 |
| caraway | 3 | 1 |
| **Total** | **57** | **17** |

**Angulos mais comuns:**
science-authority, quiz-funnel, testimonial-transformation, creator-endorsement, problem-agitation, attack-competitor, luxury-ritual, before-after, ugc-reaction, fear-pfas, titanium-pure, warranty-anchor, comparison

---

## SPY_REPORTS (linha 472) - 3 relatorios

Array de objetos.

| Campo | Tipo | Descricao |
|---|---|---|
| id | string | Slug unico |
| title | string | Titulo do relatorio |
| nicho | string | Key do NICHE_CONFIG |
| date | string | Data (YYYY-MM-DD) |
| type | string | "trend-analysis", "fear-funnel", "ingredient-war" |
| brands | string[] | Marcas analisadas |
| content | string | HTML do conteudo do relatorio |

---

## NICHE_KW_LANG (linha 592) - Keywords multilang

Objeto: `{ [nicho]: { en:[], pt:[], es:[] } }`

Keywords pra busca na Meta Ad Library em 3 idiomas. Usadas na tab Keywords (chips clicaveis) e na tab Config (lista).

**Volume por nicho:**
| Nicho | EN | PT | ES |
|---|---|---|---|
| health | 23 | 23 | 23 |
| skincare | 21 | 21 | 21 |
| cookware | 35 | 35 | 35 |
| apparel | 16 | 16 | 16 |
| accessories | 16 | 16 | 16 |

---

## AWARENESS_LEVELS (linha 871)

5 niveis de awareness (Eugene Schwartz):
1. `unaware` (#ef4444)
2. `problem-aware` (#f97316)
3. `solution-aware` (#eab308)
4. `product-aware` (#3b82f6)
5. `most-aware` (#22c55e)

---

## ALL_FORMATS / ALL_ANGLES (linhas 876-877)

Formatos possiveis: video, static, carousel, ugc, advertorial, listicle
Angulos: lista extensa extraida dos ads do catalogo.

---

## localStorage keys

| Key | Tipo | Descricao |
|---|---|---|
| spy_cs_discovery | JSON string | Array de discovery items |
| spy_cs_kw_status | JSON string | Objeto com status de keywords |
| spy-cs-sb-url | string | URL do Supabase |
| spy-cs-sb-key | string | API key do Supabase |
