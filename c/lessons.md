# Programmation en C

Création d'un fichier `name.c`

Point d'entrée dans un programme :
`function main()`

Toutes les functions en C ont un type, c'est le type de l'argument renvoyé par la function

Voici la function main classique qui ne renvoie que 0. Ce `return(0)` prévient que le traitement est fini

    int main()
    {
        return(0);
    }

Pour compiler le programme \
 `gcc -o jour02 jour02.c`
 
 Le paramètre `-o` permet de préciser le file executable
 
 Pour que la function renvoie quelque chose on peut afficher un caractère
     
     #include <unistd.h>
     
     int main()
     {
        write(1, "@", 1);
        return(0);
     }
     
La function système `write()` comprend 3 paramètres :
* 1 -> sortie exigée (1 étant la standard)
* "@" -> la chaine de caractère à afficher
* 1 -> Le nombre d'octets de la chaine

De plus la function `write()` nécessite l'include `<unistd.h>`

Dans ce programme on définit avant la function `ft_putchar()`, qui fera le traitement et sera appelée par le `main()`. Dans la function `write()` on renseigne __l'adresse__ du caractère c : `&c`
 

    #include <unistd.h>
    
    int ft_putchar(char c)
    {
        write(1, &c, 1);
        return(0);
    }
    
    int main()
    {
        ft_putchar('@');
        ft_putchar('\n');
        return(0);
    }
    
Par la suite on peut continuer sur un traitement permettant d'afficher n fois un nombre. Ceci est possible gràace la function `ft_nputchar()`

    #include <unistd.h>
    
    int ft_putchar(char c)
    {
        write(1, &c, 1);
        return(0);
    }
    
    int ft_nputchar(char c, int n)
    {
        int i;
    
        i = 0;
    
        while (i < n)
        {
            ft_putchar(c);
    
            i = i + 1;
        }
        return(0);
    }
    
    int main()
    {
        ft_nputchar('@', 42);
        ft_putchar('\n');
        return(0);
    }
    
Après avoir compilé et executé le programme, on peut compter les caractères renvoyés avec : \
`./nomDeLexecutable | wc -c` qui devrait renvoyer 43 car on prend en compte le `\n`


**Les types composés**

arrays : Une suite d'objets du même type répétés

    type name[numeric_value] 
    int tab[20]
C'est la déclaration d'un tableau de 20 int

Structure de données, la structure permet de définir une représentation d'objets en mémoire 

    struct test
    {
        int a;
        char b[21];
    };
    
    struct test a;
    
**Les expressions**

Les expressions sont une suite d'évaluations qui vont donner des résultats

Les computations \
`1 + 3 ( a * 20 + b[0] / sum(21, 34 - b[10]))`

On remarque que le compilateur va calculer lu-même b[0] et les functions etc...

Les comparateurs
` == != < > <= => `, il va comparer les contenus des 2 variables, si c'est egal il renvoi `1` sinon `0`