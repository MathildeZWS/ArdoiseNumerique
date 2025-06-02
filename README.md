# ArdoiseNumerique
Ce projet a été effectué dans le cadre de l'UE442-ENS d'Informatique embarquée du M1E3A de l'ENS Paris Saclay. Elaboré sur STM32IDE avec un microcontrôleur STM32G-DISCO.
---
C'est une ardoise numérique sur laquelle, on peut dessiner de la couleur souhaitée et de taille souhaitée et gommer. Les autres fonctionnalités de dessin n'ont pas été implémentées à ce jour.
---
## Interface Ecran
![PAint](https://github.com/user-attachments/assets/72bd83c8-361a-4530-afb6-fba360252926)

🔴L'affichage des image bmp ne marche pas bien.

**Menu mode/couleur possibles:**<br>
![menu](https://github.com/user-attachments/assets/49e8bad0-239b-4296-9911-1c2829765791)

**Grisage des dernières touches sélectionnées:**<br>

![artist](https://github.com/user-attachments/assets/f32b81ea-913e-4dfc-8e79-af2d84633ef1)


## Fonctionnalités souhaitées
L'interface permet à l'utilisateur de sélectionner dans le menu déroulant de gauche:
- Le mode d'écriture :
  - Stylo
  - Gomme
  - ligne verticale (🔴non implémenté)
  - ligne horizontale (🔴non implémenté)
  - ligne oblique (🔴non implémenté)
  - écriture de texte (🔴non implémenté)
    
- La taille du stylo

- Le mode dessin de contours de formes :
  - rectangle (🔴non implémenté)
  - carré (🔴non implémenté)
  - cercle (🔴non implémenté)
  - ellipse (🔴non implémenté)
  - polygone (🔴non implémenté)
  - étoile (🔴non implémenté)
 
- Le mode dessin de formes remplies :
  - rectangle (🔴non implémenté)
  - carré (🔴non implémenté)
  - cercle (🔴non implémenté)
  - ellipse (🔴non implémenté)
  - polygone (🔴non implémenté)
  - triangle (🔴non implémenté)<br> <br>
L'interface permet à l'utilisateur de sélectionner dans le menu déroulant de droite, la couleur d'écriture associée.<br> <br>
Sur la barre en haut , il est supposé saisir sur son clavier comment il souhaite nommer le fichier qui dans l'idée était supposé être enregistré sur la carte SD. (🔴 non implémenté) 
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
**Tâches**<br>
L'organisation est répartie en 4 tâches:
- ```TacheAppui```(```DetectionAppui```) : se charge de la détection de l'appui sur l'écran<br>
   - Si on touche l'écran blanc (zone de dessin) <br>→ Envoie les coordonnées touchées dans la queue ```Ecran```
  - Si on touche les touches couleurs du menu de droite <br>→ Envoie la structure ```Touche``` correspondant au bouton appuyé dans la queue ```CouleurAppuyee```
  - Si on touche les touches mode de dessin du menu de gauche <br>→ Envoie la structure ```Touche``` correspondant au bouton appuyé dans la queue ```ModeAppuyee```
  - Si on touche la touche *Enregistrer* ou la zone de saisie du nom de fichier <br>→ Envoie la structure ```Touche``` correspondant au bouton appuyé dans la queue ```ToucheAppuyee```
  - Si on touche les flèches du menu déroulant de gauche / droite <br>→ Envoie la structure ```Touche``` correspondant au bouton flèche appuyé dans la queue ```FlecheGauche```/```FlecheDroite```

- ```TacheMode```(```SelectionMode```) : se charge d'agir en conséquence quand on a appuyé sur les flèches du menu de gauche ou les cases de sélection du mode <br>
Reçoit les queues ```FlecheGauche``` et ```ModeAppuyee```.
  - appui d'une flèche <br> → fait changer le numéro du menu_gauche et l'envoie dans la queue ```MenuGauche```
  - appui d'un mode <br> → envoie le numéro de la case associée dans la queue  ```Mode ```
    - Si on est dans le mode 1 (taille du stylo), on envoie la taille choisie dans la queue  ```TailleStylo ```
      
-  ```TacheCouleur```(```Couleur```): se charge d'agir en conséquence quand on a appuyé sur les flèches du menu de droite ou les cases de sélection de la couleur <br>
Reçoit les queues ```FlecheDroite``` et ```CouleurAppuyee```.
  -  appui d'une flèche <br> → fait changer le numéro du menu_droit et l'envoie dans la queue ```MenuDroit```
  - appui d'une couleur <br> → envoie le numéro de la case associée dans la queue  ```Color ```
    <br> → envoie le numéro de liste de couleurs dans la queue  ```CouleurSelectionnee```
   
- ```TacheDisplay``` (```Affichage```): se charge de tout l'affichage <br>
Affiche un affichage initial au premier lancement de la tâche.<br>
Reçoit les queues ```MenuGauche```, ```Mode```, ```MenuDroit```, ```Color```, ```CouleurSelectionnee```, ```TailleStylo```, ```Ecran```.
  - ```MenuGauche``` reçu : <br>
→ affiche le menu mode correspondant
  - ```Mode``` reçu : <br>
→ grise la case mode correspondante
  - ```MenuDroit``` reçu : <br>
→ affiche le menu couleur correspondant
  -  ```Color``` reçu : <br>
→ grise la case couleur correspondante
  -  ```CouleurSelectionnee``` reçu : <br>
→   récupère la couleur sélectionnée
  -  ```CouleurSelectionnee``` reçu : <br>
→   récupère la couleur sélectionnée
  -  ```TailleStylo``` reçu : <br>
→   récupère la taille du stylo
   -  ```Ecran``` reçu : <br>
→   récupère les coordonnées et dessine selon le mode choisi avec la taille du stylo et la couleur choisis.
<br> 🔴 seulement le mode stylo et gomme ont été implémentés à ce jour. 🔴



## Schéma synoptique
![Synoptic](https://github.com/user-attachments/assets/eee281d0-e236-4d53-aacf-57149e8eaaf3)


