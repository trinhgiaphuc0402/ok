```mermaid
graph TB

%% User Input
U[👤 User<br/>Query:<br/>Tôi bị đau đầu,<br/>có thuốc gì không?] --> Q

%% RAG Process Box
subgraph RAG ["🤖 RAG Chatbot Service"]
    direction TB

    %% Indexing Section
    subgraph IDX ["📚 Indexing"]
        direction TB
        DOC1[📄 Documents<br/>Multidatabase Medicine Data]
        DOC2[📄 Documents<br/>MySQL Database Medicine Data]
        EMB[🧠 Embeddings<br/>Vector Cache (384-dimensional)]
        MODEL[🔤 SentenceTransformer<br/>paraphrase-multilingual-MiniLM-L12-v2]
        DOC1 --> EMB
        DOC2 --> EMB
        MODEL --> EMB
    end

    %% Retrieval Section
    RET[🔍 Retrieval<br/>Hybrid Search:<br/>Exact Match + Semantic Search<br/>Cosine Similarity ≥ 0.3]

    %% Relevant Documents
    REL[📋 Relevant Documents<br/>Top 5 thuốc liên quan:<br/>Aspirin - 0.85<br/>Paracetamol - 0.72<br/>Ibuprofen - 0.88]

    EMB --> RET
    RET --> REL
end

%% Combine context + prompts
REL --> COMB[🔄 Combine Context & Prompts<br/>Format thông tin thuốc<br/>+ System prompt<br/>Dược sĩ chuyên nghiệp]

%% LLM
COMB --> LLM[🧠 LLM<br/>OpenAI GPT-4<br/>Temperature: 0.7<br/>Max tokens: 300]

%% Output Answer
LLM --> ANS[📤 Answer<br/>- Giới thiệu & công dụng: Aspirin là thuốc giảm đau, hạ sốt…<br/>- Cách sử dụng: Uống 1 viên mỗi 8 tiếng<br/>- Giá cả: 25,000 VND<br/>- Lưu ý: Không dùng cho trẻ dưới 12 tuổi]

%% Response with RAG
subgraph RESP ["✅ with RAG"]
    FINAL[📊 Response JSON<br/>AI response + Medicine list<br/>Similarity scores + Metadata]
end

ANS --> FINAL
FINAL --> OUT[📱 Output<br/>React Frontend<br/>Hiển thị tư vấn + danh sách thuốc]
