# Laravel

Afficher toutes les routes

    php artisan route:list
    
__Insertion en base de donnée__

Créer une nouvelle méthode dans le Controller prenant l'argument `$request` de type `Request`

* `$request->all()` permet de récupérer toutes les infos du `$request`

On instancie un nouvel objet Produit.

On y ajoute les informations récupérées et éxecute l'insertion via la méthode `save()`.


    public function enregistrerProduit(Request $request)
    {
        # dump($request->all());
        # dump($request->titre);
        # dump($request->reference);
        # dump($request->prix);
        # dump($request->quantite);
        # dd($request);

        $produit = new Produit();

        $produit->titre = $request->titre;
        $produit->reference = $request- >reference;
        $produit->prix = $request->prix;
        $produit->quantite = $request->quantite;
        
        $produit->save();
    }
    

**Créer une classe pour filtrer les données du formulaire**

    php artisan make:request ProduitRequest
    
Le fichier `app/Http/Requests/ProduitRequest.php` est créé

Dans ce fichier, 2 méthodes sont prédéfinies

* `authorize()` permet dd'accéder au formulaire, la valeur retournée doit être sur `true`

    
    public function authorize()
    {
        return false;
    }
    
* `rules()` permet de définir les règles nécessaire à l'insertion

A gauche, les titres du formulaire.\
A droite, les règles à appliquer __(CF: DOC LARAVEL -> Available Validation Rules)__ 

    public function rules()
    {
        return [
            'titre' => 'string',
            'reference' => 'unique:produits, reference'
        ];
    }
    


__Les relations__

Un user possede un article.
Dans le fichier model exemple `Article.php`\
Un article appartient au user
    
    public function user()
    {
        return $this->belongsTo('Eemi\User', 'user_id');
    }

Le 2e paramètre `foreignKey` n'est pas obligatoire si la column id est sous la forme :
`singulier_id`


# Virtual disk

Configurer un disk virtuel perso. Qui sera indiqué comme disk par défaut plus tard

Fichier `config\filesystems.php`

* `driver`, le dossier va pointer vers le driver `local`
* `root`, chemin du dossier. `public_path()` pointe vers le dossier /public, il faut donc concaténer avec le repertoire
* `url`, url pointant vers notre dossier stockage. `asset()` est la fonction prédéfinie qui donne `http://votre_chemin/public`


    'public_perso' => [
        'driver' => 'local',
        'root' => public_path('stockage'),
        'url' => env('APP_URL') . '/stockage',
        'visibility' => 'public'
    ],   
    
( plus haut dans le fichier )

    'default' => env('FILESYSTEM_DRIVER', 'public_perso'),
    

Dans le fichier `Controller`
   
La méthode `putFileAs()` : 
* le `1er` argument, le nom du dossier du disk virtuel   
* le `2eme` argument, le fichier photo
* le `3eme` argument, le nom de la photo   
    
Si le dossier n'existe pas, Laravel le créera automatiquement\
Ensuite il est enregistré en base de donnée avec `update()` ou `save()`
    
    if ($request->hasFile('photo'))
    {
        $nomPhoto = 'photo_' . $produit->id . '.' . $request->file('photo')->extension()
    
        $path = Storage::putFileAs(
            'photos', $request->file('photo'), $nomPhoto);
    
        $produit->photo = 'photo_' . $produit->id . '.' .   $request->file('photo')->extension();
        $produit->update();
    }
    
    
Modification
-

Aller dans le fichier de routes `web.php`
Spécifier l'url d'edition de produits

    Route::get('produit/modifier/{id}', 'ProduitController@formModifierProduit')->name('modifier.produit');
    
Dans le fichier d'affichage des produits, créer un lien vers la route créée

    <a href= {{ route('produit.modifier', $value->id }}"></a>
    
Dans le Controller, créer la méthode modifierProduit

    public function formModifierProduit($id)
    {
        $data['action'] = 'modifier';
        try
        {
            $produitAmodifier = Produit::findOrFail($id);
            ( A completer )
        }
        catch (ModelNotFoundException $e)
        {
           ( message flash d'erreur )
        }
    
    }
    
Dans la vue du formulaire d'ajout de produits, vérifier la route.
L'action du formulaire dépendra de l'url par laquelle on accède à la vue (modification ou ajout)

    <form action="{{ route(Route::currentRouteName(), (!is_null($produitAmodifier) ? $produitAmodifier->id : '')) }}" method="post">

Dans les values des champs inputs, vérifier si `$produitAmodifier` est présent sinon utiliser la function `old()`.\
Exemple, liste quantité : 

    
    value="{{ ((!empty($produitAmodifier) && $produitAmodifier->quantite == $i) || old('quantite') == $i) ? 'selected' : '' }}"
    
Dans `web.php`, créer une route en méthode `post`

    Route::post('produit/modifier/{id}', ProduitController@modifierProduit);
    
Dans le controller

    public function modifierProduit($id, Request $request)
    {
        $data['action'] = 'modifier';
        try
        {
            $produitAmodifier = Produit::findOrFail($id);
        }
        catch (ModelNotFoundException $e)
        {
            flash('Produit introuvable')->error();
            return redirect()->route('produit.liste');
        }
        
        $produit->titre = $request->titre;    
        $produit->reference = $request->reference;    
        
        [ ... ]
        
        $update = $produit->update();
        
        if ($update) 
        {
            flash('Produit' . $produit->titre . 'modifié avec succès');
        }
        
        return redirect()->route('produit.liste');
    }