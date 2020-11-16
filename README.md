# presentacion
Presentacion
<?php
define("KEY","develoteca");
define("COD","AES-128-ECB");

define("SERVIDOR","localhost");
define("USUARIO","root");
define("PASSWORD","");
define("BD","tienda");

?>

<?php 
$servidor="mysql:dbname=".BD.";host=".SERVIDOR;

try{

    $pdo= new pdo($servidor,USUARIO,PASSWORD,
                array(pdo::MYSQL_ATTR_INIT_COMMAND=>"SET NAMES utf8")
            );
    //echo "<script>alert('Conectado...')</script>";        

}catch(PDOException $e){

    //echo "<script>alert('Error...')</script>"; 
}


?>
