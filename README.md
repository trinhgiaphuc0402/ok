flowchart LR
  classDef client fill:#f7fbff,stroke:#60a5fa,stroke-width:1px;
  classDef backend fill:#0f172a,stroke:#111827,stroke-width:1px,color:#fff;
  classDef service fill:#ffffff,stroke:#e6eef8,stroke-width:1px;
  classDef ext fill:#fff7ed,stroke:#f59e0b,stroke-width:1px;

  A[User / Browser<br/>React SPA]:::client -->|HTTPS| APIGW[API Gateway / Keycloak]
  APIGW -->|Auth + API| Django[Django REST API]:::backend

  subgraph BACKEND ["Backend & Infra"]
    direction TB
    Django --> MySQL[(MySQL)]
    Django --> Cloudinary[(Cloudinary)]
    Django --> RAG[RAG Chatbot Service]
    RAG -->|calls| OpenAI[(OpenAI / LLM)]
    RAG --> S3[(S3 / Object Storage)]
  end

  style BACKEND stroke-dasharray: 5 5
  APIGW --> Django

  class A client;
  class Django backend;
  class MySQL,Cloudinary,S3 ext;
  class RAG service;
  class OpenAI ext;
