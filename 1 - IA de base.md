# 1 - Création d'une IA de Base sur Unreal Engine 5

**Objectif :** Apprendre à configurer un PNJ (Personnage Non-Joueur) capable de patrouiller de manière autonome, de détecter le joueur par la vue et d'enclencher une action de défaite au contact.

---

## Chapitre 1 : Mise en place de l'IA

### 1. Création du Blueprint

Dans le **Content Browser**, créez un dossier nommé `IA`.

![image 1](/Images/1_1_dossier.png)

Faites un clic droit > **Blueprint Class** > Sélectionnez **Character**.
Nommez-le `PNJ_Attaquant`.

![image 2](/Images/1_2_creerIA.png)

### 2. Configuration visuelle et sensorielle

Dans le Blueprint `PNJ_Attaquant`, dans l'onglet **Components**, sélectionnez le composant **Mesh**.

> Dans l'exemple, nous allons utiliser le Skeletal Mesh `SKM_Quinn`.

![image 3](/Images/1_3_mesh.png)

**Ajout de la vision :** Cliquez sur **Add** et cherchez le composant **Pawn Sensing**.

![image 4](/Images/1_4_pawnsensing.png)

   - *Réglage :* Dans les détails du Pawn Sensing, vous pouvez ajuster le `Peripheral Vision Angle` (ex: 90 degrés) pour définir le champ de vision de l'IA.

![image 5](/Images/1_5_parametre.png)

### 3. Navigation dans le niveau
Dans l'éditeur de niveau, cherchez l'acteur **Nav Mesh Bounds Volume**.

![image 6](/Images/1_6_navmesh.png)

Placez-le et redimensionnez-le pour couvrir toute la zone de déplacement de l'IA.

> **Astuce :** Appuyez sur la touche **P** pour afficher la zone de navigation (en vert).

![image 7](/Images/1_7_taillenav.png)

---

## Chapitre 2 : Poursuite et Collision

### 1. Logique de détection (Poursuite)

Dans l'**Event Graph** de `PNJ_Attaquant`, Ajouter le programme pour que le PNJ se mette à poursuivre le joueur lorsqu'il le vois. 

![image 8](/Images/1_8_poursuite.png)

Testez le jeu pour vérifier que le `PNJ_Attaquant` se dirige vers le joueur lorsque ce dernier lui passe devant.

### 2. Gestion du contact (Game Over)

Dans les **Components** de votre `PNJ_Attaquant`, ajoutez une **Capsule Collision**.

![image 9](/Images/1_9_collision.png)

Placez-là un peu en avant du personnage afin qu'il puisse plus facilement toucher le personnage du joueur en avançant vers vous. à l'inverse, il aura plus de mal à toucher un personnage de dos, permettant de le contourner plus facilement.

Ajoutez ensuite le programme pour que le niveau se recharge en cas de contact entre le `PNJ_Attaquant` et le personnage du joueur. 

![image 10](/Images/1_10_contact.png)

Testez votre jeu pour vérifier que le niveau recommence si votre personnage se fait attraper par le PNJ. 

---

## Chapitre 3 : Système de Patrouille

Pour l'instant, notre IA ne bouge que si elle détecte un ennemi : c'est une IA statique qui ne fait rien d'autre que garder une position. 

Nous allons créer évènement de patrouille au travers d'un itinéraire.

![image 11](/Images/1_11_imagepatrouille.png)

### 1. Variables nécessaires

Créez les variables suivantes dans le Blueprint de `PNJ_Attaquant`:

- `Patrouilleur` (Boolean) : Pour activer/désactiver le comportement.

- `IndexItineraire` (Integer) : L'étape actuelle du trajet.

- `Itineraire` (Array de type **Target Point**).

![image 12](/Images/1_12_variable.png)

Cliquez sur l'icône "œil" `Itineraire` et `Patrouilleur` pour les rendre publiques (Instance Editable).

### 2. Création de l'événement de patrouille

Créez un **Custom Event** nommé `Patrouille`.

Dans cet évènement, on demande au `PNJ_Attaquant` de ce déplacer en direction d'un **Target Point** de la liste `Itineraire`. Une fois qu'il a réussis, il se dirige vers le prochain, jusqu'à recommencer. 

![image 13](/Images/1_13_patrouille.png)

### 3. Activation

À l'événement **BeginPlay**, appelez le nœud `Patrouille`.

![image 14](/Images/1_14_beginplay.png)

**Reprise de patrouille :** Dans la logique de poursuite (Chapitre 2), ajoutez un **Delay** après la perte de vue du joueur, puis rappelez l'événement `Patrouille`. Cela permet de reprendre la patrouille si le joueur parviens à se cacher du PNJ.

![image 15](/Images/1_15_reprise.png)

---

## Protocole de Test
1. Placez plusieurs **Target Points** dans votre niveau.
2. Posez le `PNJ_Attaquant` dans la scène.
3. Dans le panneau **Details** de l'IA placée, cochez la variable `Patrouilleur`
4. Remplissez le tableau `Itineraire` en sélectionnant les Target Points.

![image 16](/Images/1_16_explication.png)

Testez le jeu pour vérifier que l'IA se déplace selon son itinéraire. Vérifiez aussi qu'elle vous attaque lorsqu'elle vous vois. 
