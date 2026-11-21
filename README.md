# videoprodutos

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

Pasta ou link de loja â†’ produtos descobertos â†’ imagens em presets de estilo â†’ vÃ­deo promocional
automÃ¡tico (16:9 e 9:16) â†’ projeto salvo, tudo por uma pÃ¡gina web com **fila de produÃ§Ã£o**.
Roda **100% local e sem custo** (ffmpeg + sharp + rembg). LLM e imagem por IA sÃ£o **opcionais**.

ðŸ“– **Guia de uso (landing):** https://inematds.github.io/videoprodutos/guia/

## O que faz

1. **Busca produtos** numa pasta (subpasta = produto, ou fotos soltas) ou numa URL
   (Shopify `/products.json`, WooCommerce Store API, JSON-LD `Product`, OpenGraph, ou imagens grandes da pÃ¡gina).
2. **Classifica** o produto (tecnologia, moda, alimento, casa, petâ€¦) e escreve o copy
   (tÃ­tulo, tagline, 3 benefÃ­cios, CTA, cenÃ¡rio) â€” com Ollama local, API OpenAI-compatÃ­vel, ou template.
3. **Imagens**: recorte de fundo (rembg local) e recomposiÃ§Ã£o em 9 presets
   (estÃºdio branco, dark premium, vibrante, minimal, quente/vintage, neon, natural, original, cenÃ¡rio por IA),
   em 1:1, 16:9 e 9:16, versÃ£o limpa e versÃ£o com tÃ­tulo/preÃ§o.
4. **VÃ­deo**: intro â†’ benefÃ­cios â†’ oferta/CTA, com zoom/pan, transiÃ§Ãµes, grade de cor por estilo
   (dinÃ¢mico, elegante, tech, natural, promoÃ§Ã£o, cinemÃ¡tico), mÃºsica CC0 do Freesound (via inemavox)
   ou trilha sintÃ©tica, narraÃ§Ã£o TTS opcional. Sai 16:9 + 9:16 + um catÃ¡logo com todos os produtos.
5. **Fila** persistente (`data/queue.json`): vÃ¡rias tarefas em sÃ©rie, por formulÃ¡rio, lote de linhas ou CLI.

## Rodar

```bash
npm install
python3 -m venv .venv && .venv/bin/pip install rembg onnxruntime pillow numpy   # recorte de fundo (opcional, mas recomendado)
npm start          # http://localhost:3080
```

CLI:

```bash
node cli.js ~/produtos --preset dark-premium --estilo tech --marca "MINHA LOJA"
node cli.js https://loja.com.br --max 5 --formato 9:16
node cli.js --lote lista.txt      # "fonte | preset | estilo | formato | max=N | marca=X | sem-video"
```

## Formato da pasta

```
produtos/
  fone-x1/              â† um produto (vÃ¡rias fotos)
    1.jpg 2.jpg
    produto.json        â† {"name":"Fone X1","price":"R$ 199","desc":"...","category":"tecnologia"}
  caneca-azul/
    foto.png
    produto.md          â† 1Âª linha = nome, resto = descriÃ§Ã£o
  camiseta.jpg          â† fotos soltas: um produto cada
```

## ConfiguraÃ§Ã£o (opcional)

Tela **ConfiguraÃ§Ãµes** na UI ou `.env` na raiz (veja `.env.example`). Defaults: Ollama (`qwen3.8:27b`)
para copy, inemaimg (`flux2-klein`) para cenÃ¡rio por IA, inemavox para mÃºsica/TTS. Qualquer um pode
ser desligado (`none`) ou trocado por uma API OpenAI-compatÃ­vel / Agnes â€” nada Ã© obrigatÃ³rio.

## Motor de vÃ­deo por IA (opcional, plugÃ¡vel)

`VIDEO_ENGINE=ffmpeg` (default) faz zoom/pan local. `VIDEO_ENGINE=agnes` gera clipes de cÃ¢mera reais
a partir das imagens do produto pela API Agnes (`agnes-video-v2.0`, US$ 0, 720p, ~1 min por cena,
limite de 5 req/min) â€” as primeiras `AI_SCENES` cenas de cada produto vÃ£o pela IA e o resto fica no
ffmpeg; se a API falhar, a cena cai no ffmpeg sozinha. Outros motores (kie.ai etc.) entram em
`lib/videoai.js` implementando `generateClip()`.

## Deploy numa VPS (sem GPU)

Perfil testado: Ubuntu, 2 vCPU, 4 GB. Copy, cenÃ¡rio e vÃ­deo pela Agnes; narraÃ§Ã£o por `edge-tts`;
recorte por rembg em CPU; render libx264. Senha HTTP Basic na frente (`APP_PASSWORD`).

```bash
ssh root@VPS
git clone https://github.com/inematds/videoprodutos /root/projetos/videoprodutos
cd /root/projetos/videoprodutos && bash deploy/deploy.sh   # deps, venv, fontes, systemd
nano .env                                                  # AGNES_API_KEY e APP_PASSWORD (modelo: deploy/env.vps.example)
systemctl restart videoprodutos                            # â†’ http://IP:3080
```

Atualizar depois: `cd /root/projetos/videoprodutos && bash deploy/deploy.sh`.

## SaÃ­da

`~/projetos/output/videoprodutos/<id>/` com `projeto.json`, `originais/`, `imagens/<preset>/`, `videos/`.

## Stack

Node 20+, Express, sharp, cheerio, ffmpeg (h264_nvenc ou libx264), Python + rembg, edge-tts. Sem serviÃ§os pagos (Agnes Ã© US$ 0).

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>