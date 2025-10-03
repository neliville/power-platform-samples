# ⚙️ Exemple de règle métier pour une application Model‑driven

Cette règle métier illustre comment **définir automatiquement une date
d’échéance** lorsqu’un enregistrement est créé dans Dataverse. Elle
peut être adaptée à vos propres tables et champs.

## Scénario

Dans une entité « Tâche », vous disposez des champs suivants :

- `Titre` (texte)
- `Date de création` (date/heure)
- `Date d’échéance` (date/heure)

Vous souhaitez que la **Date d’échéance** soit automatiquement fixée
à **7 jours après** la Date de création si l’utilisateur n’en saisit
pas lui‑même.

## Étapes pour créer la règle métier

1. Ouvrez le **Concepteur de règles métier** dans Dataverse (ou
   dans Power Apps sous **Solutions**).
2. Sélectionnez l’entité « Tâche » puis choisissez **Nouvelle
   règle métier**.
3. Ajoutez une **Condition** intitulée « Date d’échéance
   vide » :
   - Field : `Date d’échéance`
   - Operator : `Is blank` (est vide)
4. Dans la branche **Vrai**, ajoutez une **Action** de **Définir une
   valeur** sur le champ `Date d’échéance` :
   - Field : `Date d’échéance`
   - Type : `Formula`
   - Value : `AddDays([Date de création], 7)`
5. Activez la règle et publiez-la.

## Résultat

Lors de la création d’une tâche, si l’utilisateur n’entre pas de
date d’échéance, le système remplira automatiquement ce champ avec
la date de création plus sept jours. Cette logique garantit une
échéance par défaut cohérente sans avoir à écrire du code
javascript.

## Conseils

- Vous pouvez combiner plusieurs conditions pour gérer différents
  scénarios (priorité, type de tâche…).
- N’oubliez pas de **publier** la règle après chaque modification
  pour qu’elle soit appliquée.
- Pour des logiques plus avancées, envisagez les **plugins
  Dataverse** ou des **flows Power Automate**.
