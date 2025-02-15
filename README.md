<div style="text-align:center;">
<img src="https://magicbuddy.chat/img/whisper.jpg">
</div>

# OpenAI Whisper API

This is is a OpenAI Whisper API microservice using Node.js / Bun.sh / Typescript that can run on Docker. With zero dependencies.

It listens to the `/transcribe` route for MP3 files and returns the text transcription.

## Running locally

Install [bun.sh](https://bun.sh/) first, clone this directory and run these commands:

```bash
bun install
bun run dev
```

You can now navigate to http://localhost:3000 or the PORT provided, see the Usage section below.

## Docker

- See: https://hub.docker.com/r/illyism/openai-whisper-api

## Google Cloud Run Deployment

Install [bun.sh](https://bun.sh/) first, clone this directory and run these commands:
Change the project ID to your own.

```bash
docker build --platform linux/amd64 -t gcr.io/magicbuddy-chat/whisper-docker .
docker push gcr.io/magicbuddy-chat/whisper-docker

gcloud run deploy whisper-docker \
  --image gcr.io/magicbuddy-chat/whisper-docker  \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --project magicbuddy-chat
```

You should receive a Service URL, see the Usage section below.

## Usage

You can test normal HTTP by opening the /ping endpoint on the URL.

Connect to the /transcribe and send a POST request with the following body:

```json
{
  "audio": "BASE64_ENCODED_AUDIO"
}
```

### API Key

You need to pass the OpenAI API Key as a HEADER:

```
Authorization: Bearer OPENAI_KEY
```

Or you can launch the docker image or server with `OPENAI_KEY` in the env:
  
```bash
OPENAI_KEY=YOUR_KEY_HERE bun run dev

# or

docker run -p 3000:3000 -e OPENAI_KEY=YOUR_KEY_HERE gcr.io/magicbuddy-chat/whisper-docker

# or set it as env in Cloud Run with the below command or in the Cloud Console UI

gcloud run deploy whisper-docker \
  --image gcr.io/magicbuddy-chat/whisper-docker  \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --project magicbuddy-chat \
  --set-env-vars OPENAI_KEY=YOUR_KEY_HERE
```

# Live example

We are using this Whisper API with [MagicBuddy, a Telegram ChatGPT bot](https://magicbuddy.chat/).

You can use the [OpenAI Whisper Docker](https://magicbuddy.chat/openai-whisper) as a live example here:

- https://magicbuddy.chat/openai-whisper
