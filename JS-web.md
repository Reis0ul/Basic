# JS en web

## Les sélecteurs

### Elements Selector

Pour sélectionner des éléments  du code HTML :

```javascript
var variable = document.getElementsByTagName("Nom de la balise"); // Balise HTML
var variable = document.getElementsByClassName("Nom de la class"); // Class CSS
var variable = document.getElementById("Nom de l'ID"); // ID
```



### Query Selector

Pour sélectionner des éléments dans d'autres :

```javascript
var variable = document.querySelectorAll("p"); // Séléctionne tous les para
var variable = document.querySelectorAll("#contenu p"); // Séléctionne tous les para dans balise avec id=contenu
var variable = document.querySelectorAll(".existe"); // Séléctionne tous les éléments ayant class=existe
var variable = document.querySelectorAll("#contenu > .existe"); // Séléctionne tous les éléments fis avec id=contenu ayant .existe
var variable = document.querySelector("p"); // Séléctionne le first para
```

| Nb éléments à avoir | Critères de sélection         | Méthode opti                  |
| ------------------- | ----------------------------- | ----------------------------- |
| Pls                 | balise                        | ```getElementsByTagName```    |
| Pls                 | class                         | ```getElementsByeClassName``` |
| Pls                 | autre que balise ou par class | ```querySelectorAll```        |
| un seul             | par ID                        | ```getElementById```          |
| le premier          | autre que ID                  | ```querySelector```           |



## Contenu HTML

### Contenu complet balise

Propriété ```.innerHTML``` permet de récup tt le contenu HTML

-> ```.textContent``` idem que `innerHTML` mais sans le balisage HTML



### Contenu détaillé balise

`.getAttribute` méthode pour avoir attribut d'une balise ex :

```javascript
var variable = document.querySelector("a").getAttribute("href") // Donne le lien contenu dans la première balise a de la page
```

Pour les attributs href, id et value pas besoin d'écrire `getAttribute`avant :

````javascript
var variable = document.querySelector("a").href // idem que le premier
````

Pour vérifier la présence d'attribut dans une balise choisie : 

````javascript
if (document.querySelector("a").hasAttribute("target")) {
    console.log("Il y a un attribut target");
} else {
    console.log("Il n'y a pas d'attribut target");
}
````



 ### Contenu class balise

`classList` récup liste class d'un élément (fonctionne comme un tableau) :

```javascript
var variable = document.getElementById("antique").classList;
console.log(variable.lenght); // Affiche le nb de class de balise ayant Id=antique
console.log(variable[0]); // Affiche le nom de la first class
```

Pour vérifier la présence d'une class dans une balise choisie : 

```javascript
if (document.getElementById("antique").classList.contains("merveille")) {
    console.log("L'élément possède une class=merveille");
} else {
    console.log("L'élément ne possède pas une class=merveille");
}
```



## Modifier la structure

### Modifier élément existant

#### HTML

Pour ajouter un élément dans une balise : 

````javascript
document.getElementById("langages").innerHTML += '<li id="c">C</li>'; // Ajoute dans la balise ac id=langages le code <li id="c">C</li>
````

Pour vider les éléments dans une balise :

````javascript
document.getElementById("langages").innerHTML = "";
````

#### Texte

Pour ajouter du texte contenu dans une balise :

```javascript
document.querySelector("h1").textContent += " ajout de texte"; // Ajoute à la fin du premier titre h1 " ajout de texte" à celui déjà existant
```

#### Attributs

Pour ajouter un attribut à une balise :

```javascript
document.querySelector("h1").setAttribute("id", "titre"); // Ajoute au first titre h1 l'attribut id="titre"
```

On peut réduire pour certains attribut comme id, href, value :

```javascript
document.querySelector("h1").id = "titre"; // Ajoute au first titre h1 l'attribut id="titre"
```

#### Class

Pour ajouter /supprimer class de balise :

```javascript
var titreClass = document.querySelector("h1"); // Sélectionne first h1
titreClass.classList.remove("debut"); // Supprime class "debut"
titreClass.classList.add("fin"); // Add class "fin"
```

### Ajouter élément

1.  Pour créer un élément HTML, lui donner attribut et texte :

````javascript
var newElt = document.createElement("li"); // Création élément li
newElt.id = "identifiant"; // Donne un id au li
newElt.textContent = "texte affiché"; // Donne contenu textuel
document.getElementById("langages").appendChild(newElt); // Insert newElt dans la balise dans id ="langages"
// résultat :
// <... id="langages">...
// 		<li id="identifiant"> texte affiché</li>
// </>
````

2.  Pour créer un élément HTML, lui donner attribut et texte  :

```javascript
var newElt = document.createElement("li"); // Création élément li
newElt.id = "identifiant"; // Donne un id au li
newElt.appendChild(document.createTextNode("texte affiché")); // Donne contenu textuel
document.getElementById("langages").appendChild(newElt); // Insert newElt dans la balise dans id ="langages"
// résultat :
// <... id="langages">...
// 		<li id="identifiant"> texte affiché</li>
// </>
```

Pour créer un élément HTML, lui donner attribut et texte avant / après un autre :

````javascript
var perlElt = document.createElement("li"); // Création d'un élément li
perlElt.id = "perl"; // Définition de son identifiant
perlElt.textContent = "Perl"; // Définition de son contenu textuel
// Ajout du nouvel élément avant l'identifiant identifié par "php"
document.getElementById("langages").insertBefore(perlElt, document.getElementById("identifiant")); // insertAfter pour après
````

il y a méthode plus compact avec insertion bcp plus précise :

````javascript
document.gerElementById("langages").insertAdjacentHTML("afterbegin", '<li id="javascript">JS</li>'); // Création élément li ac id="javascript" contenant JS dans la balise ayany id="langages" étant avant le first child
````

il y a aussi :

-    `beforebegin` : avant élément existant lui-même
-   `afterbegin` :  premier enfant de la balise sélectionné
-   `beforeend` : après le dernier child
-   `afterend` : après l'élément existant lui-même



## Modifier un noeud

### Remplacer nœud

Pour remplacer éléments :

```javascript
var bashElt = document.createElement("li"); // Création d'un élément li
bashElt.id = "bash"; // Définition de son identifiant
bashElt.textContent = "Bash"; // Définition de son contenu textuel
// Remplacement de l'élément identifié par "identifiant" par le nouvel élément
document.getElementById("langages").replaceChild(bashElt, document.getElementById("identifiant"));
```

### Supprimer nœud

```javascript
document.gerElementById("langages").removeChild(document.getElementById("bash")); // Supprime élément ayant id="bash"
```

Il faut mieux ne pas abuser de la création d'élément de cette manière car demande bcp de ressources



## Style CSS sur JS

### Modifier style d'un élément

#### Fn `style`

```javascript
var pElt = document.querySelector("p");
pElt.style.color = "red"; // On met style avant la propriété CSS
pElt.style.margin = "50px";
pElt.style.fontFamily = "Arial"; // Pour propriété ac tiret : on enlève tiret et add maj au 2e mot (convention camelCase)
```

#### Fn `getComputedStyle` pour afficher style

```javascript
var stylePara = getComputedStyle(document.getElementById("para"));
console.log(stylePara.fontStyle); // Affiche les style des elts ayant id="para" 
```



## Evènement

### Pour créer un event à l'appui d'un bouton :

```javascript
function clicBouton() {
    console.log("Bouton cliqué");
}
var boutonElt = document.getElementById("bouton");
boutonElt.addEventListener("click", clicBouton); // Qd appuie sur Btn, appelle Fn ClickBouton
// autre méthode 
boutonElt.addEventListener("click", function () {
    console.log("Bouton cliqué");
});
// dernière méthode
// sur HTML code : <button onclick="clicBouton">Cliquez-moi</button>
function clicBouton() {
    console.log("Bouton cliqué");
    // Dès que bouton cliqué : appel de la Fn
}
```

### Pour supprimer un event d'un bouton :

```javascript
boutonElm.removeEventListener("click", clicBouton); // Enlève event clicBouton de l'activation du bouton
```

### Pour avoir des infos sur event d'un élément :

#### Bouton (basique) :

````javascript
doncument.getElementById("bouton").addEventListener("click", function (e) {
    console.log("Event : " + e.type + ", " + e.target.textContent);
    // Affiche "Event : (nom de l'event (ici click)), (le txt de l'élément HTML)"
});
````

#### Souris (complet) :

Avec les coordonnées et tout ça voir [ça](https://openclassrooms.com/fr/courses/3306901-creez-des-pages-web-interactives-avec-javascript/3545746-reagissez-a-des-evenements#/id/r-3678951)

#### Clavier (basique) :

````javascript
document.addEventListener("keypress", function (e) {
    console.log("Vous appuyé sur la touche : " + String.fromCharCode(e.charCode)); // Transforme le unicode en caractère
    // Affiche "Vous avez appuyé sur la touche : (lettre de la touche)
});
````

#### Clavier (complet) :

```javascript
function InfoClavier(e) {
    console.log("Event clavier : " + e.type + " touche : " + e.keyCode);
}
document.addEventListener("keydown", InfoClavier); // Lancé qd appuie sur touche
document.addEventListener("keyup", IndoClavier); // Lancé qd relachement touche
```

L'appuie d'une touche en JS suit la logique keydown > keypress > keyup

#### Chargement page

````javascript
window.addEventListener("load", function () {
    console.log("Page entièrement chargée");
    // Affiche "Page entièrement chargée qd page chargée
});
````

#### Fermeture page

````javascript
window.addEventListener("beforeunload", function (e) {
    var message = "On est bien ici";
    e.returnValue = message; // Affiche demande de confirmation de sortie (standard)
    return message; // Affiche demande de confirmation de sortie (certains nav)
})
````

## Formulaire

*On utilisera ce code pour la partie suivante :*

````html
<label for="pseudo">Pseudo</label> :
<input type="text" name="pseudo" id="pseudo" required>
<span id="aidePseudo"></span>
````



### Pour remplir le champ d'une zone de txt :

````javascript
var pseudoElt = document.GetElementById("pseudo");
pseudoElt.value = "MonPseudo";
// Affiche "MonPseudo" dans la zone de txt ayant id="pseudo"
````

### Focus :

````javascript
pseudoElt.addEventListener("focus", function() {
    document.GetElementById("aidePseudo").textContent = "Entrer votre pseudo";
}); // Affiche le txt qd focus sur form "pseudo"
pseudoElt.addEventListener("blur", function () {
    document.GetElementById("aidePseudo").textContent = "";
}); // Vide le txt qd focus perdu sur form "pseudo"
````

*On utilisera ce code pour la partie suivante :*

````html
<label for="mdp">Mot de passe</label> :
<input type="password" name="mdp" id="mdp" required>
<span id="aideMdp"></span>
````

Pour afficher en temps réel des choses en Fn de la saisie :

````javascript
document.GetElementById("mdp").addEventListener("input", function () {
    var mdp = e.target.value;
    var longueurMdp = "faible";
    var couleurMsg = "red";
    if (mdp.lenght >= 8) {
        longueurMdp = "suffisante";
        couleurMsg = "green";
    } else if {
        longueurMdp = "moyenne";
        couleurMsg = "orange";
    }
    var aideMdpElt = document.GetElmentById("aideMdp");
    aideMdpElt.textContent = "Longueur" + longueurMdp;
    aideMdpElt.style.color = couleurMsg;
})
````

### Validation :



