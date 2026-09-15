# Motivation-

Une page web d'un seul fichier : un proverbe sur la persévérance, un bouton, et derrière, un quiz de basketball de 105 questions.

L'idée de départ était une simple carte de motivation. Elle est devenue une épreuve : le proverbe annonce que celui qui est mis à l'épreuve peut la réussir, autant en fournir une.

## Aperçu

À l'ouverture, la citation et le bouton **Commencer l'épreuve**. Chaque partie tire 20 questions au hasard dans la banque et mélange aussi l'ordre des propositions, donc deux parties ne se ressemblent pas. Correction immédiate après chaque réponse, score en direct, barre de progression, et un récapitulatif complet à la fin avec les erreurs et les bonnes réponses.

## Le quiz

105 questions réparties en quatre catégories :

| Catégorie | Contenu |
|---|---|
| Règles | dimensions, chronos, fautes, violations, arbitrage |
| Histoire | Naismith, naissance de la NBA, FIBA, Jeux olympiques, BAL |
| Légendes | Jordan, Kobe, LeBron, Curry, Giannis, les joueurs africains |
| Compétitions | franchises, trophées, records, AfroBasket, Coupe du monde |

## Utilisation

Ouvrir `index.html` dans un navigateur. Rien à installer, aucune dépendance, aucun appel réseau.

Pour publier via GitHub Pages : `Settings` → `Pages` → branche `main`, dossier racine. Le site sera servi à `https://pro1gramer-hic.github.io/Motivation-`.

## Modifier les questions

Tout se trouve dans le tableau `BANQUE`, dans le `<script>` en bas de `index.html`. Une question par ligne, au format :

```js
["Catégorie", "Énoncé de la question ?", ["Choix A", "Choix B", "Choix C", "Choix D"], 1]
```

Le dernier nombre est l'indice de la bonne réponse dans le tableau des choix, en partant de zéro. Dans l'exemple ci-dessus, la bonne réponse est « Choix B ».

Pour changer le nombre de questions par partie, modifier la constante juste en dessous de la banque :

```js
const NB_QUESTIONS = 20;
```

## Personnalisation

Les couleurs sont regroupées dans les variables CSS en haut du fichier :

```css
--bleu-fond: #101c3e;
--bleu-clair: #1e3c72;
--or: #d4af37;
```

Le proverbe d'accueil se trouve dans la section `#ecran-accueil`, les messages de fin dans la fonction `afficherResultat()`.

## Technique

HTML, CSS et JavaScript natifs dans un seul fichier. Pas de framework, pas de build, pas de CDN. La page est responsive et tient sur un écran de téléphone.

## Structure

```
.
├── index.html    # la page complète : structure, styles, logique, questions
└── README.md
```

## Licence

Libre d'utilisation et de modification.
