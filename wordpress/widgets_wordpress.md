Création de widgets Wordpress
-

Objectifs : 

* Définir depuis le BO :
    * Un titre
    * Des valeurs diverses
    * Puis une URL de flux RSS
    

__Créer un plugin__

Dans le dossier wp-content/plugins, créer le dossier portant le nom du plugin
Dans le dossier principal (à la racine du plugin), inscrire les commentaires nécessaires dans le principal fichier

    <?php
    /*
     * Plugin Name: Nom_plugin
     * Description: la_description
     * Version: 0.0.1
     * Author: Author
     */
     
Ensuite déclarer l'initialisation du widget

    add_action("widgets_init", "elSancho_widget_init")
    {
        register_widget('elSancho_widget');
    }

Pour créer le widget, il faut déclarer une classe par widget

Celle-ci devra prévoir 4 méthodes pour :
* Configuration
    * `__construct()`
* Affichage
    * `widget()`
* Mise à jour
    * `update()`
* Formulaire BO
    * `form()`
    


    class nom_widget extends WP_widget
    {
    
    function __construct()
    {
        $options = array(
                'classname' => 'elSancho_widget_class',
                'description' => 'Le widget qui fait rêver'
        );

        $this->WP_Widget("elsancho_widget", "Le widget elSancho", $options);
    }
        
        function widget($args, $data){}
        
        function update($new, $old){}
        
        function form($data){}
    }
    
Ces 4 méthodes sont obligatoires.

La méthode `register_widget()` défini un id de type string

        register_widget('elSancho_widget');
        
Celui-ci doit être réutilisé en tant que nom de méthode et en paramètre de la méthode `WP_Widget()`

    function __construct()
    {
        $options = array(
                'classname' => 'elSancho_widget_class',
                'description' => 'Le widget qui fait rêver'
        );

        $this->WP_Widget("elsancho_widget", "Le widget elSancho", $options);
    }
    

La méthode `widget()` permet de gérer l'affichage du widget.
Le tableau `$data` est issu des names renseignés dans la méthode `form()`

    function widget($args, $data)
    {
        #var_dump($args);
        extract($args);
        echo $before_widget;
        echo $before_title . $data["titre"] . $after_title;

        include_once(ABSPATH . WPINC . '/rss.php');
        wp_rss($data["rss"]);

        #include_once(ABSPATH . WPINC . '/feed.php');
        #var_dump(fetch_feed($data["rss"]));

        echo $after_widget;
    }

Créer un formulaire dans l'onglet widget grâce à la méthode `form()`

    function form($data)
    {
        $default = array(
                "titre" => "mon_titre"
        );
        $data = wp_parse_args($data, $default);
    ?>
       <p>
        <label id='<? $this->get_field_id("titre"); ?>'>Titre :</label>
        <input type='text' value='<?= $data["titre"]; ?>' name='<?= $this->get_field_name("titre"); ?>' id='<?= $this->get_field_id("titre"); ?>'>
       </p>
    <?php
    }

Pour sauvegarder le titre créé dans le formulaire, utiliser la méthode `update()`

    function update($new, $old)
    {
        /*
        var_dump($new);
        var_dump($old);
        die();
        */
        return $new;
    }
    
Avec ces étapes, la création du widget est effectuée, par la suite on peut élaborer le traitement du widget


BONUS : 

Depuis un thème scratch, il faut permettre l'onglet de widget du BackOffice

Dans le fichier `functions.php`, insérer :

    function header_widgets_init()
    {
        register_sidebar(array
        (
            'name' => 'Ma nouvelle zone de widget',
            'id' => 'new-widget-area',
            'before_widget' => '<div class="nwa-widget">',
            'after_widget' => '</div>',
            'before_titlle' => '<h2 class="nwa-title">',
            'after_title' => '</h2>'
        ));
    }
    
    add_action('widgets_init', 'header_widgets_init');
    
Par la suite, définir l'emplacement où apparaitra le widget en question :

    <?php if (is_active_sidebar('new-widget-area')) : ?>
        <div id="new-widget-area" class="nwa-header-widget widget-area" role="complementary"></div>
    <?php dynamic_sidebar('new-widget-area'); ?>
    <?php endif; ?>