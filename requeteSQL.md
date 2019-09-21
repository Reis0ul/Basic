# MCD utilisé pendant le cours

![MCD utilisé pendant les exercices](C:\Users\hippo\Documents\MMI\BdD\SQL\Capture.PNG)

# Requête 1 :

## Donner le nom et le prénom de tous les instituteurs :

```mysql
SELECT instituteur.nominstit, instituteur.preinstit
FROM instituteur ;
```

# Requête 2 :

## Donner le nom et le prénom des instituteurs qui travaillent dans la classe de CM2 :

```mysql
SELECT instituteur.nominstit, instituteur.preinstit
FROM instituteur
WHERE instituteur.classe = 'CM2';
-- Sortie: Jean severe, marc travail, albert topaze 
```

# Requête 3 :

## Donner le nom et le prénom des élèves qui ont l'instituteur dont le nom est severe :

### PRODUIT CARTÉSIEN SOUS SQL AVEC LE JOKER ÉTOILE :

```mysql
SELECT eleve.nomeleve, eleve.preeleve -- -> projection
FROM eleve, instituteur
WHERE eleve.numinstit = instituteur.numinstit -- -> jointure
AND instituteur.nominstit = 'severe'; -- -> restriction
```

```mysql
SELECT eleve.nomeleve, eleve.preeleve -- -> projection
FROM eleve, instituteur
WHERE eleve.numinstit = instituteur.numinstit -- -> jointure
AND instituteur.nominstit = 'severe'; -- -> restriction
-- Sortie: durand pollux, dubois marguerite
```

**Dans ce cas, on ne garde que les lignes justes. On parle de jointure et d'une projection**

# Requête 4 :

## Donner le nom et le prénom des élèves qui dépendent de l'inspecteur dont le nom est gadget :

```mysql
SELECT eleve.nomeleve, eleve.preeleve
FROM eleve, instituteur, ecole, inspecteur
WHERE eleve.numinstit = instituteur.numinstit
AND instituteur.codeeco = ecole.codeeco
AND ecole.numinspect = inspecteur.numinspect
AND inspecteur.nominspect = 'gadget';
-- Sortie: Durant pollux, Dubois Marguerite
```


# Requête 5 :

## Donner le nom et le prénom des élèves qui dépendent de l'inspecteur dont le nom est duval :

```mysql
SELECT eleve.nomeleve, eleve.preeleve
FROM eleve, instituteur, ecole, inspecteur
WHERE eleve.numinstit = instituteur.numinstit
AND instituteur.codeeco = ecole.codeeco
AND ecole.numinspect = inspecteur.numinspect
AND inspecteur.nominspect = 'duval';
```


# Requête 6 :

## Donner le nom et le prénom, ainsi que le jour, des élèves qui participent à l'atelier dont le nom est peinture.

```mysql
SELECT eleve.nomeleve, eleve.preeleve, participe.jour
FROM eleve, atelier, participe
WHERE  eleve.numeleve = participe.numeleve
AND participe.codeatel = atelier.codeatel
AND atelier.nomatel = 'peinture';
-- Sortie: martin, jacques, jeudi
```



# Requête 7 :

## Donner le nom et le prénom des élèves qui ont un instituteur qui a obtenu une note supérieure ou égale à 12 dans la spécialité dont le libellé est "dessin"

```mysql
SELECT eleve.nomeleve, eleve.preeleve
FROM specialite, inspection, instituteur, eleve
WHERE specialite.codesp = inspection.codesp
AND inspection.numinstit = instituteur.numinstit
AND instituteur.numinstit = eleve.numinstit
AND specialite.libelle = 'dessin'
AND inspection.note >= 12;
```



# Les fonctions:

### COUNT : Compte le nombre d'enregistrements dans une table ou une requête

### MAX : Extrait la valeur maximum dans une table ou une requête

### MIN : Extrait la valeur minimum dans une table ou une requête

### AVG : Calcule la moyenne d'un champ dans une table ou une requête

### SUM : Calcule la somme d'un champ dans une table ou une requête

**On ne peut utiliser une fonction que dans une projection (SELECT)**

## Exercices :

### Calculer la moyenne des notes dans inspection :

```mysql
SELECT AVG(inspection.note)
FROM inspection;
-- Sortie: 13.8
```

### Donner le nb d'élève de l'instituteur M. Sévère:

```mysql
SELECT COUNT(Eleve.Numeleve)
FROM Eleve, Instituteur
WHERE 
```



# Exercice 1 :

## Donner le nom et le prénom de l'instituteur qui a obtenu la meilleure note:

```mysql
SELECT MAX(inspection.note)
FROM inspection; 
-- Sortie: 16
```

## Donner le nom et prénom de l'instituteur qui a obtenu la note de 16

```mysql
SELECT instituteur.nominstit, instituteur.preinstit
FROM instituteur, inspection
WHERE inspection.numinstit = instituteur.numinstit
AND inspection.note = 16;

```



## Donner le nom et prénom de l'instituteur qui a obtenu la meilleure note

```mysql
SELECT instituteur.nominstit, instituteur.preinstit
FROM instituteur, inspection
WHERE inspection.numinstit = instituteur.numinstit
AND inspection.note = (
    SELECT MAX(inspection.note) 
    FROM inspection);
-- SOrtie: Punition Pierre
```

**On a réalisé l'imbrication de deux requêtes SQL** 



```mysql
SELECT instituteur.preinstit, insituteur.nominstit
FROM Insituteur, inspection
WHERE Isntituteur.numinstit = inspection.numinstit
AND inspection.note = (
    SELECT MIN(inspection.note)
    FROM inspection);
-- Sortie: Severe Jean
```





 # Exercice 2 :


## Donner le nom et le prénom des instituteurs qui ont obtenu une note supérieure ou égale à la moyenne des notes.

*Il y a un petit soucis d'affichage ......... Lequel ? Certains noms s'affichent 2 fois*

```mysql
SELECT DISTINC instituteur.nominstit, instituteur.preinstit
FROM instituteur, inspection
WHERE instituteur.numinstit = inspection.numinstit
AND inspection.note >= (SELECT AVG(inspection.note) FROM inspection);
-- DISTINC dans la projection permet de supprimer les doublons
```



# Exercice 3 :

## Vérifier par le calcul dans une requête que la moyenne fonctionne

SELECT (SUM(inspection.note) / COUNT(inspection.note))
FROM inspection;

# Exercice 4 :

## Réaliser le calcul de la moyenne pondérée dans inspection

SELECT SUM(inspection.note * inspection.coef) / SUM(inspection.coef)
FROM inspection;

# Exercice 5 :

## Donner le nom et le prénom des instituteurs qui n'ont jamais été inspectés

SELECT instituteur.nominstit, instituteur.preinstit
FROM instituteur
WHERE instituteur.numinstit <>  (SELECT inspection.numinstit
FROM inspection);
On ne peut pas utiliser un opérateur de comparaison si la sous-requête renvoie une liste de données.
SELECT instituteur.nominstit, instituteur.preinstit
FROM instituteur
WHERE instituteur.numinstit not in  (SELECT inspection.numinstit
FROM inspection);
On parler de soustraction. Tous les instits de la table instituteurs
moins les instits de la table inspection.

# Exercice 6 :

## Donner le nom et le prénom des élèves qui ne font pas parti d'un atelier.

```mysql
SELECT eleve.nomeleve, eleve.preeleve
FROM eleve
WHERE eleve.numeleve not in (
    SELECT participe.numeleve 
    FROM participe);
```





# Exercice 7 :

## Donner le nom et le prénom des inspecteurs qui n'ont de responsabilités dans une école

SELECT inspecteur.nominspect, inspecteur.preinspect
FROM inspecteur
WHERE inspecteur.numinspect not in (SELECT ecole.numinspect FROM ecole);

# Exercice 8 :

## Donner le numéro des instituteurs ainsi que leur moyenne.

SELECT inspection.numinstit, avg(inspection.note)
FROM  inspection
GROUP BY inspection.numinstit;
1 --> Il prend la table inspection
2 --> Il créé des paquets sur numinstit
3 --> Pour chaque paquets, il affiche le numéro de l'instit et sa moyenne du paquet