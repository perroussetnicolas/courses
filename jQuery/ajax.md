# Ajax en jQuery

__Post de formulaire__

* Envoyer un formulaire en post
* Fabriquer un contenu sur le serveur
* Ajouter le contenu dans la page
    - Concaténer = append
    
L'évenement `submit` du formulaire permet de faire des controles de saisies avant que le formulaire ne parte. 

Créer son formulaire avec quelques champs

    <form id="formMsg">
        <input type="text" name="login" placeholder="Votre pseudo" id="login">
        <input type="text" name="msg" placeholder="Votre message" id="msg">
        <input type="submit">
    </form>
    <div id="responseText" style="background-color: yellowgreen"></div>
    
Les données du formulaire sont récupérées en jQuery 

C'est le jQuery va executer le submit du formulaire et afficher la réponse après traitement PHP.

    $(document).ready(function(){
        $('#formMsg').submit(function(){
            $.post('treatment.php', $('#formMsg').serialize(), function(text){
                $("#responseText").append(text);
            });
            return false;
        });
    });
    
La function de callback permet d'afficher les réponses à la suite de la div de réception `#responseText
`

Et renvoyées vers un traitement PHP : 

    $login = $_POST["login"];
    $msg = $_POST["msg"];
    
    echo "<p>$login a écrit le " . date("d-m-Y à G:i:s"). ":</p><br>";
    echo "<p><i>$msg</i></p>";
    
    
__Connection et reconnection automatique__

Créer un hash md5 du mot de passe avec clé de salage

    md5("password" . "cle_de_salage);

Connection

Créer un formulaire 

    <form id="formMsg">
        <input type="text" name="login" placeholder="Votre pseudo" id="login">
        <input type="text" name="password" placeholder="Votre mot de passe" id="password">
        <input type="submit">
    </form>
    
Un traitement ajax qui lance le traitement php

    $(document).ready(function(){
        $('#formMsg').submit(function(){
            $.post('treatment.php', $('#formMsg').serialize(), function(msg){
                    console.log('connected');
                    $('#formMsg').html(msg);
            });
            return false;
        });
    });
    
Un traitement php qui va vérifier si le login rentré est présent en DB.
Si oui, une session est créée.
    
    
    <?php
    require_once('pdo.inc.php');
    
        if (isset($_POST))
        {
            extract($_POST);
    
            $query = "SELECT * FROM users WHERE login=:login AND password=:password";
            # var_dump($query);
    
            $req = $pdo->prepare($query);
            $req->bindValue(':login', $login);
            $req->bindValue(':password', $password);
            $req->execute();
            $data = $req->fetchAll();
    
            $nb = $req->rowCount();
    
            if($nb == 0)
            {
                echo "Pas trouvé !";
            } else
            {
                echo "Trouvé !";
                $_SESSION['user'] = $data;
                # var_dump($_SESSION['user']);
            }
        }


Dans une `responseText`, on ne peut pas découper sans bricolage la réponse. \
Ainsi on utilise les `flux` qui permettent de structurer la reponse comme l'on souhaite.

Listes dynamiques
-
Gérer le contenu d'une liste en fonction du choix dans une autre liste
