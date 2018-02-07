Bootstrap

Le dossier bootstrap à la racine du projet n'est pas le Bootstrap CSS


`fichier.blade.php`

extends nomDuDossier.NomDuFichier.php


La function asset() permet d'appeler des fichiers js / css / img (front) en prenant pour racine le dossier public

    {{ asset('bootstrap/css/bootstrap.min.css') }}
    

Manipulation Controller & gestion DB


Commandes artisan :

__La commande `artisan` n'est utilisable qu'à la racine du projet__

Changer le nom de son application 

    php artisan app:name nom_projet
    
Créer un controller dans `app/Http/Controllers`

    php artisan make:controller nom_controller

Créer une table MySQL, par convention mettre au pluriel

    php artisan make:migration create_produits_table --create=produits

`--create=produits` défini le nom de la table
Ainsi le fichier `/database/migrations/[date]_create_produits_table.php`

Insérer les columns dans les tables. Cette function execute la function up()

    php artisan migrate

Permet de revenir en arriere, cela execute la function down(), qui est l'inverse la function up()

    php artidsan migrate:rollback

Création du modèle rattaché à ma table, par convention le nom du model est au singulier, premiere lettre majuscule
    
    php artisan make:model Produit
Le fichier est créé dans /app/Produit.php 

------------------------------------------------------------

Dans le fichier `web.php`

Création de l'url et appel via NomCOntroller@MethodeDuController
    
    Route::get('/produits', 'ProduitController@listeProduits');

Et création de la méthode liste_produits() dans le fichier `app/Http/Controllers/NameController.php`

Celle-ci est appelée dans `routes/web.php` via l'url `/produits`
    
    public function NameMethod()
    {
        return view('NameView');
    }
    
Création de la DB

Celle-ci se créer manuellement et se configure dans le fichier `.env`

Lier la DB
* Cliquer sur l'onglet DataBase 
* Cliquer sur la croix verte dans la fenêtre
* Remplir les champs obligatoires et faire un test de connexion

Par la suite on peut modifier les valeurs de sa DB
* Modifier le champ
* Quand le champ modifié est bleu clair, faire ctrl + entrée pour valider


Dans le fichier `/database/migrations/[date]_create_name_table.php`
Ajouter des columns dans la table

    $table->string('titre', 25);
    $table->string('reference', 25);
    $table->string('slug', 50);
    $table->string('photo', 50);
    $table->float('prix', 7, 2);
    $table->integer('quantite');
    $table->text('description');
        
Dans ces exemples, string correspond au VARCHAR & nullable() permet des champs null

Incremente signifie() que l'on auto_incremente la column

    $table->increments('id');

Créer updated_at and created_at columns

    $table->timestamps();

Créer un enum (Une column qui ne propose que les options Mr / Mme)
        
    $table->enum('civilite', ['Mr', 'Mme'])->default('Mr');
    
Une fois les modifications apportées, faire :

    php artisan migrate
    

Afficher les produits :

Dans le Controller

    public function listeProduits()
    {
        $data['produits'] = Produit::all();
        return view('product_list', $data);
    }
    
Le Controller recupère les produits dans $data['produits']
et les envoient dans la view product_list soit `product_list.blade.php`

