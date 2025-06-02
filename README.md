# ArdoiseNumerique
Ce projet a été effectué dans le cadre de l'UE442-ENS d'Informatique embarquée du M1E3A de l'ENS Paris Saclay.

---
## Organisation des tâches 
**Queues**<br>
Un certain nombre de queues sont utilisées pour communiquer entre les tâches:
- ```CouleurAppuyee```
- ```Ecran```
- ```ModeAppuyee```
- ```ToucheAppuyee```
- ```FlecheDroite```
- ```FlecheGauche```
- ```MenuGauche```
- ```MenuDroit```
**Tâches**<br>
L'organisation est répartie en 4 tâches:
- ```TacheAppui``` : se charge de la détection de l'appui sur l'écran<br>
   - Si on touche l'écran blanc (zone de dessin) <br>→ Envoie les coordonnées touchées dans la queue ```Ecran```
  - Si on touche les touches couleurs du menu de droite <br>→ Envoie la structure ```Touche``` correspondant au bouton appuyé dans la queue ```CouleurAppuyee```
  - Si on touche les touches mode de dessin du menu de gauche <br>→ Envoie la structure ```Touche``` correspondant au bouton appuyé dans la queue ```ModeAppuyee```
  - Si on touche la touche *Enregistrer* ou la zone de saisie du nom de fichier <br>→ Envoie la structure ```Touche``` correspondant au bouton appuyé dans la queue ```ToucheAppuyee```
  - Si on touche les flèches du menu déroulant de gauche / droite <br>→ Envoie la structure ```Touche``` correspondant au bouton flèche appuyé dans la queue ```FlecheGauche```/```FlecheDroite```

- ```TacheMode```: se charge de tout l'affichage
-  ```TacheCouleur```: se charge de tout l'affichage
- ```TacheDisplay```: se charge de tout l'affichage

**Structures**<br>
Des structures supplémentaires ont été implémentées:
- ```Touche```: comprend le nom, le coin supérieur gauche x, le coin supérieur gauche y et la hauteur du bouton
- ```Appui```: comprend les coordonnées x et y 
## Schéma synoptique



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
  - ligne (non implémentée)
  - 
L'interface permet à l'utilisateur de sélectionner dans le menu déroulant de droite, la couleur d'écriture associée.
(non implémenté) Sur la barre en haut , il est supposé saisir sur son clavier comment il souhaite nommer le fichier qui dans l'idée était supposé être enregistré sur la carte SD.

## touche artistique 
