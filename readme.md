# TP : Programmation orientée objet en PHP
> Téléchargez le repository et travaillez dans le dossier `project` !

Nous allons travailler à améliorer notre jeu HB Spaceships, qui est un combat entre vaisseaux spatiaux, développé en PHP procédural. Et si on mettait un peu de POO dedans ?

Les règles du jeu sont simples : on choisit un vaisseau dans chaque camp, un nombre de vaisseau qui va combattre, et c'est parti !

Chaque vaisseau possède un nom `name`, une force d'attaque `weapon_power`, un booster Spatiodrive `spatiodrive_booster` et une résistance `strength`.

À chaque tour, chaque groupe attaque l'autre avec sa force d'attaque. Si la résistance d'un des groupes de vaisseaux tombe à 0, le groupe a perdu.

À chaque tour, chaque groupe peut réussir à déclancher son booster Spatiodrive qui détruit instantanément l'autre ! Par exemple, le Jedi Starfighter a un booster Spatiodrive de 15 : il a 15 chances sur 100 de réussir à le déclancher pendant son tour.

## Chapitre 1 : Le projet
Avant tout, explorez le projet tel qu'il est actuellement : il s'agit d'un jeu de bataille spatiale en 3 fichiers :
- `index.php` qui contient l'interface du jeu
- `functions.php` qui contient une fonction qui retourne la liste des vaisseaux, et la fonction principale de bataille spatiale, `battle()`
- `battle.php` qui appelle la fonction `battle()` et affiche le résultat

Comprennez bien tout le projet avant de continuer !


## Chapitre 2 : Une classe et un objet

### Créer une classe
Créez un fichier `play.php` dans le projet dans lequel on va pouvoir tester des choses. Commençons tout de suite avec une classe : créez enfin votre première classe en PHP nommée `Ship` dans ce fichier !

```php
class Ship {

}
```

### Créer un objet
La POO, c'est des classes et des... objets. Créez donc votre premier objet en PHP !

```php
$myShip = new Ship();
```

Ce code ressemble un peu à si nous avions appelé une fonction `Ship()`, excepté pour ce nouveau mot clé `new` qui indique à PHP que nous voulons un nouvel *objet instancié de la classe `Ship`*. On expliquera ce que tout ça veut dire plus tard !

Si on actualise la page, toujours aucun changement. Créer une classe et l'instancier ne fait rien pour le moment, c'est comme appeler une fonction vide : on n'a pas encore expliqué quoi faire à nos classes et objets !

### Alors c'est quoi, une classe et un objet ?

#### Une classe
Imaginez que vous êtes le manager d'un dock d'arrivée de vaisseaux sur la Deathstar. Pour éviter tout embouteillage à l'arrivée, vous avez besoin de faire l'inventaire des arrivées : leur nom, taille, est-ce qu'ils ont un moteur de distorsion, quel est leur niveau de carburant... Et imaginez aussi que même si vous êtes sur un vaisseau-planète géant, vous êtes obligé de tenir tout ce registre à la main en imprimant un modèle de fichier vide créé sur Word !

Une classe, c'est un peu comme cette fiche d'inventaire, avec des cases à remplir pour le nom et la taille, une coche oui/non pour le moteur de distorsion...

Cette classe n'est donc pas un vaisseau, mais c'est tout comme : elle définit tous les attributs qu'un vaisseau pourrait avoir.

#### Un objet
Un objet, ça correspond donc à... une fiche d'inventaire remplie pour un vaisseau. Ce n'est pas exactement le vaisseau lui-même, c'est sa fiche d'inventaire.

#### Les objets sont des sortes d'arrays, en mieux
Rapellez-vous les arrays : les arrays contiennent des données attachées à des clés (`keys`) : par exemple, on pourrait avoir la clé `niveauCarburant` et sa valeur `1450`, ou encore `moteurDeDistortion` et une valeur `true`.

Un objet fonctionne exactement pareil ! Il y a cependant une grosse différence entre les objets et les arrays: dans un array, on peut rajouter à la volée une nouvelle clé et sa valeur, le nombre de passagers par exemple. On peut aussi ne pas remplir une clé déjà existante, le nom ou la taille par exemple.

Avec un objet, on est obligé de prévoir en avance les données qui vont exister, et on sera obligé de ne remplir que celles-ci, pas une de plus.

Si on reprend notre rôle de manager dans le dock d'arrivée de la Deathstar, un array est un peu comme un fichier d'inventaire avec deux cases blanches sur une ligne : on peut rajouter de nouvelles informations en plus de celles prévues si l'on veut :
```
FICHE D'INVENTAIRE :
Nom     : ...
Taille  : ...
_______ : ...  <= espace pour informations additionelles si besoin
_______ : ...  <= espace pour informations additionelles si besoin
```

Par exemple, pour ce vaisseau, je remplis son nom, sa taille, mais pas son niveau de carburant, j'ai envie de renseigner sa couleur en plus, pour l'autre je renseigne la musique qui passe à la radio parce que le boulot est ennuyant... Si je donne 10 feuilles à mon manager, il n'a aucune garantie sur les données que j'aurais enregistré ou non.

Pour un objet, on aura la liste exact et complète des données dont j'ai besoin ! Si j'ai besoin d'enregistrer le nom de chaque vaisseau, je l'indique dans ma classe, on appelle ça un **attribut** :

```php
class Ship {
    public $name;
}

$myShip = new Ship();
```

Pour le moment, rien ne se passe. Mais maintenant, mes objets `Ship` ont l'autorisation d'avoir une donnée `name` ! Comment on l'indique ?

```php
$myShip = new Ship();
$myShip->name = "TIE Fighter";
```

Et comment je lis cette donnée ?
```php
$myShip = new Ship();
$myShip->name = "TIE Fighter";

echo "Nom du vaisseau : " . $myShip->name;
```

Essayez !

Un objet fonctionne exactement comme un array : au lieu d'avoir des propriétés accessibles en crochets `[]`, on a des attributs accessibles avec une flèche `->`. De plus, l'array peut contenir toutes sortes de données : l'objet lui, est limité par les données définies dans la classe.

**ATTENTION !** Techniquement, vous pourriez immédiatement écrire quelque chose comme `$myShip->weight = 1000;` et créer une nouvelle propriété `weight` immédiatement, sans avoir créé l'attribut dans la classe. Ne le faites surtout pas ! C'est du code qu'on ne doit pas voir dans un code propre.

### Un autre avantage sur les arrays

Les objets ont un autre avantage indéniable sur les arrays : essayez de mettre `$myShip` dans un `var_dump`. Vous avez accès à tous les attributs possibles pour cet objet ! Lorsque l'on `var_dump` un array, on ne sait pas quelles données sont possibles ou non, l'array peut contenir tout et n'importe quoi sans contrôle. Lorsque vous `var_dump` un objet, vous voyez immédiatement qu'il pourra contenir un `name`, mais jamais de `e-mail`, `phone`... Car ils ne sont pas inscrits dans le `var_dump` !

```php
var_dump($myShip);

// Affiche le résultat suivant :
object(Ship)[1]
  public 'name' => string 'TIE Fighter' (length=5)
```

## Chapitre 3 : Les méthodes
Pour le moment, nos objets ne sont que des "super-arrays", pas de quoi changer le monde. La grande force des objets est ailleurs: ce sont les **méthodes** !

Les méthodes ne sont rien d'autre que des **fonctions**. En fait, ce sont des fonctions qui existent... dans une classe. Pour créer une méthode, déclarez-la dans la classe avec `public function` :

```php
class Ship {
    public $name;

    public function sayHello() {
        echo "Hello !";
    }
}
```

Pour appeler une méthode, on utilise la même flèche `->` que pour les attributs :

```php
$myShip->sayHello();
```

On remarque que la seule différence avec les attributs (par exemple `$myShip->name`) est l'ajout des parenthèses dans `->sayHello()`, comme pour une fonction classique. Ne les oubliez pas !

Mais à quoi ça sert, une méthode ? Elles nous permettent maintenant d'avoir des classes qui sont des petits packs de données, comme un array, mais qui peuvent aussi effectuer des actions, ce qu'un array ne peut absolument pas faire !

Notre fonction `sayHello` est juste un exemple, rapellez-vous que les fonctions *retournent* quelque chose. Ajoutons une nouvelle propriété à notre classe qui retourne un nom. Pour le moment, on retourne un faux nom :

```php
class Ship {
    // ...
    public function getName() {
        return "Fake name !";
    }
}
```

Appelons cette méthode :
```php
$myShip = new Ship();
$myShip->name = "TIE Fighter";

echo "Nom du vaisseau : " . $myShip->getName();
```

Testez... Et voilà, on a notre faux nom ! Bien sûr, on aimerait plutôt avoir notre *vrai* nom de vaisseau. On sait que, à l'extérieur de la classe, avec notre variable `$myShip`, en mettant une flèche et le nom, on a notre nom de vaisseau (`->name`). Mais comment appeler ce nom de vaisseau *à l'intérieur de* la classe ?

### Appeler un attribut à l'intérieur d'une classe

On utilise une variable un peu spéciale appelée `$this`. Modifiez la méthode `getName()` ainsi :

```php
class Ship {
    // ...
    public function getName() {
        return $this->name;
    }
}
```

Voilà comment ça marche : quand on est **dans** une classe, on a accès à une variable un peu particulière nommée `$this`. Et `$this`, c'est en fait... N'importe quel `Ship` créé par la classe ! Dans notre exemple, on a un vaisseau `$myShip` nommé `TIE Fighter` : lorsque j'appelle `getName()` sur `$myShip`, `$this->name` vaut `TIE Fighter`. En fait, `$this` correspond à `$myShip` lui-même, et n'importe quels objets `Ship` eux-mêmes. Par exemple :

```php
$myShip = new Ship();
$myShip->name = "TIE Fighter";

echo "Nom du vaisseau : " . $myShip->getName(); // Affiche "TIE Fighter", car $this correspond à $myShip ici

$mySecondShip = new Ship();
$mySecondShip->name = "X-Wing Fighter";

echo "Nom du vaisseau : " . $myShip->getName(); // Affiche "X-Wing Fighter", car $this correspond à $mySecondShip ici
```

#### Ajouter d'autres attributs
Notre classe `Ship` ne contient pour le moment qu'une seule propriété. Allez jeter un oeil à notre projet de base, dans `functions.php`, pour voir la liste des attributs de nos vaisseaux : `name`, `weapon_power`, `spatiodrive_booster`, `strength`. Ajoutons-les à la classe !

```php
class Ship {
    public $name;
    public $weaponPower;
    public $spatiodriveBooster;
    public $strength;
    // ...
}
```

Vous remarquerez que les attributs étaient en `snake_case` dans l'array, et sont maintenant en `camelCase` lorsqu'ils sont notés en variable. C'est de la convention : les clés d'arrays sont en `snake_case`, les variables ou attributs en `camelCase`.

**NOTE :** Toujours dans la convention, vous remarquerez que la classe possède une majuscule : elle est en `PascalCase`. C'est une convention pour les noms de classe, respectez-la aussi !

On a maintenant accès à toutes ces propriétés !

```php
$myShip->weaponPower = 150;
echo 'Force : ' . $myShip->weaponPower;
```

### Valeurs par défaut
On peut bien sûr donner des valeurs par défaut à nos attributs : c'est pratique pour créer un vaisseau dont on n'a pas toutes les valeurs tout de suite, par exemple.

```php
class Ship {
    public $name = '';
    public $weaponPower = 0;
    public $spatiodriveBooster = 0;
    public $strength = 0;
    // ...
}
```

Faites un `var_dump` de l'attribut `strength` pour voir ce que ça donne : `var_dump($myShip->stength);`

La valeur est de... zéro, ça fonctionne ! Essayez de changer sa valeur, puis refaites un `var_dump`:

```php
$myShip->strength = 100;
var_dump($myShip->strength);
```

La valeur a bien évidemment changé.

## Chapitre 4 : Des méthodes qui servent vraiment à quelque chose

Pour le moment, nos méthodes sont simples et ne font pas grand chose. Et si on commençait à créer des méthodes vraiment utiles ?

Par exemple, nos managers vont avoir besoin d'imprimer les informations sur les vaisseaux un peu partout dans la Deathstar. Jusque-là, ce qu'ils savent faire, c'est ça :

```php
echo "Vaisseau :" . $myShip->name . "(Force:" . $myShip->weaponPower . ", Booster spatiodrive: " . $myShip->spatiodriveBooster . ", Résistance : " . $myShip->strength . ")";
```

Ce qui affiche quelque chose comme : 
```
Vaisseau : X-Wing Fighter (Force: 100, Booster spatiodrive: 120, Résistance: 30)
```

Pratique, mais un peu long. Et puis, pour l'instant, ça ne fonctionne que pour `$myShip` : si on devait afficher les informations de `$mySecondShip`, on devrait réécrire la même chose avec cette nouvelle variable : 
```php
echo "Vaisseau :" . $mySecondShip->name . "(Force:" . $mySecondShip->weaponPower . ", Booster spatiodrive: " . $mySecondShip->spatiodriveBooster . ", Résistance : " . $mySecondShip->strength . ")";
```

Pas très pratique maintenant !

Et si on indiquait à nos objets comment communiquer eux-mêmes ces informations, sans avoir à le refaire à chaque fois ? Ajoutons une méthode à notre classe :

```php
class Ship {
    // ...

    public function getNameAndSpecs() {
        return "Vaisseau :" . $this->name . "(Force:" . $this->weaponPower . ", Booster spatiodrive: " . $this->spatiodriveBooster . ", Résistance : " . $this->strength . ")";
    }
}
```

Remarquez qu'on a la même chose, mais avec `$this` (c'est à dire : n'importe quel objet lui-même, qui vient de cette classe), et qu'on a ajouté `return` au lieu de `echo` (eh bien oui, ça reste une fonction, on **retourne** une valeur !).

Maintenant, pour afficher les informations de nos vaisseaux, on n'a plus qu'à écrire : 

```php
echo $myShip->getNameAndSpecs();
echo $mySecondShip->getNameAndSpecs();
```

Et voilà ! Quels que soient les vaisseaux, les managers n'ont **plus à se soucier de comment faire**, ils ont juste à appeler cette méthode sur le vaisseau pour imprimer ses spécifications. Pratique pour les managers stagiaires dans la Deathstar : aucune idée de comment ça marche, mais... ça marche.

### Arguments de méthodes
Comme pour une fonction classique, on peut ajouter des arguments à nos méthodes de classe. Par exemple, ajoutons un argument à notre méthode : est-ce que je veux la version courte, ou la version longue du texte ?

Pour ça, on ajoute un argument... booléen !

```php
class Ship {
    // ...

    public function getNameAndSpecs(bool $useShortFormat) {

        if ($useShortFormat) {
            return $this->name . "(F:" . $this->weaponPower . ", BS: " . $this->spatiodriveBooster . ", R : " . $this->strength . ")"; 
        }
        else {
            return "Vaisseau :" . $this->name . "(Force:" . $this->weaponPower . ", Booster spatiodrive: " . $this->spatiodriveBooster . ", Résistance : " . $this->strength . ")";
        }
    }
}
```

On essaie :

```php
echo $myShip->getNameAndSpecs(true);
// Affiche : "TIE Fighter (F:100, BS: 150, R:30)"

echo $myShip->getNameAndSpecs(false);
// Affiche : "Vaisseau : TIE Fighter (Force: 100, Booster spatiodrive: 150, Résistance: 30)"
```

### Améliorons notre méthode
Le manager en chef vient de passer derrière nous en râlant dans sa barbe : notre méthode est pratique, mais elle est un peu moche à lire. Voyons sa version corrigée :

```php
class Ship {
    // ...
    public function getNameAndSpecs(bool $useShortFormat) {

        if ($useShortFormat) {
            return sprintf(
                '%s (F:%s, BS:%s, R:%s)',
                $this->name,
                $this->weaponPower,
                $this->spatiodriveBooster,
                $this->strength);
        }
        else {
            return sprintf(
                'Vaisseau : (Force: %s, Booster spatiodrive: %s, Résistance: %s)',
                $this->name,
                $this->weaponPower,
                $this->spatiodriveBooster,
                $this->strength);
        }
    }
} 
```

Wow, nettement plus clair ! Plutôt que de concaténer, il a utilisé `sprintf` qui prend en premier argument la phrase complète à afficher, avec des `%s` à la place des variables, puis en autres arguments la liste des variables à remplacer à la place des `%s`, dans le bon ordre. Bizarre mais pratique !

Il râle aussi parce que les stagiaires managers sont perdus avec cette histoire de booléens en argument, ils les oublient tout le temps et tout bugue, il aimerait plutôt une valeur par défaut pour éviter d'avoir à le mettre à chaque fois :

```php
class Ship {
    // ...
    public function getNameAndSpecs(bool $useShortFormat = false) {
        // ...
    }
```

Et voilà. Fort, ce manager en chef. Si on essaie :
```php
// Ces deux-là sont pareils :
echo $myShip->getNameAndSpecs();
echo $myShip->getNameAndSpecs(false);

// Celui-là est la version longue :
echo $myShip->getNameAndSpecs(true);
```

En fait, une méthode, ça fonctionne vraiment comme une fonction sur tous les points.

## Chapitre 5 : Différents objets
Là où la POO est vraiment intéressante, c'est quand on créée plusieurs objets.

**Exercice :** le manager en chef vous demande d'essayer votre nouveau système de saisie d'inventaire. Trois vaisseaux ne vont pas tarder à arriver, vous devez saisir leurs informations à l'arrivée du quai de débarquement :

```
Vaisseau 1 : "Jedi Super Greater"
- Weapon Power de 150
- Spatiodrive Booster de 12
- Strength de 130

Vaisseau 2 : "Android Extra Fighter"
- Weapon Power de 400
- Spatiodrive Booster de 12
- Strength de 130

Vaisseau 3 : "Cargo Jumper"
- Weapon Power de 150
- Spatiodrive Booster de 12
- Strength de 130
```

Créez les 3 objets Ship pour ces 3 vaisseaux et affichez les informations des trois vaisseaux, en version courte et en version longue.

--- 

**Correction :** par exemple, pour chaque vaisseau :

```php
$firstShip = new Ship();
$firstShip->weaponPower = 130;
$firstShip->spatiodriveBooster = 20;
$firstShip->strength = 40;

echo $firstShip->getNameAndSpecs();
echo "<hr>";
echo $firstShip->getNameAndSpecs(true);
```

## Chapitre 6 : Faire interagir des objets
Comme le but de notre application est de faire combattre deux vaisseaux, on va pouvoir rendre les choses un petit peu plus intéressantes.

Pour commencer, essayons de créer une méthode dans notre classe qui permettent à nos vaisseaux de comparer leur force avec un autre vaisseau : c'est une méthode qui prend donc un autre vaisseau en paramètres, et qui compare cet autre vaisseau en paramètres avec le vaisseau lui-même.

```php
class Ship {
    // ...

    // "Est-ce que ce vaisseau a plus de résistance que moi ? (En anglais: Does this ship has more strength than me ?)"
    public function doesThisShipHasMoreStrengthThanMe($ship) {
        // ...
    }
}
```

Avant de remplir cette fonction, voyons comment nous aimerions l'utiliser. Partons du principe que nous ayons créé deux vaisseaux, deux objets `Ship` : `$myShip` et `$otherShip`. On aimerait pouvoir écrire des choses comme :

```php
// $myShip appelle une méthode ( doesThisShipHasMoreStrengthThanMe() ) qui permet de se comparer lui-même avec un autre vaisseau, nommé $otherShip, qu'on passe en paramètres à (doesThisShipHasMoreStrengthThanMe() ).

if ($myShip->doesThisShipHasMoreStrengthThanMe($otherShip)) {
    echo $otherShip->name.' a plus de résistance.';
} else {
    echo $myShip->name.' a plus de résistance.';
}
```

Il faudrait que la méthode retourne `true` ou `false`, `true` si **l'autre vaisseau** a plus de force, `false` si c'est **le vaisseau qui appelle la méthode** qui a plus de force.

On aurait donc quelque chose comme :
```php
class Ship {
    // ...

    public function doesThisShipHasMoreStrengthThanMe($ship) {

        // On teste en fait : "l'autre vaisseau > ce vaisseau".
        
        // Si c'est effectivement vrai, ça retournera true. Sinon, si c'est faux, ça retournera false.

        // L'autre vaisseau, c'est celui passé en paramètres, $ship.

        // Ce vaisseau, c'est à dire celui qui appelle la méthode pour se comparer, c'est... $this!

        return $ship->strength > $this->strength;
    }
}
```
Et voilà, testez !

Pour être un peu plus cohérent, notre méthode prend bien évidemment en paramètres, un autre vaisseau. Précisons-le dans notre méthode :

```php
public function doesThisShipHasMoreStrengthThanMe(Ship $ship) {
        return $ship->strength > $this->strength;
    }
```

En ajoutant `Ship` devant l'argument `$ship`, on précise que l'argument doit forcément être un objet Ship ! Exactement comme on a précisé qu'il fallait un `bool` dans `getNameAndSpecs` avec `getNameAndSpecs(bool $useShortFormat = false)`.


## Chapitre 7 : Un peu de documentation
Gardons de bonnes habitudes en codant proprement dès le départ ! Nous avons de belles méthodes dans notre classe, ajoutons de la documentation pour que notre éditeur de texte ou IDE nous aide un peu lorsqu'on les utilise :

```php

class Ship {
    // ...

    /**
     * @param bool $useShortFormat
     */
    public function getNameAndSpecs(bool $useShortFormat = false)
    {
        // ....
    }
    
    /**
     * @param Ship $ship
     */
    public function doesThisShipHasMoreStrengthThanMe(Ship $ship)
    {
        // ...
    }
}
```

## Chapitre 8 : Utiliser des objets dans notre projet
Notre fichier `play.php` est bien pratique pour faire quelques tests, mais maintenant, codons réellement notre projet en programmation orientée objet !

### Déplacer la classe `Ship` dans un fichier à part
Une convention est d'avoir un fichier spécifique pour chaque classe, qui contient la classe et... **rien d'autre** !

Comment appeler ce fichier ? Eh bien, plutôt que de lui donner un nom compliqué ou hazardeux, comme il contiendra la classe `Ship`... appelons-le simplement `Ship.php` ! (Notez la majuscule, le fichier a **exactement** le même nom que la classe !)

Dans le projet, créez un dossier `/lib` et créez le fichier `Ship.php` dedans. Il ne contiendra QUE la classe `Ship`. Pour rappel, voici la classe complète :

```php
<?php

class Ship {

    public $name = "";
    public $weaponPower = 0;
    public $spatiodriveBooster = 0;
    public $strength = 0;

    public function sayHello()
    {
        echo 'Hello!';
    }
    public function getName()
    {
        return $this->name;
    }
    
    /**
     * @param bool $useShortFormat
     */
    public function getNameAndSpecs(bool $useShortFormat = false)
    {

        if ($useShortFormat) {
            return sprintf(
                '%s (F:%s, BS:%s, R:%s)',
                $this->name,
                $this->weaponPower,
                $this->spatiodriveBooster,
                $this->strength);
        }
        else {
            return sprintf(
                'Vaisseau : (Force: %s, Booster spatiodrive: %s, Résistance: %s)',
                $this->name,
                $this->weaponPower,
                $this->spatiodriveBooster,
                $this->strength);
        }
    }

    /**
     * @param Ship $ship
     */
    public function doesThisShipHasMoreStrengthThanMe(Ship $ship)
    {
        return $ship->strength > $this->strength;
    }
}
```

**NOTE :** Pas besoin de `?>` à la fin du fichier : comme on n'a rien d'autre après, PHP fermera la balise lui-même !

Plus besoin de tout ce code dans `play.php`, vous pouvez supprimer la classe qui est dans `play.php` et garder tout le reste.

Il faut bien sûr importer la classe `Ship` dans `play.php`, mettez ce code tout en haut de `play.php` :

```php
<?php
require __DIR__ . '/lib/Ship.php';
```

Oulà, c'est quoi `__DIR__` ?! C'est une constante qui veut dire `le chemin actuel du fichier dans lequel je suis`. C'est à dire, le chemin actuel de `play.php`. C'est simplement pour s'assurer que `play.php` recherche le fichier au bon endroit !

**NOTE :** Remarquez que `require` ne requiert pas de parenthèses ;)

### On a vraiment besoin des `require` ?
C'est vrai ça, et si on travaille avec de nombreuses classes, est-ce que je dois `require` chacune des classes ?

En fait non : il y a une manière qui s'appelle l'`autoloading`, qui permet de charger les classes aux bons endroits lorsque l'on en a besoin (d'où la super importance de nommer les fichiers EXACTEMENT comme le nom des classes !). On verra ça plus tard.

Rechargez votre page `play.php`. Si tout se passe bien, tout fonctionne encore !

### Créer de vrais objets !
Maintenant que notre classe fonctionne bien, allons modifier le projet !

Pour commencer, ajoutez dans `functions.php` la ligne suivante : 
```php
require __DIR__.'/lib/Ship.php';
```

Ensuite, plutôt que d'utiliser des tableaux, utilisez des objets ! Dans la fonction `getShips()`, remplacez chacun des vaisseaux par des objets, que vous mettrez ensuite dans un array (un tableau d'objets donc !) :

```php
function getShips()
{
    $ships = []; // Futur array d'objets vaisseaux Ships

    // On créée un Ship
    $ship1 = new Ship();
    $ship1->name = 'Jedi Starfighter';
    $ship1->weaponPower = 5;
    $ship1->spatiodriveBooster = 15;
    $ship1->strength = 30;

    // On ajoute le ship "$ship1" créé dans le tableau de vaisseaux, $ships.
    $ships['starfighter'] = $ship1;

    // Recommencez avec tous les vaisseaux du tableau...

    // On retourne l'array de vaisseaux
    return $ships;
}
```

Pour tester, allez dans `index.php` et modifiez le code ainsi :

```php
<?php
require __DIR__. '/functions.php';
$ships = getShips();
var_dump($ships);
die;
```

Première étape, on `var_dump` la fonction `getShips` qui doit retourner un array contenant plusieurs objets `Ship`. Ensuite, on arrête notre code ici grâce à `die` : en effet, la suite va sans toute buguer à cause de nos modifications, mieux vaut mettre une pause ici et modifier le code petit à petit !

### Traitons les objets... Comme des objets !