# Créer une page d'accueil avec menu de navigation

Les applications efficaces disposent souvent d’une page d’accueil qui oriente l’utilisateur vers les différents écrans. Cette page doit être claire, responsive et facile à maintenir. Voici une méthode simple pour créer un **menu latéral** réutilisable et une page d’accueil personnalisée dans Power Apps.

## 1. Préparer la source de données du menu

Commencez par définir une collection contenant le titre du menu, l’icône et la référence de l’écran à naviguer :

```PowerFx
// À placer dans OnStart ou OnVisible de l'écran principal
ClearCollect(
    colMenuItems,
    { Title: "Accueil", Icon: Icon.Home, TargetScreen: scrAccueil },
    { Title: "Projets", Icon: Icon.Apps, TargetScreen: scrProjets },
    { Title: "Tâches", Icon: Icon.Tasks, TargetScreen: scrTaches },
    { Title: "Paramètres", Icon: Icon.Settings, TargetScreen: scrParametres }
);
// Variable contrôlant l’ouverture du menu (true = ouvert)
Set(varMenuOpen, true);
```

## 2. Créer le contrôle de navigation

1. **Insérez une galerie verticale** et nommez-la `galMenu`.
2. Définissez sa propriété **Items** sur `colMenuItems`.
3. Dans la **TemplateFill**, utilisez une couleur différente lorsque l’élément est sélectionné :

   ```PowerFx
   If(ThisItem.IsSelected, ColorFade(Theme.ColorPrimary, -10%), ColorFade(Theme.ColorPrimary, 40%))
   ```
4. Ajoutez deux contrôles dans la galerie :
   - Un **Label** avec le texte `ThisItem.Title`.
   - Un **Icon** (propriété **Icon** : `ThisItem.Icon`).
5. Sur **OnSelect** de la galerie :

   ```PowerFx
   Navigate(ThisItem.TargetScreen, ScreenTransition.Fade);
   UpdateContext({ varMenuOpen: false });
   ```

## 3. Concevoir la page d'accueil (écran `scrAccueil`)

Placez sur l’écran d’accueil :

- Un en‑tête avec le logo et le titre de l’application.
- Plusieurs cartes de résumé utilisant des **Container** ou des **Rectangle** pour afficher des indicateurs (nombre de projets, tâches en cours, etc.).
- Un bouton ou une icône pour basculer l’affichage du menu :

  ```PowerFx
  // Icône hamburger dans l'en-tête
  OnSelect: UpdateContext({ varMenuOpen: !varMenuOpen })
  ```

## 4. Rendre le menu responsive

Pour les écrans larges, laissez le menu visible en permanence. Pour les petits écrans (mobile), masquez‑le par défaut :

```PowerFx
// Dans la propriété Visible de la galerie galMenu
If(App.Width >= 600, true, varMenuOpen)
```

## 5. Conseils supplémentaires

- Utilisez des **components** pour réutiliser le menu dans toutes vos applications.  
- Faites transiter les variables globales dans le **OnStart** pour centraliser la navigation.  
- Pensez à ajouter un contrôle `Spacer` ou `Rectangle` en haut de la galerie pour gérer les marges et l’alignement.
