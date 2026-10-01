# 🛍️ Agentic E-Commerce Assistant

An AI-powered e-commerce chatbot combining **Retrieval-Augmented Generation (RAG)** with **Agentic AI tool calling**. The system answers product questions from a PDF catalog, checks live inventory, and executes purchase operations through Supabase.

## ✨ Features

- 🔎 Product Q&A using semantic retrieval
- 🧠 RAG pipeline with Qdrant vector search
- 🤖 Agentic AI with tool calling
- 📦 Real-time stock checking with Supabase
- 🛒 Purchase operation with inventory updates
- 💬 Streamlit conversational interface
- ⚡ Groq LLM using `openai/gpt-oss-120b`
- 🔐 `.env`-based secret management

## 🏗️ Architecture

```text
                         User
                          │
                          ▼
                  ┌─────────────────┐
                  │  Streamlit UI   │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │ Agent / Router  │
                  └───────┬─┬───────┘
                          │ │
             Product Q&A  │ │  Stock / Purchase
                          │ │
                          ▼ ▼
                    ┌───────────┐
                    │  Tools    │
                    └─────┬─────┘
                          │
                    ┌─────┴─────┐
                    │           │
                    ▼           ▼
               check_stock  buy_product
                    │           │
                    └─────┬─────┘
                          ▼
                      Supabase

Product Questions
       │
       ▼
   Retriever
       │
       ▼
     Qdrant
       │
       ▼
 Retrieved Context
       │
       ▼
     Groq LLM
       │
       ▼
    Response
```

## 🧠 RAG Pipeline

```text
Product PDF
    ↓
Document Loader
    ↓
Text Splitting
    ↓
Hugging Face Embeddings
    ↓
Qdrant Vector Database
    ↓
Semantic Retrieval
    ↓
Retrieved Context + User Query
    ↓
Groq LLM
    ↓
Final Answer
```

## 🤖 Agentic Workflow

For requests requiring live information or actions, the agent uses database tools.

### Stock Check

```text
User → Agent → check_stock(product_id) → Supabase → Current Stock → Response
```

### Purchase

```text
User → Agent → check_stock() → buy_product() → Supabase
                                      ↓
                              Inventory Updated
```

## 🛠️ Tech Stack

| Category | Technology |
|---|---|
| Language | Python |
| LLM | Groq — `openai/gpt-oss-120b` |
| Framework | LangChain |
| GenAI | RAG, Agentic AI, Tool Calling |
| Embeddings | Hugging Face Sentence Transformers |
| Vector Database | Qdrant |
| Database | Supabase |
| Frontend | Streamlit |
| Infrastructure | Docker |
| Version Control | Git / GitHub |

## 📁 Project Structure

```text
Agentic-E-Commerce-Assistant/
│
├── Backend/
│   ├── Agent/
│   │   ├── db/
│   │   ├── tools/
│   │   ├── agent.py
│   │   └── main.py
│   │
│   └── Rag/
│       ├── document_loader_text_splitter_01.py
│       ├── vector_store_02.py
│       ├── retriver_03.py
│       ├── rag_chain_04.py
│       ├── main_05.py
│       └── ecommerce_products_rag.pdf
│
├── Frontend/
│   └── app.py
│
├── requirements.txt
├── .gitignore
└── README.md
```

## ⚙️ Local Setup

### 1. Clone

```bash
git clone https://github.com/rahulj2122005/Agentic-E-Commerce-Assistant.git
cd Agentic-E-Commerce-Assistant
```

### 2. Create virtual environment

```powershell
py -3.13 -m venv venv
.env\Scripts\Activate.ps1
```

### 3. Install dependencies

```powershell
pip install -r requirements.txt
```

## 🔐 Environment Variables

### `Backend/Rag/.env`

```env
GROQ_API_KEY=your_groq_api_key
```

### `Backend/Agent/db/.env`

```env
SUPABASE_URL=your_supabase_project_url
SUPABASE_KEY=your_supabase_key
```

**Never commit API keys or `.env` files to GitHub.**

## 🗄️ Supabase Setup

Create the products table:

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    product_id INT UNIQUE NOT NULL,
    name TEXT NOT NULL,
    stock INT NOT NULL DEFAULT 2
);
```

Seed the demo products:

```powershell
python Backend/Agent/db/seed.py
```

## 🔎 Qdrant Setup

Start Qdrant with Docker:

```powershell
docker run -p 6333:6333 -v ${PWD}/qdrant_storage:/qdrant/storage qdrant/qdrant
```

Qdrant runs locally on port `6333`.

## 📚 Test RAG

With Qdrant running, open another terminal:

```powershell
.env\Scripts\Activate.ps1
python Backend/Rag/main_05.py
```

The first run loads the PDF, creates chunks, generates embeddings, and stores vectors in Qdrant.

## 🖥️ Run Streamlit

```powershell
streamlit run Frontend/app.py
```

Then open the local URL shown by Streamlit, normally:

```text
http://localhost:8501
```

## 💬 Example Queries

```text
What is Product 5?
```

```text
What is the stock of Product 5?
```

```text
I want to buy Product 5
```

```text
Tell me about Product 5
```

## 🧪 Tested Workflow

- ✅ PDF loading
- ✅ Text chunking
- ✅ Hugging Face embeddings
- ✅ Qdrant vector insertion
- ✅ Semantic retrieval
- ✅ Groq LLM generation
- ✅ Product information queries
- ✅ Agent tool calling
- ✅ Supabase stock retrieval
- ✅ Purchase execution
- ✅ Inventory update
- ✅ Streamlit chatbot

## 🎯 Key Learning Outcomes

- Retrieval-Augmented Generation (RAG)
- Embeddings and semantic search
- Vector databases
- LLM integration
- Agentic AI
- Tool/function calling
- Database-backed AI actions
- Conversational AI
- Streamlit development
- Docker
- Environment and secret management

## 🚀 Future Improvements

- Persistent multi-turn product context
- Product recommendations
- User authentication
- Order history
- Payment integration
- Product filtering and ranking
- Production deployment
- Stronger database security and RLS policies
- Retrieval and response evaluation

## 📌 Attribution

This project was developed using the open-source [Ecommerce-Agent](https://github.com/Pukar77/Ecommerce-Agent) repository as a reference/base.

The project was configured, tested, and extended in my development environment with the RAG, Groq, Qdrant, Supabase, Agent tool-calling, and Streamlit workflow documented above.

## 👤 Author

**Rahul Jadhav**

GitHub: https://github.com/rahulj2122005

Repository: https://github.com/rahulj2122005/Agentic-E-Commerce-Assistant
