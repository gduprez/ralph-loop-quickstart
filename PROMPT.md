@prd.md @activity.md

Nous construisons le projet selon le PRD dans ce dépôt.

Lisez d'abord activity.md pour voir ce qui a été accompli récemment.

## Démarrer l'Application

<!-- START_COMMAND_PLACEHOLDER: Mettez à jour cette section après la création de votre PRD -->
Démarrez le site localement. Utilisez la commande appropriée pour votre stack technique :
- `npm run dev` (pour Next.js, Vite, ou similaire)
- `pnpm dev` (si vous utilisez pnpm)
- `bun dev` (si vous utilisez Bun)
- `python3 -m http.server 8000 --bind 127.0.0.1` (pour HTML statique)

Si le port est pris, essayez un autre port.
<!-- END_START_COMMAND_PLACEHOLDER -->

## Travailler sur les Tâches

Ouvrez prd.md et trouvez la tâche unique de plus haute priorité où `"passes": false`.

Travaillez sur exactement UNE tâche :
1. Implémentez le changement selon les étapes de la tâche
2. Exécutez toutes les vérifications disponibles :
   - `npm run lint` (si disponible)
   - `npm run typecheck` (si disponible)
   - `npm run build` (si disponible)

## Vérifier dans le Navigateur

Après l'implémentation, utilisez agent-browser pour vérifier votre travail :

1. Ouvrez l'URL du serveur local :
   ```
   agent-browser open http://localhost:3000
   ```

2. Prenez un instantané pour voir la structure de la page :
   ```
   agent-browser snapshot -i -c
   ```

3. Prenez une capture d'écran pour la vérification visuelle :
   ```
   agent-browser screenshot screenshots/[nom-de-la-tache].png
   ```

4. Vérifiez s'il y a des erreurs dans la console ou des problèmes de mise en page

5. Si la tâche implique des éléments interactifs, testez-les :
   ```
   agent-browser click "[selecteur]"
   agent-browser fill "[selecteur]" "valeur test"
   ```

## Consigner les Progrès

Ajoutez une entrée datée dans activity.md décrivant :
- Ce que vous avez changé
- Les commandes que vous avez exécutées
- Le nom du fichier de capture d'écran
- Tout problème rencontré et comment vous l'avez résolu

## Mettre à Jour le Statut de la Tâche

Lorsque la tâche est confirmée comme fonctionnelle, mettez à jour le champ `"passes"` de cette tâche dans prd.md de `false` à `true`.

## Valider les Changements

Faites un seul commit git pour cette tâche uniquement avec un message clair et descriptif :
```
git add .
git commit -m "feat: [brève description de ce qui a été implémenté]"
```

N'exécutez PAS `git init`, ne changez PAS les remotes git, et ne faites PAS de push.

## Règles Importantes

- Travaillez UNIQUEMENT sur une SEULE tâche par itération
- Vérifiez toujours dans le navigateur avant de marquer une tâche comme réussie
- Consignez toujours vos progrès dans activity.md
- Validez toujours après avoir terminé une tâche

## Achèvement

Lorsque TOUTES les tâches ont `"passes": true`, affichez :

<promise>COMPLETE</promise>
