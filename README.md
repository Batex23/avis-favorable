# Avis Favorable !

Le jeu télé de la prévention incendie. Deux équipes, un animateur, des buzzers, et un seul établissement à faire ouvrir : le Complexe des Dunes.

Séance collective de 45 min à 1 h, de 6 à 16 joueurs en 2 à 4 équipes (2 par défaut, composition au choix ou tirée au sort), du chef d'agrès au PRV2. Le niveau (Découverte, Chef d'agrès, PRV2) se règle au début de chaque épreuve. En cas de fermeture de la page, l'accueil propose de reprendre la partie en cours. Le jeu se projette au vidéoprojecteur, avec deux modes au choix :

- **Sans téléphone** : tout se joue à l'écran, avec le clavier ou des buzzers USB.
- **Avec téléphones** : façon Kahoot, chacun rejoint la partie en scannant un QR code (sans compte ni installation), répond, mise, buzze et vote depuis son téléphone. Le Saboteur reçoit son rôle secret sur son téléphone.

**Jouer :** https://batex23.github.io/avis-favorable/

## Les épreuves

- **Les 4 coins** : chaque réponse est un coin de la salle, tout le monde se lève. Jusqu'à 3 points par question et par équipe, selon la part de ses joueurs qui a bien répondu (la taille des équipes ne compte pas).
- **Tabou** : faire deviner un mot du métier sans les mots interdits.
- **Money Drop** : 10 jetons à répartir, les mauvaises réponses tombent dans la trappe.
- **Guerre des lances** (avec téléphones) : bataille en 2D vue de dessus dans le Complexe des Dunes. Joystick et bouton « Arroser » sur le téléphone, foyers à éteindre (+2), adversaires à tremper (+1, gelés 3 s), réserve d'eau à recharger aux poteaux incendie en répondant aux questions.
- **Désamorçage** : un opérateur face au tableau SSI, son équipe dos à l'écran avec le manuel du PC sécurité.
- **Fausse piste** : lire le SSI, le confronter au plan d'intervention (qui peut être faux) et aux indices, trouver le vrai local, repérer les erreurs, choisir où engager le premier engin.
- **Le Tribunal** : le juge examine le registre de sécurité après un incendie ; la traçabilité des actions sur le SSI.
- **Le Saboteur (fil rouge)** : un joueur par équipe travaille en secret pour l'adversaire, avec un contrat à chaque épreuve. Révélation après la finale : démasqué, +3 pour son équipe ; sinon chaque contrat réussi rapporte 2 points à l'adversaire. Sans téléphone : `cartes.html`, à imprimer.
- **Le Coup de tampon** (finale) : duel au buzzer, le capitaine gagnant tamponne le PV et gagne +5. Le podium se fait aux points.

Toujours disponible dans « Composer ma séance » : le Désamorçage (manipulation du tableau SSI, avec `manuel.html`).

## Mode test concepteur

Sur l'écran d'accueil, « Mode test concepteur » ouvre l'écran projeté avec, à droite, quatre téléphones simulés (« Toi » et trois joueurs automatiques), un menu pour aller directement à une épreuve, et les explications de chaque écran, lues à voix haute si « Voix off » est coché. Rien ne passe par Firebase dans ce mode.

## Commandes

| Touche | Action |
|---|---|
| A, 1 ou manette 1 | Buzzer équipe 1 |
| P, 2 ou manette 2 | Buzzer équipe 2 |
| 3 ou manette 3 | Buzzer équipe 3 |
| 4 ou manette 4 | Buzzer équipe 4 |
| V / X | Bonne / mauvaise réponse (animateur) |
| Espace | Révéler / continuer |
| R | Ouvrir la régie (réponse, points) |

Des buzzers USB réglés sur les touches A et P fonctionnent directement.

## Fichiers

- `index.html` : le jeu.
- `manuel.html` : le manuel du PC sécurité pour le Désamorçage, à imprimer en 2 exemplaires ou à ouvrir sur téléphone (QR code dans le jeu).
- `cartes.html` : les cartes du Saboteur pour le mode sans téléphone.
- `questions.json` : la banque de questions, reprise de La Tournée du Préventionniste. Les questions sont à faire valider par un PRV2.

Dans « Banque de questions », le formateur peut ajouter ses propres questions directement dans l'interface (enregistrées dans le navigateur, exportables en CSV) ou importer un fichier CSV.

## Mode téléphone : Firebase

Le jeu est relié au projet Firebase `avis-favorable` (Realtime Database, europe-west1). Pour le relier à un autre projet, suivez les étapes ci-dessous.

### Relier le jeu à un projet Firebase

Les téléphones communiquent avec l'écran par une base Firebase Realtime Database (offre gratuite « Spark », jusqu'à 100 joueurs connectés en même temps).

1. Sur https://console.firebase.google.com, créez un projet (Google Analytics inutile).
2. Menu **Build → Realtime Database → Create database**, région **europe-west1**, démarrage en **mode verrouillé**.
3. Onglet **Rules**, remplacez tout par les règles ci-dessous, puis **Publish**.
4. Roue crantée → **Paramètres du projet → Vos applications → icône Web `</>`**, donnez un nom, puis copiez l'objet `firebaseConfig`.
5. Dans `index.html`, remplacez `const FIREBASE_CONFIG = null;` par cet objet.

```json
{
  "rules": {
    "rooms": {
      "$code": {
        ".read": "$code.length === 4",
        ".write": "$code.length === 4"
      }
    }
  }
}
```

Le jeu ne stocke que des prénoms, des réponses et des scores. Les parties restent dans la base : on peut les vider de temps en temps depuis la console Firebase.

Pour tester le mode téléphone sur un seul ordinateur, sans Firebase : ajoutez `?net=local` à l'adresse, puis ouvrez les « téléphones » dans d'autres onglets du même navigateur.
