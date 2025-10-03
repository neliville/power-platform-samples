# 🧩 Exemple de composant : Boîte de dialogue personnalisée

Cette documentation décrit comment créer une **boîte de dialogue réutilisable**
dans Power Apps (Canvas). Une boîte de dialogue permet d’afficher un
message ou de demander une confirmation à l’utilisateur dans une
interface modale.

## Objectif

Fournir un composant prêt à l’emploi qui :

- se superpose au contenu de l’application ;
- affiche un titre et un message personnalisables ;
- expose deux boutons (OK/Annuler) dont les actions sont définies
  depuis l’extérieur du composant ;
- peut être appelé depuis n’importe quel écran sans dupliquer le code.

## Structure du composant

Le composant se compose des éléments suivants :

1. **Rectangle d’arrière‑plan** avec une légère transparence
   (propriété `Fill` à `RGBA(0,0,0,0.5)`) pour assombrir l’écran.
2. **Carte de contenu** (Rectangle blanc) avec bords arrondis et
   ombre portée. Cette carte contient :
   - un **Label** pour le titre ;
   - un **Label** pour le message ;
   - deux **Boutons** (OK et Annuler).

## Propriétés personnalisées

Pour rendre le composant réutilisable, exposez les propriétés suivantes :

| Propriété      | Type      | Description                                      |
|---------------|-----------|--------------------------------------------------|
| `Visible`     | Booléen   | Contrôle l’affichage de la boîte de dialogue.    |
| `Title`       | Texte     | Texte du titre affiché en haut de la boîte.     |
| `Message`     | Texte     | Texte du message principal.                      |
| `OnOk`        | Comportem.| Action à exécuter lorsque l’utilisateur confirme. |
| `OnCancel`    | Comportem.| Action à exécuter en cas d’annulation.           |

> **Remarque :** Dans le studio Power Apps, vous pouvez ajouter ces
> propriétés via le menu **Advanced** ➜ **Custom properties** du
> composant.

## Exemple d’utilisation

Supposons que vous vouliez demander confirmation avant de supprimer un
enregistrement :

1. Ajoutez votre composant `Dialog` sur l’écran.
2. Sur le bouton **Supprimer**, définissez :

   ```powerfx
   Set(showConfirm, true);
   ```

3. Associez la propriété `Visible` du composant à `showConfirm` :

   ```powerfx
   Dialog.Visible = showConfirm
   ```

4. Configurez les actions :

   ```powerfx
   Dialog.OnOk = Remove(Tasks, ThisItem);
   Dialog.OnCancel = Set(showConfirm, false)
   ```

5. Définissez `Title` et `Message` selon le contexte (par exemple :
   « Confirmer la suppression », « Êtes‑vous sûr de vouloir supprimer
   cet élément ? »).

Ainsi, à chaque fois que vous devez afficher une confirmation, vous
réutilisez ce composant sans recréer la logique.

## Aller plus loin

- Ajoutez un **contrôle Icône** pour fermer la boîte via la croix.
- Exposez davantage de propriétés (couleurs, taille, nombre de
  boutons) pour personnaliser le composant selon vos besoins.
- Regroupez le tout dans un **fichier .msapp** afin de l’importer
  directement dans vos projets.
