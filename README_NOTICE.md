# Guide d'Installation et de Configuration d'Open WebUI

Ce guide vous explique comment installer, lancer et configurer Open WebUI, une plateforme d'IA auto-hébergée, extensible et conviviale conçue pour fonctionner entièrement hors ligne.

## Table des matières

1. [Installation](#installation)
2. [Configuration des API Keys](#configuration-des-api-keys)
3. [Configuration d'OpenRouter](#configuration-dopenrouter)
4. [Utilisation des Agents](#utilisation-des-agents)

## Installation

Plusieurs méthodes d'installation sont disponibles selon vos besoins.

### Installation via Python pip

1. Assurez-vous d'utiliser **Python 3.11** pour éviter les problèmes de compatibilité.
2. Installez Open WebUI avec la commande suivante :
   ```bash
   pip install open-webui
   ```
3. Lancez Open WebUI :
   ```bash
   open-webui serve
   ```
4. Accédez à l'interface via [http://localhost:8080](http://localhost:8080)

### Installation avec Docker

#### Si Ollama est sur votre ordinateur

```bash
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

#### Si Ollama est sur un autre serveur

```bash
docker run -d -p 3000:8080 -e OLLAMA_BASE_URL=https://exemple.com -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

#### Pour utiliser Open WebUI avec support GPU Nvidia

```bash
docker run -d -p 3000:8080 --gpus all --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:cuda
```

#### Pour utiliser uniquement l'API OpenAI

```bash
docker run -d -p 3000:8080 -e OPENAI_API_KEY=votre_clé_secrète -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

#### Installation d'Open WebUI avec Ollama intégré

Avec support GPU :
```bash
docker run -d -p 3000:8080 --gpus=all -v ollama:/root/.ollama -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:ollama
```

CPU uniquement :
```bash
docker run -d -p 3000:8080 -v ollama:/root/.ollama -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:ollama
```

Après l'installation, accédez à Open WebUI via [http://localhost:3000](http://localhost:3000).

## Configuration des API Keys

Open WebUI permet d'utiliser différentes API comme OpenAI, OpenRouter, et d'autres services compatibles avec l'API OpenAI.

### Configuration via l'interface utilisateur

1. Connectez-vous à Open WebUI
2. Accédez aux paramètres (icône d'engrenage)
3. Allez dans la section "Modèles" puis "OpenAI"
4. Ajoutez votre URL d'API et votre clé API

### Configuration via les variables d'environnement

Vous pouvez également configurer les API keys en utilisant des variables d'environnement :

```bash
OPENAI_API_KEY=votre_clé_api
OPENAI_API_BASE_URL=https://api.openai.com/v1
```

Pour Docker, ajoutez ces variables avec l'option `-e` :

```bash
docker run -d -p 3000:8080 -e OPENAI_API_KEY=votre_clé_api -e OPENAI_API_BASE_URL=https://api.openai.com/v1 -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

## Configuration d'OpenRouter

[OpenRouter](https://openrouter.ai) est un service qui vous permet d'accéder à de nombreux modèles d'IA via une seule API compatible avec OpenAI.

### Configuration d'OpenRouter via l'interface utilisateur

1. Créez un compte sur [OpenRouter](https://openrouter.ai)
2. Obtenez votre clé API depuis le tableau de bord OpenRouter
3. Dans Open WebUI, accédez aux paramètres > Modèles > OpenAI
4. Ajoutez une nouvelle connexion avec :
   - URL de l'API : `https://openrouter.ai/api/v1`
   - Clé API : votre clé API OpenRouter

### Configuration d'OpenRouter via les variables d'environnement

```bash
OPENAI_API_KEY=votre_clé_openrouter
OPENAI_API_BASE_URL=https://openrouter.ai/api/v1
```

Pour Docker :

```bash
docker run -d -p 3000:8080 -e OPENAI_API_KEY=votre_clé_openrouter -e OPENAI_API_BASE_URL=https://openrouter.ai/api/v1 -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

## Utilisation des Agents

Open WebUI permet de créer et d'utiliser des agents personnalisés via son constructeur de modèles.

### Création d'un agent

1. Accédez à l'espace de travail (Workspace) > Modèles
2. Cliquez sur "Créer un modèle"
3. Configurez votre agent :
   - Nom et description
   - Image de profil
   - Modèle de base (sélectionnez un modèle Ollama ou OpenAI/OpenRouter)
   - Prompt système (instructions pour définir le comportement de l'agent)
   - Paramètres du modèle (température, top_p, etc.)

### Configuration des agents via l'interface

Les agents peuvent être configurés directement depuis l'interface utilisateur. Vous pouvez :
- Définir des instructions système spécifiques
- Configurer les paramètres du modèle
- Ajouter des outils et des capacités spécifiques

### Partage et importation d'agents

Open WebUI permet également de partager et d'importer des agents via la communauté Open WebUI. Vous pouvez :
1. Partager vos agents personnalisés avec la communauté
2. Importer des agents créés par d'autres utilisateurs
3. Modifier les agents importés selon vos besoins

## Dépannage

Si vous rencontrez des problèmes de connexion, consultez la [documentation Open WebUI](https://docs.openwebui.com/troubleshooting/) ou rejoignez la [communauté Discord Open WebUI](https://discord.gg/5rJgQTnV4s) pour obtenir de l'aide.

### Erreur de connexion au serveur Open WebUI

Si vous rencontrez des problèmes de connexion, c'est souvent parce que le conteneur Docker de WebUI ne peut pas atteindre le serveur Ollama à 127.0.0.1:11434 (host.docker.internal:11434) à l'intérieur du conteneur. Utilisez l'option `--network=host` dans votre commande Docker pour résoudre ce problème. Notez que le port change de 3000 à 8080, ce qui donne le lien : `http://localhost:8080`.

**Exemple de commande Docker** :

```bash
docker run -d --network=host -v open-webui:/app/backend/data -e OLLAMA_BASE_URL=http://127.0.0.1:11434 --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```
