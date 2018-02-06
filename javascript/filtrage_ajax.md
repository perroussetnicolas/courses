**Function de callback**

Une function qui prend en argument une autre function

    element.load('page.php', function(){
        alert('ok');
    });


**AJAX en jQuery**

La function change permet de détecter quand une option du select change

Cette function permet d'afficher le changement d'option d'un select

    $(document).ready(function(){
       $('#form').change(function(){
           //console.log('change !');
          $('#infoDpt').load("showInfo.php?dpt="+$("#dpt").val());
       });
    });

La page showInfo.php va effectuer le traitement des parametres GET : 
        
    switch ($_GET['dpt'])
    {
        case 'Info':
        echo "<p>Le département informatique</p>";
        break;
        case "Chimie":
            echo "<p>Le département de chimie</p>";
            break;
    }
    
Et le jQuery l'affichera sur la page où s'execute le script, précisement à l'id #infoDpt.


**TP de cours : filtrage catégories**

`qchampeno.eemi.tech/courses/javascript/filtrage_ajax.zip` 

