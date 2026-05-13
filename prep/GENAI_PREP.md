# GenAI Interview Prep — Deep Topics Guide

> Comprehensive prep across all GenAI areas recommended by Sangeeth (referrer) and your network.
> Use this as your **second-pass deep dive** after the `ibm_genai_prep.md` runbook.
> **Last updated:** May 2026

---

## Table of Contents

1. [Transformer Architecture](#1-transformer-architecture)
2. [LangChain Deep Dive](#2-langchain-deep-dive)
3. [HuggingFace Usage](#3-huggingface-usage)
4. [Model Inferencing](#4-model-inferencing)
5. [Supervised & Unsupervised Learning + Metrics](#5-ml-metrics)
6. [RAG Deep Dive](#6-rag-deep-dive)
7. [Vector Search Deep Dive](#7-vector-search)
8. [AI Agents](#8-ai-agents)
9. [Your RAG Project — Talking Points](#9-rag-project)
10. [Python Interview Questions](#10-python-interview)
11. [Python Coding Questions](#11-python-coding)
12. [Quick Cheat Sheet](#12-cheat-sheet)
13. [Final Revision Strategy](#13-revision)

---

<a id="1-transformer-architecture"></a>
## 1. Transformer Architecture

The transformer is THE foundation of every modern LLM. Expect questions on this. Understand it deeply — not just buzzwords.

### 1.1 What is a Transformer?

A **neural network architecture** introduced in the 2017 paper *"Attention Is All You Need"* (Vaswani et al., Google). It uses **self-attention** to process sequences in parallel — replacing the sequential RNN/LSTM models that came before.

**Why it matters:**
- Parallelizable (unlike RNNs) → trainable on GPUs at huge scale
- Captures long-range dependencies via attention
- Foundation of GPT, Claude, Llama, BERT, T5, every modern LLM

### 1.2 High-level architecture

```
                    INPUT TEXT
                        │
                        ▼
                   TOKENIZER
                        │
                        ▼
                EMBEDDING + POSITIONAL ENC
                        │
                        ▼
            ┌───────────────────────┐
            │   TRANSFORMER BLOCK   │
            │  ┌─────────────────┐  │
            │  │ Multi-Head      │  │
            │  │ Self-Attention  │  │  ← Repeats N times
            │  └────────┬────────┘  │     (N = 12, 24, 48, ...)
            │           │           │
            │  ┌────────▼────────┐  │
            │  │ Add & Norm      │  │
            │  └────────┬────────┘  │
            │           │           │
            │  ┌────────▼────────┐  │
            │  │ Feed Forward    │  │
            │  │ Network         │  │
            │  └────────┬────────┘  │
            │           │           │
            │  ┌────────▼────────┐  │
            │  │ Add & Norm      │  │
            │  └─────────────────┘  │
            └───────────┬───────────┘
                        │
                        ▼
                   OUTPUT HEAD
                        │
                        ▼
              NEXT TOKEN PROBABILITIES
```

### 1.3 Core components — explained

**Tokenizer** — converts text → integer IDs.
- Uses subword algorithms (BPE, WordPiece, SentencePiece)
- Common words = 1 token, rare words split into pieces
- Example: "tokenization" might be `["token", "ization"]` → `[256, 489]`

**Embeddings** — each token ID becomes a dense vector (typically 768-12288 dim).
- Learned during training
- Captures semantic meaning

**Positional Encoding** — added to embeddings since attention is permutation-invariant (doesn't know order).
- Original: sinusoidal functions (sin/cos)
- Modern (Llama, GPT): RoPE (Rotary Position Embedding)

**Self-Attention** — the heart of the transformer.

For each token, compute three vectors: **Query (Q)**, **Key (K)**, **Value (V)** via learned linear projections.

```
Attention(Q, K, V) = softmax(Q·K^T / √d_k) · V
```

What this does (intuitively):
- For each token, compare it against all others (Q·K)
- The dot products become attention weights (after softmax)
- Weighted sum of values gives a context-aware representation

**Multi-Head Attention** — run attention multiple times in parallel with different Q/K/V projections, then concat.
- Each "head" learns different relationships (syntax, semantics, coreference)
- Typical: 12, 16, 32 heads per layer

**Feed-Forward Network (FFN)** — applied to each token independently.
- 2 linear layers with a non-linearity (GeLU, ReLU, SwiGLU)
- Expands dimension (e.g., 768 → 3072 → 768)

**Residual connections + Layer Norm** — `x + Sublayer(LayerNorm(x))`.
- Stabilizes training, allows very deep networks

### 1.4 Encoder vs Decoder vs Encoder-Decoder

| Type | Examples | Purpose |
|---|---|---|
| **Encoder-only** | BERT, RoBERTa | Understanding tasks: classification, NER, embeddings |
| **Decoder-only** | GPT, Llama, Claude, Mistral | Generation tasks: text completion, chat |
| **Encoder-Decoder** | T5, BART | Sequence-to-sequence: translation, summarization |

**Key difference:**
- Encoder uses **bidirectional** attention (sees all tokens)
- Decoder uses **causal/masked** attention (only sees previous tokens) — required for next-token prediction

### 1.5 Interview Q&A

**Q1. Explain self-attention in one sentence.**
> Self-attention computes a weighted average of all token representations in a sequence, where the weights are based on dot-product similarity between query and key projections of those tokens.

**Q2. Why do we divide by √d_k in attention?**
> Without scaling, dot products grow large as dimensionality increases, pushing softmax into saturation regions where gradients vanish. Dividing by √d_k keeps variance stable.

**Q3. What's the time complexity of self-attention?**
> O(n² · d) where n = sequence length, d = embedding dim. The quadratic cost in n is why long-context models are hard. Optimizations: FlashAttention, sparse attention, sliding window attention.

**Q4. What's positional encoding and why is it needed?**
> Attention is permutation-invariant — it doesn't know the order of tokens. Positional encoding adds order information. Original transformer used fixed sinusoidal encodings; modern LLMs use learned or rotary embeddings (RoPE).

**Q5. Why are decoder-only models popular for LLMs?**
> Decoder-only (causal) models can be trained on a single unified objective: next-token prediction. This is simpler, scales better, and the architecture works well for both completion AND chat (with the right fine-tuning).

**Q6. What is a context window?**
> The maximum number of tokens a model can attend to in a single forward pass. GPT-4: 128K. Claude 3.5: 200K. Limited by the O(n²) attention cost.

**Q7. What's the difference between pretraining and fine-tuning?**
> Pretraining: train from scratch on huge corpora with self-supervised objectives (next-token prediction). Fine-tuning: take a pretrained model and continue training on a smaller, task-specific dataset to specialize.

**Q8. What's RLHF?**
> Reinforcement Learning from Human Feedback. After supervised fine-tuning, train a reward model on human preferences over pairs of outputs, then use PPO or similar to optimize the LLM against the reward model. This makes models more helpful and aligned. Anthropic uses Constitutional AI; OpenAI uses RLHF.

**Q9. What's the GELU vs ReLU activation?**
> ReLU: `max(0, x)` — simple but cuts gradients sharply at 0.
> GELU: `x · Φ(x)` (smooth, gaussian-error-based) — used in BERT, GPT, smoother gradients.
> SwiGLU: combination of Swish + GLU, used in Llama-2/3 — empirically best for LLMs.

**Q10. What's the role of FFN in transformers?**
> FFN provides per-token, position-wise non-linear transformation. Attention handles cross-token relationships; FFN handles per-token feature transformation. Together they form the basic block.

**Q11. What are LayerNorm and pre-norm vs post-norm?**
> LayerNorm normalizes activations across the feature dimension per token. Post-norm (original): `LayerNorm(x + Sublayer(x))`. Pre-norm (modern): `x + Sublayer(LayerNorm(x))`. Pre-norm is more stable for deep networks.

**Q12. What is tokenization? BPE specifically?**
> Tokenization splits text into discrete units. BPE (Byte-Pair Encoding) starts with characters, iteratively merges the most-frequent adjacent pair into a new token, until vocabulary size is reached. Common subwords end up as single tokens, rare words split.

**Q13. Why are LLMs called "auto-regressive"?**
> They generate one token at a time, conditioning on all previously generated tokens. Each token = `argmax P(token | previous tokens)` (or sampled).

**Q14. What is KV caching?**
> During generation, you recompute attention for each new token. But the Keys and Values of previous tokens never change. KV cache stores them, dropping the per-step cost from O(n²) to O(n). Used in every production LLM serving.

**Q15. What's the role of softmax?**
> Converts arbitrary real-valued scores into a probability distribution (non-negative, sums to 1). Used in attention weights AND in the final next-token distribution.

---

<a id="2-langchain-deep-dive"></a>
## 2. LangChain Deep Dive

LangChain is the most popular framework for building LLM apps. You used it in your RAG project — expect deep questions.

### 2.1 What is LangChain?

An **open-source Python (and JS) framework** that provides modular building blocks for LLM applications. Abstracts over the messy details of LLM APIs, document handling, vector stores, and orchestration.

**Why use LangChain over raw API calls?**
- Swap LLM providers with one-line changes (OpenAI → Groq → Anthropic)
- Standard interfaces for chunking, embedding, retrieval
- Built-in retry, error handling, streaming
- Composable chains via LCEL
- Active community, large ecosystem of integrations

**Trade-off — when NOT to use LangChain:**
- Simple single-call apps (overkill)
- Need full control (LangChain's abstractions can be leaky)
- Performance-critical paths (some overhead)

### 2.2 Core building blocks

**LLMs / ChatModels** — wrappers around model providers.
```python
from langchain_groq import ChatGroq
llm = ChatGroq(model="llama-3.1-8b-instant", api_key="...")
response = llm.invoke("Hello!")
```

**Prompts** — templated instructions.
```python
from langchain_core.prompts import PromptTemplate
prompt = PromptTemplate.from_template(
    "You are a helpful assistant. Question: {question}\nAnswer:"
)
```

**Output Parsers** — convert raw LLM output to structured data.
```python
from langchain_core.output_parsers import StrOutputParser, JsonOutputParser
chain = prompt | llm | StrOutputParser()
```

**Document Loaders** — read documents from various sources.
```python
from langchain_community.document_loaders import PyPDFLoader, WebBaseLoader, CSVLoader
loader = PyPDFLoader("file.pdf")
docs = loader.load()
```

**Text Splitters** — chunk documents.
```python
from langchain_text_splitters import RecursiveCharacterTextSplitter
splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
chunks = splitter.split_documents(docs)
```

**Embeddings** — convert text to vectors.
```python
from langchain_community.embeddings import HuggingFaceEmbeddings
embed = HuggingFaceEmbeddings(model_name="sentence-transformers/all-MiniLM-L6-v2")
vec = embed.embed_query("hello world")
```

**Vector Stores** — store and search embeddings.
```python
from langchain_community.vectorstores import Chroma
vs = Chroma.from_documents(chunks, embed, persist_directory="./chroma")
retriever = vs.as_retriever(search_kwargs={"k": 4})
```

**Chains** — compose components.

**Agents** — LLMs that can use tools (see Section 8).

**Memory** — maintain conversation history across turns.

### 2.3 LCEL (LangChain Expression Language)

The **modern way** to compose chains. Uses the `|` (pipe) operator.

```python
chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)
result = chain.invoke("What is RAG?")
```

**What LCEL gives you for free:**
- `.invoke()` — sync call
- `.ainvoke()` — async call
- `.stream()` — token-by-token streaming
- `.batch()` — batch inputs
- Automatic retries, fallbacks, logging via callbacks

### 2.4 Common patterns

**Simple Q&A chain:**
```python
chain = prompt | llm | StrOutputParser()
```

**RAG chain (yours):**
```python
chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt | llm | StrOutputParser()
)
```

**Conversational with memory:**
```python
from langchain_core.runnables.history import RunnableWithMessageHistory
chain_with_history = RunnableWithMessageHistory(chain, get_session_history)
```

**Tool-using agent:**
```python
from langchain.agents import create_react_agent
agent = create_react_agent(llm, tools, prompt)
```

### 2.5 Interview Q&A

**Q1. What is LangChain?**
> An open-source framework for building LLM applications, providing standard interfaces over LLMs, prompts, retrievers, chains, and agents.

**Q2. Why did you use LangChain in your RAG project?**
> It gave me out-of-the-box integrations with PyPDF, HuggingFace embeddings, ChromaDB, and Groq — wiring them all together with LCEL was clean. Swapping from Ollama to Groq later took one line.

**Q3. What is LCEL?**
> LangChain Expression Language — composing chains using the `|` operator. Each component implements a runnable interface with `invoke`, `stream`, `batch`. Composable like Unix pipes.

**Q4. What's `RunnablePassthrough`?**
> A no-op component that passes input through unchanged. Useful when composing dicts where one key needs no transformation. Example: pass the user question through while transforming context separately.

**Q5. What's `StrOutputParser`?**
> Extracts the string content from an LLM's structured output (e.g., a `BaseMessage`). Last step in most simple chains.

**Q6. Difference between `LLM` and `ChatModel` in LangChain?**
> `LLM` is older interface — string in, string out. `ChatModel` is newer — takes a list of messages (system, user, assistant), returns a message. Most modern providers (GPT, Claude, Groq) use ChatModel.

**Q7. What's a retriever in LangChain?**
> An interface that returns relevant `Document` objects for a query. Vector stores have a `.as_retriever()` method. Can also wrap BM25, hybrid search, multi-query, etc.

**Q8. What's the difference between Chains and Agents?**
> Chains have a fixed sequence of steps. Agents are dynamic — the LLM decides which tool to call next based on the user query, can loop, can decide to stop.

**Q9. What is LangSmith?**
> LangChain's observability platform — traces LLM calls, tracks token usage, evaluates outputs. Like Datadog for LLM apps.

**Q10. What's LangGraph?**
> A newer LangChain library for building stateful, multi-actor agent workflows as directed graphs. Better for complex agentic systems than the original chain abstraction.

**Q11. How does streaming work in LangChain?**
> Use `chain.stream(input)` instead of `invoke`. Yields tokens one at a time. Requires the underlying LLM to support streaming (most do).

**Q12. How do you handle errors in LangChain?**
> Use `.with_fallbacks()` to define backup behavior. Example: try Groq, fall back to Anthropic on failure. Callbacks for logging. Use try/except around `.invoke()` for catch-all.

**Q13. What document loaders have you used?**
> `PyPDFLoader` for my RAG project. LangChain has loaders for web pages, CSVs, Notion, Slack, GitHub, etc. — over 100 integrations.

**Q14. What's the difference between `CharacterTextSplitter` and `RecursiveCharacterTextSplitter`?**
> `CharacterTextSplitter` splits on a fixed character (default `\n\n`). `Recursive` tries hierarchical: paragraphs → sentences → words → chars, only splitting finer when chunks are too big. Preserves semantic units better.

**Q15. How does LangChain handle conversation history?**
> Memory modules: `ConversationBufferMemory` (full history), `ConversationSummaryMemory` (LLM-summarized), `ConversationBufferWindowMemory` (last k turns), etc. Modern approach: `RunnableWithMessageHistory`.

---

<a id="3-huggingface-usage"></a>
## 3. HuggingFace Usage

The de facto hub for open-source ML models and datasets. You used `sentence-transformers/all-MiniLM-L6-v2` in your RAG project.

### 3.1 The HuggingFace ecosystem

**HuggingFace Hub** — repository of 1M+ models, 200K+ datasets.

**Key libraries:**

| Library | Purpose |
|---|---|
| `transformers` | Load and run transformer models |
| `sentence-transformers` | Easy embeddings |
| `datasets` | Load and process datasets |
| `tokenizers` | Fast tokenizers (Rust-backed) |
| `accelerate` | Distributed training abstraction |
| `peft` | Parameter-Efficient Fine-Tuning (LoRA, etc.) |
| `trl` | Transformer Reinforcement Learning (RLHF, DPO) |
| `diffusers` | Stable Diffusion and image generation |

### 3.2 Basic transformers usage

```python
from transformers import AutoTokenizer, AutoModelForCausalLM

# Load model + tokenizer
tokenizer = AutoTokenizer.from_pretrained("meta-llama/Llama-3.2-1B")
model = AutoModelForCausalLM.from_pretrained("meta-llama/Llama-3.2-1B")

# Generate
inputs = tokenizer("The capital of France is", return_tensors="pt")
outputs = model.generate(**inputs, max_new_tokens=20)
print(tokenizer.decode(outputs[0]))
```

**The `pipeline` shortcut for inference:**
```python
from transformers import pipeline

generator = pipeline("text-generation", model="gpt2")
result = generator("Once upon a time", max_length=50)

classifier = pipeline("sentiment-analysis")
print(classifier("I love this product"))

embedder = pipeline("feature-extraction", model="sentence-transformers/all-MiniLM-L6-v2")
embedding = embedder("hello world")
```

### 3.3 sentence-transformers (what you used)

```python
from sentence_transformers import SentenceTransformer

model = SentenceTransformer("sentence-transformers/all-MiniLM-L6-v2")
embeddings = model.encode(["sentence one", "sentence two"])
# embeddings.shape: (2, 384)
```

Why MiniLM-L6-v2:
- 22M params, fast on CPU
- 384-dim output
- Trained on 1B+ sentence pairs with contrastive loss
- MTEB benchmark: respectable on retrieval tasks
- Permissive license

### 3.4 Datasets

```python
from datasets import load_dataset

ds = load_dataset("squad")
print(ds["train"][0])  # {'question': '...', 'context': '...', 'answers': {...}}
```

### 3.5 Inference Endpoints

HuggingFace offers managed inference:
- **Inference API** — free for small models, rate-limited
- **Inference Endpoints** — dedicated GPU/CPU instances, paid
- **TGI (Text Generation Inference)** — open-source HF inference server

### 3.6 Interview Q&A

**Q1. What is HuggingFace?**
> An AI company best known for the `transformers` library and the HF Hub — a central repository of pre-trained models and datasets. The de facto place for open-source ML.

**Q2. What's the difference between `AutoModel` and a specific model class?**
> `AutoModel` auto-detects the architecture from the model card. `AutoModelForCausalLM`, `AutoModelForSequenceClassification`, etc., add task-specific heads. Specific classes (e.g., `LlamaForCausalLM`) hard-code the architecture.

**Q3. What's a `pipeline` in HuggingFace?**
> A high-level abstraction that bundles tokenizer + model + post-processing for a specific task (text-generation, sentiment, NER, etc.). Quick to use but less flexible than the low-level API.

**Q4. How do you use HuggingFace embeddings?**
> Either with `sentence-transformers` library (easiest), or via `transformers` with mean-pooling over token embeddings. The first is what most people use.

**Q5. What is fine-tuning a HuggingFace model?**
> Loading a pretrained model and continuing training on your task data. Can be:
> - **Full fine-tuning** — update all weights (expensive, lots of GPU)
> - **PEFT/LoRA** — update only a few % of weights (cheap, fast)

**Q6. What's LoRA?**
> Low-Rank Adaptation. Freeze the original model weights, add small trainable rank-r matrices on top. Reduces trainable params 1000x while maintaining most of the performance. Standard for fine-tuning LLMs cheaply.

**Q7. What is QLoRA?**
> LoRA + 4-bit quantization. Lets you fine-tune 65B+ models on a single GPU.

**Q8. What's the difference between BERT and GPT models on HF?**
> BERT (and family) — encoder-only, bidirectional, good for classification/NER/embeddings.
> GPT (and family) — decoder-only, causal, good for generation.

**Q9. How do you handle a HF model that's too big for your GPU?**
> Options: quantization (`bitsandbytes`), CPU offload (`accelerate`), model parallelism, or just pick a smaller model.

**Q10. What is MTEB?**
> Massive Text Embedding Benchmark — a standardized benchmark for evaluating embedding models on diverse tasks. The MTEB leaderboard is the go-to place to pick an embedding model.

---

<a id="4-model-inferencing"></a>
## 4. Model Inferencing

Model inferencing = running a trained model to produce outputs (as opposed to *training* it). For an AI Engineer role, this is critical.

### 4.1 Training vs Inference

| Aspect | Training | Inference |
|---|---|---|
| Goal | Learn weights | Use weights |
| Direction | Forward + backward | Forward only |
| Compute | Massive (weeks/months) | Per-request |
| GPU mem | Weights + gradients + activations + optimizer state | Weights + activations |
| Frequency | Once | Continuously in production |

For an 8B LLM:
- **Training:** ~80GB GPU memory (with optimizer states)
- **Inference:** ~16GB GPU memory (fp16 weights)
- **Inference with int4 quant:** ~5GB

### 4.2 Inference modes

**Batch inference** — process N inputs at once.
- High throughput
- Higher latency per request
- Used for offline jobs (e.g., embed 10M documents)

**Real-time / online inference** — process one request, return quickly.
- Low latency
- Lower throughput
- Used for chat, search, user-facing apps

**Streaming inference** — return tokens as they're generated.
- Better UX (user sees tokens appearing)
- Lower time-to-first-token

### 4.3 Optimization techniques

**Quantization** — reduce precision of weights.

| Precision | Bits | Size for 7B model | Quality |
|---|---|---|---|
| FP32 | 32 | 28 GB | Reference |
| FP16 / BF16 | 16 | 14 GB | ~Same |
| INT8 | 8 | 7 GB | Slight drop |
| INT4 | 4 | 3.5 GB | Noticeable drop |

**Quantization formats:**
- **GGUF** (llama.cpp) — for local CPU/GPU inference, used by Ollama
- **GPTQ** — post-training quantization for GPU
- **AWQ** — activation-aware weight quantization
- **bitsandbytes** — easy quantization in HF transformers

**Other optimizations:**
- **KV caching** — cache attention keys/values across generation steps
- **FlashAttention** — fused attention kernel, faster + less memory
- **Speculative decoding** — small model drafts, big model verifies
- **Continuous batching** — dynamically combine concurrent requests (vLLM, TGI)
- **Tensor parallelism** — split model across GPUs
- **Pipeline parallelism** — split layers across GPUs

### 4.4 Inference servers

| Server | Built by | Best for |
|---|---|---|
| **vLLM** | UC Berkeley | High-throughput LLM serving, PagedAttention |
| **TGI** (Text Generation Inference) | HuggingFace | Production HF model serving |
| **TensorRT-LLM** | NVIDIA | Maximum NVIDIA GPU performance |
| **Triton** | NVIDIA | Multi-framework serving (PyTorch, TF, ONNX) |
| **TorchServe** | PyTorch | PyTorch model serving |
| **Ollama** | Open-source | Local development, easy install |
| **Groq Cloud** | Groq | Custom LPU hardware, ultra-fast |

### 4.5 Inference metrics

| Metric | What it measures | Typical good value |
|---|---|---|
| **TTFT** (time to first token) | How fast does the first token appear? | <500ms |
| **TPS** (tokens per second) | Generation speed | >30 tok/s for 7B on GPU |
| **Throughput** | Requests per second per server | Varies |
| **p50, p95, p99 latency** | Percentile latencies | p99 < 2s |
| **GPU utilization** | How busy is the GPU? | >80% |
| **Cost per token** | $$$ | Depends on model and infra |

### 4.6 Interview Q&A

**Q1. What's the difference between training and inference?**
> Training updates weights to learn from data; inference uses fixed weights to produce outputs. Training requires gradients, activations, optimizer state → way more memory. Inference is forward-only.

**Q2. Why is inference latency important?**
> User experience — slow apps lose users. SLA compliance. Cost — slower inference per request = more GPU-hours to serve same traffic.

**Q3. What is quantization?**
> Reducing the numerical precision of model weights (e.g., 16-bit floats → 4-bit integers). Trades a small quality drop for much smaller memory + faster inference.

**Q4. Why is Groq so fast?**
> Custom hardware — **LPUs (Language Processing Units)** designed specifically for LLM inference, with deterministic execution and massive memory bandwidth. Compare to NVIDIA GPUs, which are general-purpose.

**Q5. What's KV caching?**
> During autoregressive generation, you'd recompute attention against all previous tokens at each step. KV cache stores the Keys and Values of previous tokens so you only compute attention for the new token. Reduces O(n²) to O(n) per step.

**Q6. What's continuous batching?**
> Traditional batching waits for a full batch before processing — wastes GPU when requests arrive at different times. Continuous batching (vLLM, TGI) dynamically adds new requests to an in-flight batch, freeing slots as sequences finish. Much higher throughput.

**Q7. What's the trade-off between batch size and latency?**
> Larger batch → higher throughput (better GPU utilization), but higher per-request latency (have to wait for batch to fill). Smaller batch → lower latency but worse utilization.

**Q8. What's GGUF? Why is it popular?**
> A quantization format used by `llama.cpp` and Ollama. Supports running LLMs on CPU efficiently, in formats like Q4_K_M (4-bit mixed). Lets people run 7B+ models on a MacBook.

**Q9. What's the difference between TTFT and TPS?**
> TTFT = Time to First Token — how long until the first output token appears. TPS = Tokens Per Second — how fast subsequent tokens stream. For chat, TTFT matters most (user is waiting for response to start).

**Q10. What's speculative decoding?**
> Use a small "draft" model to generate k tokens, then verify them all in one pass of the big model. If they all match → 4x speedup. If they diverge → revert. Net wins because most tokens are easy and small model is fast.

**Q11. Why might you choose CPU inference over GPU?**
> Smaller models (7B and below), cost (CPUs cheaper at low volume), simplicity (no GPU ops), edge/mobile deployment. Trade-off: 10-50x slower than GPU.

**Q12. How do you deploy a model in production?**
> Options:
> - **API providers** (OpenAI, Groq) — simplest, just call API
> - **Managed inference** (HF Endpoints, Vertex AI, SageMaker)
> - **Self-hosted server** (vLLM/TGI on K8s)
> - **Embedded** (TFLite, ONNX, llama.cpp) for edge

---

<a id="5-ml-metrics"></a>
## 5. Supervised & Unsupervised Learning + Metrics

This is **critical** — these are the metrics interviewers will grill you on. Don't just memorize names; understand WHEN each applies.

### 5.1 Types of Machine Learning

| Type | Definition | Examples |
|---|---|---|
| **Supervised** | Labeled data (X → y) | Classification, regression |
| **Unsupervised** | No labels, find structure | Clustering, dim reduction |
| **Semi-supervised** | Few labels + lots of unlabeled | Pseudo-labeling, self-training |
| **Self-supervised** | Labels derived from data itself | BERT pretraining (masked LM) |
| **Reinforcement** | Agent learns from rewards | AlphaGo, RLHF |

### 5.2 Supervised — Classification

**Binary classification:** 2 classes (spam / not-spam, fraud / not-fraud).
**Multi-class:** >2 classes, mutually exclusive (digit recognition: 0-9).
**Multi-label:** >2 classes, can have multiple labels (movie genres).

**Models:** Logistic Regression, SVM, Random Forest, XGBoost, Neural Networks, BERT (fine-tuned).

### 5.3 Supervised — Regression

Predict a continuous value (house price, temperature).

**Models:** Linear Regression, Ridge, Lasso, Random Forest Regressor, Gradient Boosting, Neural Networks.

### 5.4 Unsupervised

**Clustering:** group similar data points.
- K-Means, DBSCAN, Hierarchical, Gaussian Mixture Models

**Dimensionality reduction:**
- PCA, t-SNE, UMAP, autoencoders

**Anomaly detection:**
- Isolation Forest, One-Class SVM

**Topic modeling:**
- LDA (Latent Dirichlet Allocation)

### 5.5 Classification Metrics — DEEP DIVE

**Confusion matrix** (for binary classification):

```
                    Predicted
                  Pos      Neg
        Pos    [  TP    |  FN  ]
Actual         |--------|-------|
        Neg    [  FP    |  TN  ]
```

- **TP** (True Positive): correctly predicted positive
- **TN** (True Negative): correctly predicted negative
- **FP** (False Positive / Type I error): predicted positive, actually negative
- **FN** (False Negative / Type II error): predicted negative, actually positive

#### Accuracy

```
Accuracy = (TP + TN) / (TP + TN + FP + FN)
```

✅ Use when: classes are balanced, errors equally costly.
❌ Don't use when: classes are imbalanced (e.g., 99% legit transactions, 1% fraud → always-predict-legit gets 99% accuracy but is useless).

#### Precision

```
Precision = TP / (TP + FP)
```

> "Of everything I called positive, how many actually are?"

✅ Use when: false positives are costly. Examples:
- Spam filter (don't want to mark real emails as spam)
- Medical diagnosis triggering invasive procedure

#### Recall (Sensitivity, True Positive Rate)

```
Recall = TP / (TP + FN)
```

> "Of all actual positives, how many did I catch?"

✅ Use when: false negatives are costly. Examples:
- Cancer screening (don't miss cancer)
- Fraud detection (don't miss fraud)

#### F1 Score

```
F1 = 2 · (Precision · Recall) / (Precision + Recall)
```

> Harmonic mean of precision and recall.

✅ Use when: balance both, especially with imbalanced classes.

#### False Positive Rate (FPR)

```
FPR = FP / (FP + TN)
```

> "Of all actual negatives, how many did I incorrectly call positive?"

#### False Negative Rate (FNR)

```
FNR = FN / (FN + TP)  = 1 - Recall
```

#### Specificity (True Negative Rate)

```
Specificity = TN / (TN + FP)  = 1 - FPR
```

> "Of all actual negatives, how many did I correctly call negative?"

#### ROC Curve & AUC

- **ROC curve:** plot of TPR vs FPR at different classification thresholds
- **AUC-ROC:** area under the curve. Range [0, 1]. 0.5 = random, 1.0 = perfect.
- AUC-ROC > 0.7 = reasonable, > 0.9 = excellent.

#### PR Curve & AUC-PR

- **Precision-Recall curve:** plot of Precision vs Recall.
- Better than ROC for **imbalanced datasets**.

### 5.6 Regression Metrics

**MAE (Mean Absolute Error):**
```
MAE = (1/n) Σ |y_pred - y_true|
```
- Same units as target
- Robust to outliers

**MSE (Mean Squared Error):**
```
MSE = (1/n) Σ (y_pred - y_true)²
```
- Penalizes large errors more (square)
- Differentiable everywhere

**RMSE (Root Mean Squared Error):**
```
RMSE = √MSE
```
- Same units as target
- More interpretable than MSE

**R² (Coefficient of Determination):**
```
R² = 1 - (SS_res / SS_tot)
```
- Range [-∞, 1]. 1 = perfect, 0 = same as mean baseline.

### 5.7 How to Improve Model Metrics

**1. Better data**
- More training examples
- Better labels (manual review of mislabels)
- Augmentation (synthetic data)
- Address class imbalance (oversampling/SMOTE, undersampling, class weights)

**2. Better features (feature engineering)**
- Domain knowledge — add derived features
- One-hot encoding, scaling, normalization
- Handling missing values (mean/median/mode imputation, model-based)
- Drop irrelevant features (feature importance from model)

**3. Better model**
- Try different algorithms
- Ensembles (bagging, boosting, stacking)
- Deep learning if data justifies it

**4. Hyperparameter tuning**
- Grid search, random search, Bayesian optimization (Optuna, Hyperopt)

**5. Regularization (prevent overfitting)**
- L1 (Lasso) — induces sparsity, feature selection
- L2 (Ridge) — shrinks weights
- Dropout (NN) — randomly drop neurons during training
- Early stopping — stop training when val loss plateaus
- Data augmentation

**6. Cross-validation**
- K-fold CV for reliable performance estimate
- Stratified K-fold for imbalanced data

**7. Threshold tuning**
- Default 0.5 for binary classification — often suboptimal
- Pick threshold based on business cost of FP vs FN

**8. Address overfitting / underfitting**
- Overfitting: model too complex, memorizes training. Symptoms: train acc >> val acc. Fix: more data, regularization, simpler model.
- Underfitting: model too simple. Symptoms: low train acc. Fix: more complex model, more features.

### 5.8 Overcoming Error Rate

**Steps to systematically reduce error:**

1. **Error analysis** — manually inspect 50-100 wrong predictions. Categorize errors.
2. **Address most common error categories** first.
3. **Augment training data** for under-represented patterns.
4. **Adjust loss function** — focal loss for class imbalance, weighted cross-entropy.
5. **Ensemble** — combine multiple models (often boosts 1-3%).
6. **Calibrate predictions** — Platt scaling, isotonic regression — when probability values matter.
7. **Specialized models** for rare cases — cascade architecture.

### 5.9 Bias-Variance Trade-off

```
Total Error = Bias² + Variance + Irreducible Error
```

- **High bias** = underfitting (too simple)
- **High variance** = overfitting (too complex, memorizing)

**Reducing bias:** more complex model, more features, less regularization.
**Reducing variance:** more data, more regularization, ensembling, simpler model.

### 5.10 Cross-Validation

**K-fold CV:**
- Split data into K folds (typically 5 or 10)
- Train on K-1 folds, evaluate on 1
- Repeat K times, average metrics
- More reliable than single train/val split

**Stratified K-fold:** ensures each fold has same class distribution as overall — critical for imbalanced data.

### 5.11 Interview Q&A

**Q1. What's the difference between supervised and unsupervised learning?**
> Supervised has labels — you learn a function `f(X) → y`. Unsupervised has no labels — you find structure in `X` alone (clusters, low-dim representations, anomalies).

**Q2. Why isn't accuracy always the right metric?**
> Because it ignores class distribution. On a 99/1 imbalanced dataset, always predicting the majority gives 99% accuracy but zero useful signal.

**Q3. When would you optimize for precision over recall?**
> When false positives are costly. Example: a fraud-prediction model that triggers an account freeze. False positives → angry customers, support tickets. So you want high precision: when you cry fraud, it really is fraud.

**Q4. When would you optimize for recall over precision?**
> When false negatives are costly. Example: cancer screening. Missing a real cancer (FN) is catastrophic; a false alarm (FP) leads to more tests, which is bad but not life-threatening.

**Q5. What is F1 score?**
> Harmonic mean of precision and recall. Used when you want a single number that balances both. Harmonic mean (vs arithmetic) penalizes large gaps — F1 is low if either P or R is low.

**Q6. What is AUC-ROC?**
> Area under the ROC curve. Measures classifier's ability to rank positives above negatives across all thresholds. Threshold-independent metric. 0.5 = random, 1.0 = perfect.

**Q7. What's the difference between Type I and Type II errors?**
> Type I = False Positive (rejecting a true null hypothesis).
> Type II = False Negative (failing to reject a false null hypothesis).
> Mnemonic: Type I = "Innocent guilty" (1 letter, jail innocent person); Type II = "guilty Innocent" (2, set guilty person free).

**Q8. How do you handle imbalanced classes?**
> Multiple options:
> - Class weights in loss function
> - Oversampling (SMOTE, ADASYN) or undersampling
> - Use precision/recall/F1 instead of accuracy
> - Adjust classification threshold
> - Specialized loss (focal loss)

**Q9. What's overfitting? How do you detect and fix it?**
> Overfitting = model fits training data well but generalizes poorly. Detect via train-val gap (train acc 99%, val acc 70% → overfitting). Fix: more data, regularization (L1/L2/dropout), simpler model, early stopping, augmentation.

**Q10. What's the bias-variance trade-off?**
> Bias = systematic error from wrong assumptions; high bias → underfitting. Variance = sensitivity to training data; high variance → overfitting. Total error = bias² + variance + irreducible. Goal: balance both.

**Q11. What's k-fold cross-validation?**
> Split data into k subsets, train on k-1, validate on 1, repeat k times, average. Provides more reliable performance estimate than a single split. K=5 or K=10 typical.

**Q12. What's regularization?**
> Technique to prevent overfitting by adding a penalty on model complexity to the loss function.
> - L1 (Lasso): penalty on |weights|; induces sparsity.
> - L2 (Ridge): penalty on weights²; shrinks all weights.
> - Dropout (NN): randomly zero out neurons during training.

**Q13. What's the difference between L1 and L2 regularization?**
> L1 (Lasso) penalty: `Σ |w_i|`. Pushes weights to exactly 0 → feature selection.
> L2 (Ridge) penalty: `Σ w_i²`. Shrinks weights toward 0 but rarely to exactly 0.
> Elastic Net = both.

**Q14. What's precision-recall trade-off?**
> Lowering classification threshold → more positives predicted → higher recall (catch more) but lower precision (more false alarms). Raising threshold → opposite. Choose based on business cost of FP vs FN.

**Q15. How do you evaluate a clustering model?**
> No labels, so:
> - **Silhouette score** — how well-separated clusters are
> - **Davies-Bouldin index** — lower = better
> - **Inertia** (within-cluster sum of squares) — for K-means elbow plot
> - **Adjusted Rand Index (ARI)** — if you have ground truth labels

**Q16. How do you handle missing data?**
> Options:
> - Drop rows (if few missing)
> - Drop column (if mostly missing)
> - Imputation: mean/median/mode, KNN imputation, model-based
> - Indicator variable for missingness
> - For tree models, some natively handle missing

**Q17. What's data leakage?**
> When information from outside training set "leaks" into training, causing inflated performance on validation. Examples:
> - Computing scaler params on full data (including val) before split
> - Using future info to predict past
> - Target leakage (feature derived from label)
> Always: split first, then preprocess on train only.

**Q18. What's a baseline model?**
> A simple model used as a benchmark. Examples:
> - Always predict majority class
> - Always predict mean (regression)
> - Logistic regression
> If your fancy model can't beat baseline, something is wrong.

**Q19. What metrics for object detection?**
> mAP (mean Average Precision) — average precision computed at different IoU thresholds, averaged across classes.

**Q20. What metrics for NER?**
> Precision/recall/F1 at the entity level (must match exact span AND type).

---

<a id="6-rag-deep-dive"></a>
## 6. RAG Deep Dive

You built one — you're expected to know this cold. Some overlap with `ibm_genai_prep.md` Section 5 — this one is more advanced.

### 6.1 RAG taxonomy

**Naive RAG** (what most projects are):
1. Chunk docs → embed → store
2. Embed query → top-k retrieval
3. Stuff context → LLM → answer

Your project = Naive RAG.

**Advanced RAG** adds:
- Pre-retrieval: query rewriting, query expansion, HyDE
- Retrieval: hybrid (BM25 + dense), metadata filters
- Post-retrieval: reranking, compression, deduplication
- Iterative retrieval

**Modular RAG**: decompose into independent pluggable modules — orchestrated by an agent.

### 6.2 Chunking strategies — deeper

| Strategy | When |
|---|---|
| **Fixed-size** (what you did) | Simple text, fast prototyping |
| **Sentence-based** | When sentences = natural unit |
| **Paragraph-based** | Well-structured docs |
| **Recursive** | Mixed content (paragraphs → sentences → words) |
| **Semantic chunking** | Embed sentences, group by similarity threshold |
| **Document structure-aware** | Split on headings, tables, code blocks separately |
| **Hierarchical / parent-child** | Index small chunks, return large parents |

### 6.3 Retrieval strategies

**Dense retrieval** (embeddings): semantic, captures synonyms, but misses exact-keyword cases.

**Sparse retrieval** (BM25/TF-IDF): exact keyword match, fast, surprisingly strong baseline.

**Hybrid:** combine both. Methods:
- **Linear combo:** `score = α · dense + (1-α) · sparse`
- **Reciprocal Rank Fusion (RRF):** `score = Σ 1/(k + rank_i)` across retrievers. No need to normalize scores.

**Multi-query retrieval:** ask LLM to generate 3-5 query variants, retrieve for each, deduplicate. Improves recall.

**HyDE (Hypothetical Document Embeddings):** ask LLM to generate a hypothetical answer to the query, embed that, retrieve docs similar to it. Counter-intuitive but works because the hypothetical answer looks more like a real document than the query does.

**Self-querying:** LLM extracts metadata filters from the query before retrieval (e.g., "find papers by Hinton from 2018" → `filter: author=Hinton, year=2018`).

### 6.4 Reranking

**Why:** initial retrieval (vector or BM25) prioritizes speed over precision. A cross-encoder reranker takes the top-k results and re-scores them with a smaller, more accurate model.

**How:**
1. Retrieve top-100 with vector search (fast, broad)
2. Rerank with cross-encoder (slow, precise)
3. Return top-4 reranked

**Cross-encoder vs bi-encoder:**
- **Bi-encoder** (what you used): embed query and doc independently → cosine similarity. Fast.
- **Cross-encoder**: query+doc together through transformer → score. Slow but accurate.

**Popular rerankers:**
- `cross-encoder/ms-marco-MiniLM-L-6-v2` (open, on HF)
- Cohere Rerank API
- Voyage AI Rerank

### 6.5 RAG evaluation

**RAGAS framework metrics:**

| Metric | What it measures |
|---|---|
| **Faithfulness** | Is the answer grounded in the retrieved context? (Hallucination check) |
| **Answer relevance** | Does the answer address the question? |
| **Context relevance** | Are the retrieved chunks relevant? |
| **Context recall** | Did retrieval catch the necessary info? |
| **Context precision** | Are retrieved chunks ranked well? |

Implementation: use a strong LLM (GPT-4) as judge.

### 6.6 Common RAG failures

| Failure | Cause | Fix |
|---|---|---|
| LLM hallucinates despite context | Bad prompt, weak grounding | "Only answer from context; say 'I don't know' otherwise" |
| Retrieves wrong chunks | Embedding model mismatch | Better embeddings, hybrid search, reranking |
| Answer ignores context | Context too long, lost in middle | Reduce k, summarize context, rerank |
| Question paraphrased badly | Embedding doesn't capture intent | Query rewriting, HyDE |
| User asks meta-question | RAG retrieves nothing relevant | Detect & route to non-RAG handler |

### 6.7 Advanced techniques (be aware of)

- **Self-RAG** — the model decides when to retrieve, what to retrieve, and whether to use it.
- **CRAG (Corrective RAG)** — retrieval quality is graded; on low quality, fall back to web search.
- **GraphRAG** (Microsoft) — extract entities + relations into a graph, retrieve subgraphs instead of chunks. Better for "summarize this whole corpus" queries.
- **Long-context vs RAG** — modern 200K+ context LLMs let you skip RAG for medium corpora. RAG still wins on cost and large/dynamic data.

### 6.8 Interview Q&A

**Q1. What is RAG?**
> Retrieval-Augmented Generation. Inject relevant retrieved content into the LLM prompt as context before generation, grounding answers in real sources.

**Q2. RAG vs fine-tuning — when each?**
> RAG: adding *facts* / dynamic knowledge / private data without training. Fine-tuning: teaching *style*, *format*, or *task behavior* that can't be expressed in a prompt.

**Q3. Why RAG?**
> Frozen LLMs have stale knowledge cutoffs and don't know your private data. Fine-tuning is expensive and doesn't help with rapidly-changing info. RAG is cheap, flexible, and provides citation/traceability.

**Q4. Walk me through a RAG pipeline end to end.**
> *(See Section 6.1 + 5.1 in ibm_genai_prep.md.)*

**Q5. What chunk size do you pick and why?**
> Depends on content. For my project (PDFs): 500 chars / 50 overlap — enough to capture a paragraph, small enough for precise retrieval. For dense technical text I'd go 200; for narrative 1000.

**Q6. How do you handle cases when retrieval fails?**
> Several layers:
> - Threshold the lowest similarity score; if below, return "I don't know"
> - Self-check with LLM: "Does the context cover the question?"
> - Fall back to non-RAG response or web search

**Q7. What's hybrid search?**
> Combining sparse (BM25/keyword) and dense (embedding) retrieval, then merging results. Beats either alone in most benchmarks. Use Reciprocal Rank Fusion to combine without normalizing scores.

**Q8. What's reranking? Why?**
> Initial retrieval optimizes for speed; rerankers (cross-encoders) re-score the top-k with a more accurate model. Workflow: retrieve top-100 → rerank to top-4.

**Q9. What is HyDE?**
> Hypothetical Document Embeddings. Ask the LLM to generate a hypothetical answer first, embed it, then retrieve documents similar to the hypothetical answer. Better recall because the hypothetical answer looks more like real documents than a question does.

**Q10. How do you evaluate a RAG system?**
> RAGAS metrics: faithfulness (no hallucination), answer relevance, context relevance, context recall, context precision. Use a strong LLM as judge. Also: maintain a golden set of (Q, A, expected context) for regression testing.

**Q11. Why might RAG hallucinate even with good retrieval?**
> Prompt doesn't constrain enough, model ignores context, context is too long, model has strong priors that override context. Fix: strict prompts, shorter context, better instruction tuning.

**Q12. RAG vs putting everything in the context window?**
> RAG wins when:
> - Data > context window
> - Data changes frequently (don't re-embed everything in prompt)
> - Cost matters (per-token charges)
> - Need citations
> Context-stuffing wins when data is small and static.

**Q13. What's the role of metadata in retrieval?**
> Filter results based on attributes (date, author, source type) before similarity search. Lets a query like "tickets from Q1 2024" filter date range, then search semantically within that subset.

**Q14. Can you have RAG without embeddings?**
> Yes — BM25-only RAG (sparse retrieval, no embeddings). Surprisingly strong baseline. Modern systems use both (hybrid).

**Q15. What's GraphRAG?**
> Microsoft's variant. Extract entities and relationships from documents into a knowledge graph. Retrieve subgraphs (not chunks) when answering. Better for global queries ("summarize the themes"); worse for narrow factoid lookups.

---

<a id="7-vector-search"></a>
## 7. Vector Search Deep Dive

### 7.1 Embeddings basics

An **embedding** is a fixed-size dense vector representing a piece of data (text, image, audio) in a way that **semantic similarity → vector closeness**.

**Dimensions:** typical text embeddings are 384, 768, 1024, 1536, 3072.

**Trade-off:** higher dim → more precision but more storage/compute.

### 7.2 Similarity metrics

**Cosine similarity** (most common for text):
```
cos(a, b) = (a · b) / (||a|| · ||b||)
```
Range: [-1, 1] (or [0, 1] for normalized vectors).
Cares about *direction*, not magnitude.

**Dot product:**
```
a · b = Σ a_i · b_i
```
Cares about magnitude AND direction. Equivalent to cosine for normalized vectors.

**Euclidean (L2) distance:**
```
||a - b|| = √(Σ (a_i - b_i)²)
```
Cares about absolute position in space.

### 7.3 Approximate Nearest Neighbor (ANN) algorithms

**Why approximate?** Exact NN is O(N · d). For N=10M, d=384 → too slow.

**HNSW (Hierarchical Navigable Small World)** — current state-of-the-art for most use cases:
- Build a multi-layer proximity graph
- Search: greedy traversal starting from top layer
- Pros: fast, high recall, dynamic insertion
- Cons: memory-heavy
- Used by: ChromaDB, Qdrant, Weaviate

**IVF (Inverted File Index):**
- Cluster vectors into Voronoi cells
- Search only nearby cells
- Pros: memory-efficient
- Cons: lower recall than HNSW
- Used by: FAISS

**LSH (Locality-Sensitive Hashing):**
- Hash vectors so similar ones collide
- Pros: simple
- Cons: lower recall in high dimensions
- Used less now

**PQ (Product Quantization):**
- Split vector into sub-vectors, quantize each
- Massive memory reduction (often 32x)
- Used in combination with IVF (IVF-PQ)

### 7.4 Index parameters

**HNSW parameters:**
- `M` — number of neighbors per node (12-64 typical)
- `ef_construction` — search depth during index build (200+ for quality)
- `ef_search` — search depth during query (50-200, balance speed/recall)

**Trade-offs:**
- Higher M → better recall, more memory
- Higher ef → better recall, slower

### 7.5 Vector DB comparison

| DB | Persistence | Best for | Notes |
|---|---|---|---|
| **FAISS** | Library only | Raw speed, max control | No persistence, no metadata, no server |
| **Annoy** | File-based | Static datasets | Spotify-built, simple |
| **HNSWlib** | Library | Fast in-memory | C++ with Python bindings |
| **ChromaDB** | SQLite-backed | Prototypes, small-medium | Easiest setup, your choice |
| **Qdrant** | Disk + memory | Production, filtering | Rust, fast |
| **Weaviate** | Disk + memory | Production, hybrid | GraphQL API |
| **Pinecone** | Managed cloud | Hands-off scale | Vendor lock-in |
| **Milvus** | Cloud-native | Massive scale (>1B) | Distributed by design |
| **pgvector** | Postgres extension | Already on Postgres | Use existing infra |

### 7.6 Performance tuning

For a vector DB at scale:
1. **Right index** — HNSW for most cases
2. **Tune index params** — find sweet spot via recall@10 measurement
3. **Quantize** — int8 or PQ for memory wins
4. **Pre-filter** — apply metadata filters before similarity search
5. **Sharding** — distribute across nodes
6. **Caching** — Redis for hot queries

### 7.7 Interview Q&A

**Q1. What's an embedding?**
> A dense vector that represents semantic meaning. Similar inputs → similar vectors (close in vector space).

**Q2. Why cosine similarity over dot product?**
> Cosine is normalized — doesn't care about vector magnitude, only direction. Dot product favors longer vectors. For normalized vectors (most embeddings), they're equivalent.

**Q3. What's the difference between L2 distance and cosine similarity?**
> L2 measures absolute spatial distance; cosine measures angular similarity. For normalized vectors, they're monotonically related. For non-normalized, very different.

**Q4. What's an ANN algorithm?**
> Approximate Nearest Neighbor — sacrifices a small amount of recall for huge speed gains. Exact NN is O(N); HNSW is sub-linear.

**Q5. How does HNSW work?**
> Multi-layer graph. Top layer has few sparse nodes (long-range "highways"); bottom layer has all nodes densely connected. Search starts at top, greedily moves toward query, descending layers as it homes in.

**Q6. What's recall@k?**
> Of the true top-k nearest neighbors, how many did the ANN return in its top-k? Standard quality metric for ANN.

**Q7. Why use embeddings instead of keyword search?**
> Embeddings capture semantics: "car" and "automobile" return similar results. Keyword search would miss this without query expansion.

**Q8. When would keyword search beat embeddings?**
> When exact terms matter — product SKUs, code identifiers, rare names. Hybrid is best.

**Q9. What's the curse of dimensionality?**
> In high dimensions, distances become less meaningful (all pairs of points become roughly equidistant). Affects naive NN; ANN algorithms still work because they exploit local structure.

**Q10. How do you choose embedding dimensionality?**
> Trade-off: higher = more semantic capacity but more storage/compute. 384 (MiniLM) for fast prototyping; 1024-1536 (bge-large, OpenAI) for production. Newer "Matryoshka" embeddings let you truncate to multiple dims.

**Q11. What's product quantization?**
> Split each vector into sub-vectors, quantize each independently via clustering. Vector becomes a tuple of cluster IDs (1 byte each). 32x memory reduction with small accuracy loss.

**Q12. How do you handle metadata filtering with vector search?**
> Two approaches:
> - **Pre-filtering**: filter by metadata first, then search within subset (better recall, slower for small results)
> - **Post-filtering**: search first, filter results (faster, but may return fewer than k after filter)
> Modern DBs (Qdrant, Weaviate) optimize pre-filtering well.

**Q13. What does it mean for an embedding model to be "trained" on a corpus?**
> The encoder neural network has its weights adjusted (via contrastive learning) so that semantically-related text pairs in the corpus produce vectors close together. The patterns it learns transfer to unseen data.

**Q14. Why is MTEB important?**
> Standardized benchmark across embedding tasks (retrieval, classification, clustering, reranking). The MTEB leaderboard is how the community compares embedding models objectively.

**Q15. What's the difference between dense and sparse vectors?**
> Dense = most dims are non-zero (e.g., 384 dim, all populated). Sparse = mostly zeros (e.g., TF-IDF over 50K vocab). Dense from neural models; sparse from term-frequency methods.

---

<a id="8-ai-agents"></a>
## 8. AI Agents

Agents are LLM-driven systems that can plan, use tools, and act autonomously. Expect questions given the role title is "AI Engineer."

### 8.1 What is an AI agent?

An **autonomous system** that uses an LLM as its reasoning engine to:
1. Understand a goal
2. Plan steps
3. Call tools (functions, APIs, code)
4. Observe results
5. Iterate until done

Versus a **chain**: fixed sequence; you decide steps in advance.
Versus an **agent**: dynamic sequence; LLM decides next step based on prior observations.

### 8.2 The ReAct framework

**Re**asoning + **Act**ing. Most foundational agent pattern.

Loop:
1. **Thought** — LLM thinks about what to do
2. **Action** — LLM picks a tool and arguments
3. **Observation** — system runs tool, returns result
4. **Thought** — LLM thinks about result
5. ... repeat until final answer

Example:
```
User: What's the weather in Tokyo and the time difference from NYC?

Thought: I need to look up current weather in Tokyo. Tool: get_weather.
Action: get_weather(city="Tokyo")
Observation: 22°C, partly cloudy.

Thought: Now I need the time difference. Tool: get_time_diff.
Action: get_time_diff(from="NYC", to="Tokyo")
Observation: +14 hours.

Thought: I have all info. Final answer.
Final Answer: Weather in Tokyo is 22°C, partly cloudy. Tokyo is 14 hours ahead of NYC.
```

### 8.3 Tool use / function calling

Modern LLMs (GPT-4, Claude, Llama-3.1 with tool use, etc.) support **structured function calling** — you provide function schemas (name, description, parameters), the LLM responds with JSON specifying which function to call and its arguments.

```python
tools = [{
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Get current weather for a city",
        "parameters": {
            "type": "object",
            "properties": {
                "city": {"type": "string"}
            },
            "required": ["city"]
        }
    }
}]

response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "What's the weather in Tokyo?"}],
    tools=tools
)
# response.choices[0].message.tool_calls → list of function calls
```

### 8.4 Multi-agent systems

**Patterns:**

**Supervisor pattern** — one "manager" agent delegates to specialist agents.

**Hierarchical** — tree of supervisors and workers.

**Network** — agents communicate peer-to-peer.

**Frameworks:**
- **AutoGen** (Microsoft) — chat-based multi-agent
- **CrewAI** — role-based agents with shared goals
- **LangGraph** — explicit state-machine for agents

### 8.5 Agent architectures

| Pattern | When |
|---|---|
| Single agent + ReAct | Simple multi-tool workflows |
| Plan-and-execute | Complex tasks needing upfront planning |
| Reflection/critique | Self-improving outputs |
| Hierarchical | Large, decomposable problems |
| Swarm | Parallel exploration |

### 8.6 LangGraph

LangChain's modern agent framework.

```python
from langgraph.graph import StateGraph

builder = StateGraph(MyState)
builder.add_node("agent", agent_node)
builder.add_node("tool", tool_node)
builder.add_edge("agent", "tool")
builder.add_conditional_edges("tool", should_continue, {"continue": "agent", "end": END})
graph = builder.compile()
```

**Why graphs?** Explicit state, cycles, conditional edges. Easier to debug, observe, and persist than chains.

### 8.7 Common pitfalls

| Pitfall | Symptom | Fix |
|---|---|---|
| **Infinite loops** | Agent calls same tool repeatedly | Max iteration limit, loop detection |
| **Hallucinated tools** | Agent invents non-existent function | Strict schema validation, retry with error |
| **Hallucinated params** | Wrong arg names/types | Schema validation, retry |
| **Token blowout** | Conversation history grows huge | Summarize old turns, sliding window |
| **Slow** | Many sequential LLM calls | Parallelize independent tool calls |
| **Cost spike** | Unbounded execution | Per-task token budget |

### 8.8 Interview Q&A

**Q1. What's an AI agent?**
> An autonomous system using an LLM to plan, use tools, and act. Unlike chains (fixed steps), agents decide dynamically what to do based on observations.

**Q2. What's the ReAct framework?**
> Reasoning + Acting. Loop of: Thought → Action (tool call) → Observation → Thought → ... → Final Answer. Foundational pattern for tool-using agents.

**Q3. How does function calling work?**
> You provide function schemas to the LLM. It responds with JSON specifying which function to call and arguments. Your code executes the function and feeds results back to the LLM.

**Q4. Single agent vs multi-agent — when each?**
> Single: simple, predictable, fewer surprises. Multi-agent: complex tasks with sub-problems that benefit from specialization. Trade-off: multi-agent is harder to debug and costs more.

**Q5. What's the supervisor pattern?**
> One "manager" agent breaks down the task and delegates to specialized worker agents. Manager aggregates and returns.

**Q6. What's LangGraph?**
> LangChain's agent framework based on directed graphs. Explicit state, conditional edges, cycles. Better for complex agents than chain-based abstractions.

**Q7. Why might an agent loop forever?**
> LLM decides to call the same tool repeatedly without making progress. Mitigations: max iterations, detect repeated tool calls, force "give up" path.

**Q8. How do you handle errors in tool execution?**
> Catch the error, send it back to the LLM as an observation with a clear message: "Tool failed: <error>. Please try a different approach." LLM usually adapts.

**Q9. How do you reduce agent latency?**
> Parallelize independent tool calls, smaller LLM for simple subtasks, cache repeated calls, streaming for visible progress.

**Q10. AutoGen vs CrewAI vs LangGraph?**
> AutoGen: chat-based multi-agent, Microsoft. CrewAI: role-based (a "researcher" agent, a "writer" agent), goal-oriented. LangGraph: explicit graph + state, more control. Pick based on style preference.

**Q11. What's planning in an agent?**
> Instead of step-by-step ReAct, generate the full plan upfront ("first do X, then Y, then Z"), then execute. Works better for complex tasks where ReAct could go off-track.

**Q12. What's "reflection" in agents?**
> Agent critiques its own output and revises. Pattern: generate → reflect ("is this correct?") → revise. Improves quality, especially for code or writing.

**Q13. How do you give an agent memory?**
> Conversation buffer (full history), summary memory (compressed), vector memory (retrieve relevant past turns from a vector store). Modern: LangGraph state + checkpointing.

**Q14. What's a "tool" in agent context?**
> A function the agent can call. Could be: web search, calculator, code executor, database query, API call, custom logic. Each has a name, description, and parameter schema.

**Q15. How would you build an agent for the IBM AI Engineer role?**
> An agent that helps with build system issues — given a failing build log, it could:
> - Tool 1: parse log for error patterns
> - Tool 2: search past issues in JIRA
> - Tool 3: suggest CMake/Makefile fixes
> - Tool 4: trigger reruns
> Built on LangGraph for state, Groq for fast inference.

---

<a id="9-rag-project"></a>
## 9. Your RAG Project — Talking Points

Already covered in depth in `ibm_genai_prep.md` Section 6. Quick refresher of key numbers + design decisions:

| What | Value | Why |
|---|---|---|
| Chunk size | 500 chars | Semantic coherence vs precision balance |
| Chunk overlap | 50 chars | Prevent splitting mid-thought |
| Embedding | MiniLM-L6-v2 (384d) | Fast, good quality, free, HF-hosted |
| Vector store | ChromaDB | Easy persistence, no separate server |
| Retrieval k | 4 | Enough context, not too noisy |
| LLM | Groq llama-3.1-8b-instant | Free tier, sub-second latency, full Llama-3.1 quality |
| Originally | Ollama (llama3, then llama3.2:1b) | Local, free, no API; switched due to 8GB RAM constraint |
| API | Flask | Familiar, lightweight |
| Port | 7000 | Avoid Windows port 5000/8000 conflicts |
| Composition | LangChain LCEL | Pipe-style composability, async-ready |
| Citations | `doc.metadata['source']` | User can verify answer source |

**Two-sentence pitch** (memorize):
> *"I built a RAG-based PDF Q&A system using LangChain, ChromaDB, HuggingFace embeddings, and Groq's LLM API. Users upload a PDF, ask questions in natural language, and get context-grounded answers with source citations — all served through a Flask app with a browser chat UI."*

**One-liner on the technical depth:**
> *"It's not just a tutorial copy — I made every design choice myself (chunk size, embedding model, vector DB, LLM, prompt template), iterated on issues like RAM limits and Groq model deprecation, and the architecture is fully provider-agnostic via LangChain."*

---

<a id="10-python-interview"></a>
## 10. Python Interview Questions

### 10.1 Data structures

**Q1. List vs tuple — when to use which?**
> Lists are mutable, tuples are immutable. Use tuples for fixed records (e.g., coordinates), as dict keys, when you want a hashable collection. Lists when you need to modify.

**Q2. Dict internals — how does it work?**
> Hash table. Each key is hashed; the hash determines the bucket. CPython 3.7+ preserves insertion order. Operations are O(1) average.

**Q3. Set vs dict — what's the difference?**
> Set = keys only, no values. Both are hash-based, O(1) lookup. Use set for membership tests and uniqueness; dict for key-value mappings.

**Q4. List comprehension vs generator expression?**
> `[x for x in range(10)]` — list, all in memory.
> `(x for x in range(10))` — generator, lazy, one at a time. Memory-efficient for large data.

**Q5. What's `defaultdict`?**
> `from collections import defaultdict` — auto-initializes missing keys with a default factory. `d = defaultdict(list); d["k"].append(1)` works without checking if "k" exists.

**Q6. What's `Counter`?**
> `from collections import Counter` — dict subclass for counting. `Counter([1,2,2,3]) → {1:1, 2:2, 3:1}`. `.most_common(k)` returns top-k.

**Q7. How is a string represented in Python?**
> Sequence of Unicode code points (since Python 3). Immutable. Internally stored as one of 1/2/4 byte representations based on the widest character.

### 10.2 OOP

**Q8. What is `__init__`?**
> The initializer (commonly called constructor) called when you create an instance. Sets up initial state.

**Q9. `__init__` vs `__new__`?**
> `__new__` creates the object (rarely overridden). `__init__` initializes the already-created object (commonly overridden).

**Q10. What is `self`?**
> Reference to the instance. Passed automatically when you call a method on an instance. Convention, not keyword.

**Q11. What's inheritance?**
> A class can inherit attributes and methods from a parent class.
```python
class Dog(Animal):
    def bark(self): return "Woof"
```

**Q12. What's method resolution order (MRO)?**
> Order in which Python searches for methods in inheritance trees. Uses C3 linearization. Check with `ClassName.__mro__` or `mro()`.

**Q13. What's polymorphism?**
> Same method name behaves differently across classes. Achieved via inheritance or duck typing.

**Q14. What's encapsulation in Python?**
> Bundling data and methods. Python uses naming conventions:
> - `_var` — protected by convention
> - `__var` — name-mangled (`_ClassName__var`)
> No true private — Python prefers "we're all consenting adults."

**Q15. What are class methods, static methods, instance methods?**
> - `@staticmethod` — no `self` or `cls`, just a utility function in the class namespace
> - `@classmethod` — receives `cls` (the class itself), used for alternative constructors
> - Instance methods — receive `self`

**Q16. What's `super()`?**
> Returns a proxy to the parent class, used to call parent methods. `super().__init__(...)` in subclass.

**Q17. Abstract base classes?**
> `from abc import ABC, abstractmethod`. Define abstract methods that subclasses must implement. Enforces interface.

### 10.3 Functions

**Q18. *args and **kwargs?**
> `*args` collects positional args into a tuple; `**kwargs` collects keyword args into a dict. Lets functions accept arbitrary arguments.

**Q19. What's a lambda?**
> Anonymous one-line function. `lambda x: x * 2`. Limited to a single expression.

**Q20. map / filter / reduce?**
> `map(f, iterable)` — apply f to each. `filter(pred, iterable)` — keep where pred is True. `reduce(f, iterable, init)` — fold via accumulator.

**Q21. What's a closure?**
> A function that captures variables from its enclosing scope.
```python
def make_counter():
    count = 0
    def increment():
        nonlocal count
        count += 1
        return count
    return increment
```

**Q22. What's a decorator?**
> A function that takes another function and returns a modified version. Used for cross-cutting concerns: logging, timing, caching.
```python
@timer
def slow():
    time.sleep(1)
# Equivalent to: slow = timer(slow)
```

**Q23. How does `functools.wraps` work?**
> Preserves the original function's metadata (`__name__`, `__doc__`) when wrapping. Without it, the wrapped function looks like the wrapper.

### 10.4 Iteration & generators

**Q24. What's an iterator?**
> An object with `__iter__` and `__next__` methods. Returns next value or raises `StopIteration`.

**Q25. What's a generator?**
> A function with `yield`. Returns an iterator that produces values lazily. Memory-efficient.
```python
def fib():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b
```

**Q26. yield vs return?**
> `return` ends the function. `yield` pauses, preserves state, resumes on next call.

**Q27. What's `itertools`?**
> Stdlib module for iterator algebra: `chain`, `combinations`, `permutations`, `islice`, `groupby`, `cycle`, `accumulate`.

### 10.5 Concurrency

**Q28. What's the GIL?**
> Global Interpreter Lock — CPython lets only one thread execute bytecode at a time. Limits CPU-bound multithreading. I/O-bound threading still works (threads release GIL during I/O).

**Q29. threading vs multiprocessing vs asyncio?**
> - **threading** — multiple threads in one process, share memory, blocked by GIL for CPU work, good for I/O concurrency
> - **multiprocessing** — multiple processes, separate memory, bypasses GIL, good for CPU work
> - **asyncio** — single thread, cooperative concurrency via event loop, great for I/O at scale

**Q30. async / await?**
> `async def` declares a coroutine. `await` suspends until awaited completes. Runs on an event loop.
```python
async def fetch(url):
    async with aiohttp.ClientSession() as s:
        async with s.get(url) as r:
            return await r.text()
```

**Q31. What's a coroutine?**
> A function that can be paused and resumed. In Python: `async def` functions.

### 10.6 Context managers

**Q32. What's the `with` statement?**
> Sets up a runtime context (acquire resource, release after block). Implements `__enter__` and `__exit__`.

**Q33. How do you implement a context manager?**
> Either:
> - Class with `__enter__` and `__exit__`
> - `@contextlib.contextmanager` decorator on a generator

```python
@contextmanager
def open_file(path):
    f = open(path)
    try:
        yield f
    finally:
        f.close()
```

### 10.7 Exceptions

**Q34. try / except / else / finally?**
> - `try` — code that might raise
> - `except` — handle exception
> - `else` — run if no exception (clean separation)
> - `finally` — always run (cleanup)

**Q35. How do you raise a custom exception?**
> Inherit from `Exception`:
```python
class MyError(Exception):
    pass
raise MyError("Something broke")
```

**Q36. EAFP vs LBYL?**
> **EAFP** (Easier to Ask Forgiveness than Permission) — try/except. Pythonic.
> **LBYL** (Look Before You Leap) — check conditions before acting. C-style.
> Python prefers EAFP.

### 10.8 Memory & performance

**Q37. How does Python manage memory?**
> Reference counting + cyclic garbage collector. Each object has a ref count; when it drops to 0, object is freed. GC handles cycles.

**Q38. What's `__slots__`?**
> Declare instance attributes statically; avoids per-instance `__dict__`. Saves memory for many instances. Trade-off: no dynamic attributes.

**Q39. shallow copy vs deep copy?**
> `copy.copy(x)` — shallow: copies the object but references inside still point to originals.
> `copy.deepcopy(x)` — recursively copies everything.

**Q40. mutable default argument pitfall?**
```python
def f(items=[]):    # BAD
    items.append(1)
    return items
```
The list is shared across calls (created once at def). Fix: use `items=None`, then `items = items or []`.

### 10.9 Pythonic idioms

**Q41. What's a list comprehension?**
```python
squares = [x*x for x in range(10)]
even_squares = [x*x for x in range(10) if x % 2 == 0]
```

**Q42. enumerate? zip?**
```python
for i, x in enumerate(items): ...
for a, b in zip(list1, list2): ...
```

**Q43. Dictionary comprehension?**
```python
{k: v for k, v in pairs}
```

**Q44. Walrus operator (:=)?**
```python
# Python 3.8+
if (n := len(data)) > 10:
    print(f"Long data of length {n}")
```

**Q45. Type hints?**
```python
def greet(name: str) -> str:
    return f"Hello, {name}"
```
Not enforced at runtime; tooling like mypy checks them. Helps IDE autocomplete and catch bugs.

---

<a id="11-python-coding"></a>
## 11. Python Coding Questions

### 11.1 Pattern 1 — Array / String basics

**Two Sum**
```python
def two_sum(nums, target):
    seen = {}
    for i, x in enumerate(nums):
        if target - x in seen:
            return [seen[target - x], i]
        seen[x] = i
```

**Best Time to Buy & Sell Stock**
```python
def max_profit(prices):
    min_price = float('inf')
    profit = 0
    for p in prices:
        min_price = min(min_price, p)
        profit = max(profit, p - min_price)
    return profit
```

**Reverse a string**
```python
def reverse(s):
    return s[::-1]
```

### 11.2 Pattern 2 — HashMap

**Valid Anagram**
```python
def is_anagram(s, t):
    return Counter(s) == Counter(t)
```

**Group Anagrams**
```python
def group_anagrams(strs):
    groups = defaultdict(list)
    for s in strs:
        key = "".join(sorted(s))
        groups[key].append(s)
    return list(groups.values())
```

**First non-repeating character**
```python
def first_unique(s):
    c = Counter(s)
    for i, ch in enumerate(s):
        if c[ch] == 1:
            return i
    return -1
```

### 11.3 Pattern 3 — Sliding window

**Longest substring without repeating characters**
```python
def length_of_longest_substring(s):
    seen = {}
    left = 0
    longest = 0
    for right, ch in enumerate(s):
        if ch in seen and seen[ch] >= left:
            left = seen[ch] + 1
        seen[ch] = right
        longest = max(longest, right - left + 1)
    return longest
```

**Max sum of subarray of size k** (similar to your IBM question)
```python
def max_sum_k(nums, k):
    current = sum(nums[:k])
    best = current
    for i in range(k, len(nums)):
        current += nums[i] - nums[i-k]
        best = max(best, current)
    return best
```

### 11.4 Pattern 4 — Two pointers

**Reverse list in place**
```python
def reverse_inplace(arr):
    l, r = 0, len(arr) - 1
    while l < r:
        arr[l], arr[r] = arr[r], arr[l]
        l += 1
        r -= 1
```

**Container with most water**
```python
def max_area(heights):
    l, r = 0, len(heights) - 1
    best = 0
    while l < r:
        h = min(heights[l], heights[r])
        best = max(best, h * (r - l))
        if heights[l] < heights[r]:
            l += 1
        else:
            r -= 1
    return best
```

### 11.5 Pattern 5 — Stack

**Valid Parentheses**
```python
def is_valid(s):
    stack = []
    pairs = {')': '(', ']': '[', '}': '{'}
    for ch in s:
        if ch in '([{':
            stack.append(ch)
        else:
            if not stack or stack.pop() != pairs[ch]:
                return False
    return not stack
```

### 11.6 Pattern 6 — Heap / Priority Queue

**Top K frequent elements**
```python
def top_k(nums, k):
    counts = Counter(nums)
    return [x for x, _ in counts.most_common(k)]

# Or with heap explicitly:
import heapq
def top_k_heap(nums, k):
    counts = Counter(nums)
    return heapq.nlargest(k, counts.keys(), key=counts.get)
```

### 11.7 Pattern 7 — Recursion / DP

**Fibonacci (memoized)**
```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n < 2: return n
    return fib(n-1) + fib(n-2)
```

**Climbing stairs**
```python
def climb(n):
    a, b = 1, 1
    for _ in range(n):
        a, b = b, a + b
    return a
```

### 11.8 Pattern 8 — REST API (similar to your IBM Q2)

**Paginated API consumption template**
```python
import requests

def fetch_filtered(filter_value):
    results = []
    page = 1
    total_pages = 1
    while page <= total_pages:
        resp = requests.get("https://api.example.com/data", params={"page": page})
        data = resp.json()
        total_pages = data["total_pages"]
        for item in data["data"]:
            if item["some_field"] == filter_value:
                results.append(item)
        page += 1
    return results
```

### 11.9 Pattern 9 — Practical Python (likely in interview)

**Count word frequencies in text**
```python
from collections import Counter
text = "the quick brown fox jumps over the lazy dog"
freq = Counter(text.split())
print(freq.most_common(3))
```

**Read CSV, group by, aggregate**
```python
import csv
from collections import defaultdict

totals = defaultdict(float)
with open("data.csv") as f:
    reader = csv.DictReader(f)
    for row in reader:
        totals[row["category"]] += float(row["amount"])
```

**Flatten nested dict**
```python
def flatten(d, parent_key="", sep="."):
    items = []
    for k, v in d.items():
        new_key = f"{parent_key}{sep}{k}" if parent_key else k
        if isinstance(v, dict):
            items.extend(flatten(v, new_key, sep).items())
        else:
            items.append((new_key, v))
    return dict(items)
```

### 11.10 Practice plan

**Daily (1 hour, until interview):**
- 1 LeetCode Easy (15 min)
- 1 LeetCode Medium (30-40 min)
- 15 min reviewing patterns above

**Topics priority for IBM:**
1. HashMap / Counter
2. Sliding window
3. Two pointers
4. Stack / queue
5. API consumption / JSON parsing
6. Basic recursion

---

<a id="12-cheat-sheet"></a>
## 12. Quick Cheat Sheet

### Numbers to remember

| Topic | Number |
|---|---|
| Your experience | 2 years |
| Trade events/day | 50,000+ |
| Spizen dedup accuracy | 95% |
| Algonox accuracy improvement | 40% |
| Algonox QA reduction | 20% |
| B.Tech CGPA | 7.86 |
| Chunk size (RAG) | 500 chars |
| Chunk overlap (RAG) | 50 chars |
| Embedding dim (MiniLM) | 384 |
| Retrieval k (RAG) | 4 |
| IBM Job ID | 109351 |
| Candidate ID | 11427702 |

### Names to remember

| Item | Value |
|---|---|
| Embedding model | sentence-transformers/all-MiniLM-L6-v2 |
| LLM (current) | llama-3.1-8b-instant (Groq) |
| LLM (originally) | llama3 then llama3.2:1b (Ollama) |
| Vector store | ChromaDB |
| Framework | LangChain (LCEL) |
| API | Flask |
| Referrer | Sangeeth Pogula |

### Key formulas

```
Precision = TP / (TP + FP)
Recall    = TP / (TP + FN)
F1        = 2·P·R / (P + R)
Accuracy  = (TP + TN) / Total
FPR       = FP / (FP + TN)
Attention(Q,K,V) = softmax(QK^T / √d) · V
Cosine(a,b) = (a·b) / (||a|| ||b||)
```

### One-line answers

| Question | Answer |
|---|---|
| What is RAG? | Retrieval-Augmented Generation — retrieve relevant docs, inject as context, then generate |
| What is an embedding? | Dense vector representing semantic meaning |
| Why ChromaDB? | Easy persistence, no separate server, LangChain-native |
| Why MiniLM? | Fast, 384-d, decent quality, free, MTEB-respectable |
| Why Groq? | Free tier, sub-second latency, full Llama-3.1 quality |
| What's overfitting? | Model fits train data well but generalizes poorly |
| Why F1 over accuracy? | F1 balances precision/recall — better for imbalanced classes |
| GIL? | Global Interpreter Lock — only one thread runs Python bytecode at a time |
| What's LCEL? | Composing LangChain components via `|` operator |

---

<a id="13-revision"></a>
## 13. Final Revision Strategy

### Week before live interview

**Day 1-2: Foundations**
- Re-read Sections 1 (Transformers), 5 (ML metrics), 6 (RAG)
- Answer Section 11 Q&A out loud

**Day 3-4: Practical**
- Run your RAG project locally — be fluent demoing it
- Do 10 LeetCode mediums (Array / HashMap / Sliding Window)
- Practice explaining your project architecture in 90 seconds

**Day 5-6: IBM-specific**
- Read IBM watsonx page, Granite models, IBM Z + Telum AI
- Read `ibm_genai_prep.md` Sections 1, 2, 10, 11
- Mock interview with a friend (NOT during actual interview!)

**Day 7: Light review + rest**
- Cheat sheet only (Section 12)
- Sleep well

### Hour before interview

- Cheat sheet (Section 12) — names, numbers, formulas
- "Tell me about yourself" — say once aloud
- Project pitch — say once aloud
- Deep breath. You're ready.

### During interview

- **Listen** to the question fully before answering
- **Pause** 1-2 seconds — shows you're thinking, not rambling
- **Structure** answers: state position → evidence → trade-off
- **Stuck?** Think out loud: "Let me consider X first..."
- **Don't know?** Honestly: "I haven't worked with that. From what I understand, [say what you know]. I'd ramp up via [resource]."
- **Always ask questions back** when prompted

---

> **Build effort, not anxiety. You've done the work — show it.**

> *Last updated: May 2026 — `prep/GENAI_PREP.md`*
