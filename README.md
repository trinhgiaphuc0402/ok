graph TB
%% User Input
U[👤 User<br/>Câu hỏi về thuốc] --> Q{📝 Query<br/>Tôi bị đau đầu,<br/>có thuốc gì không?}

%% RAG Process Box
subgraph RAG ["🤖 RAG Chatbot Service"]
    direction TB
    
    %% Indexing Section
    subgraph IDX ["📚 Indexing"]
        direction TB
        DOC[📄 Documents<br/>MySQL Database<br/>Medicine Data]
        CHUNK[🔤 Chunk/Vectors<br/>SentenceTransformer<br/>paraphrase-multilingual-MiniLM-L12-v2]
        EMB[🧠 Embeddings<br/>Vector Cache<br/>384-dimensional]
        DOC --> CHUNK --> EMB
    end
    
    %% Retrieval Section
    RET[🔍 Retrieval<br/>Hybrid Search:<br/>Exact Match + Semantic Search<br/>Cosine Similarity ≥ 0.3]
    
    %% Relevant Documents
    REL[📋 Relevant Documents<br/>Top 5 thuốc liên quan:<br/>Aspirin (0.85)<br/>Paracetamol (0.72)<br/>Ibuprofen (0.68)]
    
    EMB --> RET
    RET --> REL
end

%% LLM Generation
Q --> RAG
REL --> COMB[🔄 Combine Context<br/>and Prompts<br/>Format thông tin thuốc<br/>+ System prompt<br/>Dược sĩ chuyên nghiệp]

COMB --> LLM[🧠 LLM<br/>OpenAI GPT-4<br/>Temperature: 0.7<br/>Max tokens: 500]

%% Output
LLM --> ANS[📤 Answer<br/>Giới thiệu và công dụng: Aspirin là thuốc giảm đau...<br/>Cách sử dụng: Uống 1 viên mỗi 8 tiếng...<br/>Giá cả: 25,000 VND<br/>Lưu ý: Không dùng cho trẻ dưới 12 tuổi]

%% Response with RAG
subgraph RESP ["✅ with RAG"]
    FINAL[📊 Response JSON<br/>AI response + Medicine list<br/>Similarity scores + Metadata]
end

ANS --> FINAL
FINAL --> OUT[📱 Output<br/>React Frontend<br/>Hiển thị tư vấn + danh sách thuốc]

%% Styling
classDef userStyle fill:#e1f5fe,stroke:#01579b,stroke-width:2px
classDef ragStyle fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
classDef llmStyle fill:#fff3e0,stroke:#e65100,stroke-width:2px
classDef outputStyle fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px

class U,Q userStyle
class RAG,RET,REL,COMB ragStyle
class LLM llmStyle
class ANS,FINAL,OUT outputStyle
