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
| Position sur le terrain | cliquer sur le terrain **après** avoir tagué — ça place le tag en surbrillance |
| Reprendre un tag | cliquer sa ligne dans le tableau · <kbd>Échap</kbd> pour revenir au dernier |
| Corriger | <kbd>Ctrl</kbd>+<kbd>Z</kbd> · la croix dans le tableau · bouton *Réussi ⇄ Raté* |
| Compter une passe | <kbd>+</kbd> réussie · <kbd>−</kbd> ratée (sur l'équipe armée) |
| Note | <kbd>N</kbd> |
| Vidéo | <kbd>Espace</kbd> · <kbd>←</kbd> <kbd>→</kbd> ± 3 s · <kbd>Maj</kbd>+ ± 10 s · vitesse 0,5× à 2× |
| Replier le terrain | <kbd>V</kbd> — rend toute la hauteur à la vidéo |

### La direction des passes

Une passe qui n'est qu'un point ne dit rien de la progression. Les actions qui
ont un **sens** — passe, passe clé, centre, dégagement, coup franc, corner —
prennent donc **deux clics sur le terrain** : le départ, puis l'arrivée. Une
flèche apparaît. Un troisième clic reprend le départ, pour corriger.

Les autres actions gardent le clic unique, qui se corrige en recliquant.

L'export CSV gagne `x2, y2, x2_att, y2_att` et **`progression_m`** : le gain en
mètres dans le sens d'attaque de l'équipe, signé — une passe en retrait est une
information, pas une erreur.

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

### Revenir sur un tag

Cliquer une ligne du tableau **reprend** ce tag : la vidéo revient 3 secondes
avant, la ligne passe en surbrillance, et tout ce qui édite le vise — le clic sur
le terrain, *Réussi ⇄ Raté*, la note. Le bandeau du bas affiche alors le badge
**reprise**. <kbd>Échap</kbd> ou **↩ dernier** ramène au dernier tag posé, et
taguer une nouvelle action reprend la main toute seule.

Sans reprise, c'est le dernier tag qui est visé — le comportement d'avant.

Attention : **⤺ Annuler retire le dernier tag posé**, jamais celui qu'on a repris.
Pour supprimer une ligne précise, c'est la croix au bout de sa ligne.

### Tagger au téléphone, en direct

La page hébergée s'ouvre sur mobile et s'y réorganise pour un seul usage :
**le match passe sur la TV ou au stade, on tague sur le téléphone**. Il n'y a
alors ni fichier vidéo ni clavier — les deux choses sur lesquelles la version
bureau est construite.

Trois idées viennent des outils du métier (Nacsport Tag&view, Hudl Sportscode
Coda), qui codent en direct sur iPad depuis des années :

- **La matrice en direct, posée sur les boutons.** Chez Nacsport, une matrice
  affiche pendant la saisie combien de fois chaque code a été tagué — c'est ce
  qui permet de se relire sans quitter le tagging. Sur un téléphone il n'y a pas
  la place d'une matrice à côté, donc **chaque bouton d'action porte son compte
  pour l'équipe armée**. Zéro pixel de plus, et le compte qui bouge est en même
  temps la preuve que le tag est parti. Par équipe : « 8 tirs » ne veut rien dire
  si on ne sait pas de qui.
- **Le retour sur le bouton lui-même.** On tague en regardant le terrain, pas
  l'écran : le bandeau du bas ne se lit qu'en baissant les yeux. Le bouton
  **flashe en orange (réussi) ou en rouge (raté)** et le téléphone **vibre**,
  avec deux motifs distincts. C'est la seule confirmation qui arrive au moment
  où on en a besoin.
- **La grille passe au-dessus de la carte équipe.** Tenu à une main, le haut d'un
  écran de 760 px ne s'atteint pas au pouce, or c'est la grille qu'on touche
  plusieurs fois par minute et le sélecteur d'équipe seulement aux changements de
  possession. L'équipe armée reste lisible : la grille porte **son liseré de
  couleur et son nom** dans l'entête.

Le reste :

- **Le cadre vidéo, les commandes de lecture et le terrain disparaissent.**
  Ils reviennent si on dépose quand même une vidéo. Le terrain se rouvre avec
  **▦ Terrain** (le choix est retenu).
- **Les joueurs passent sur une ligne qui défile** au lieu d'une grille : 25
  joueurs en grille coûtent 200 px, soit la moitié de la grille d'actions.
- **Un bouton `— aucun`** ouvre la ligne des joueurs : c'est l'<kbd>Échap</kbd>
  du téléphone. En direct on tague souvent avant d'avoir lu le numéro, et un tag
  attribué au joueur précédent est pire qu'un tag sans joueur — rien ne le
  signale ensuite.
- **L'appui long sur une action la tague ratée**, à la place de
  <kbd>Maj</kbd>+touche. C'est le geste qui devait rester unitaire : les actions
  ratées font la moitié de ce qu'on tague.
- **Le tableau se replie**, `▾ voir le détail` le rouvre. En direct on tague, on
  relit après.
- **La barre du dernier tag colle en bas**, pour que `⤺ Annuler` reste sous le
  pouce.

Les cibles font au moins 44 px. Tout tient **sans défiler** à partir de
375 × 667 (iPhone SE) ; en dessous (360 × 640) la dernière rangée demande un
petit défilement — masquer quelques actions dans **Configuration** le règle.

Le sélecteur **Passage** est masqué sur téléphone, sauf si un passage est déjà
actif : c'est un outil de relecture, et il faut pouvoir le retirer depuis le
mobile s'il a été laissé depuis le bureau.

### Le match en direct — bouton **● Live**

Le direct n'est pas une redif sans le fichier vidéo : il n'y a **pas de temps de
lecteur** à recopier, seulement le **chrono du bandeau**, dans l'image. Le bouton
**● Live** bascule l'outil dessus :

- Le champ **⚑ Caler sur le bandeau** prend la minute affichée à l'écran —
  `23:45` — et l'horloge de match se met dessus. Elle démarre le chrono toute
  seule. Un seul geste, et **il n'est plus nécessaire d'avoir vu le coup
  d'envoi** : on peut arriver à la 20e minute et être juste en trois secondes.
- Le cadre vidéo et les commandes de lecture disparaissent : **le terrain prend
  toute la place** (776 px de large au lieu de 228 sur un écran de portable).
  À 0,13 m par pixel au lieu de 0,46, le départ et l'arrivée d'une passe se
  posent enfin plus finement que ce que `progression_m` prétend mesurer.
- Un avertissement s'affiche si la minute calée ne va pas avec la période armée
  (50:00 en MT1, 20:00 en MT2). C'est l'erreur qui ne se voit pas autrement :
  tout continue de marcher, et le match entier sort décalé de 45 minutes.

Un calage ne corrige **que la suite** : les tags déjà posés gardent la minute
qu'ils avaient. Donc caler avant de commencer, et **recaler en début de MT2**.

> Le chrono manuel lit une horloge monotone, il n'accumule plus le temps image
> par image. L'ancienne version se figeait dès que l'onglet passait en
> arrière-plan — ce que fait YouTube en plein écran — et tous les tags sortaient
> à la minute où on avait quitté l'onglet, sans que rien ne le signale.
> Mesuré avant correctif : **0 seconde écoulée en 4 s d'arrière-plan**.

### Les joueurs ciblés

Une consigne d'avant-match — « les passes progressives de ces quatre-là » — ne
se retient pas dans une grille de 25 numéros : on cherche le bouton au lieu de
regarder le terrain. Dans **Configuration**, le champ *Joueurs ciblés* prend
leurs numéros (`6 97 8 5`). Ils portent un liseré et un point dans la grille, et
le bouton **⦿ ciblés** n'affiche qu'eux — **quatre boutons au lieu de
vingt-cinq**. Ils vivent hors de l'effectif : réécrire le roster ne les efface
pas.

Combiné au sélecteur **Passage** réglé sur *Passe*, l'écran ne montre plus que
les quatre joueurs et la seule action à taguer.

### Match lu ailleurs (YouTube en redif, VLC, la TV)

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
  x, y, x_att, y_att, x2, y2, x2_att, y2_att, progression_m, note`.

  `x`/`y` sont les coordonnées du terrain affiché (0→100 de gauche à droite,
  de bas en haut). `x_att`/`y_att` sont les mêmes retournées pour que **l'équipe
  qui fait l'action attaque toujours vers la droite** — c'est celles-là qu'on
  utilise pour cumuler les deux mi-temps ou comparer deux joueurs.

- **JSON** — la sauvegarde complète (actions + effectif + réglages + coups d'envoi).
  C'est le seul qui se recharge pour reprendre une session.

*Nouveau match* efface les actions mais garde effectifs et réglages.

## Tests

`_test.html` (101 vérifications : clavier, effectifs, terrain, undo, calage d'horloge,
exports, persistance, compteur de passes, passages successifs, reprise d'un tag, migration,
matrice en direct, passes à direction,
et la mise en page téléphone mesurée dans une iframe de 390 × 760) et `_test_xml.html` (le XML produit relu par le moteur de la
Feuille de Match). Ils tournent dans un navigateur mais ont besoin de `http://`,
pas de `file://`. Chaque iframe charge `index.html?v=<horodatage>` : sans ça le
navigateur ressert la version en cache et **la suite passe au vert sur l'ancien
fichier**, ce qui est arrivé le 4 septembre.


```bash
python -m http.server 8777 --bind 127.0.0.1   # depuis le dossier parent
# puis ouvrir http://127.0.0.1:8777/kazma-tagger/_test.html
```

`_test_xml.html` charge aussi `../kazma-match-report/dist/rapport-match.html`.

---

## `grille.html` — la grille de codage du club

Un **second outil**, dans le même dépôt, qui ne remplace pas `index.html` :
**https://haris692.github.io/kazma-tagger/grille.html**

Le panneau de codage envoyé par le staff compte 46 cases. Le rapport
`kazma-bdd/grille_codage.pdf` les a triées en quatre statuts :

| statut | nb | où c'est |
|---|---|---|
| **Disponible** | 22 | sort des XML du fournisseur, déjà en ligne sur `kazma-web` |
| **Calculable** | 9 | se déduit des XML, le calcul reste à écrire |
| **À taguer** | **10** | **aucune donnée ne le porte — c'est ce que fait `grille.html`** |
| **Impossible** | 5 | demande la position des 22 joueurs en continu (tracking) |

`grille.html` ne tague **que les dix cases du troisième groupe**. Re-taguer une
passe ou un tir serait du gâchis : le fournisseur les livre déjà, pour tous les
matchs et sans erreur de fatigue. Ici on ne code que le jugement, ce qu'aucun
export ne remplacera.

| Bloc | Cases | Saisie |
|---|---|---|
| Build up | Under press · Without press | point · réussi/raté |
| Final 1/3 | Playing in active zone | point |
| Positional Attack | Creativity | départ → arrivée · réussi/raté |
| Decision Making | In Context · Out Of Context | point |
| Phases défensives | Pressure · Aggressivity | point · réussi/raté |
| Nature de la passe | Combination | départ → arrivée · réussi/raté |
| Duels | 1 vs 1 | point · réussi/raté |

### Les définitions font partie de l'outil

Chaque case porte une définition écrite, affichée pendant qu'on tague et
modifiable dans **Réglages**. Ce n'est pas de la documentation : sans elle,
« Pressure » veut dire deux choses différentes à deux séances d'intervalle et
les matchs ne sont plus comparables. Les définitions livrées sont des
**propositions à faire valider par le staff avant la première session** ; une
fois arrêtées, elles ne bougent plus en cours de saison. Elles s'exportent avec
les données (`…-definitions.csv`) pour qu'un chiffre reste lisible dans six mois.

### Le sens d'attaque est réglé ici, plus dans l'analyse

`index.html` ignore le changement de camp : pour lui l'équipe A attaque toujours
vers la droite, ce qui oblige à retourner la MT2 après coup. `grille.html` a un
réglage **par période** (bouton dans la barre d'horloge, ou onglet Réglages).

On tague **ce qu'on voit à l'écran**, sans jamais inverser mentalement, et le
CSV sort avec `x_att` / `y_att` déjà normalisés dans le sens d'attaque —
`x_att = 0` son propre but, `x_att = 100` le but adverse, quelle que soit la
mi-temps. L'inversion applique une **rotation de 180°** (`x → 100−x` *et*
`y → 100−y`) : n'inverser que `x` permuterait les couloirs droite et gauche et
ferait croire qu'un joueur a changé de côté à la pause.

`progression_m` est calculée sur `x_att`, donc juste dans les deux périodes.

### Raccourcis

`P` Under press · `O` Without press · `Z` active zone · `C` Creativity ·
`I` In Context · `U` Out Of Context · `R` Pressure · `A` Aggressivity ·
`M` Combination · `D` 1 vs 1.
**Maj + touche = raté.** `Tab` change d'équipe, `Espace` lit/pause,
`←` `→` reculent et avancent, `Retour arrière` annule, `V` replie le terrain.

### Exports

- `…-grille.csv` — une ligne par tag, `x`/`y` tels que cliqués **et**
  `x_att`/`y_att` normalisés. Lire avec `encoding="utf-8-sig"` (BOM).
- `…-definitions.csv` — les dix définitions telles qu'elles étaient au moment
  du relevé.
- `…-grille.xml` — instances Sportscode, fenêtre de clip réglable.
- `…-grille.json` — sauvegarde complète, rechargeable.
