# Global RESET

\*,  
\*::after,  
\*::before {  
 margin: 0;  
 padding: 0;  
 box-sizing: border-box;  
}

---

# Homogénésier la taille de la police et des distances pour tout le site

## On pose la valeur qui nous intésse dans le root élément concernant le font-size pour après que nos valeurs en rem soient plus simples à calculer car 1rem = valeur de la font-size du root élément

/_ ATTENTION ! On ne pose pas la valeur directement en px parce que cela créé des problèmes avec les gens qui modifient la taille de la police sur leur navigateur. On va préférer passer par un pourcentage pour laisser l'option à tous de modifier la taille de la police tout en gardant la possiblité pour nous d'homogénéiser nos tailles à travers tout le site _/

html {  
 font-size: 62.5%;  
}

---

# Centrer une div (box text)

## Méthode position absolute + transform: translate()

Avec position absolute et top 50%, left 50%, la box sera centrée à partir de l'angle haut-gauche et déborera donc sur le bas droit à partir de l'élément parent. Si on veut que la box soit centrée à partir du centre de la box, on utilise transform: translate() pour bouger/rectifier la box de 50% vers le haut et la gauche sur l'élément lui même et pas l'élément parent :

.text-box {  
position: absolute;  
top: 50%;  
left: 50%;  
transform: translate(-50%, -50%);  
}

## Méthode text-align: center

Avec text-align: center sur un élément, comme un tag a, le texte dans le tag a sera centré mais si on le pose sur l'élément parent le tag a lui même sera aussi centré

Ici une div avec la class text-box qui englobe un h1 et le a:

.text-box {  
position: absolute;  
top: 50%;  
left: 50%;  
transform: translate(-50%, -50%);  
text-align: center;  
}

---

# Créer une animation

## Créer l'animation avec @keyframes

@keyframes moveInLeft {  
0% {  
opacity: 0;  
transform: translateX(-100px);  
}

80% {  
transform: translateX(10px);  
}

100% {  
opacity: 1;  
transform: translate(0);  
}  
}

## Poser l'animation créée sur l'élément à animer

.heading-primary-main {  
display: block;  
font-size: 60px;  
font-weight: 400;  
letter-spacing: 35px;

animation-name: moveInLeft;  
animation-duration: 1s;  
}

## Animation sans @keyframes

Parfois on va créer une animation sans keyframes, on va simplement poser la propriété "transition" sur l'élément initial et toutes les "transform" sur les éléments modifiés de l'élément initial (par exemple element:hover) seront activés :

.btn:link,  
.btn:visited {  
 text-decoration: none;  
 text-transform: uppercase;  
 padding: 15px 40px;  
 display: inline-block;  
 text-align: center;  
 border-radius: 5px;  
 **transition: all 0.2s;**  
}

.btn:hover {  
 **transform: translateY(-3px);**  
 **box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);**  
}

.btn:active {  
 **transform: translateY(-1px);**  
 **box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);**  
}

---

# HACK

## Supprimer petit tremblement de texte lors d'animation

.heading-primary {  
 color: #fff;  
 text-transform: uppercase;

/_ hack pour empêcher un petit tremblement du titre _/  
 backface-visibility: hidden;  
}

---

# Création d'un bouton complet avec animation et hacks

## On créé un second bouton avec la pseudo class after et on le place derrière le bouton d'origine avec position absolute (donc position relative sur le bouton initial) ET z-index : -1

.btn::after {  
content: " ";  
display: inline-block;  
height: 100%;  
width: 100%;  
border-radius: 5px;  
position: absolute;  
top: 0;  
left: 0;  
z-index: -1;  
transition: all 0.4s;  
}

.btn-white::after {  
background-color: white;  
}

## Quand on hover le bouton initial, on applique les changements voulus au second bouton

**.btn:hover::after** {  
transform: scaleX(1.4) scaleY(1.6);  
opacity: 0;  
}

.btn-animated {  
animation: moveInBottom 0.5s ease-out 0.75s;

/_ Astuce qui permet d'utiliser la valeur de l'animation à l'étape 0% AVANT de lancer l'animation. Ca veut dire dans notre cas que le bouton sera en opacity 0 AVANT de commencer l'animation vu que à la première étape on a posé opacity:0.
Ca permet de retarder l'arrivée d'un élément, ou retarder le déclenchement d'une animation sans avoir l'élément en plein milieu (ce qui est dégueulasse) _/

**animation-fill-mode: backwards;**  
}

@keyframes moveInBottom {  
0% {  
opacity: 0;  
transform: translateY(30px);  
}

80% {  
transform: translateY(0px);  
}

100% {  
opacity: 1;  
transform: translate(0);  
}  
}

---

# Hack pour avoir linear-gradient sur text

.heading-secondary {  
 font-size: 3.5rem;

display: inline-block;  
 background-image: linear-gradient(  
 to right,  
 $color-primary-light,  
 $color-primary-dark  
 );  
 **-webkit-background-clip: text;**  
 color: transparent;  
}

---

# Hack pour une image et une couleur "blend" ensemble, c'est à dire superposer sur l'image la couleur choisie

&\_\_picture {  
background-size: cover;  
height: 23rem;  
**background-blend-mode: screen;**

    &--1 {
      <!-- On ne remonte que d'un niveau car le code sera compilé de sass vers css et posé dans style.css. Or depuis style.css, le dossier img n'est que d'un niveau au-dessus -->

      background-image: linear-gradient(
          to right bottom,
          $color-secondary-light,
          $color-secondary-dark
        )
        url(../img/nat-5.jpg);
    }

---

# Hack pour donner un "padding arrondi" autour d'un élément

width: 15rem;  
height: 15rem;  
float: left;

**-webkit-shape-outside: circle(50% at 50% 50%);**
**shape-outside: circle(50% at 50% 50%);**

---

# Sibling Selector

// On utilise le sibling sélector "+" mais le second élément (label ici) DOIT être APRES (input ici) dans le html

&\_\_input:placeholder-shown + &\_\_label {  
opacity: 0;  
visibility: hidden;  
transform: translateY(-4rem);  
 }

---

# Custom Radio Button for Form

## html

\<div class="form\_\_group">  
\<div class="form\_\_radio-group">  
 \<input type="radio" class="form\_\_radio-input" id="small" name="size" \/>  
 \<label for="small" class="form\*\*radio-label">  
\<span class="form\_\_radio-button"></span>  
Small tour group</label  
    >  
\</div>  
 \<div class="form\_\_radio-group">  
\<input type="radio" class="form\_\_radio-input" id="large" name="size" />  
\<label for="large" class="form\_\_radio-label">  
\<span class="form\_\_radio-button"></span>  
Large tour group</label>  
\</div>  
\</div>

## css

.form {  
&\_\_radio-group {  
width: 49%;  
display: inline-block;  
}

&\_\_radio-input {  
 display: none;  
 }

&\_\_radio-label {  
font-size: $default-font-size;  
cursor: pointer;  
position: relative;  
padding-left: 4.5rem;  
}

&\_\_radio-button {  
height: 3rem;  
width: 3rem;  
border: 0.5rem solid $color-primary;  
border-radius: 50%;  
display: inline-block;  
position: absolute;  
left: 0;  
top: -0.4rem;

&::after {  
 content: "";  
 display: block;  
 border-radius: 50%;  
 height: 1.3rem;  
 width: 1.3rem;  
 position: absolute;  
 top: 50%;  
 left: 50%;  
 transform: translate(-50%, -50%);  
 background-color: $color-primary;  
 opacity: 0;  
 transition: opacity 0.2s;  
 }  
}

&**radio-input:checked ~ &**radio-label &\_\_radio-button::after {  
opacity: 1;  
}  
}

---

# Hack différence entre linear et radial gradient

**linear-gradient** part d'un point (un angle ou un côté par exemple) et se développe dans une direction  
**radial-gradient** part d'un point et se développe tout autour dans toutes les direction

---

# Techniques pour utiliser des images responsive en HTML

## Density Switching

On fournit deux sources d'images pour laisser le choix au navigateur en fonction de la résolution de l'écran. C'est le 1x pour low résolution ou 2x pour haute résolution qui donne l'indication nécessaire.

## Art Direction

On utilise le html tag "picture" avec l'attribut "media" dans lequel on précise la taille de l'écran (le navigateur ne "choisit" pas)

picture class="footer\_\_logo">  
 source srcset="  
 img/logo-green-small-1x.png 1x,  
 img/logo-green-small-2x.png 2x  
 "  
 media="(max-width: 37.5em)"  
 />  
 img  
 srcset="img/logo-green-1x.png 1x, img/logo-green-2x.png 2x"  
 alt="Full logo"  
 src="img/logo-green-2x.png"  
 />  
/picture>

## Resolution Switching

Comme pour **Density Switching** , on va indiquer dans le tag html les informations nécessaires pour que le navigateur choisisse la meilleure image mais au lieu d'utiliser 1x ou 2x, on utilise une information sur la largeur 200w par exemple pour 200 px en largeur, EN PLUS de l'attribut "sizes" qui fournira un complément d'informations nécessaires.

img  
srcset="img/nat-1.jpg 300w, img/nat-1-large.jpg 1000w"
sizes="(max-width: 56.25em) 20vw, (max-width: 37.5em) 30vw, 300px"  
alt="Photo 1"  
class="composition\_\_photo composition\_\_photo--p1"  
src="img/nat-1-large.jpg"  
/>

---

# Techniques pour utiliser des images responsive en CSS

## On cible la résolution de l'écran et la largeur

@media only screen and (min-resolution: 192dpi) and (min-width: 37.5em),  
 only screen and (-webkit-min-device-pixel-ratio: 2) and (min-width: 37.5em),  
 only screen and (min-width: 125em) {  
 background-image: linear-gradient(  
 to right bottom,  
 rgba(\$color-primary-light, 0.8),  
 rgba(\$color-primary-dark, 0.8)  
 ),  
 url(../img/hero.jpg);  
 }

---

# Hack pour changer la couleur quand on sélectionne du texte sur le site

::selection {  
 background-color: $color-primary;  
 color: $color-white;  
}

---

# Hack pour changer le style sur les devices en fonction du sélecteur "hover"

@media only screen and (max-width: 56.25em), only screen and (hover: none) {  
 ...  
 }

**OU**

@media only screen and (max-width: 56.25em), only screen and (hover: hover) {  
 ...  
 }
