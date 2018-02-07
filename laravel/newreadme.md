# Laravel 

Créer une page affichant des produits récupérés de la base de donnée 

* View
* Controller
* Migrations

( Mise en place de son projet )


__Lier la DB au projet via PhpStorm__

Dans PhpStorm, cliquer sur l'onglet **Database** > ajout (croix verte) > Data Source > MySQL \
Remplir les champs de la fenêtre de configuration.\
Attention aux numéros de ports

Ensuite, ouvrir le fichier `.env` et modifier :
* APP_URL
* DB_DATABASE
* DB_USERNAME
* DB_PASSWORD

Désormais la DB est utilisable directement depuis Phpstorm (Requêtes SQL, création de tables, etc...)

__Créer une table MySQL__

Utiliser la commande :

    php artisan make:migration nomTable_table --create=nomTable

`--create=nomTable` défini le nom de la table et permet la création du fichier `/database/migrations/[date]_create_produits_table.php`
    
**Important**: La commande `artisan` n'est utilisable qu'à la **racine** du projet

__Préparer les columns à insérer__

Dans le fichier `/database/migrations/[date]_create_name_table.php`, y rajouter les colonnes que l'on souhaite ajouter.

    $table->string('titre', 25);
    $table->string('reference', 25);
    $table->string('slug', 50);
    $table->string('photo', 50);
    $table->float('prix', 7, 2);
    $table->integer('quantite');
    $table->text('description');
    
Dans cet exemple les colonnes titre, reference, slug, photo, prix, quantite, description sont renseignées ainsi que leur type.

string correspond au VARCHAR\
nullable() permet des champs null

Increments() signifie que l'on auto_incremente la colonne

    $table->increments('id');

timestamps() créer les colonnes updated_at and created_at

    $table->timestamps();

Créer un enum (Une column qui ne propose que les options Mr / Mme)
        
    $table->enum('civilite', ['Mr', 'Mme'])->default('Mr');
    
Une fois les modifications apportées, faire :

    php artisan migrate
    
Par la suite on va créer un modele de page qui sera attaché à la table créée

    php artisan make:model Nommodel
Par convention le nom du modele est au singulier abvec la premiere lettre en majuscule

**Note :**

La commande `php artisan migrate` déclenche la function up() dans `/database/migrations/[date]_create_produits_table.php` et insère les colonnes dans la table

La commande `php artisan migrate:rollback` déclenche la function down() dans `/database/migrations/[date]_create_produits_table.php` et permet un retour en arrière

__Créer une page d'affichage des éléments__

Dans le fichier `/routes/web.php` on y renseigne les URL des pages disponibles

    Route::get('/affichage_exemple', function() {
        return view('exemple.affichage');
    });

Où l'on peut aussi gérer les routes avec un Controller 

    Route::get('/exemple_controller', 'MonController@laMethodeDuController');

**Important :** Votre serveur doit permettre la réécriture d'URL

Pour gérer les routes avec un Controller, il faut le créer au préalable dans `app/Http/Controllers/` via la commande

    php artisan make:controller nomController

__Configurer le Controller__

Dans le fichier `app/Http/Controllers/nomController.php`, définir la méthode appelée dans le fichier `web.php` 

    use Eemi\Nommodel;
    use Illuminate\Http\Request;
    
    class ProduitController extends Controller
    { 
        public function laMethodeDuController()
        {
            $nomArray['nomIndice'] = Nommodel::all();
            return view('/nomView', $nomArray);
        }
    }
        
Cette méthode récupère tous les éléments dans la table et les envoient vers la page `/nomView` et effectue la redirection\
**IMPORTANT** Ne pas oublier de faire l'import de la classe Nommodel (cf: `use Eemi/Nommodel`)

__Créer une nouvelle vue__

Dans le dossier `/resources/views/`, créer un fichier `nomView.blade.php`

**Important :** `.blade` est impératif à renseigner dans le nom de la vue.

