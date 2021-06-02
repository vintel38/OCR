# OCR
Répertoire pour explorer les capacités du Tesseract OCR (Optical Character Recognition) de Google

## Pytesseract 

Le package Pytesseract est un wrapper de [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) qui permet d'utiliser ses fonctions écrites en C+ dans le langage Python. Quelques prétraitements sont cependant nécessaires avant de laisser [Pytesseract](https://github.com/madmaze/pytesseract) effectuer la détection. Après quelques essais sur des images brutes entières avec une bonne luminosité, il semble effectivement que Pytesseract ait besoin de d'un peu d'aide pour effectuer correctement la détection. 

Une séquence possible de prétraitements possibles d'après la vidéo du [TheAIGuy](https://www.youtube.com/watch?v=AAPZLK41rek) :


Opération # | Transformation | Code
:----------:|:--------------|:--------
1 | Convertir l'image en nuances de gris |`cv2.cvtColor(img , cv2.color_BGR2GRAY)`
2 | Augmenter la taille comme Pytesseract est optimisé pour une image de taille donnée | `cv2.resize`
3 | Appliquer un premier flou Gaussien | `cv2.GaussianBlur`
4 | Seuillage Otsu | `cv2.threshold(img, cv2.THRESH_OTSU)`
5 | Dilatation pour trouver facilement le contour | `cv2.get_StructuringElement & cv2.dilatation`
6 | Trouver les contours de chaque élément | `cv2.findContours`
7 | Diverses vérifications sur la taille, forme des objets| 
8 | Recadrer chaque lettre ou chiffre | Sur les index de l'image 
9 | Inverser les couleurs blanches et noires | `cv2.bitwise_not`
10 | Appliquer un nouveau flou | `cv2.median_Blur`
11 | Utiliser Pytesseract | `image_to_string or image_to_data`

La vitesse de fonctionnement est limitée par Tesseract en terme de complexité temporelle. Une bonne idée est de stocker uniquement la plaque d'immatriculation détectée et cela seulement un nombre raisonnable de fois par seconde (<< 50 FPS).