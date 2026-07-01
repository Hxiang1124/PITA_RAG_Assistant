# Dify Local RAG Setup Guide

_A Practical Learning Journal_

## Disclaimer

This repository does not include any **Large Language Models (LLMs), Embedding Models, datasets, or copyrighted materials**.

**If your dataset contains copyrighted materials (for example, company training manuals, internal documentation, university lecture slides, textbooks, or any other proprietary content), it is strongly recommended that you keep your RAG system local and do not publish or redistribute those materials.**

The purpose of this project is to document the complete setup process, troubleshooting experience, and lessons learned while building a local Retrieval-Augmented Generation (RAG) system using Dify.

Users are expected to download and configure their own models, embedding models, and datasets.

## 1. Introduction

**What is this project?**

Built a local Retrieval-Augmented Generation (RAG) assistant using Dify and Ollama to support PITA insurance study. Integrated Qwen3 LLM and embedding models, implemented document chunking, vector retrieval, citation-based responses, and knowledge-base driven question answering over training materials. Using Chinese dataset is to test how the performance of the Qwen embedding model.

<img width="800" height="337" alt="Desktop2026 07 01-15 48 40 01-ezgif com-video-to-gif-converter" src="https://github.com/user-attachments/assets/01af9b5f-f70e-4f74-a928-dd439c2c5148" />


**Objective**

To understand how a local RAG system works by using Dify.

PDF / txt / Word  
↓  
slicing chunks  
↓  
embedding  
↓  
Vector DB  
↓  
ask question  
↓  
Retrieval testing  
↓  
Select the "knowledge"  
↓  
generate answer

## 2. System Architecture

Dify + Docker + Ollama + PostgreSQL + LLM + Embedding + Knowledge Base

## 3. Software Requirements

- Docker Desktop
- Ollama
- Git
- PostgreSQL
- Recommended RAM 5 ~ 6GB

## 4. Installation Guide

1\. Install ollama (<https://ollama.com/download>)

- Model install (qwen3:8b)
- Model install (qwen3-embedding:8b)

2\. Install "Docker Desktop"

3\. PostgreSQL <https://www.postgresql.org/download/>

4\. Install Dify <https://github.com/langgenius/dify?utm_source=chatgpt.com>

- Open git bash enter \[git clone <https://github.com/langgenius/dify.git\>]
- Open powershell type \[cd C:/Users/you/dify/docker\]
- \[copy .env.example .env\]
- \[docker compose up -d\]
- \[docker ps\] to check all containers
- go to browser \[<http://localhost\>]
- register:
<img width="782" height="320" alt="image" src="https://github.com/user-attachments/assets/61ec0e91-77d6-44af-82ca-172b9d0e6c0d" />


## 5. Connecting Ollama

Open 'cmd' and type \[ollama pull qwen3:8b\] and \[ollama pull qwen3-embedding:8b\]

Type "ollama list" to check the model list

<img width="940" height="234" alt="image" src="https://github.com/user-attachments/assets/1ee4f46c-dc9e-4555-9308-fd629d7c09a2" />

How to configure Model Provider.

1. search ollama and install

<img width="940" height="166" alt="image" src="https://github.com/user-attachments/assets/e56d0a85-aada-47c6-a8e5-ef55d10099fe" />

2. Add LLM model and embedding model

<img width="940" height="139" alt="image" src="https://github.com/user-attachments/assets/9f95059f-1959-4781-824c-18917a625b3d" />

3. Select model

<img width="505" height="541" alt="image" src="https://github.com/user-attachments/assets/72a9ab82-4807-4b6f-b7c9-00e4363eb9ba" />

- Add LLM model

<img width="528" height="567" alt="image" src="https://github.com/user-attachments/assets/a8b63b2e-cd4c-4ff4-8ffc-514d7f8d779a" />

- Add model embedding model

<img width="528" height="567" alt="image" src="https://github.com/user-attachments/assets/2a0ae0fc-5417-4e15-9c0b-7a80a78888c9" />



Why [**http://host.docker.internal:11434**](http://host.docker.internal:11434) ?

host.docker.internal is a special hostname provided by Docker. It allows a Docker container to communicate with applications running on the host machine (Windows or macOS).

For example:

- Ollama is installed and running on your local computer.
- Dify is running inside Docker containers.

Since localhost inside a Docker container refers to the container itself rather than your computer, Dify cannot access Ollama using localhost.

Instead, Dify should use:

<http://host.docker.internal>

to communicate with the Ollama service running on the host machine.

**11434** is the default port used by the Ollama API.

**How to verify connection**.

- create 'chatbot'

<img width="940" height="213" alt="image" src="https://github.com/user-attachments/assets/010fafdd-6962-485f-941f-ced9f9cb069a" />

- name the chatbot

<img width="940" height="432" alt="image" src="https://github.com/user-attachments/assets/9451ac34-b4c0-4877-ab1b-f56053817a6d" />

- select LLM model

<img width="940" height="263" alt="image" src="https://github.com/user-attachments/assets/ff3a916f-017e-4313-a6b4-d956e8cc4503" />

- give an instruction and try to ask question

<img width="940" height="262" alt="image" src="https://github.com/user-attachments/assets/e13b3b75-4716-4f7a-91fa-f7bea91fbb37" />

- click 'Publish'

## 6. Creating a Knowledge Base

create knowledge

<img width="940" height="211" alt="image" src="https://github.com/user-attachments/assets/733a9d8b-a686-4ecc-962c-e576dc095ec1" />

Import documents
attach the dataset and press 'next'

<img width="655" height="368" alt="image" src="https://github.com/user-attachments/assets/463a190a-fc1b-4f8f-8839-6cbaa11f577b" />

Chunking options.

<img width="940" height="364" alt="image" src="https://github.com/user-attachments/assets/0cff1b5b-6cf3-4047-bbe9-1b2e487e6eea" />

Embedding model.

<img width="940" height="339" alt="image" src="https://github.com/user-attachments/assets/026506fc-0dc2-4f7b-aa51-4900acc0a76d" />

Retrieval settings. Top K shows Top result. If 3, then it will shows Top 3 result.

<img width="940" height="312" alt="image" src="https://github.com/user-attachments/assets/f005e1db-f245-4ead-b31e-8d3f84f00637" />

Preview chunk

<img width="940" height="200" alt="image" src="https://github.com/user-attachments/assets/e5db0e4b-7943-4d80-867b-efa4171a740f" />

## 7. Creating a Chatbot

Check Knowledge Base

click inside the document to check the paragraph chunk correct or not. If not correct, then need to decide compatibility of the embedding model.

<img width="940" height="120" alt="image" src="https://github.com/user-attachments/assets/a0dabffb-ab20-42c3-8101-fdcf41a0995d" />

Retrieval Testing

<img width="670" height="352" alt="image" src="https://github.com/user-attachments/assets/c8aa4a46-1691-4273-b6d3-3c7fd3fc2207" />

Ask relevent question

<img width="940" height="334" alt="image" src="https://github.com/user-attachments/assets/f5a1c988-3da6-4b9a-b538-22693d7ac267" />

<img width="940" height="433" alt="image" src="https://github.com/user-attachments/assets/de4cbfe7-36f8-4bca-8597-5940622e65e8" />

Connect to Knowledge Base.
go to 'Studio', select 'chatbot'

<img width="940" height="190" alt="image" src="https://github.com/user-attachments/assets/35440353-d9c6-42cf-9586-baf912f5ae7c" />

Insert the instruction and select knowledge

<img width="940" height="521" alt="image" src="https://github.com/user-attachments/assets/05c4dc56-63ee-4c22-87b1-2f2aca6d3feb" />

Select the knowledge just now

<img width="519" height="275" alt="image" src="https://github.com/user-attachments/assets/be76da51-def1-4fc8-b1f2-b84db4cfd0f3" />

Testing (ask question)

<img width="940" height="270" alt="image" src="https://github.com/user-attachments/assets/c0409787-73ab-4563-bbd4-9fb5c9bcdc3b" />

Click 'Publish' to save all.

## 8. Troubleshooting

Every problem encountered during the project.

- Installation Error

If encounter the error "**failed to copy: httpReadSeeker…cloudfront.docker.com/…**" during the installation.

<img width="940" height="377" alt="image" src="https://github.com/user-attachments/assets/ec399779-5fe5-4e39-980c-7e4aa5b4491d" />

Then please change the network Preferred DNS: 1.1.1.1 and Alternate DNS: 1.0.0.1 or download Cloudflare WARP and open it.

- Browser cache issue

<img width="940" height="178" alt="image" src="https://github.com/user-attachments/assets/de04fd84-297d-4f05-9632-020dfc3d794c" />

If **<http://localhost>** encounter the infinity loading or page couldn't load issue. Please try clean the cache and cookies or use the Incognito browser.

## 9. Daily Startup Procedure

please open the following application to avoid hit any error in every initial.

- Docker Desktop
- Ollama
- PostgreSQL
- Cloudflare WARP
- cd C:/Users/you/dify/docker
- docker compose up -d
- open the browser, go to [**http://localhost**](http://localhost) and login

## 10. Why PostgreSQL

Although Dify is an AI application, it still requires a relational database to store application data and configurations.

PostgreSQL is used to persist information that must remain available even after the application is restarted.

Without PostgreSQL, every chatbot, workspace, and configuration would be lost when the containers stop.

PostgreSQL stores information such as:

- User accounts

- Workspaces

- Chatbot configurations

- System prompts

- Knowledge Base metadata

- Model provider settings

- Conversation history (if enabled)

- Application settings

It is important to note that **the uploaded documents (PDF, Word, TXT, etc.) are NOT stored inside PostgreSQL**.

Instead:

- The original files are stored in Dify's storage directory.
- PostgreSQL stores the metadata, including file information, ownership, indexing status, and storage location.

This separation improves performance and allows Dify to efficiently manage both structured data and uploaded documents.

PostgreSQL is the brain for application data, while Weaviate is the brain for semantic search. Ollama is responsible for generating responses.

- **PostgreSQL** → Stores application data and configurations.

- **Weaviate** → Stores vector embeddings for semantic retrieval.

- **Ollama** → Runs the Large Language Model to generate answers.

## 11. Understanding Dify Containers


| **Container**     | **Purpose**                                                                                                                     |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **nginx**         | Reverse proxy and web server. Receives HTTP requests from the browser and forwards them to the Dify services.                   |
| **web**           | The Dify frontend (Next.js) that provides the user interface.                                                                   |
| **api**           | The core backend service. Handles authentication, chatbot logic, knowledge bases, workflows, model providers, and API requests. |
| **worker**        | Executes background tasks such as document parsing, chunking, embedding generation, and indexing.                               |
| **db**            | PostgreSQL database. Stores users, workspaces, chatbot configurations, prompts, and metadata.                                   |
| **redis**         | Message queue and cache used for background jobs and temporary data.                                                            |
| **weaviate**      | Vector database that stores embeddings and performs semantic similarity search for RAG.                                         |
| **plugin_daemon** | Manages plugins and external tool integrations.                                                                                 |
| **sandbox**       | Executes user code in an isolated environment for security.                                                                     |
| **ssrf_proxy**    | Protects the system by controlling outbound network requests.                                                                   |
