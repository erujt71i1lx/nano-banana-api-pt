# Nano Banana API (`gemini-2.5-flash-image-preview`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.0125 per image** — um único endpoint compatível com OpenAI em `https://api.apimart.ai/v1`, sem plano mensal. *(observado em 2026-10-12)*

**[Obter uma chave de API](https://go.apimart.ai/k-3c5874)** · **[Preços ao vivo](https://go.apimart.ai/k-59ce87)** · **[Página do modelo](https://go.apimart.ai/k-fc52ec)** · [⚡ Início rápido em 60 segundos](#quickstart)

**Por que os times usam Nano Banana (`gemini-2.5-flash-image-preview`) via APIMart**

- **Uma chave, o catálogo inteiro.** A mesma base URL `https://api.apimart.ai/v1` e o mesmo cabeçalho `Authorization` alcançam Nano Banana (`gemini-2.5-flash-image-preview`) e mais de 300 modelos de imagem, vídeo e linguagem — troque o campo `model`, não o seu cliente.
- **Mínimo de $1, pague conforme o uso.** Sem assinatura e sem plano pré-pago para dimensionar antes: recarregue a partir de $1 e gaste em chamadas.
- **A cobrança volta na resposta.** Toda tarefa concluída informa o valor cobrado (`cost` / `credits_cost`); uma tarefa marcada como `failed` é reembolsada.
- **Assíncrono por design.** Envie, pegue o `task_id`, consulte `GET /v1/tasks/{id}` — batching e retries são trabalho comum de fila, não uma integração sob medida.

<!-- /conv-kit:v1 -->



Nano Banana is Google's Gemini 2.5 Flash Image route on APIMart: fast text-to-image and reference-image editing through one OpenAI-compatible endpoint.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `gemini-2.5-flash-image-preview` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-fc52ec) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.015625 | 0.0125 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-59ce87) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-fc52ec) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"gemini-2.5-flash-image-preview","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "gemini-2.5-flash-image-preview", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**Qual é o id do modelo Nano Banana?**

`gemini-2.5-flash-image-preview`. Veja a tabela de rotas acima.

**Como é a cobrança?**

**from $0.0125 per image**. A tarefa concluída informa o valor exato no campo `cost`; uma tarefa com falha é reembolsada.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-3c5874) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [nano-banana-prompts-pt](https://github.com/erujt71i1lx/nano-banana-prompts-pt)
- **Same model, other languages** — 9 more language builds: 中文 [nano-banana-api-cn](https://github.com/mpflv/nano-banana-api-cn) · Deutsch [nano-banana-api-de](https://github.com/uh6cob8op/nano-banana-api-de) · Español [nano-banana-api-es](https://github.com/gbm5mfl1/nano-banana-api-es) · Français [nano-banana-api-fr](https://github.com/f84q06w110/nano-banana-api-fr) · Relay/gateway [nano-banana-api-gateway](https://github.com/gix2kust/nano-banana-api-gateway) · Bahasa [nano-banana-api-id](https://github.com/vsa185x9ecrd/nano-banana-api-id) · 日本語 [nano-banana-api-ja](https://github.com/e4ylndl3g-v/nano-banana-api-ja) · 한국어 [nano-banana-api-ko](https://github.com/l-2jb09sq/nano-banana-api-ko) · Русский [nano-banana-api-ru](https://github.com/ggpbi40ss/nano-banana-api-ru)
- **More models in this language** — [midjourney-api-pt](https://github.com/l-ca0/midjourney-api-pt) · [gemini-2-5-flash-image-preview-api-pt](https://github.com/ycx8l0/gemini-2-5-flash-image-preview-api-pt) · [gemini-3-1-flash-image-preview-api-pt](https://github.com/hutjwux0nt9h/gemini-3-1-flash-image-preview-api-pt) · [nano-banana-2-api-pt](https://github.com/ydkd8kdkl8n/nano-banana-2-api-pt) · [gemini-3-pro-image-preview-api-pt](https://github.com/qss2d-66qb2n7/gemini-3-pro-image-preview-api-pt) · [nano-banana-pro-api-pt](https://github.com/f0xtn9oh8g7ia6/nano-banana-pro-api-pt)
- **Topic hubs** — every build of this model, all languages: [nano-banana](https://github.com/topics/nano-banana) · [nano-banana-api](https://github.com/topics/nano-banana-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-fc52ec> | `apimart.ai/pt/model/nano-banana-api` |
| Live pricing | <https://go.apimart.ai/k-59ce87> | `apimart.ai/pt/pricing` |
| Get an API key | <https://go.apimart.ai/k-3c5874> | `apimart.ai/pt/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Nano Banana is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
