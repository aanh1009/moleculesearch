# Molecule Semantic Search

Natural‑language and structure‑aware search engine for small‑molecule libraries. Type a phrase like **“beta‑lactam antibiotic with carboxylate”** or paste a **SMILES** string, and get the closest compounds ranked by semantic similarity—with an interactive force‑graph to explore neighbourhoods.

> **Live demo →** <https://molecular-search.vercel.app>

---

## ✨  Why it’s cool

| Feature | Details |
|---------|---------|
| **Dual‑mode search** | Accepts free‑text *or* SMILES; backend embeds both into the *same* vector space so you can mix chemistry and plain English. |
| **Vector DB** | Uses **Pinecone** for sub‑second K‑NN over >50 k molecules (adjust size via env). |
| **OpenAI/Instructor‑XL embeddings** | Molecular descriptions are embedded once and cached; queries embedded on the fly. |
| **React + Tailwind UI** | Minimalistic search bar, result table, and **react‑force‑graph** visualisation. |
| **Express API** | Lightweight Node server (`/api/search`, `/api/graph/:id`). |
| **Reproducible pipeline** | `biology.ipynb` and `script.py` show how the index is built with **nomic‑atlas**. |

---

## 🏗️  Architecture

```txt
                ┌──────────────────────┐
                │   React front‑end    │
                │  (Vite + Tailwind)   │
                └───────▲───────┬──────┘
                        │search│graph
                        ▼       │
                ┌──────────────────────┐
                │   Express REST API   │
                └───────▲───────┬──────┘
                        │embed  │K‑NN
                        ▼       │
             ┌────────────────────────────┐
             │  Pinecone vector index     │
             └────────────────────────────┘
```

---

##  🚀  Quick start (local dev)

### 1 · Clone & install

```bash
git clone https://github.com/aanh1009/molecule-semantic-search.git
cd molecule-semantic-search
npm install            # installs both FE + BE deps
```

### 2 · Set env vars

Create `.env.local` (read by both CRA & Express):

```env
# Pinecone
PINECONE_API_KEY=xxx
PINECONE_ENV=us-east4-gcp
PINECONE_INDEX=molecules

# Embeddings
OPENAI_API_KEY=sk-...
EMBED_MODEL=text-embedding-3-large   # or use instructor-xl in script.py

# Server
PORT=5000   # Express
REACT_APP_API_URL=http://localhost:5000
```

### 3 · Import the dataset (optional)

If you want your own set:

```bash
python script.py          # reads description_df.pkl → upserts to Pinecone
```

### 4 · Run dev servers

```bash
# terminal 1 – API
auth0 login   # if you guard API, else skip
node server.js

# terminal 2 – React client
npm start     # CRA runs on :3000 and proxies /api → :5000
```

Open <http://localhost:3000> and start searching.

---

##  🔌 API Reference

| Method | Endpoint | Body | Description |
|--------|----------|------|-------------|
| `POST` | `/api/search` | `{ "query": "penicillin" , "topK": 10 }` | Returns top‑`k` matches from Pinecone with metadata. |
| `GET`  | `/api/graph/:id` | – | Returns nearest‑neighbour IDs for force‑graph expansion. |

---

##  🗂  Folder structure

```
.
├─ public/               # favicon, index.html
├─ src/                  # React code (components/, hooks/, styles/)
│  ├─ api/               # axios wrappers
│  ├─ pages/             # Home.jsx, About.jsx
│  └─ graph/             # ForceGraph.jsx
├─ server.js             # Express entry
├─ script.py             # Index‑building utility
├─ biology.ipynb         # Exploratory data prep
└─ tailwind.config.js
```

---

##  🛣️  Roadmap

- [ ] Enable 3‑D conformer visualisation via **NGL Viewer**.
- [ ] Add filters (molecular weight, logP, etc.).
- [ ] Switch to **pgvector** Postgres alternative for self‑hosting.
- [ ] Auth (Clerk) to save favourite molecules.

---

##  🤝  Contributing

PRs welcome! Please open an issue first to discuss what you plan to change.

###  Development guidelines

* 2‑space indent, Prettier enforced.
* Use semantic commit messages (`feat:`, `fix:`, etc.).
* Keep components atomic, co‑locate styles.

---

##  🔒  License

MIT © 2025 Tuan Anh Ngo

