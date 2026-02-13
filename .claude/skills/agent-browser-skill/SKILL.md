---
name: agent-browser
description: Automatise les interactions du navigateur pour les tests web, le remplissage de formulaires, les captures d'écran et l'extraction de données. Utilisez ceci lorsque l'utilisateur a besoin de naviguer sur des sites web, d'interagir avec des pages web, de remplir des formulaires, de prendre des captures d'écran, de tester des applications web ou d'extraire des informations de pages web.
allowed-tools: Bash(agent-browser:*)
---

# Automatisation du Navigateur avec agent-browser

## Démarrage rapide

```bash
agent-browser open <url>        # Naviguer vers la page
agent-browser snapshot -i       # Obtenir les éléments interactifs avec refs
agent-browser click @e1         # Cliquer sur l'élément par ref
agent-browser fill @e2 "text"   # Remplir l'entrée par ref
agent-browser close             # Fermer le navigateur
```

## Flux de travail principal

1. Naviguer : `agent-browser open <url>`
2. Instantané : `agent-browser snapshot -i` (renvoie les éléments avec refs comme `@e1`, `@e2`)
3. Interagir en utilisant les refs de l'instantané
4. Re-prendre un instantané après la navigation ou des changements DOM significatifs

## Commandes

### Navigation
```bash
agent-browser open <url>      # Naviguer vers l'URL
agent-browser back            # Retourner en arrière
agent-browser forward         # Aller en avant
agent-browser reload          # Recharger la page
agent-browser close           # Fermer le navigateur
```

### Instantané (analyse de page)
```bash
agent-browser snapshot            # Arbre d'accessibilité complet
agent-browser snapshot -i         # Éléments interactifs uniquement (recommandé)
agent-browser snapshot -c         # Sortie compacte
agent-browser snapshot -d 3       # Limiter la profondeur à 3
agent-browser snapshot -s "#main" # Portée au sélecteur CSS
```

### Interactions (utiliser @refs de l'instantané)
```bash
agent-browser click @e1           # Cliquer
agent-browser dblclick @e1        # Double-cliquer
agent-browser focus @e1           # Focus sur l'élément
agent-browser fill @e2 "text"     # Effacer et taper
agent-browser type @e2 "text"     # Taper sans effacer
agent-browser press Enter         # Appuyer sur une touche
agent-browser press Control+a     # Combinaison de touches
agent-browser keydown Shift       # Maintenir la touche enfoncée
agent-browser keyup Shift         # Relâcher la touche
agent-browser hover @e1           # Survoler
agent-browser check @e1           # Cocher la case
agent-browser uncheck @e1         # Décocher la case
agent-browser select @e1 "value"  # Sélectionner dans la liste déroulante
agent-browser scroll down 500     # Faire défiler la page
agent-browser scrollintoview @e1  # Faire défiler l'élément dans la vue
agent-browser drag @e1 @e2        # Glisser-déposer
agent-browser upload @e1 file.pdf # Télécharger des fichiers
```

### Obtenir des informations
```bash
agent-browser get text @e1        # Obtenir le texte de l'élément
agent-browser get html @e1        # Obtenir le innerHTML
agent-browser get value @e1       # Obtenir la valeur de l'entrée
agent-browser get attr @e1 href   # Obtenir l'attribut
agent-browser get title           # Obtenir le titre de la page
agent-browser get url             # Obtenir l'URL actuelle
agent-browser get count ".item"   # Compter les éléments correspondants
agent-browser get box @e1         # Obtenir la boîte englobante
```

### Vérifier l'état
```bash
agent-browser is visible @e1      # Vérifier si visible
agent-browser is enabled @e1      # Vérifier si activé
agent-browser is checked @e1      # Vérifier si coché
```

### Captures d'écran et PDF
```bash
agent-browser screenshot          # Capture d'écran vers stdout
agent-browser screenshot path.png # Enregistrer dans un fichier
agent-browser screenshot --full   # Page entière
agent-browser pdf output.pdf      # Enregistrer en PDF
```

### Enregistrement vidéo
```bash
agent-browser record start ./demo.webm    # Démarrer l'enregistrement (utilise l'URL actuelle + état)
agent-browser click @e1                   # Clé d'actions
agent-browser record stop                 # Arrêter et enregistrer la vidéo
agent-browser record restart ./take2.webm # Arrêter l'actuel + démarrer un nouvel enregistrement
```
L'enregistrement crée un nouveau contexte mais préserve les cookies/stockage de votre session. Si aucune URL n'est fournie, il revient automatiquement à votre page actuelle. Pour des démos fluides, explorez d'abord, puis commencez l'enregistrement.

### Attendre
```bash
agent-browser wait @e1                     # Attendre l'élément
agent-browser wait 2000                    # Attendre millisecondes
agent-browser wait --text "Success"        # Attendre le texte
agent-browser wait --url "**/dashboard"    # Attendre le modèle d'URL
agent-browser wait --load networkidle      # Attendre l'inactivité réseau
agent-browser wait --fn "window.ready"     # Attendre la condition JS
```

### Contrôle de la souris
```bash
agent-browser mouse move 100 200      # Déplacer la souris
agent-browser mouse down left         # Appuyer sur le bouton
agent-browser mouse up left           # Relâcher le bouton
agent-browser mouse wheel 100         # Molette de défilement
```

### Localisateurs sémantiques (alternative aux refs)
```bash
agent-browser find role button click --name "Submit"
agent-browser find text "Sign In" click
agent-browser find label "Email" fill "user@test.com"
agent-browser find first ".item" click
agent-browser find nth 2 "a" text
```

### Paramètres du navigateur
```bash
agent-browser set viewport 1920 1080      # Définir la taille de la fenêtre
agent-browser set device "iPhone 14"      # Émuler un appareil
agent-browser set geo 37.7749 -122.4194   # Définir la géolocalisation
agent-browser set offline on              # Basculer le mode hors ligne
agent-browser set headers '{"X-Key":"v"}' # En-têtes HTTP supplémentaires
agent-browser set credentials user pass   # Authentification HTTP basic
agent-browser set media dark              # Émuler le schéma de couleurs
```

### Cookies et Stockage
```bash
agent-browser cookies                     # Obtenir tous les cookies
agent-browser cookies set name value      # Définir un cookie
agent-browser cookies clear               # Effacer les cookies
agent-browser storage local               # Obtenir tout le localStorage
agent-browser storage local key           # Obtenir une clé spécifique
agent-browser storage local set k v       # Définir une valeur
agent-browser storage local clear         # Tout effacer
```

### Réseau
```bash
agent-browser network route <url>              # Intercepter les requêtes
agent-browser network route <url> --abort      # Bloquer les requêtes
agent-browser network route <url> --body '{}'  # Simuler une réponse
agent-browser network unroute [url]            # Supprimer les routes
agent-browser network requests                 # Voir les requêtes suivies
agent-browser network requests --filter api    # Filtrer les requêtes
```

### Onglets et Fenêtres
```bash
agent-browser tab                 # Lister les onglets
agent-browser tab new [url]       # Nouvel onglet
agent-browser tab 2               # Basculer vers l'onglet
agent-browser tab close           # Fermer l'onglet
agent-browser window new          # Nouvelle fenêtre
```

### Cadres (Frames)
```bash
agent-browser frame "#iframe"     # Basculer vers l'iframe
agent-browser frame main          # Retour au cadre principal
```

### Dialogues
```bash
agent-browser dialog accept [text]  # Accepter le dialogue
agent-browser dialog dismiss        # Rejeter le dialogue
```

### JavaScript
```bash
agent-browser eval "document.title"   # Exécuter JavaScript
```

## Exemple : Soumission de formulaire

```bash
agent-browser open https://example.com/form
agent-browser snapshot -i
# La sortie montre : textbox "Email" [ref=e1], textbox "Password" [ref=e2], button "Submit" [ref=e3]

agent-browser fill @e1 "user@example.com"
agent-browser fill @e2 "password123"
agent-browser click @e3
agent-browser wait --load networkidle
agent-browser snapshot -i  # Vérifier le résultat
```

## Exemple : Authentification avec état sauvegardé

```bash
# Se connecter une fois
agent-browser open https://app.example.com/login
agent-browser snapshot -i
agent-browser fill @e1 "username"
agent-browser fill @e2 "password"
agent-browser click @e3
agent-browser wait --url "**/dashboard"
agent-browser state save auth.json

# Sessions ultérieures : charger l'état sauvegardé
agent-browser state load auth.json
agent-browser open https://app.example.com/dashboard
```

## Sessions (navigateurs parallèles)

```bash
agent-browser --session test1 open site-a.com
agent-browser --session test2 open site-b.com
agent-browser session list
```

## Sortie JSON (pour l'analyse)

Ajoutez `--json` pour une sortie lisible par machine :
```bash
agent-browser snapshot -i --json
agent-browser get text @e1 --json
```

## Débogage

```bash
agent-browser open example.com --headed              # Afficher la fenêtre du navigateur
agent-browser console                                # Voir les messages de la console
agent-browser errors                                 # Voir les erreurs de la page
agent-browser record start ./debug.webm   # Enregistrer depuis la page actuelle
agent-browser record stop                            # Enregistrer l'enregistrement
agent-browser open example.com --headed  # Afficher la fenêtre du navigateur
agent-browser --cdp 9222 snapshot        # Se connecter via CDP
agent-browser console                    # Voir les messages de la console
agent-browser console --clear            # Effacer la console
agent-browser errors                     # Voir les erreurs de la page
agent-browser errors --clear             # Effacer les erreurs
agent-browser highlight @e1              # Mettre en évidence l'élément
agent-browser trace start                # Démarrer l'enregistrement de la trace
agent-browser trace stop trace.zip       # Arrêter et enregistrer la trace
```