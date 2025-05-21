---
published: false
layout: post
tags: [Tech, AI, OCR]
---

# Typhoon OCR with n8n

I used the following tools:

- [n8n](https://n8n.io/)
- [ollama](https://ollama.com/)
- [Typhoon OCR](https://opentyphoon.ai/blog/en/typhoon-ocr-release)


## Download the model from Huggingface

Typhoon OCR is an openweight AI model that's freely available on HuggingFace. Since I'm using my personal Macbook Air (i.e. no GPU), The original model is too large to run locally. So instead, I'm using the quantized [version of the model by mradermacher](https://huggingface.co/mradermacher/typhoon-ocr-7b-i1-GGUF)

Make sure you have [ollama](https://ollama.com/) installed. Then run the following command:

```bash

ollama run hf.co/mradermacher/typhoon-ocr-7b-i1-GGUF:Q4_K_M
pulling manifest
pulling 3c720f401e89: 100% ▕██████████████████▏ 4.7 GB
pulling a242d8dfdc8f: 100% ▕██████████████████▏  487 B
pulling f6460fc7dd9f: 100% ▕██████████████████▏   22 B
pulling 826dbc4f44a8: 100% ▕██████████████████▏  558 B
verifying sha256 digest
writing manifest
success
>>> hello
{"natural_text": "สวัสดีครัช เราเคยรู้จักกันหรือเปล่า"}
```

The model is ~7GB so this can take awhile to download. Note that the model's response to my Hello is in Thai AND with a typo.

Ollama supports running any GGUF quantized models. For more info, check out [the docs](https://huggingface.co/docs/hub/en/ollama)

## Connect to n8n workflow

Now that we have the model running, we can connect it to n8n workflow. If you haven't got n8n setup locally, the easiest way is to run it in a docker container. Follow this [guide](https://docs.n8n.io/hosting/installation/docker)

```bash
docker volume create n8n_data

docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

Now you can visit http://localhost:5678 to access n8n. 

Create a new workflow and add Ollama Model node. For credential, since we're running n8n in a docker container, make sure it's set to host's instance at http://host.docker.internal:11434

![n8n]()

Select Model and add Options Sampling Temperature to 0.0

![n8n-ollama](/assets/images/n8n-ollama.png)

Try chatting with the model. Interestingly enough, at 0.0 temperature, we get the same response as before.

![n8n-chat](/assets/images/n8n-chat.png)

