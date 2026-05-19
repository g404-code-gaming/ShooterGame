# Système de Ramassage d'Arme

**Objectif :** Apprendre à migrer et adapter le système d'équipement d'arme du template First Person vers le propre projet personnel.

![image 1](/Images/2_debut.png)

---

## Introduction

Dans Unreal Engine 5, le template "First Person" utilise un système de **Actor Component** pour gérer les armes. Cela permet de séparer la logique du personnage de celle de l'équipement. Nous allons dupliquer ce système pour l'intégrer proprement à notre dossier de travail.

---

## 1. Duplication et Organisation
Avant de modifier le code, nous devons isoler les ressources pour ne pas impacter le template d'origine.

1. Dans le **Content Browser**, allez dans : `All > Content > FirstPerson > Blueprints`.
2. Localisez les deux fichiers suivants, puis dupliquez-les :
   * `BP_PickupRifle` 
   * `BP_Weapon_Component`

![image 2](/Images/2_duplique.png)

4. Renommez-les immédiatement :
   * `BP_PickupRifle_Stealthgame`
   * `BP_Weapon_Component_stealthgame`
5. Déplacez ces deux copies dans votre dossier personnel (ex: `Content/StealthGame/Blueprints`).

---

## 2. Modification des Éléments

### A. Configuration du Pickup (BP_PickupRifle_Stealthgame)
Cet objet est celui qui détecte la collision avec le joueur pour lui donner l'arme.

Dans `BP_PickupRifle_Stealthgame`, modifiez tout les nodes qui ne sont pas adapté à votre personnage : 
1. Cast to FirstPersonCharacter ==> Cast to StealthgameCharacter
2. Rebranchez les sorties du Cast vers les nœuds suivants (Target, Character, etc.).

![image 3](/Images/2_pickuprifle1.png)

3. Cast to BP_Weapon_Component ==> Cast to BP_Weapon_Component_stealthgame.
4. Add BP_weapon_Component ==> BP_weapon_Component_stealthgame.

![image 4](/Images/2_pickuprifle2.png)

### B. Configuration du Component (BP_Weapon_Component_stealthgame)
C'est ici que se trouve la logique de tir et l'attachement au personnage.

Dans `BP_Weapon_Component_stealthgame`, modifiez tout les nodes qui ne sont pas adapté à votre personnage : 

1. Cast to FirstPersonCharacter ==> Cast to StealthgameCharacter
2. Rebranchez les sorties du Cast vers les nœuds suivants (Target, Character, etc.).

![image 5](/Images/2_weaponcomponent.png)

---

## 3. Mise en place et Test

1. Glissez-déposez le `BP_PickupRifle_Stealthgame` depuis votre dossier vers le niveau de jeu.

2. Lancez le jeu et marcher sur votre arme.

Si tout fonctionne, l'arme disparait, et une arme apparait dans les mains de votre personnage, vous permettant de tirer des balles avec le clic gauche de la souris.

![image 5](/Images/2_fin.png)

## 4. Personnalisation 

### Modifier le projectile 

Si vous souhaitez modifier votre projectile, il faut créer votre propre blueprint de `balle` (vous pouvez duppliquer celle du projectile déjà existant).  

Il est possible de modifier l'apparence du projectiel dans le blueprint du Weapon_Component : 

![image balle](/Images/2_balle.png)

Pour empécher le projectile de rebondir, il est possible de supprimer le code par défaut d'Unreal Engine. 

![image balle](/Images/2_ballesupr.png)
