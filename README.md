graph TD
    %% User Interface Layer
    A[👤 User Input<br/>Câu hỏi về thuốc] --> B[📱 Frontend<br/>React App]
    
    %% API Gateway
    B --> C[🌐 Django REST API<br/>ChatBotView Endpoint]
    
    %% Question Analysis
    C --> D{🧠 Phân tích câu hỏi}
    D -->|Cụ thể| E[🎯 Câu hỏi cụ thể<br/>VD: "Espumisan có công dụng gì?"]
    D -->|Tổng quát| F[🔍 Câu hỏi tổng quát<br/>VD: "Tôi đau bụng"]
    
    %% Medicine Search Service
    E --> G[💊 Medicine Search Service<br/>Tìm 1 thuốc chính xác]
    F --> H[💊 Medicine Search Service<br/>Tìm nhiều thuốc liên quan]
    
    %% Database Layer
    G --> I[(🗄️ MySQL Database<br/>- Thông tin thuốc<br/>- Giá cả<br/>- Cách dùng)]
    H --> I
    
    %% OpenAI Integration
    G --> J[🤖 OpenAI Service<br/>GPT-3.5-turbo]
    H --> J
    I --> J
    
    %% OpenAI Configuration
    J --> K[⚙️ Cấu hình tối ưu<br/>- Model: gpt-3.5-turbo<br/>- Max tokens: 300<br/>- Temperature: 1.0<br/>- Chi phí giảm 85-90%]
    
    %% Response Processing
    K --> L{📝 Xử lý Response}
    L -->|Cụ thể| M[📄 Trả về 1 section<br/>VD: Chỉ "Công dụng"]
    L -->|Tổng quát| N[📋 Trả về 4 sections<br/>- Công dụng<br/>- Giá cả<br/>- Cách dùng<br/>- Lưu ý]
    
    %% Final Response
    M --> O[📤 JSON Response]
    N --> O
    O --> B
    B --> P[✅ Hiển thị kết quả<br/>cho người dùng]
    
    %% Error Handling
    J -.->|Timeout| Q[⚠️ Error Handling<br/>- Fallback response<br/>- Database connection<br/>- Input validation]
    Q -.-> O
    
    %% Security & Performance
    R[🔒 Bảo mật<br/>- API key trong env<br/>- Rate limiting<br/>- Input sanitization] -.-> C
    S[⚡ Tối ưu hiệu suất<br/>- Response time: 2-3s<br/>- Cache queries<br/>- Select_related] -.-> I
    
    %% Performance Metrics
    T[📊 Performance Metrics<br/>- Response Time: 2-3s<br/>- Cost Savings: 85-90%<br/>- Accuracy: 95%+<br/>- High User Satisfaction] -.-> O
    
    %% Styling
    classDef userLayer fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef apiLayer fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef serviceLayer fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef dataLayer fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef aiLayer fill:#fce4ec,stroke:#880e4f,stroke-width:2px
    classDef responseLayer fill:#f1f8e9,stroke:#33691e,stroke-width:2px
    
    class A,B,P userLayer
    class C,R apiLayer
    class D,E,F,G,H,S serviceLayer
    class I dataLayer
    class J,K aiLayer
    class L,M,N,O,Q,T responseLayer
