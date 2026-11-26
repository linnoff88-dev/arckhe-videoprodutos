ï»¿# videoprodutos

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

Pasta ou link de loja ê¢â â produtos descobertos ê¢â â imagens em presets de estilo ê¢â â vêÂ­deo promocional
automêÂ¡tico (16:9 e 9:16) ê¢â â projeto salvo, tudo por uma pêÂ¡gina web com **fila de produêÂ§êÂ£o**.
Roda **100% local e sem custo** (ffmpeg + sharp + rembg). LLM e imagem por IA sêÂ£o **opcionais**.

ê°Å¸ââ **Guia de uso (landing):** https://inematds.github.io/videoprodutos/guia/

## O que faz

1. **Busca produtos** numa pasta (subpasta = produto, ou fotos soltas) ou numa URL
   (Shopify `/products.json`, WooCommerce Store API, JSON-LD `Product`, OpenGraph, ou imagens grandes da pêÂ¡gina).
2. **Classifica** o produto (tecnologia, moda, alimento, casa, petê¢â¬Â¦) e escreve o copy
   (têÂ­tulo, tagline, 3 benefêÂ­cios, CTA, cenêÂ¡rio) ê¢â¬â com Ollama local, API OpenAI-compatêÂ­vel, ou template.
3. **Imagens**: recorte de fundo (rembg local) e recomposiêÂ§êÂ£o em 9 presets
   (estêÂºdio branco, dark premium, vibrante, minimal, quente/vintage, neon, natural, original, cenêÂ¡rio por IA),
   em 1:1, 16:9 e 9:16, versêÂ£o limpa e versêÂ£o com têÂ­tulo/preêÂ§o.
4. **VêÂ­deo**: intro ê¢â â benefêÂ­cios ê¢â â oferta/CTA, com zoom/pan, transiêÂ§êÂµes, grade de cor por estilo
   (dinêÂ¢mico, elegante, tech, natural, promoêÂ§êÂ£o, cinemêÂ¡tico), mêÂºsica CC0 do Freesound (via inemavox)
   ou trilha sintêÂ©tica, narraêÂ§êÂ£o TTS opcional. Sai 16:9 + 9:16 + um catêÂ¡logo com todos os produtos.
5. **Fila** persistente (`data/queue.json`): vêÂ¡rias tarefas em sêÂ©rie, por formulêÂ¡rio, lote de linhas ou CLI.

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
  fone-x1/              ê¢â Â um produto (vêÂ¡rias fotos)
    1.jpg 2.jpg
    produto.json        ê¢â Â {"name":"Fone X1","price":"R$ 199","desc":"...","category":"tecnologia"}
  caneca-azul/
    foto.png
    produto.md          ê¢â Â 1êÂª linha = nome, resto = descriêÂ§êÂ£o
  camiseta.jpg          ê¢â Â fotos soltas: um produto cada
```

## ConfiguraêÂ§êÂ£o (opcional)

Tela **ConfiguraêÂ§êÂµes** na UI ou `.env` na raiz (veja `.env.example`). Defaults: Ollama (`qwen3.8:27b`)
para copy, inemaimg (`flux2-klein`) para cenêÂ¡rio por IA, inemavox para mêÂºsica/TTS. Qualquer um pode
ser desligado (`none`) ou trocado por uma API OpenAI-compatêÂ­vel / Agnes ê¢â¬â nada êÂ© obrigatêÂ³rio.

## Motor de vêÂ­deo por IA (opcional, plugêÂ¡vel)

`VIDEO_ENGINE=ffmpeg` (default) faz zoom/pan local. `VIDEO_ENGINE=agnes` gera clipes de cêÂ¢mera reais
a partir das imagens do produto pela API Agnes (`agnes-video-v2.0`, US$ 0, 720p, ~1 min por cena,
limite de 5 req/min) ê¢â¬â as primeiras `AI_SCENES` cenas de cada produto vêÂ£o pela IA e o resto fica no
ffmpeg; se a API falhar, a cena cai no ffmpeg sozinha. Outros motores (kie.ai etc.) entram em
`lib/videoai.js` implementando `generateClip()`.

## Deploy numa VPS (sem GPU)

Perfil testado: Ubuntu, 2 vCPU, 4 GB. Copy, cenêÂ¡rio e vêÂ­deo pela Agnes; narraêÂ§êÂ£o por `edge-tts`;
recorte por rembg em CPU; render libx264. Senha HTTP Basic na frente (`APP_PASSWORD`).

```bash
ssh root@VPS
git clone https://github.com/inematds/videoprodutos /root/projetos/videoprodutos
cd /root/projetos/videoprodutos && bash deploy/deploy.sh   # deps, venv, fontes, systemd
nano .env                                                  # AGNES_API_KEY e APP_PASSWORD (modelo: deploy/env.vps.example)
systemctl restart videoprodutos                            # ê¢â â http://IP:3080
```

Atualizar depois: `cd /root/projetos/videoprodutos && bash deploy/deploy.sh`.

## SaêÂ­da

`~/projetos/output/videoprodutos/<id>/` com `projeto.json`, `originais/`, `imagens/<preset>/`, `videos/`.

## Stack

Node 20+, Express, sharp, cheerio, ffmpeg (h264_nvenc ou libx264), Python + rembg, edge-tts. Sem serviêÂ§os pagos (Agnes êÂ© US$ 0).

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>