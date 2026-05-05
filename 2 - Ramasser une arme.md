# Fiche de Cours : Système de Ramassage d'Arme (UE5)

**Objectif :** Apprendre à migrer et adapter le système d'équipement d'arme du template First Person vers votre propre projet "Stealth Game".

---

## Introduction
Dans Unreal Engine 5, le template "First Person" utilise un système de **Actor Component** pour gérer les armes. Cela permet de séparer la logique du personnage de celle de l'équipement. Nous allons dupliquer ce système pour l'intégrer proprement à notre dossier de travail.

---

## 1. Duplication et Organisation
Avant de modifier le code, nous devons isoler les ressources pour ne pas impacter le template d'origine.

1. Dans le **Content Browser**, allez dans : `All > Content > FirstPerson > Blueprints`.
2. Localisez les deux fichiers suivants :
   * `BP_PickupRifle` (Le socle au sol)
   * `BP_Weapon_Component` (La logique de tir et l'animation)
3. Sélectionnez-les, faites un clic droit et choisissez **Duplicate**.
4. Renommez-les immédiatement :
   * `BP_PickupRifle_Stealthgame`
   * `BP_Weapon_Component_stealthgame`
5. Déplacez ces deux copies dans votre dossier personnel (ex: `Content/StealthGame/Blueprints`).

---

## 2. Modification des Éléments

### A. Configuration du Pickup (BP_PickupRifle_Stealthgame)
Cet objet est celui qui détecte la collision avec le joueur pour lui donner l'arme.

1. Ouvrez `BP_PickupRifle_Stealthgame`.
2. Allez dans l'**Event Graph**.
3. Repérez le nœud **Add Weapon Component**. 
4. Dans le menu déroulant **Weapon Component Class**, remplacez l'ancienne classe par votre nouvelle classe : `BP_Weapon_Component_stealthgame`.
5. *Note :* Si vous utilisez un personnage personnalisé (différent du FirstPersonCharacter par défaut), assurez-vous que le "Cast to" en début de chaîne correspond bien à votre classe de personnage.

### B. Configuration du Component (BP_Weapon_Component_stealthgame)
C'est ici que se trouve la logique de tir et l'attachement au personnage.

1. Ouvrez `BP_Weapon_Component_stealthgame`.
2. Dans l'**Event Graph**, cherchez l'événement **Attach Weapon**.
3. Vérifiez le nœud **Cast to FirstPersonCharacter**. 
   * *Si vous utilisez votre propre Blueprint de personnage :* Supprimez ce nœud et remplacez-le par un Cast vers votre Blueprint (ex: `Cast to BP_MonPerso`).
4. Rebranchez les sorties du Cast vers les nœuds suivants (Target, Character, etc.).
5. Compilez et Sauvegardez.

---

## 3. Mise en place et Test

1. Glissez-déposez le `BP_PickupRifle_Stealthgame` depuis votre dossier vers le niveau de jeu.
2. Appuyez sur **Play**.
3. Marchez sur l'arme :
   * L'arme doit disparaître du sol.
   * Elle doit s'attacher aux mains de votre personnage.
   * Vous devez pouvoir tirer avec le clic gauche.
