**Création de plugins** 

Créer un dossier dans\
`wp-content/plugin/monPlugin`

Créer le fichier principal

nom_du_fichier.php et  mettre les commentaires de description du plugin

    Plugin Name: Nom du plugin
    Description: La description
    Version: 0.0.1
    Author: L'auteur du plugin

Activer le thème dans le BackOffice

Par convention, on définit les functions du plugin par : \
`function nomDuPlugin_nomFunction()`


On peut créer un nouveau menu dans le menu de BackOffice

        register_post_type('slide', array(
            'public' => true,
            'label' => 'nomDuPlugin_AsYouWant'
        ));
 Il est possible de placer le plugin dans le menu de BackOffice et le positionner où l'on souhaite 
 
         register_post_type('slide', array(
             'public' => true,
             'label' => 'nomDuPlugin_AsYouWant'
             'menu_position' => 65
         ));
         
Le chiffre 65 correspond à "Below plugins", voici la liste :

* 5 - below Posts
* 10 - below Media
* 15 - below Links
* 20 - below Pages
* 25 - below comments
* 60 - below first separator
* 65 - below Plugins
* 70 - below Users
* 75 - below Tools
* 80 - below Settings
* 100 - below second separator

On peut définir qui a le droit de modifier les slides, ici c'est la personne ayant le droit d'administrer les articles

            'capability_type' => 'post'

Définir ce qui apparait dans le slider

            'supports' => array('title', 'thumbnail')
**IMPORTANT !**

Votre thème doit permettre les images mis en avant dans les posts \
Dans le fichier functions.php : 

            add_theme_support('post-thumbnails');

On peut resize les images du slider : 
    
            add_image_size('slider', 1000, 300, true);


Apres avoir créer tous les slides dans le BackOffice, on les retrouvent dans la base de donnée

On peut par la suite les appeler dans une function
   
    function nomDuPlugin_slider($limit = 5)
    {
        $slides = new WP_Query("post_type=slide&posts_per_page=$limit");
        var_dump($slides);
    }

Où la classe WP_Query créer une requête SQL avec le parametre spécifié

Ensuite, on récupère dynamiquement les images que l'on souhaite intégrer au slider

Placer le css de son slider dans le repertoire du plugin et le charger avec wordpress : 

        wp_enqueue_style('nomDuPlugin', plugins_url() . '/nomDuPlugin/fileName.css');


Une fois que votre plugin est développé, appelé le où vous le souhaitez dans votre site ! 

        <?php nomDeLaFunctionDuPlugin(); ?>