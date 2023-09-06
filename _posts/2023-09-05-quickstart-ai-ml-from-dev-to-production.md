---
published: false
layout: post
---
## Quickstart AI/ML from Development to Production

I've been curious about the effort required to develop a production-ready AI/ML service in 2023. After extensive research, I'm compiling the list of steps I did to take my side project up to cloud. There are several vendors to choose from for each components starting from cloud infrastructure to model training pipeline. Depending on the nature of the service, you may also need additional components such as moderation, or even additional models, etc.

First thing is setting up version control for your repo. Here I'm using git on github.com (https://github.com/tholapz/quickstart-aiml)

Secondly, dockerfiles. Here we have a few services we need to define: Static Hosting for SPA, REST Service, Proxy, Model Inference Service. To do this, I'm using docker compose? (We can use Kubernete or other docker orchestration solution as well). I'm using Debian-based image for OS. Since we do static site to serve HTML, CSS, and other assets, nginx is perfect for the job. For UI, I'm using React with Typescript. The server is using Python FastAPI. An alternative would be to use gRPC to take care of the client-server logic. 
