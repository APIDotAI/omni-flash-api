# Omni Flash API with APIDot

Build with the Omni Flash API using APIDot: cURL, Node.js, request shapes, polling, webhooks, and production notes.

[Try on APIDot](https://apidot.ai/models/omni-flash) | [Get API Key](https://apidot.ai/dashboard/api-key) | [API Docs](https://apidot.ai/docs/omni-flash) | [Pricing](https://apidot.ai/pricing) | [Main Examples](https://github.com/APIDotAI/apidot-examples)

## What this example shows

- How to submit Omni Flash multimodal video jobs through APIDot.
- How to choose text-only, image-guided, or one-video-guided request shapes.
- How to avoid invalid media combinations, including two image inputs or duration with video input.
- How to store `data.task_id`, poll status, and use webhook callbacks.

## When to use it

Use this repo when a video workflow may start from a prompt, one image, three images, or one source video. It is useful for multimodal product previews, short-video revisions, visual direction tests, and backend queues that need one APIDot async workflow across several input shapes.

## Requirements

- An APIDot account.
- An APIDot API key stored server-side.
- Node.js 18 or newer for the Node.js example.
- `curl` for the cURL example.
- Public image or video URLs when using guided workflows.

## Environment variables

Use placeholders only. Do not commit real credentials.

```env
APIDOT_API_KEY=YOUR_API_KEY_HERE
APIDOT_BASE_URL=https://api.apidot.ai
# Optional: set only when you have a real public webhook receiver.
# APIDOT_CALLBACK_URL=https://example.com/api/apidot/webhook
```

## How to run

```bash
cp .env.example .env
# Edit .env and set APIDOT_API_KEY
cd node
npm start
```

Use [curl/generate.md](curl/generate.md) when you want a copy-paste cURL request instead of the Node.js polling example.

## Minimal API request

Submit to APIDot's unified async generation endpoint:

```json
{
  "model": "omni-flash",
  "callback_url": "https://example.com/api/apidot/webhook",
  "input": {
    "prompt": "A cinematic product reveal of a translucent speaker on wet black stone, slow push-in, realistic reflections, soft morning light.",
    "resolution": "720p",
    "duration": 6,
    "aspect_ratio": "16:9"
  }
}
```

## Request variants

### Text-only video

```json
{
  "model": "omni-flash",
  "input": {
    "prompt": "A clean product reveal in a bright studio with a slow push-in and realistic reflections.",
    "resolution": "720p",
    "duration": 6,
    "aspect_ratio": "16:9"
  }
}
```

### One-image-guided video

```json
{
  "model": "omni-flash",
  "input": {
    "prompt": "Use the product image as the visual anchor and create a short premium motion clip.",
    "image_urls": [
      "https://example.com/product-reference.webp"
    ],
    "resolution": "1080p",
    "duration": 6,
    "aspect_ratio": "16:9"
  }
}
```

### Three-image visual direction

```json
{
  "model": "omni-flash",
  "input": {
    "prompt": "Create a short launch clip using these three images as product, scene, and style references.",
    "image_urls": [
      "https://example.com/product.webp",
      "https://example.com/scene.webp",
      "https://example.com/style.webp"
    ],
    "resolution": "1080p",
    "duration": 8,
    "aspect_ratio": "9:16"
  }
}
```

### One-video-guided revision

```json
{
  "model": "omni-flash",
  "input": {
    "prompt": "Revise the source clip with a cleaner product finish and brighter studio lighting.",
    "video_urls": [
      "https://example.com/source-video.mp4"
    ],
    "resolution": "1080p",
    "aspect_ratio": "16:9"
  }
}
```

## Request parameters

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| model | string | yes | Use `omni-flash`. |
| callback_url | string | no | Optional webhook callback URL for terminal task updates. |
| input.prompt | string | yes | Scene, motion, camera, and revision instruction. |
| input.image_urls | string[] | no | Send zero, one, or three public image URLs. |
| input.video_urls | string[] | no | Send at most one public video URL. |
| input.resolution | string | no | Supported values are `720p`, `1080p`, and `4k`. |
| input.duration | integer | no | Supported values are `4`, `6`, `8`, and `10` for jobs without `input.video_urls`. |
| input.aspect_ratio | string | no | Supported values are `16:9` and `9:16`. |

## Expected response

Submit response:

```json
{
  "code": 200,
  "data": {
    "task_id": "task-unified-example",
    "status": "not_started",
    "created_time": "2026-04-19T21:19:42"
  }
}
```

Shortened finished response:

```json
{
  "code": 200,
  "data": {
    "task_id": "task-unified-example",
    "status": "finished",
    "output": {
      "files": [
        {
          "file_url": "https://example.com/generated-video.mp4",
          "file_type": "video"
        }
      ]
    }
  }
}
```

## Production notes

- Keep APIDot API keys in server-side environment variables.
- Persist `task_id`, request shape, media URLs, selected resolution, user ID, and status together.
- Validate image count before submission; two images are not accepted by the current request contract.
- Omit `input.duration` whenever `input.video_urls` is present.
- Use polling for local tests and webhook callbacks for production queues.
- Avoid logging API keys, private prompts, private media URLs, or production callback URLs.

## Common mistakes

- Sending two image URLs.
- Sending more than one video URL.
- Sending `duration` with `video_urls`.
- Assuming unsupported aspect ratios are accepted.
- Losing `data.task_id` before polling or webhook reconciliation.
- Committing a real `.env` file or API key.

## Related links

- Website: https://apidot.ai
- Docs: https://apidot.ai/docs
- GitHub: https://github.com/APIDotAI
- Omni Flash model page: https://apidot.ai/models/omni-flash
- Omni Flash API docs: https://apidot.ai/docs/omni-flash
- APIDot quickstart: https://apidot.ai/docs/quickstart
- APIDot webhooks: https://apidot.ai/docs/webhooks
- Main APIDot examples repo: https://github.com/APIDotAI/apidot-examples

## Promotion

- Topic: Omni Flash multimodal video request-shape selection and async APIDot workflow.
- Audience: Backend developers adding prompt, image-guided, or source-video-guided generation to product workflows.
- Tweet angle: Choose the Omni Flash input shape first: prompt only, one image, three images, or one video, then wire polling or webhooks.
- Landing page: https://apidot.ai/models/omni-flash
- Source status: verified

## Quality review

- Quality score: 38/40
- Promotion candidate: yes
- Blocking issues: None for request-shape and async workflow promotion. Verify the model page and docs again before making cost, ranking, or availability claims.
