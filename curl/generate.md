# Omni Flash cURL Quickstart

## What this example shows

This example submits one Omni Flash text-only video task and then polls the shared APIDot task status endpoint.

## How to run

```bash
export APIDOT_API_KEY="YOUR_API_KEY_HERE"

curl --fail-with-body --request POST \
  --url https://api.apidot.ai/api/generate/submit \
  --header "Authorization: Bearer $APIDOT_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "model": "omni-flash",
    "input": {
      "prompt": "A cinematic product reveal of a translucent speaker on wet black stone, slow push-in, realistic reflections, soft morning light.",
      "resolution": "720p",
      "duration": 6,
      "aspect_ratio": "16:9"
    }
  }'
```

Store the returned `data.task_id`, then poll status:

```bash
curl --fail-with-body --request GET \
  --url https://api.apidot.ai/api/generate/status/task-unified-example \
  --header "Authorization: Bearer $APIDOT_API_KEY"
```

