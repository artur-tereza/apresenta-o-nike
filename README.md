# Nike Air Stride · conceito 3D

Landing page interativa de um tênis conceitual em **5 colorways**, feita em um único `index.html` com **Three.js / WebGL** e JavaScript puro (sem build, sem framework).

O destaque é **um único tênis 3D que viaja pela página inteira**: você escolhe a cor no carrossel do topo, e esse mesmo par sobe para a seção de tecnologia, gira a cada etapa, atravessa uma "tempestade" das próprias peças e pousa exatamente no card dele na loja.

> Projeto conceitual de portfólio. Sem afiliação, patrocínio ou endosso da Nike, Inc. Nike e o Swoosh são marcas da Nike, Inc. Nenhuma compra é processada no site.

---

## Seções

| # | Seção | O que acontece |
|---|---|---|
| 0 | **Intro / loader** | O swoosh se desenha conforme o modelo 3D e as texturas carregam de verdade; contador em odômetro; carimbo do logo; cortina que sobe revelando o hero. |
| 1 | **Cores (hero)** | Arco com 7 tênis 3D em perspectiva. Arrastar (com inércia), clicar num tênis, seta → ou teclado ← →. A cor nova cresce como um círculo com borda suave a partir do tênis central; o título troca letra por letra; autoplay a cada 3,2 s. |
| 2 | **Tecnologia** | Seção fixada (500vh). O par gira 180° a cada etapa (lateral, medial, solado), com títulos, número vazado e estatística em odômetro. No fim, o fundo escurece e 20 peças (bolhas Air, cadarços, espuma, waffle, swooshes) explodem uma única vez enquanto o tênis gira. |
| 3 | **Loja** | O tênis 3D pousa no próprio card e acompanha a inclinação do card (tilt com luz que segue o cursor). Clique = "squash"; **+** voa até a sacola. |
| 4 | **Monte seu pack** | 3 slots, miniaturas, preço em odômetro, desconto de 15%. "Adicionar ao carrinho" faz os três pares voarem até a sacola e solta confete. |
| 5 | **Rodapé** | Manifesto que "acende" palavra por palavra com a rolagem, contagem regressiva do próximo drop e o nome gigante revelado por máscara. |

## Animações usadas

Além do roteiro original, a página aplica efeitos inspirados na vitrine de animações JS do [naocodei.com/free-code](https://naocodei.com/free-code/), todos reescritos para este site:

- rolagem com inércia (roda do mouse suavizada no desktop)
- cursor personalizado (anel que vira "ARRASTE" / "ESCOLHER" sobre o carrossel)
- botões magnéticos com mola (seta, Lista VIP, comprar, pular intro)
- embaralhar letras nos rótulos em fonte mono
- odômetro (loader, estatísticas, preço, contagem regressiva)
- contador regressivo
- texto que se preenche com a rolagem
- máscara que revela (nome gigante no rodapé)
- faixas em movimento que aceleram e invertem com a velocidade da rolagem
- luz que segue o cursor nos cards
- confete
- revelação de linhas nos títulos e cards em cascata

Tudo respeita `prefers-reduced-motion`: sem intro, sem autoplay, trocas instantâneas e sem partículas.

## Como funciona por dentro

- **Um só `<canvas>` fixo** (`z-index: 2`, sem eventos de ponteiro) desenha todos os tênis. O DOM decide onde cada um fica: o código lê o retângulo de elementos-âncora (`#inBar`, o `.pk` de cada card) a cada frame e interpola o estado do tênis (posição, tamanho, rotação, opacidade) entre **hero → seção 2 → card**.
- **Câmera calibrada para pixels**: a `fov` é calculada para que 1 unidade do mundo = 1 px CSS em z = 0, então coordenadas da tela viram coordenadas 3D diretamente.
- **Colorways gerados em tempo real**: o modelo tem 3 variantes de textura. O código compara as três pixel a pixel para descobrir quais regiões são cabedal, detalhe ou solado/forro, e repinta a textura em 5 combinações de cor.
- **Swoosh projetado** nos dois lados do cabedal com `DecalGeometry`, desenhado em canvas a partir de um path SVG.
- **Sprites dos cards** são renderizados uma vez a partir do próprio modelo 3D, então o card e o tênis 3D são sempre o mesmo par.
- O loop de renderização para quando nenhum tênis está visível.

## Rodar localmente

Precisa de um servidor estático (módulos ES não abrem via `file://`). Qualquer um serve:

```bash
# Python
python -m http.server 5173

# Node
npx serve .
```

Depois abra `http://localhost:5173`. No VS Code, a extensão **Live Server** também funciona.

## Publicar no GitHub Pages

1. Crie o repositório e envie os arquivos:
   ```bash
   git init
   git add index.html README.md .gitignore
   git commit -m "Nike Air Stride: conceito 3D"
   git branch -M main
   git remote add origin https://github.com/<seu-usuario>/<seu-repo>.git
   git push -u origin main
   ```
2. No GitHub: **Settings → Pages → Build and deployment → Source: Deploy from a branch**, branch `main`, pasta `/ (root)`.
3. Em ~1 minuto o site fica em `https://<seu-usuario>.github.io/<seu-repo>/`.

## Personalizar

No topo do `<script type="module">` em `index.html`:

- `CW`: nome, cor de fundo e cores do cabedal / detalhe / solado / forro / swoosh de cada colorway
- `START`: colorway inicial
- `PRICE` e `PACK_OFF`: preço e desconto do pack
- `DROP_AT`: data da contagem regressiva
- `STEPS`: textos, ângulos e estatísticas da seção de tecnologia

## Créditos

- Modelo 3D **"Materials Variants Shoe"** © Shopify, licença [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), distribuído nos exemplos do [three.js](https://github.com/mrdoob/three.js) e carregado via jsDelivr.
- [Three.js](https://threejs.org) r160 (MIT).
- Fontes Anton, Inter e DM Mono via Google Fonts (OFL).
- Referência de animações: [naocodei.com/free-code](https://naocodei.com/free-code/).
