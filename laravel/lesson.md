# Laravel 

Créer une page affichant des produits récupérés de la base de donnée 

* View
* Controller
* Migrations

_Cela nécessite d'avoir au préalable un projet Laravel_


__Lier la DB au projet via PhpStorm__

Dans PhpStorm, cliquer sur l'onglet **Database** > ajout (croix verte) > Data Source > MySQL \
Remplir les champs de la fenêtre de configuration et cliquer sur `tester la connexion`\
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

__Préparer les colonnes à insérer__

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

Exemple de vue avec affichage :

    @extends('structure.layout')
    
    @section('title','Définition du title (onglet)')
    
    @section('contenu')

    <h1>Affichage des données récupérées</h1>
        @foreach($nomArray as $eachElement)
            {{ $eachElement->titre }}<br>
            {{ $eachElement->reference }}<br>
            {{ $eachElement->slug }}<br>
            {{ $eachElement->prix }}<br>
            {{ $eachElement->description }}<br>
        @endforeach
    @endsection 
    
Les `@` sont des services apportés par Laravel ils permettent plusieurs possibilités :
* `@foreach` / `@endforeach`
* `@if` / `@endif`
* `@section` / `@endsection`
* `@extends`

Les `{{ }}` servent à afficher un élément (correspond au `echo`)

La notation ` $element->titre ` correspond à `$element['titre']` en php procédural

__Créer la structure de page__

La ligne `@extends('structure.layout')` fait appel au fichier `resources/views/structure/layout.blade.php`

Pour ce faire il faut créer un dossier `structure` dans le dossier `resources/views/` et y créer le fichier `layout.blade.php`

Exemple de fichier de structure : 

    <!doctype html>
    <html lang="{{ app()->getLocale() }}">
        <head>
            <meta charset="utf-8">
            <meta http-equiv="X-UA-Compatible" content="IE=edge">
            <meta name="viewport" content="width=device-width, initial-scale=1">

        <title>@yield('title')</title>

        <!-- Fonts -->
        <link rel="stylesheet" href="{{ asset('bootstrap/css/bootstrap.min.css') }}">
        <link href="{{ asset('css/style.css') }}" rel="stylesheet" type="text/css">
        <link href="https://fonts.googleapis.com/css?family=Raleway:100,600" rel="stylesheet" type="text/css">

        </head>
        <body>
            <div class="flex-center position-ref full-height">
                <div class="content">
                   @yield('contenu')
                </div>
            </div>
        <script src="{{ asset('bootstrap/js/bootstrap.min.js') }}"></script>
        </body>
    </html>