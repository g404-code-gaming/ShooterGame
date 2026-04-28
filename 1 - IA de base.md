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

### 2.1. Logique de détection (Poursuite)
1. Allez dans l'**Event Graph** de `PNJ_Attaquant`.
2. Sélectionnez le composant **Pawn Sensing** dans la liste à gauche.
3. Dans le panneau Details, cliquez sur le "+" vert à côté de l'événement **On See Pawn**.
4. Reliez le nœud à un **AI MoveTo** :
   - **Pawn :** Self
   - **Target Actor :** Reliez-le à la sortie "Pawn" de l'événement de détection.

### 2.2. Gestion du contact (Game Over)
1. Dans les **Components**, ajoutez une **Capsule Collision**.
2. Placez-la devant le mesh de l'IA (zone d'attaque).
3. Faites un clic droit sur cette capsule > **Add Event** > **On Component Begin Overlap**.
4. **Logique :**
   - Utilisez un **Cast to BP_ThirdPersonCharacter** (ou votre classe de joueur).
   - Si le cast réussit, appelez le nœud **Open Level (by Name)** en entrant le nom de votre map actuelle pour recommencer la partie.

---

## Chapitre 3 : Système de Patrouille

Pour éviter que l'IA ne reste statique, nous allons créer un itinéraire.

### 3.1. Variables nécessaires
Créez les variables suivantes dans le Blueprint :

- `Patrouilleur` (Boolean) : Pour activer/désactiver le comportement.

- `IndexItineraire` (Integer) : L'étape actuelle du trajet.

- `Itineraire` (Array de type **Target Point**) : Cliquez sur l'icône "œil" pour la rendre publique (Instance Editable).

### 3.2. Création de l'événement de patrouille
1. Créez un **Custom Event** nommé `Patrouille`.
2. Ajoutez une **Branch** (condition `Patrouilleur` == True).
3. Si True, utilisez un nœud **AI MoveTo** :
   - **Destination :** `Get Actor Location` du Target Point correspondant à l'index actuel de la liste `Itineraire`.
4. Sur la sortie **On Success** du AI MoveTo :
   - Incrémentez `IndexItineraire` (+1).
   - Si l'index est supérieur ou égal à la taille du tableau, remettez-le à 0.
   - Ajoutez un **Delay** (ex: 1s) puis rappelez l'événement `Patrouille` (boucle).

### 3.3. Activation
1. À l'événement **BeginPlay**, appelez le nœud `Patrouille`.
2. **Reprise de patrouille :** Dans la logique de poursuite (Chapitre 2), ajoutez un **Delay** après la perte de vue du joueur, puis rappelez l'événement `Patrouille`.

---

## Protocole de Test
1. Placez plusieurs **Target Points** dans votre niveau.
2. Posez le `PNJ_Attaquant` dans la scène.
3. Dans le panneau **Details** de l'IA placée, remplissez le tableau `Itineraire` en sélectionnant les Target Points avec la pipette.
4. Lancez le jeu (**Play**).
