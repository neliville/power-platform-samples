Filtrer et manipuler de grandes listes SharePoint

Lorsque les listes SharePoint dépassent plusieurs centaines d’éléments, les limites de délégation de Power Apps peuvent empêcher certaines fonctions (comme Search ou Filter avec des opérateurs non délégués) de s’exécuter côté serveur. Dans ce cas, seules les 500 premières lignes (ou la valeur configurée dans Data row limit dans les paramètres de l’application) sont récupérées, et le reste des données est ignoré.

Ce guide explique comment combiner des filtres délégués et une recherche locale pour offrir une expérience fluide sur de grandes tables.

Exemple de cas d’usage

Vous disposez d’une liste Projets contenant plus de 400 éléments avec les colonnes suivantes :

Client (champ texte)

Statut (champ choix)

Titre et Description (champs texte) à rechercher

L’utilisateur souhaite filtrer les projets par client et statut, puis effectuer une recherche libre dans le titre ou la description.

Formule Power Fx proposée
// Texte recherché : txtRecherche.Text
// Client : txtClient.Text
// Statut : ddStatut.Selected.Result

With(
    {
        // Première étape : filtre côté serveur avec des opérateurs délégués (StartsWith, =)  
        collFiltered: Filter(
            'Projets',
            StartsWith(Client, txtClient.Text) && Status.Value = ddStatut.Selected.Result
        )
    },
    // Seconde étape : si l’utilisateur saisit un terme, effectuer une recherche locale dans la collection temporaire  
    If(
        Len(txtRecherche.Text) > 0,
        Filter(
            collFiltered,
            txtRecherche.Text in Titre || txtRecherche.Text in Description
        ),
        collFiltered
    )
)

Explications

Filtre délégué : la fonction StartsWith sur une colonne texte et l’égalité sur un champ choix sont déléguées à SharePoint. Cela réduit le dataset côté serveur avant de remonter les éléments dans l’application.

Collection temporaire collFiltered : l’utilisation de la fonction With stocke le résultat intermédiaire.

Recherche locale : l’opérateur in n’est pas délégué. En l’appliquant sur collFiltered, vous limitez son impact aux lignes déjà filtrées par SharePoint.

Limite de lignes : augmentez la valeur Data row limit (Paramètres → Paramètres avancés) si nécessaire pour récupérer davantage de lignes côté client.

Conseils complémentaires

Évitez les fonctions non déléguées comme SortByColumns sur des champs calculés ou CountRows sur la liste entière.

Affichez un message d’avertissement lorsque le nombre total d’éléments dépasse la limite définie (via If(DataSourceInfo('Projets', DataSourceInfo.MaxRecords) > 500, "Attention : seul un sous-ensemble des données est affiché", "")).
