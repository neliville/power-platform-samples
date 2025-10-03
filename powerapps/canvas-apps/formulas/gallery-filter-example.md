# 🎯 Filtrer une galerie par l’utilisateur actuel

Cette formule Power Fx permet d’afficher uniquement les éléments d’une
galerie associés à l’utilisateur actuellement connecté.

## Contexte

Dans beaucoup d’applications Power Apps, vous affichez des listes
d’éléments (tâches, demandes, projets…) dans une galerie. Il est
souvent utile de ne montrer à l’utilisateur **que les éléments qui
le concernent**, par exemple ceux qu’il a créés ou dont il est
responsable.

## Formule Power Fx

```powerfx
// Filtre une galerie pour ne montrer que les enregistrements dont
// le champ Personne (AssignedTo) correspond à l’utilisateur actuel
Filter(
    Tasks,
    AssignedTo.Email = User().Email
)
```

Cette formule :

- utilise `Filter()` pour filtrer la source de données `Tasks` ;
- compare l’adresse e‑mail du champ `AssignedTo` (type **Personne**) à
  l’adresse e‑mail renvoyée par `User().Email` ;
- retourne uniquement les éléments où la condition est vraie.

## Étapes d’implémentation

1. **Ajouter une galerie** dans votre écran (insérer ➜ galerie).
2. Définir la propriété **Items** de la galerie avec la formule ci‑dessus.
3. Vérifier que le champ `AssignedTo` est bien de type **Personne**
   et contient un sous‑champ `Email`.
4. Tester en prévisualisant l’application : seuls les éléments
   affectés à l’utilisateur connecté doivent apparaître.

## Variantes

Vous pouvez adapter cette formule en fonction de votre scénario :

- Pour filtrer sur un champ de **texte** au lieu d’un champ Personne :

  ```powerfx
  Filter(Tasks, CreatedBy = User().FullName)
  ```

- Pour filtrer par **statut** en plus de l’utilisateur :

  ```powerfx
  Filter(
      Tasks,
      AssignedTo.Email = User().Email && Status.Value = "En cours"
  )
  ```

Ces variations vous permettent de composer des filtres complexes
adaptés à vos besoins.

