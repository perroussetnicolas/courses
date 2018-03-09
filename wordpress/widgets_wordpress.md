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
* Affichage
* Mise à jour
* Formulaire BO
    

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
    
Créer un formulaire dans l'onglet widget 

    function form($data)
    {
        echo '<p>';
        echo '<label id=' . $this->get_field_id("titre") . '>Titre :</label>';
        echo '<input type=\'text\' name=' . $this->get_field_name("titre") . ' id=' . $this->get_field_id("titre") . '>';
        echo '</p>';
    }
