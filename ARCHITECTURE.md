# Architecture

## 1. Présentation générale

**Farde** est une application web autonome de création et de gestion de bibles graphiques.

Elle fonctionne entièrement dans un navigateur moderne à partir d’un seul fichier :

```text
farde.html
```

L’application ne nécessite :

- ni installation ;
- ni serveur ;
- ni base de données distante ;
- ni compte utilisateur ;
- ni dépendance à une infrastructure propriétaire.

Le fichier HTML regroupe l’interface, les styles CSS et la logique JavaScript.

---

## 2. Objectifs de l’architecture

L’architecture de Farde repose sur plusieurs principes :

- autonomie complète du programme ;
- simplicité de déploiement ;
- fonctionnement hors ligne ;
- portabilité entre ordinateurs et tablettes ;
- conservation locale des projets ;
- possibilité d’archiver et de transférer un classeur complet ;
- code lisible et modifiable ;
- interface bilingue français / anglais ;
- compatibilité avec les navigateurs modernes.

---

## 3. Organisation générale du fichier

Le fichier `farde.html` contient trois grandes parties.

### 3.1 Structure HTML

La structure HTML définit :

- l’en-tête de l’application ;
- le nom du projet ;
- la navigation entre les intercalaires ;
- la barre de recherche et les filtres ;
- la grille de fiches ;
- la fiche d’édition ;
- la fenêtre de focus image ;
- le diaporama ;
- le panneau Infos ;
- le panneau Réglages ;
- les contrôles d’import et d’export ;
- les messages et boîtes de dialogue.

### 3.2 Styles CSS

Les styles CSS gèrent :

- la charte graphique ;
- la typographie ;
- les couleurs des intercalaires ;
- la grille responsive ;
- les cartes ;
- les panneaux latéraux ;
- les boutons ;
- les fenêtres modales ;
- l’affichage sur écrans étroits ;
- l’impression et l’export PDF.

### 3.3 Logique JavaScript

Le JavaScript gère :

- l’état du projet ;
- la création et la modification des fiches ;
- le rendu des intercalaires ;
- le stockage local ;
- la gestion des images ;
- les recherches et filtres ;
- les favoris ;
- les références liées ;
- le focus image ;
- le diaporama ;
- les exports ;
- les imports ;
- l’interface bilingue ;
- l’annulation et le rétablissement.

---

## 4. Modèle de données

L’application manipule un objet principal représentant le projet.

Il contient notamment :

- le nom du projet ;
- l’intercalaire actif ;
- la liste des fiches ;
- les réglages ;
- la langue active ;
- les préférences d’affichage ;
- les informations nécessaires aux exports.

Chaque fiche contient des données telles que :

- identifiant unique ;
- intercalaire ;
- titre ;
- description ;
- tags ;
- statut favori ;
- image ;
- identifiant d’image locale ;
- nom du fichier d’origine ;
- statut de l’image ;
- intercalaires liés pour les fiches Références.

La structure exacte peut évoluer avec les versions, mais reste conçue pour être sérialisée dans les exports JSON et ZIP.

---

## 5. Intercalaires

Farde organise les fiches en intercalaires thématiques :

- Moodboard ;
- Références ;
- Idées visuelles ;
- Personnages ;
- Expressions ;
- Poses ;
- Costumes ;
- Objets ;
- Décors ;
- Couleurs / valeurs ;
- Style guide ;
- Continuité.

Chaque intercalaire possède :

- un identifiant interne ;
- un titre français ;
- un titre anglais ;
- une description française ;
- une description anglaise ;
- une couleur d’identification.

Les fiches appartiennent à un intercalaire principal.

Les fiches placées dans **Références** peuvent également être liées à un ou plusieurs autres intercalaires.

---

## 6. Rendu de l’interface

Le rendu repose sur une reconstruction dynamique de l’interface à partir de l’état courant.

Les principales opérations sont :

- génération de la navigation ;
- affichage des fiches du board actif ;
- application des filtres ;
- affichage éventuel des références liées ;
- mise à jour des compteurs ;
- adaptation des libellés à la langue choisie ;
- actualisation des boutons et panneaux.

Le DOM est mis à jour à chaque modification importante du projet.

---

## 7. Gestion des fiches

Une fiche peut contenir :

- une image ;
- un titre ;
- une description ;
- des tags ;
- un statut favori ;
- des liens vers d’autres boards lorsqu’il s’agit d’une fiche Références.

Les principales actions disponibles sont :

- créer ;
- modifier ;
- déplacer ;
- trier ;
- supprimer ;
- mettre en favori ;
- ouvrir en focus ;
- intégrer à un diaporama ;
- relier à d’autres intercalaires.

Les modifications sont enregistrées dans l’état local du projet.

---

## 8. Gestion des images

Farde accepte plusieurs sources d’images :

- import local ;
- chemin relatif ;
- URL distante ;
- image restaurée depuis une archive ZIP.

Les images locales sont stockées dans le navigateur afin de rester disponibles dans le projet courant.

Lors d’un export ZIP complet :

- les métadonnées sont enregistrées dans `projet.json` ;
- les images locales sont placées dans un dossier `images/` ;
- les chemins sont réécrits pour permettre la restauration du projet.

---

## 9. Stockage local

Le stockage local repose sur les mécanismes natifs du navigateur.

### 9.1 Données du projet

Les données textuelles et les préférences sont conservées localement afin de restaurer l’état de travail.

### 9.2 Images

Les images importées localement sont enregistrées séparément dans le stockage du navigateur.

### 9.3 Compatibilité

Les clés historiques de stockage peuvent être conservées lors d’un renommage du programme afin d’éviter la perte de projets existants.

Le stockage local dépend du navigateur utilisé. La suppression des données du navigateur peut supprimer le projet courant.

---

## 10. Import et export ZIP

L’archive ZIP constitue le format recommandé pour :

- sauvegarder un projet complet ;
- transférer un projet vers un autre appareil ;
- archiver les images et les métadonnées ensemble ;
- restaurer un classeur ultérieurement.

L’archive contient généralement :

```text
projet.json
images/
```

Le fichier `projet.json` contient les fiches, les réglages, les liens, les tags et les informations de projet.

Le dossier `images/` contient les images locales exportées.

À l’import, l’application :

1. lit le manifeste ;
2. recrée le projet ;
3. restaure les images locales ;
4. rétablit les fiches et leurs liens ;
5. recharge les réglages.

---

## 11. Recherche, filtres et tri

La recherche peut porter sur :

- les titres ;
- les descriptions ;
- les tags ;
- les intercalaires liés.

Les filtres permettent notamment :

- d’afficher uniquement les favoris ;
- d’afficher ou masquer les références liées ;
- de limiter l’affichage au board actif.

Le tri rapide permet de réorganiser les fiches sans modifier leur contenu.

---

## 12. Références liées

Les fiches de l’intercalaire **Références** disposent d’un champ spécifique permettant de les associer à d’autres intercalaires.

Exemple :

- une référence de costume peut être liée à Costumes ;
- une référence d’architecture peut être liée à Décors ;
- une référence de geste peut être liée à Poses ou Expressions.

Dans chaque board concerné, l’utilisateur peut afficher ou masquer les références liées.

Les fiches liées restent stockées dans l’intercalaire Références et ne sont pas dupliquées.

---

## 13. Focus image et diaporama

La fenêtre de focus image permet :

- d’agrandir une image ;
- de naviguer entre les fiches ;
- de lancer ou mettre en pause la lecture ;
- de parcourir les images au clavier ou par geste tactile.

Le diaporama du board actif :

- utilise l’ordre courant des fiches ;
- commence à la première image lorsqu’il est lancé depuis le bouton principal ;
- reprend la durée définie dans les réglages ;
- peut être exporté en HTML autonome.

---

## 14. Export HTML du diaporama

L’export du diaporama produit un fichier HTML autonome contenant :

- les images du board actif ;
- le titre du projet ;
- le nom du board ;
- les contrôles de navigation ;
- la lecture automatique ;
- la durée choisie dans les réglages ;
- la gestion du clavier et du tactile.

Le fichier généré peut être ouvert indépendamment de Farde.

---

## 15. Export PDF

L’application permet d’exporter :

- le board actif ;
- l’ensemble du classeur.

L’export utilise les fonctions d’impression du navigateur.

Le résultat peut légèrement varier selon :

- le navigateur ;
- le système d’exploitation ;
- les marges d’impression ;
- l’échelle choisie ;
- le format de papier.

---

## 16. Interface bilingue

Farde comprend deux langues :

- français ;
- anglais.

Le changement de langue s’effectue dans les réglages.

La traduction concerne notamment :

- les boutons ;
- les panneaux ;
- les titres ;
- les intercalaires ;
- les descriptions ;
- le manuel ;
- les messages d’interface.

Le nom public **Farde** reste identique dans les deux langues.

Le sous-titre devient :

- **Bible graphique** en français ;
- **Visual bible** en anglais.

---

## 17. Annulation et rétablissement

L’application conserve un historique des modifications principales.

Cet historique permet :

- d’annuler une action ;
- de rétablir une action annulée.

L’historique concerne l’état du projet et non le contenu interne du navigateur.

---

## 18. Responsive design

L’interface s’adapte aux différentes largeurs d’écran.

Sur les écrans étroits :

- les commandes sont réorganisées ;
- certains boutons sont réduits à leur symbole ;
- les fonctions secondaires sont regroupées dans les réglages ;
- la grille de fiches s’adapte à l’espace disponible ;
- les panneaux restent accessibles au tactile.

---

## 19. Dépendances

Farde est conçu comme un fichier autonome.

Les bibliothèques nécessaires aux exports peuvent être intégrées directement ou chargées par le programme selon la version distribuée.

L’objectif reste de limiter les dépendances et de conserver un fonctionnement aussi autonome que possible.

---

## 20. Sécurité et confidentialité

Farde ne nécessite pas de compte utilisateur.

Les projets restent dans le navigateur tant qu’ils ne sont pas exportés.

Aucune transmission distante n’est prévue par l’architecture de base.

Les URL d’images externes dépendent toutefois des serveurs qui les hébergent.

L’utilisateur reste responsable :

- des images importées ;
- des droits associés aux contenus ;
- de la conservation de ses archives ;
- de la sauvegarde régulière de ses projets.

---

## 21. Modification du code

Le programme est conçu pour pouvoir être étudié et modifié.

Les principales sections du code sont commentées en français et en anglais.

Pour effectuer une modification :

1. créer une copie de `farde.html` ;
2. ouvrir le fichier dans un éditeur de code ;
3. modifier la structure HTML, les styles CSS ou le JavaScript ;
4. enregistrer ;
5. ouvrir le fichier dans un navigateur ;
6. tester les imports, exports, sauvegardes et affichages responsive.

Il est recommandé de ne pas modifier les identifiants internes des intercalaires ni la structure des données sans prévoir une migration des anciens projets.

---

## 22. Compatibilité et tests

Avant publication, les tests doivent porter sur :

- Chrome ;
- Firefox ;
- Edge ;
- Safari ;
- ordinateur ;
- tablette ;
- écran étroit ;
- import d’images ;
- export ZIP ;
- restauration ZIP ;
- export PDF ;
- export HTML du diaporama ;
- passage français / anglais ;
- stockage local ;
- références liées ;
- navigation clavier et tactile.

---

## 23. Arborescence recommandée du dépôt

```text
farde/
├── farde.html
├── farde-fr.png
├── README.md
├── NOTICE.md
├── CHANGELOG.md
├── ARCHITECTURE.md
└── LICENSE
```

D’autres captures, exemples ou documents peuvent être ajoutés dans des dossiers dédiés.

---

## 24. Version documentée

Nom du programme :

**Farde**

Version stable :

**1.0.0**

Date :

**2026-08-01**

Développé par :

**Simon Léturgie**

dans le cadre de :

**Eigrutel BD Academy**

Publié par :

**Eigrutel Lab — Atelier d'outils libres pour la bande dessinée**

---

## 25. Licences

Code :

**GNU AGPL v3.0 ou version ultérieure**

Documentation et modèles :

**CC BY-SA 4.0**, sauf mention contraire.

Marques, logos et signes distinctifs **Eigrutel**, **Eigrutel Lab** et **Eigrutel BD Academy** :

**réservés**.
