# TP gestion du char en android
__12/02/2018__


match_parent, s'adapte au contenant

    android:layout_width="match_parent"
wrap-content, s'adapte au contenu

    android:layout_height="wrap_content"

Définir le poids d'un élément permet de définir une taille d'un élément en fonction des autres éléments : 

    android:layout_weight="1"
    
_NOTE:_ Plus le poids est élevé et plus la taille est petite.

L'alignParentStart permet de commencer dans le sens de l'écran. Soit pour des japonais, par exemple, cela commencera de droite à gauche

    android:layout_alignParentStart="true"
    
Définir ses puces dans un fichier de ressources permet de switch de langue plus facilement.


__MainActivity.java__

Il faut savoir sur quel bouton on clique 

    Commande pour savoir quand on clique
    

Différence entre float et double

Double, prend deux fois la quantité de mémoire ( et donc plus précis )

    
Ces deux functions ne sont pas les mếmes, elles ne prennent pas les mêmes arguments

    void func1(double x){};
    void func1(float x){};
    
    
Calculer l'angle en fonction du centre

    double andleRadians = Math.atan2(x-cx,y-cy);
    
Une instance est une méthode d'objet

Faire une chaine de caractère avec les degrés, de 3 manières différentes :

* `angleString = String.valueOf(angleDegres);`
* `angleString = ((Double)angleDegres).toString();`
* `angleCentre.setText(angleString + "");`

Dans l'exemple 2, `(Double)` est un `cast`, il permet de forcer le type d'angleDegres
`(Type)` est toujours un cast


    angleString = String.valueOf(angleDegres);

    angleCentre.setText( angleString );
    
Android Studio permet le debuggueur, il faut mettre un point rouge au début de la ligne et run encore une fois. Ne pas oublier d'ouvrir la fenetre debug.

`super.onCreate()` permet de charger tout l'écran alors que `super.onStart()` se lance quand tout à été chargé


    float x = ((LinearLayout)v.getParent()).getX() + v.getX();

Le `x` et `y` valent leur position plus la position du conteneur


__A retenir__

Il y a toujours une solution
Question de temps, énergie et moyen

Vite, bien, pas chère => en choisir 2.