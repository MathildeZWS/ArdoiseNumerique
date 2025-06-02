# ArdoiseNumerique
Ce projet a été effectué dans le cadre de l'UE442-ENS d'Informatique embarquée du M1E3A de l'ENS Paris Saclay.
---
## Interface Ecran
## Fonctionnalités souhaitées
L'interface permet à l'utilisateur de sélectionner dans le menu déroulant de gauche:
- Le mode d'écriture :
  - Stylo
  - Gomme
  - ligne (non implémenté)
  - écriture de texte (non implémenté)
- La taille du stylo :
  - Stylo
  - Gomme
  - ligne (non implémentée)
 
- Le mode dessin de contours de formes :
  - Stylo
  - Gomme
  - ligne (non implémentée)
 
- Le mode dessin de formes remplies :
  - Stylo
  - Gomme
  - ligne (non implémentée) <br>
L'interface permet à l'utilisateur de sélectionner dans le menu déroulant de droite, la couleur d'écriture associée.<br>
(non implémenté) Sur la barre en haut , il est supposé saisir sur son clavier comment il souhaite nommer le fichier qui dans l'idée était supposé être enregistré sur la carte SD.
---
## Organisation des tâches 
**Structures**<br>
Des structures supplémentaires ont été implémentées:
- ```Touche```: comprend le nom, le coin supérieur gauche x, le coin supérieur gauche y et la hauteur du bouton
- ```Appui```: comprend les coordonnées x et y <br>
<br>**Queues**<br>
Un certain nombre de queues sont utilisées pour communiquer entre les tâches:
- ```CouleurAppuyee``` : struc```Touche```
- ```Ecran```: struc```Appui```
- ```ModeAppuyee```: struc```Touche```
- ```ToucheAppuyee```: struc```Touche```
- ```FlecheDroite```: struc```Touche```
- ```FlecheGauche```: struc```Touche```
- ```MenuGauche```
- ```MenuDroit```
- ```CouleurSelectionnee```: struc```Touche```
- ```Color```
- ```Mode```
- ```TailleStylo```<br>
<br>**Tâches**<br>
L'organisation est répartie en 4 tâches:
- ```TacheAppui```(```SelectionMode```) : se charge de la détection de l'appui sur l'écran<br>
   - Si on touche l'écran blanc (zone de dessin) <br>→ Envoie les coordonnées touchées dans la queue ```Ecran```
  - Si on touche les touches couleurs du menu de droite <br>→ Envoie la structure ```Touche``` correspondant au bouton appuyé dans la queue ```CouleurAppuyee```
  - Si on touche les touches mode de dessin du menu de gauche <br>→ Envoie la structure ```Touche``` correspondant au bouton appuyé dans la queue ```ModeAppuyee```
  - Si on touche la touche *Enregistrer* ou la zone de saisie du nom de fichier <br>→ Envoie la structure ```Touche``` correspondant au bouton appuyé dans la queue ```ToucheAppuyee```
  - Si on touche les flèches du menu déroulant de gauche / droite <br>→ Envoie la structure ```Touche``` correspondant au bouton flèche appuyé dans la queue ```FlecheGauche```/```FlecheDroite```

- ```TacheMode```: se charge d'agir en conséquence quand on a appuyé sur les flèches du menu de gauche ou les cases de sélection du mode <br>
Reçoit les queues ```FlecheGauche``` et ```ModeAppuyee```.
  - appui d'une flèche <br> → fait changer le numéro du menu_gauche et l'envoie dans la queue ```MenuGauche```
  - appui d'un mode <br> → envoie le numéro de la case associée dans la queue  ```Mode ```
    - Si on est dans le mode 1 (taille du stylo), on envoie la taille choisie dans la queue  ```TailleStylo ```
      
-  ```TacheCouleur```: se charge d'agir en conséquence quand on a appuyé sur les flèches du menu de droite ou les cases de sélection de la couleur <br>
Reçoit les queues ```FlecheDroite``` et ```CouleurAppuyee```.
  - - appui d'une flèche <br> → fait changer le numéro du menu_droit et l'envoie dans la queue ```MenuDroit```
  - - appui d'une couleur <br> → envoie le numéro de la case associée dans la queue  ```Color ```
    <br> → envoie le numéro de liste de couleurs dans la queue  ```CouleurSelectionnee```
   
- ```TacheDisplay```: se charge de tout l'affichage <br>
Affiche un affichage initial au premier lancement de la tâche.<br>
Reçoit ```MenuGauche```, ```Mode```, ```Color```
- ```MenuGauche``` reçu : <br>
→ affiche le menu correspondant
- ```Mode``` reçu : <br>
→ grise la case mode correspondante

## Schéma synoptique





## touche artistique 
