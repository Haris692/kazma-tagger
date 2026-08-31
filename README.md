# Kazma — Tagger vidéo

Coder les actions d'un match à la main, au clavier, pendant qu'on regarde la redif.
En attendant Sportbase.

Un seul fichier, aucun serveur, aucune dépendance : **double-clic sur `index.html`**.
La vidéo reste sur le PC, rien n'est envoyé nulle part.

**En ligne : https://haris692.github.io/kazma-tagger/**

La page hébergée fait tourner exactement le même outil. La vidéo du match n'est
pas et ne sera pas hébergée ici : elle se charge depuis le disque de la machine
qui ouvre la page, et ne quitte jamais ce poste. Les tags sont gardés dans le
navigateur (localStorage), donc propres à chaque machine — passer d'un poste à
l'autre se fait avec le bouton **⇊ Sauvegarde**.

---

## Démarrer

1. Ouvrir `index.html`.
2. Onglet **Configuration** : noms des deux équipes, puis les effectifs
   (une ligne par joueur, `numéro nom`). Bouton *Remplir avec 1→25* si on n'a pas
   encore la feuille de match — on tague au numéro, on mettra les noms après.
3. Retour sur **Tag** : glisser le fichier vidéo dans le cadre noir.
   Pas de fichier vidéo (match sur une autre fenêtre, sur la TV) → bouton
   **Chrono manuel**, à lancer au coup d'envoi.
4. Au coup d'envoi, cliquer **⚑ Coup d'envoi ici**. À partir de là, l'horloge
   affiche la vraie minute de match, pas le temps de la vidéo.
   À refaire en début de 2e mi-temps après avoir cliqué **MT2**.

## Tagger

Le principe : **l'équipe et le joueur restent armés**, on n'appuie que sur l'action.

| | |
|---|---|
| Choisir le joueur | taper son **numéro** (`10`, `7`…) · <kbd>Échap</kbd> = aucun joueur |
| Changer d'équipe | <kbd>E</kbd> |
| Tagger | la touche de l'action (<kbd>T</kbd> tir, <kbd>C</kbd> centre, <kbd>R</kbd> récupération…) |
| Action ratée | <kbd>Maj</kbd> + la touche |
| Position sur le terrain | cliquer sur le terrain **après** avoir tagué — ça place le dernier tag |
| Corriger | <kbd>Ctrl</kbd>+<kbd>Z</kbd> · la croix dans le tableau · bouton *Réussi ⇄ Raté* |
| Compter une passe | <kbd>+</kbd> réussie · <kbd>−</kbd> ratée (sur l'équipe armée) |
| Note | <kbd>N</kbd> |
| Vidéo | <kbd>Espace</kbd> · <kbd>←</kbd> <kbd>→</kbd> ± 3 s · <kbd>Maj</kbd>+ ± 10 s · vitesse 0,5× à 2× |
| Replier le terrain | <kbd>V</kbd> — rend toute la hauteur à la vidéo |

### Compter les passes

Le volume des passes noierait le tableau, donc elles ont leur propre bloc, en
haut à droite : le total, les réussies et le **pourcentage de réussite**, par
équipe. <kbd>+</kbd> et <kbd>−</kbd> comptent sur l'équipe armée, les boutons
`+ − ⤺` font la même chose à la souris, `⤺` retire la dernière passe de cette
équipe.

Une passe reste **un tag comme un autre** : horodatée, elle part dans le XML et
le CSV avec le reste. Elle est juste masquée du tableau par défaut — décocher
*masquer les passes* pour les voir.

### Tagger en plusieurs passages

Regarder le match une fois par type d'action est souvent plus fiable que tout
attraper d'un coup. Le sélecteur **Passage** sert à ça : choisir *Tir*, et la
grille comme le tableau ne montrent plus que les tirs, avec le rappel
`n tagués · dernier à mm:ss` — cliquer sur le temps y ramène la vidéo.
**⏮ Début** revient au coup d'envoi de la période en cours.

Les tags étant horodatés, les passages successifs se rangent tout seuls dans
l'ordre : rien à fusionner à la main. Et les raccourcis des autres actions
**restent actifs** pendant un passage — un but repéré au vol se tague quand même.

Cliquer sur l'heure d'une ligne du tableau ramène la vidéo 3 secondes avant l'action.

### Match lu ailleurs (YouTube, VLC, la TV)

Sans fichier vidéo déposé, le tagger affiche le champ **⤢ Caler**. On y recopie
le temps affiché par l'autre lecteur et l'horloge se remet pile dessus :
`1:23:45`, `12:34`, ou un nombre seul (= des minutes).

C'est ce qui rattrape la dérive du chrono : **après chaque pause et chaque retour
en arrière**, un coup d'œil au lecteur, on recopie, <kbd>Entrée</kbd>. Le calage
et le coup d'envoi se combinent — on cale le temps du lecteur, l'horloge en
déduit la minute de match.

Une saisie qui n'est pas un temps est ignorée et reste dans le champ pour être
corrigée, l'horloge ne bouge pas.

La liste des actions et les touches se modifient dans **Configuration** : on peut
en ajouter, en renommer, en masquer, changer les raccourcis. Le réglage est retenu
d'un match à l'autre.

## Récupérer les données

Tout est enregistré dans le navigateur au fil du tagging : fermer l'onglet par
erreur ne perd rien. Trois exports, onglet **Données** ou boutons en haut :

- **XML de codage vidéo** — le format `<instance>` que **la Feuille de Match lit déjà**.
  On le dépose dans l'onglet Données du rapport et on obtient le décompte par action,
  par équipe, par joueur. Chaque tag devient un clip (5 s avant / 3 s après par défaut,
  réglable).
- **CSV** — une ligne par action, pour pandas :

  ```python
  df = pd.read_csv("2026-08-28-kazma-al-arabi-actions.csv")
  df[df.cote == "A"].groupby("action").size()
  ```

  Colonnes : `id, date, competition, equipe_a, equipe_b, periode, temps_video_s,
  temps_match_s, minute, seconde, equipe, cote, numero, joueur, action, issue,
  x, y, x_att, y_att, note`.

  `x`/`y` sont les coordonnées du terrain affiché (0→100 de gauche à droite,
  de bas en haut). `x_att`/`y_att` sont les mêmes retournées pour que **l'équipe
  qui fait l'action attaque toujours vers la droite** — c'est celles-là qu'on
  utilise pour cumuler les deux mi-temps ou comparer deux joueurs.

- **JSON** — la sauvegarde complète (actions + effectif + réglages + coups d'envoi).
  C'est le seul qui se recharge pour reprendre une session.

*Nouveau match* efface les actions mais garde effectifs et réglages.

## Tests

`_test.html` (62 vérifications : clavier, effectifs, terrain, undo, calage d'horloge,
exports, persistance, compteur de passes, passages successifs, migration) et `_test_xml.html` (le XML produit relu par le moteur de la
Feuille de Match). Ils tournent dans un navigateur mais ont besoin de `http://`,
pas de `file://` :

```bash
python -m http.server 8777 --bind 127.0.0.1   # depuis le dossier parent
# puis ouvrir http://127.0.0.1:8777/kazma-tagger/_test.html
```

`_test_xml.html` charge aussi `../kazma-match-report/dist/rapport-match.html`.
