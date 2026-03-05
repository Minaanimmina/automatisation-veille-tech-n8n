# Automatisation de Veille Tech avec N8N et Discord

Tu vas construire un système de veille technologique automatisé qui récupère des articles tech depuis des flux RSS et des rapports générés par IA, puis les publie automatiquement dans un salon Discord. Ce projet se déroule en deux phases progressives.

La première phase consiste à construire un workflow RSS → Discord sans IA.

La seconde introduit un LLM pour générer un rapport de veille stratégique enrichi.

L'ensemble repose sur N8N, auto-hébergé via Docker Compose sur ta machine. La méthode est le learning by doing : tu cherches, tu testes, tu itères. Aucune solution ne t'est donnée clé en main. Tu dois t'appuyer sur la documentation officielle, les logs d'exécution N8N et ta capacité à résoudre les problèmes par toi-même.

## Ressources

- [N8N — Docker](https://docs.n8n.io/hosting/installation/docker/)
- [Doc officielle N8N — Docker Compose](https://docs.n8n.io/hosting/installation/server-setups/docker-compose/)
- [N8N — Page hosting générale](https://docs.n8n.io/hosting/)
- [Discord Developer Docs — Webhook](https://discord.com/developers/docs/resources/webhook)
- [LLM : Groq](https://console.groq.com/)
- [Google AI Studio (Gemini)](https://aistudio.google.com/)
- [Mistral AI](https://console.mistral.ai/)
- [OpenRouter](https://openrouter.ai/)

## Contexte du projet

Dans un environnement tech en évolution rapide, un Data Engineer ou un AI Engineer doit être capable de maintenir une veille active et structurée. Lire manuellement des dizaines de blogs et newsletters chaque matin n'est pas scalable. L'automatisation de cette veille est donc une compétence concrète et immédiatement applicable en entreprise.

Ce brief te place dans la peau d'un ingénieur qui doit concevoir, déployer et maintenir un pipeline d'information automatisé. Tu utiliseras N8N, un outil d'automatisation de workflows open source très répandu dans les équipes data et ops, que tu hébergeras toi-même via Docker Compose.

Le projet se décompose en deux phases :

### **Phase 1 — Veille RSS sans IA**

Tu construis un workflow qui lit plusieurs flux RSS de blogs tech, filtre les articles publiés dans les dernières 24h, formate les données et les envoie dans un salon Discord sous forme de messages enrichis appelés Embeds. Ce workflow tourne automatiquement chaque matin via un déclencheur horaire.

### **Phase 2 — Veille augmentée par IA**

Une fois la Phase 1 validée, tu construis un second workflow qui utilise un LLM  pour générer automatiquement un rapport de veille. Ce rapport est parsé, structuré et envoyé dans Discord sous forme d'Embeds classés par niveau d'importance stratégique.

## Livrables

Un repo github contenant : 

- Un fichier docker-compose.yml fonctionnel permettant de lancer N8N en local
- Workflow RSS N8N exporté au format JSON
- Workflow LLM N8N exporté au format JSON
- Une capture d'écran de votre serveur discord contenant

## Critères de performance

- N8N self-hébergé
- Docker Compose fonctionnel, interface accessible, données persistantes
- Discord configuré
- Serveur créé, salon dédié
- Webhook opérationnel
- Lecture RSS
- Les 5 flux sont lus et fusionnés correctement
- Filtre temporel
- Seuls les articles < 24h sont conservés
- Format Embed Discord
- Titre cliquable, source, date et résumé présents dans chaque message
- Anti rate-limit
- Pause entre les envois Discord implémentée
- Déclencheur automatique
- Le workflow se lance seul à l'heure configurée
- Connexion OpenRouter LLM ou autre LLM connecté via les nœuds LangChain de N8N
- Prompt structuré
- La réponse du LLM est parsable et cohérente
