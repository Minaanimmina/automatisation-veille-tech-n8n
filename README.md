# Automatisation de Veille Tech avec N8N et Discord

Projet de veille technologique automatisée utilisant N8N self-hébergé via Docker Compose. Deux workflows envoient automatiquement des articles et rapports dans un salon Discord.

## Contexte

Projet réalisé dans le cadre d'une formation **Développeur en IA** (RNCP niveau 6) et mis en application dans le cadre d'une alternance chez AFI-SA pour assurer une veille sur l'IA et le Big Data dans les bibliothèques françaises.

## Architecture

### Phase 1 — Veille RSS automatique
Lecture de 5 flux RSS tech, filtrage des articles des dernières 24h, envoi dans Discord sous forme d'Embeds.

```
Schedule Trigger (tous les jours à 9h)
      ↓
Lecture RSS x5 en parallèle
      ↓
    Merge
      ↓
  Filtre < 24h
      ↓
  Edit Fields (formatage)
      ↓
  Code (construction payload)
      ↓
HTTP POST Discord (batching anti rate-limit)
```

### Phase 2 — Rapport IA hebdomadaire
Recherche web via Tavily, analyse et sélection par Groq/Llama, envoi d'un rapport structuré dans Discord.

```
Schedule Trigger (1x par semaine)
      ↓
HTTP Request Tavily x3 en parallèle
      ↓
    Merge
      ↓
Code (prépare le prompt)
      ↓
HTTP Request Groq (llama-3.1-8b-instant)
      ↓
Code (parse JSON)
      ↓
Code (payload Discord)
      ↓
HTTP POST Discord
```

## Prérequis

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Un serveur Discord avec un salon dédié
- Une clé API [Tavily](https://app.tavily.com)
- Une clé API [Groq](https://console.groq.com)

## Installation

### **1. Clone le repo**

```bash
git clone https://github.com/ton-username/automatisation-veille-tech-n8n
cd automatisation-veille-tech-n8n
```

### **2. Lance N8N**

```bash
docker compose up -d
```

### **3. Accède à l'interface**

Ouvre `http://localhost:5678` dans ton navigateur et crée ton compte admin.

### **4. Importe les workflows**

Dans N8N : menu "..." → Import → sélectionne les fichiers JSON.

### **5. Configure les variables**

Dans chaque workflow, remplace les valeurs suivantes :
- URL Webhook Discord
- Clé API Tavily
- Clé API Groq

### **6. Active les workflows**

Clique sur "Publish" pour chaque workflow.

## Sources RSS (Phase 1)

| Source | URL |
| -------- | ----- |
| Stat4Decision | https://www.stat4decision.com/fr/feed/ |
| Blog OCTO | https://blog.octo.com/feed/ |
| Medium Data Engineering | https://medium.com/feed/tag/data-engineering |
| Practical Data Engineering | https://practicaldataengineering.substack.com/feed |
| Towards Data Science | https://towardsdatascience.com/feed |

## Requêtes Tavily (Phase 2)

- `IA bibliothèques publiques France actualités`
- `intelligence artificielle big data bibliothèques France 2026`
- `big data données bibliothèques SIGB France`

## Livrables

```
├── docker-compose.yml          # Configuration Docker N8N
├── workflow-phase1-rss.json    # Workflow RSS → Discord
├── workflow-phase2-llm.json    # Workflow Tavily + Groq → Discord
├── screenshots/                # Captures Discord
└── README.md
```

## Stack technique

| Outil | Usage |
| ------- | ------- |
| N8N | Orchestration des workflows |
| Docker Compose | Self-hosting N8N |
| Tavily | Recherche web temps réel |
| Groq / Llama 3.1 | Analyse et sélection des articles |
| Discord Webhooks | Diffusion des résultats |
