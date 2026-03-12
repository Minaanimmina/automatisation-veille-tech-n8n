# Automatisation de Veille Tech avec N8N et Discord

Projet de veille technologique automatisée utilisant N8N self-hébergé via Docker Compose. Trois phases progressives de complexité croissante.

## Contexte

Projet réalisé dans le cadre d'une formation Développeur en IA (RNCP niveau 6) et mis en application en alternance chez AFI pour assurer une veille sur l'IA et le Big Data dans les bibliothèques françaises.

## Architecture

### Phase 1 — Veille RSS quotidienne

Lecture de 5 flux RSS tech, filtrage des articles des dernières 24h, envoi dans Discord.

**Workflow** : `Workflow RSS/Phase 1.json`
```
Schedule (9h daily)
      ↓
RSS x5 (parallèle)
      ↓
Merge → Filtre < 24h
      ↓
Format → Discord Webhook
```

**Résultats** : ~15-25 articles/jour

---

### Phase 2 — Rapport IA hebdomadaire (Discord)

Recherche web via Tavily, analyse par Groq/Llama, envoi Discord.

**Workflow** : `Workflow LLM/Phase 2.json`
```
Schedule (hebdo)
      ↓
Tavily x3 (parallèle)
      ↓
Merge → Groq (Llama 3.1 8B)
      ↓
Parse JSON → Discord Webhook
```

**Résultats** : ~5 articles/semaine  
**Livrable école** : Ce workflow constitue le rendu de la formation

---

### Phase 3 — Veille IA optimisée (Obsidian)

Version améliorée avec scoring, déduplication et archivage structuré.

**Workflow** : `Workflow Obsidian/Phase 3.json`
```
Schedule (hebdo)
      ↓
Tavily x4 (angles thématiques ciblés)
      ↓
Merge → Déduplication (Levenshtein)
      ↓
Groq (Llama 3.3 70B) : scoring + catégorisation
      ↓
Generate Markdown (frontmatter YAML)
      ↓
Write to Obsidian vault
```

**Résultats** : ~5-8 articles/semaine (score ≥ 6/10)

---

## Prérequis

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Un serveur Discord avec salon dédié
- Clé API [Tavily](https://app.tavily.com) (gratuit : 1000 req/mois)
- Clé API [Groq](https://console.groq.com) (gratuit)
- [Obsidian](https://obsidian.md) (pour Phase 3)

## Installation

### 1. Clone le repo

```bash
git clone https://github.com/ton-username/automatisation-veille-tech-n8n
cd automatisation-veille-tech-n8n
```

### 2. Configure les variables d'environnement

```bash
# Copie le template
cp .env.example .env

# Édite .env avec tes valeurs
nano .env
```

**Contenu de `.env`** :
```bash
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=ton_mot_de_passe
OBSIDIAN_PATH=/chemin/vers/ton/vault/obsidian
```

### 3. Lance N8N
```bash
docker compose up -d
```

### 4. Accède à N8N

Ouvre `http://localhost:5678` et connecte-toi avec les credentials de `.env`

### 5. Importe les workflows

- Menu "..." → Import from File
- Sélectionne les workflows JSON dans :
  - `Workflow RSS/Phase 1.json`
  - `Workflow LLM/Phase 2.json`
  - `Workflow Obsidian/Phase 3.json`

### 6. Configure les API keys

Dans chaque workflow :
- Double-clic sur chaque nœud des APIs (Tavily, Groq, Discord)
- Discord Webhook URL (Phases 1 & 2)
- Tavily API Key (Phases 2 & 3)
- Groq API Key (Phases 2 & 3)

### 7. Active les workflows souhaités

- **Phase 1** : Veille RSS quotidienne (production)
- **Phase 2** : Veille IA Discord (rendu école - CONSERVÉ)
- **Phase 3** : Veille IA Obsidian (production AFI-SA)

## Sources et requêtes

### Phase 1 : Flux RSS

| Source | URL |
|--------|-----|
| Stat4Decision | https://www.stat4decision.com/fr/feed/ |
| Blog OCTO | https://blog.octo.com/feed/ |
| Medium Data Eng | https://medium.com/feed/tag/data-engineering |
| Practical Data Eng | https://practicaldataengineering.substack.com/feed |
| Towards Data Science | https://towardsdatascience.com/feed |

### Phase 2 : Requêtes Tavily

1. `IA bibliothèques publiques France actualités`
2. `intelligence artificielle big data bibliothèques France 2026`
3. `big data données bibliothèques SIGB France`

### Phase 3 : Requêtes Tavily optimisées

1. **Projets** : `IA chatbot SIGB bibliothèque France déploiement 2025 2026`
2. **Analytics** : `analytics données usage Matomo bibliothèques tableau de bord France`
3. **ML/Recherche** : `machine learning prédiction recommandation bibliothèques`
4. **International** : `open source AI libraries digital transformation Europe`

## Stack technique

| Outil | Usage | Phase |
|-------|-------|-------|
| N8N | Orchestration workflows | 1, 2, 3 |
| Docker Compose | Self-hosting N8N | 1, 2, 3 |
| RSS | Flux tech généralistes | 1 |
| Tavily | Recherche web IA | 2, 3 |
| Groq | LLM analysis | 2, 3 |
| Discord Webhooks | Notifications | 1, 2 |
| Obsidian | Knowledge management | 3 |

## Structure du projet

```
├── Workflow RSS/
│   ├── Phase 1.json               # Workflow veille RSS quotidienne
│   └── Phase 1.png                # Capture du workflow
│
├── Workflow LLM/
│   ├── Phase 2.json               # Workflow veille IA Discord (rendu école)
│   └── Phase 2.png                # Capture du workflow
│
├── Workflow Obsidian/
│   ├── Phase 3.json               # Workflow veille IA Obsidian (production)
│   └── Phase 3.png                # Capture du workflowts/
│
├── .env.example                    # Template variables d'environnement
├── docker-compose.yml              # Configuration Docker
├── Brief.md                        # Cahier des charges
└── README.md                       # Documentation
```

---

## Licence

MIT — Usage libre pour formation et projets personnels