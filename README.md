# 🍽️ Food RAG System

A Next.js application that implements Retrieval-Augmented Generation (RAG) for food-related queries. Features local development with Ollama and multiple deployment options for different environments.

## 🚀 Quick Start

### Option 1: Automated Setup (Windows)
```powershell
# Run the setup script in PowerShell
.\setup.ps1
npm run dev
```

### Option 2: Manual Setup
```bash
# Install dependencies
npm install

# Copy environment configuration
cp .env.simple.example .env.local

# Download Ollama models (takes several minutes)
ollama pull mxbai-embed-large
ollama pull llama3.2

# Start development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) and start asking questions about food!

📖 **For detailed setup instructions, see [LOCAL_SETUP.md](LOCAL_SETUP.md)**

## 🎯 How It Works

1. **Load Data**: Click "Embed New Items" to process the food database
2. **Ask Questions**: Type questions like "What fruits are yellow and sweet?"
3. **See Results**: 
   - Your question appears immediately
   - RAG search results show first (relevant food documents)
   - AI response generates using the retrieved context

## 🏗️ Architecture Options

### Upstash Vector Database (Recommended)
```bash
# .env.local - Production Ready
VECTOR_DB_TYPE=upstash
UPSTASH_VECTOR_URL=your_upstash_url
UPSTASH_VECTOR_TOKEN=your_upstash_token
LLM_PROVIDER=groq
```
✅ Built-in embeddings via mixedbread-ai/mxbai-embed-large-v1
✅ Scalable and persistent storage
✅ Ready for Vercel deployment

### Simple Vector Database (Development)
```bash
# .env.local - Quick Local Development
VECTOR_DB_TYPE=simple
LLM_PROVIDER=ollama
```
✅ Works immediately, no setup required
✅ In-memory storage for testing

### ChromaDB (Legacy)
```bash
# .env.local - Advanced Local Setup
VECTOR_DB_TYPE=chroma
LLM_PROVIDER=ollama
```
⚠️ Requires additional setup

## 🏗️ Architecture

### File Structure
```
├── app/
│   ├── actions/
│   │   ├── data-actions.ts      # Food data loading
│   │   ├── embedding-actions.ts # Embedding operations
│   │   └── query-actions.ts     # RAG queries
│   ├── layout.tsx
│   └── page.tsx                 # Main UI
├── components/
│   ├── EmbeddingSection.tsx     # Embedding status & controls
│   ├── QuestionSection.tsx      # Question input form
│   └── ResponseSection.tsx      # Results display
├── lib/
│   ├── providers/
│   │   ├── database.ts          # Vector DB abstraction
│   │   ├── embeddings.ts        # Embedding services
│   │   └── llm.ts              # LLM services
│   ├── config.ts               # Environment configuration
│   └── types.ts                # TypeScript interfaces
├── foods.json                  # Food data
└── .env.local                  # Environment variables
```

### Provider Architecture
- **Database Layer**: ChromaDB ↔ Upstash Vector
- **Embedding Layer**: Ollama ↔ Clarifai
- **LLM Layer**: Ollama ↔ Groq
- **Factory Pattern**: Environment-based provider switching

## 🚀 Deployment

### Phase 2 - Vercel Cloud Deployment

1. **Set up cloud services:**
   - Create Upstash Vector database
   - Get Clarifai API key
   - Get Groq API key

2. **Update environment variables:**
   ```bash
   VECTOR_DB_TYPE=upstash
   EMBEDDING_PROVIDER=clarifai
   LLM_PROVIDER=groq
   UPSTASH_VECTOR_URL=your_upstash_url
   UPSTASH_VECTOR_TOKEN=your_upstash_token
   CLARIFAI_PAT=your_clarifai_key
   CLARIFAI_MODEL_URL=https://clarifai.com/mixedbread-ai/embed/models/mxbai-embed-large-v1
   GROQ_API_KEY=your_groq_key
   GROQ_MODEL=llama-3.2-3b-preview
   ```

3. **Deploy to Vercel:**
   ```bash
   npm run build
   vercel --prod
   ```

## 🔧 Configuration

### Provider Configuration
Change these environment variables to configure your setup:

#### Vector Database (Default: Upstash)
- `VECTOR_DB_TYPE`: `upstash` (recommended) | `simple` (development) | `chroma` (legacy)
- `UPSTASH_VECTOR_URL`: Your Upstash Vector instance URL
- `UPSTASH_VECTOR_TOKEN`: Your Upstash authentication token

#### LLM Provider (Required)
- `LLM_PROVIDER`: `groq` (recommended) | `ollama` (local development)
- `GROQ_API_KEY`: Your Groq API key
- `GROQ_MODEL`: Default is llama-3.2-3b-preview

Note: Embedding is handled automatically by Upstash using mixedbread-ai/mxbai-embed-large-v1

## 📊 Data Format

Food items in `foods.json`:
```json
{
  "id": "1",
  "text": "A banana is a yellow fruit that is soft and sweet.",
  "region": "Tropical",
  "type": "Fruit"
}
```

## 🐛 Troubleshooting

### Common Issues

1. **Ollama Connection Failed**
   - Ensure Ollama is running: `ollama serve`
   - Check models are pulled: `ollama list`

2. **ChromaDB Permission Errors**
   - Check `CHROMA_PATH` directory permissions
   - Try different path if needed

3. **Rate Limiting (Cloud)**
   - Use free tier responsibly
   - Implement retry logic if needed

4. **Build Errors**
   - Check TypeScript errors: `npm run build`
   - Verify all dependencies: `npm install`

## 📝 License

MIT License - see LICENSE file for details.

## 🤝 Contributing

1. Fork the repository
2. Create feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open Pull Request
