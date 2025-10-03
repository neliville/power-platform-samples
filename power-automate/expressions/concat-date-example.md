# 🧮 Exemple d’expression : concaténer une date et du texte

Dans Power Automate, les **expressions** permettent de manipuler des
données dans vos actions. Voici un exemple d’utilisation de
`concat()` et `formatDateTime()` pour construire un objet texte
comprenant une date formatée.

## Objectif

Composer un corps d’e‑mail avec un message et la date du jour
formatée en `dd/MM/yyyy`. Les variables dynamiques ne suffisent pas
toujours : les expressions offrent plus de contrôle sur le rendu.

## Expression utilisée

```expression
concat(
  'Rapport généré le ',
  formatDateTime(utcNow(), 'dd/MM/yyyy')
)
```

**Explications :**

- `utcNow()` renvoie l’heure actuelle en UTC.
- `formatDateTime()` convertit cette date en chaîne selon le format
  spécifié (`dd/MM/yyyy`).
- `concat()` assemble plusieurs chaînes en une seule.

## Utilisation dans un flow

1. Dans l’action **Envoyer un e‑mail (V2)**, placez le curseur dans
   le champ **Body**.
2. Cliquez sur **Expression** et entrez la formule ci‑dessus.
3. Cliquez sur **OK**. Le texte assemblé sera inséré dans le corps
   du mail lors de l’exécution du flow.

## Variante

Pour inclure le nom de l’expéditeur (variable dynamique) avec la
date :

```expression
concat(
  'Rapport généré par ',
  triggerOutputs()?['headers']?['x-ms-user-name'],
  ' le ',
  formatDateTime(utcNow(), 'dd MMMM yyyy')
)
```

Vous pouvez ainsi créer des messages dynamiques très personnalisés.
