# Ralph Wiggum : Développement Autonome pour Preuves de Concept

> **Crédit :** Ce guide s'inspire du [Guide Ralph Wiggum de JeredBlu](https://github.com/JeredBlu/guides/blob/main/Ralph_Wiggum_Guide.md). Je l'ai adapté et étendu pour répondre à mon flux de travail spécifique utilisant la commande `/create-prd` et le CLI agent-browser de Vercel.

---

## Qu'est-ce que Ralph Wiggum ?

Ralph Wiggum est une méthode pour exécuter Claude Code dans une boucle autonome continue. Chaque itération s'exécute dans une **nouvelle fenêtre contextuelle**, permettant à l'agent de traiter une série de tâches jusqu'à l'achèvement sans surcharge du contexte.

### Quand utiliser Ralph Wiggum

**Idéal pour :**
- Démarrer des projets à partir de zéro
- Construire des preuves de concept (POCs) avec une portée clairement définie
- Le développement "greenfield" où les exigences sont bien comprises
- Les projets où vous pouvez définir "terminé" avec précision

**Pas idéal pour :**
- Les bases de code existantes complexes
- Le "vibe coding" ou le travail exploratoire sans objectifs clairs
- Les projets avec des exigences ambiguës
- Les situations nécessitant de fréquents jugements humains

L'idée clé : **Ralph Wiggum fonctionne mieux lorsque vous avez un plan clair.** Si vous ne savez pas exactement ce que vous construisez, arrêtez-vous et déterminez-le d'abord. La commande `/create-prd` vous aide à faire exactement cela.

---

## Pourquoi pas le plugin Anthropic ?

Le plugin officiel Ralph Wiggum pour Claude Code a un défaut fondamental : il exécute tout dans une **seule fenêtre contextuelle**. Cela signifie :

- Le contexte devient surchargé avec le temps
- Risque accru d'hallucination à mesure que le contexte se remplit
- Pas de véritable séparation entre les itérations
- Vous devrez peut-être compacter manuellement à mi-parcours

La méthode de boucle bash (`ralph.sh`) démarre une **nouvelle fenêtre contextuelle** pour chaque itération. C'est la bonne approche pour les tâches autonomes de longue durée.

---

## Prérequis

### 1. Claude Code

Vous avez besoin de Claude Code installé et configuré. Voir la [documentation officielle](https://docs.anthropic.com/claude-code) pour l'installation.

### 2. Vercel Agent Browser CLI

Installez le CLI agent-browser pour l'automatisation du navigateur headless :

```bash
npm install -g agent-browser
agent-browser install  # Télécharge Chromium
```

Sur Linux, incluez les dépendances système :
```bash
agent-browser install --with-deps
```

Cet outil permet à Claude de vérifier son travail visuellement en prenant des captures d'écran et en interagissant avec votre application en cours d'exécution.

### 3. Fichiers de Configuration du Projet

Ce dépôt inclut tout ce dont vous avez besoin :
- `.claude/settings.json` - Configuration de la sandbox et des permissions
- `.claude/commands/create-prd.md` - La commande `/create-prd`
- `.claude/skills/agent-browser-skill/SKILL.md` - Instructions pour agent browser
- `PROMPT.md` - Modèle pour la boucle Ralph
- `ralph.sh` - Le script de boucle bash
- `activity.md` - Modèle de journal d'activité
- `screenshots/` - Répertoire pour la vérification visuelle

---

## Le Processus

### Étape 1 : Créez Votre PRD avec `/create-prd`

Exécutez la commande `/create-prd` dans Claude Code :

```
/create-prd
```

Cette commande interactive va :

1. **Poser des questions de découverte** une par une :
   - Quel problème résolvez-vous ?
   - Qui est votre public cible ?
   - Quelles sont les 3-5 fonctionnalités principales ?
   - Quelle stack technique voulez-vous ?
   - Quelle approche architecturale ?
   - Préférences UI/UX ?
   - Exigences d'authentification ?
   - Intégrations tierces ?
   - Critères de succès ?

2. **Rechercher des options** si vous n'êtes pas sûr de la stack technique ou de l'architecture

3. **Générer `prd.md`** avec :
   - Exigences complètes du projet
   - Décisions sur la stack technique
   - Liste de tâches JSON avec des tâches atomiques et vérifiables

4. **Mettre à jour `PROMPT.md`** avec vos spécificités :
   - Commandes de démarrage pour votre stack technique
   - Commandes de build/lint
   - Instructions spécifiques au projet

5. **Mettre à jour `.claude/settings.json`** avec les permissions pour :
   - Outils CLI requis par votre stack technique
   - Tout CLI tiers nécessaire
   - Commandes spécifiques à votre projet

6. **Créer/vérifier `activity.md`** pour consigner les progrès

### Étape 2 : Vérifiez Votre Configuration

Une fois `/create-prd` terminé, **vérifiez ces fichiers avant de lancer la boucle** :

**Vérifiez `prd.md` :**
- Toutes les fonctionnalités sont-elles capturées dans la liste des tâches ?
- Les tâches sont-elles atomiques (réalisables en une seule itération) ?
- Les tâches sont-elles dans le bon ordre de dépendance ?
- Les critères de succès sont-ils clairs ?

**Vérifiez `PROMPT.md` :**
- La commande de démarrage est-elle correcte pour votre stack technique ?
- Les commandes de build/lint sont-elles précises ?

**Vérifiez `.claude/settings.json` :**
- Tous les outils CLI nécessaires sont-ils autorisés ?
- Les fichiers sensibles sont-ils correctement refusés ?
- L'agent a-t-il ce dont il a besoin pour travailler de manière autonome ?

Cette étape de vérification est **critique**. La qualité de votre exécution Ralph Wiggum dépend entièrement de la qualité de votre PRD et de votre configuration.

### Étape 3 : Lancez la Boucle Ralph

Une fois vérifié, démarrez la boucle autonome :

```bash
./ralph.sh 20
```

Le nombre est votre maximum d'itérations. Commencez avec 10-20 pour les petits projets.

Le script va :
1. Lire `PROMPT.md` et le fournir à Claude
2. Claude travaille sur une tâche de `prd.md`
3. Vérifie avec agent-browser
4. Met à jour le statut de la tâche et consigne dans `activity.md`
5. Valide le changement
6. Répète avec une nouvelle fenêtre contextuelle

La boucle se termine lorsque :
- Toutes les tâches ont `"passes": true` (affiche `<promise>COMPLETE</promise>`)
- Le nombre maximum d'itérations est atteint

### Étape 4 : Surveillez les Progrès

Pendant que Ralph s'exécute, vous pouvez surveiller :

- **`activity.md`** - Journal détaillé de ce qui a été accompli à chaque itération
- **`screenshots/`** - Vérification visuelle de chaque tâche terminée
- **Git commits** - Un commit par tâche avec des messages clairs
- **Sortie du terminal** - Progrès en temps réel

---

## Référence des Fichiers

### `prd.md` (Généré)

Votre Document des Exigences Produit avec une liste de tâches JSON :

```json
[
  {
    "category": "setup",
    "description": "Initialize Next.js project with TypeScript",
    "steps": [
      "Run create-next-app with TypeScript template",
      "Install additional dependencies",
      "Verify dev server starts"
    ],
    "passes": false
  }
]
```

Les tâches doivent être :
- **Atomiques** : Une unité logique de travail
- **Vérifiables** : Critères de succès clairs
- **Ordonnées** : Respectent les dépendances
- **Catégorisées** : setup, feature, integration, styling, testing

### `PROMPT.md`

Instructions pour chaque itération. Fait référence à `@prd.md` et `@activity.md`. Mis à jour par `/create-prd` avec vos commandes de démarrage spécifiques.

### `.claude/settings.json`

Configuration des permissions et de la sandbox. Mis à jour par `/create-prd` en fonction de votre stack technique pour s'assurer que l'agent peut exécuter toutes les commandes nécessaires.

Exemple de permissions qui pourraient être ajoutées en fonction de votre PRD :
- `Bash(prisma:*)` pour Prisma CLI
- `Bash(supabase:*)` pour Supabase CLI
- `Bash(firebase:*)` pour Firebase CLI
- `Bash(vercel:*)` pour Vercel CLI
- `Bash(docker compose:*)` pour les workflows Docker

### `ralph.sh`

Le script de boucle bash. Fonctionnalités clés :
- Nouvelle fenêtre contextuelle par itération
- Détection de l'achèvement via `<promise>COMPLETE</promise>`
- Validation de l'existence des fichiers
- Sortie avec code couleur
- Gestion gracieuse du nombre maximum d'itérations

### `activity.md`

Journal d'activité où l'agent enregistre :
- Date et heure
- Tâche travaillée
- Changements effectués
- Commandes lancées
- Nom du fichier de capture d'écran
- Problèmes et résolutions

---

## Bonnes Pratiques

### 1. Planifiez Minutieusement

La commande `/create-prd` existe parce que **la planification est tout**. Ne sautez pas les questions de découverte. Ne les bâclez pas. Un PRD bien défini fait la différence entre une exécution Ralph réussie et des crédits API gaspillés.

### 2. Gardez une Portée Restreinte

Ralph Wiggum est pour les preuves de concept, pas pour les applications de production. Définissez la version minimale viable de votre idée. Vous pourrez toujours itérer plus tard.

### 3. Vérifiez Avant de Lancer

Examinez toujours `prd.md`, `PROMPT.md`, et `.claude/settings.json` avant de démarrer la boucle. Repérer les problèmes ici permet d'économiser des itérations.

### 4. Commencez avec Moins d'Itérations

Utilisez 10-20 itérations au début. Vous pouvez toujours en exécuter plus si nécessaire. Cela évite les coûts incontrôlés si quelque chose ne va pas.

### 5. Surveillez les Premières Itérations

Regardez les 2-3 premières itérations pour vous assurer que tout fonctionne correctement. Vérifiez que :
- Le serveur de dev démarre correctement
- Agent-browser peut accéder à localhost
- Les tâches sont marquées comme passées correctement
- Le journal d'activité est mis à jour

### 6. Utilisez la Sandbox

Le fichier `.claude/settings.json` par défaut active la sandbox. Cela fournit une isolation pour les tâches autonomes de longue durée. Ne le désactivez pas à moins d'avoir une raison spécifique.

---

## Dépannage

### L'agent reste bloqué dans une boucle
- Vérifiez si la tâche est trop ambiguë
- Examinez `activity.md` pour voir ce qu'il tente de faire
- Envisagez de diviser la tâche en étapes plus petites

### L'agent ne peut pas accéder à localhost
- Vérifiez que le serveur de dev est en cours d'exécution
- Vérifiez que le port dans `PROMPT.md` correspond à votre port réel
- Assurez-vous qu'agent-browser est installé correctement

### Erreurs de permissions
- Examinez `.claude/settings.json`
- Ajoutez les outils CLI manquants à la liste des autorisations
- Vérifiez que le modèle de commande correspond (par exemple, `Bash(npm run:*)`)

### Problèmes de contexte / hallucinations
- Cela ne devrait pas se produire avec la boucle bash (nouveau contexte à chaque itération)
- Si vous utilisez le plugin (non recommandé), passez à `ralph.sh`

### Itérations max atteintes sans achèvement
- Examinez les tâches restantes dans `prd.md`
- Vérifiez si les tâches sont trop grandes ou ambiguës
- Relancez avec `./ralph.sh 30` ou plus d'itérations

---

## Liens

- [Guide original Ralph Wiggum par JeredBlu](https://github.com/JeredBlu/guides/blob/main/Ralph_Wiggum_Guide.md)
- [Vercel Agent Browser CLI](https://github.com/vercel-labs/agent-browser)
- [Documentation Claude Code](https://docs.anthropic.com/claude-code)
- [Article de blog Anthropic sur les Agents de Longue Durée](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
