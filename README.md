**Dify Local RAG Setup Guide**

_A Practical Learning Journal_

**Disclaimer**

This repository does not include any **Large Language Models (LLMs), Embedding Models, datasets, or copyrighted materials**.

**If your dataset contains copyrighted materials (for example, company training manuals, internal documentation, university lecture slides, textbooks, or any other proprietary content), it is strongly recommended that you keep your RAG system local and do not publish or redistribute those materials.**

The purpose of this project is to document the complete setup process, troubleshooting experience, and lessons learned while building a local Retrieval-Augmented Generation (RAG) system using Dify.

Users are expected to download and configure their own models, embedding models, and datasets.

**1\. Introduction**

**What is this project?**

Built a local Retrieval-Augmented Generation (RAG) assistant using Dify and Ollama to support PITA insurance study. Integrated Qwen3 LLM and embedding models, implemented document chunking, vector retrieval, citation-based responses, and knowledge-base driven question answering over training materials. Using Chinese dataset is to test how the performance of the Qwen embedding model.

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

**2\. System Architecture**

Dify + Docker + Ollama + PostgreSQL + LLM + Embedding + Knowledge Base

**3\. Software Requirements**

- Docker Desktop
- Ollama
- Git
- PostgreSQL
- Recommended RAM 5 ~ 6GB

**4\. Installation Guide**

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

**5\. Connecting Ollama**

Open 'cmd' and type \[ollama pull qwen3:8b\] and \[ollama pull qwen3-embedding:8b\]

Type "ollama list" to check the model list


How to configure Model Provider.

- search ollama and install


- Add LLM model and embedding model


- Select model

- Add LLM model


- Add model embedding model


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

create 'chatbot'


name the chatbot


select LLM model


give an instruction and try to ask question


click 'Publish'

**6\. Creating a Knowledge Base**

create knowledge


Import documents

attach the dataset and press 'next'


Chunking options.


Embedding model.


Retrieval settings. Top K shows Top result. If 3, then it will shows Top 3 result.


Preview chunk


**7\. Creating a Chatbot**

Check Knowledge Base

click inside the document to check the paragraph chunk correct or not. If not correct, then need to decide compatibility of the embedding model.

Retrieval Testing


Ask question relevant to document



Connect to Knowledge Base.

go to 'Studio', select 'chatbot'


Insert the instruction and select knowledge


Select the knowledge just now


Testing (ask question)


Click 'Publish' to save all.

**8\. Troubleshooting**

Every problem encountered during the project.

- Installation Error

If encounter the error "**failed to copy: httpReadSeeker…cloudfront.docker.com/…**" during the installation.


Then please change the network Preferred DNS: 1.1.1.1 and Alternate DNS: 1.0.0.1 or download Cloudflare WARP and open it.

- Browser cache issue


If **<http://localhost>** encounter the infinity loading or page couldn't load issue. Please try clean the cache and cookies or use the Incognito browser.

**9\. Daily Startup Procedure**

please open the following application to avoid hit any error in every initial.

- Docker Desktop
- Ollama
- PostgreSQL
- Cloudflare WARP
- cd C:/Users/you/dify/docker
- docker compose up -d
- open the browser, go to [**http://localhost**](http://localhost) and login

**10\. Why PostgreSQL**

Although Dify is an AI application, it still requires a relational database to store application data and configurations.

PostgreSQL is used to persist information that must remain available even after the application is restarted.

Without PostgreSQL, every chatbot, workspace, and configuration would be lost when the containers stop.

PostgreSQL stores information such as:

 User accounts

 Workspaces

 Chatbot configurations

 System prompts

 Knowledge Base metadata

 Model provider settings

 Conversation history (if enabled)

 Application settings

It is important to note that **the uploaded documents (PDF, Word, TXT, etc.) are NOT stored inside PostgreSQL**.

Instead:

- The original files are stored in Dify's storage directory.
- PostgreSQL stores the metadata, including file information, ownership, indexing status, and storage location.

This separation improves performance and allows Dify to efficiently manage both structured data and uploaded documents.

PostgreSQL is the brain for application data, while Weaviate is the brain for semantic search. Ollama is responsible for generating responses.

 **PostgreSQL** → Stores application data and configurations.

 **Weaviate** → Stores vector embeddings for semantic retrieval.

 **Ollama** → Runs the Large Language Model to generate answers.

**11\. Understanding Dify Containers**


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
