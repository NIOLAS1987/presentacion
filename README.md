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

<!DOCTYPE html>
<html lang="en">
<head>

    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tienda</title>

    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous">


    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js" integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.min.js" integrity="sha384-w1Q4orYjBQndcko6MimVbzY0tgp4pWB4lZ7lr30WKz0vr/aWKhXdBNmNb5D92v7s" crossorigin="anonymous"></script>


</head>
<body>

    <nav class="navbar navbar-expand-lg navbar-light bg ligth fixed-top">
        <a claas="navbar-brand" hfre="index.php"><h2 class="display-6"> Pagina de venta de libros Harry poter</h2></a>
        <button class="navbar-toggler" data-target="#my-nav" data-toggle="collepse">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div id="my-nav" class="collapse navbar-collapse">
        <ul class="navbar-nav mr-auto">
            <li class="nav-item active">
                <a class="nav-link" href="index.php"><br><button
             class="btn btn-primary" 
             tipe="submit"
             name="btnAccion"
             Value="Inicio de la pagina"
             >Inicio de la pagina</button ></a>
             
            </li>
            <li class="nav-item active">
                <a class="nav-link" href="mostrarCarrito.php"><br><button
             class="btn btn-success" 
             tipe="submit"
             name="btnAccion"
             Value="Inicio de la pagina"
             >Carrito</button >(<?php 
                echo (empty($_SESSION['CARRITO']))?0:count($_SESSION['CARRITO']);
                
                ?>)</a>
                   
            </li>
        </ul>
        </div>
    </nav>
    <br/>
    <br/>
    <div class="container">   
    <?php 
include 'global/config.php';
include 'global/conexion.php';
include 'carrito.php';
include 'templates/cabecera.php';
?>

    <br>
        <?php if($mensaje!=""){ ?>
            <div class="alert alert-success">
        <?php echo $mensaje;?>
            <a href="mostrarCarrito.php" class="badge badge-succes">Ver carrito</a>
            </div>
        <?php }?>


            <div class="row">
        <?php 
            $sentencia=$pdo->prepare("SELECT * FROM `tblproductos`");
            $sentencia->execute();
            $listaProductos=$sentencia->fetchAll(PDO::FETCH_ASSOC);
            //print_r($listaProductos);?>

        <?php foreach($listaProductos as $producto){ ?>
        <div class="col-3"><br><br><br>
         <div class="card">

            <img title="<?php echo $producto['Nombre'];?>"
             alt="<?php echo $producto['Nombre'];?>"
             class="card-img-top"
             src="<?php echo $producto['Imagen'];?>"
             data-toggle="popover"
             data-trigger="hover"
             data-content="<?php echo $producto['Descripcion'];?> "
             height="317px">

            <div class="card-body">
                 <span><?php echo $producto['Nombre'];?></span>
                 <h5 class="card-title">$<?php echo $producto['Precio'];?></h5>
                 <p class="card-text"></p>

                <form action="" method="post">

                <input type="hidden" name="id" id="id" value="<?php echo openssl_encrypt($producto['ID'],COD,KEY);?>">
                <input type="hidden" name="nombre" id="nombre" value="<?php echo openssl_encrypt($producto['Nombre'],COD,KEY);?>">
                <input type="hidden" name="precio" id="preio" value="<?php echo openssl_encrypt($producto['Precio'],COD,KEY);?>">
                <input type="hidden" name="cantidad" id="cantidad" value="<?php echo openssl_encrypt(1,COD,KEY);?>">

                <button class="btn btn-primary"
                 name="btnAccion" 
                 value="Agregar" 
                 type="submit">
                 Agregar al carrito
                </button>
                </div>
                </form>

                </div>
            
        </div>
    </div>
    <?php }?>    
        

    </div>
    <script>

        $(function () {
        $('[data-toggle="popover"]').popover()
        })

    </script>  
    
    
<?php include 'templates/pie.php';?> 
<?php
session_start();

$mensaje="";

if(isset($_POST['btnAccion'])){

    switch($_POST['btnAccion']){

        case 'Agregar':

            if(is_numeric( openssl_decrypt($_POST['id'],COD,KEY ))){
                $ID=openssl_decrypt($_POST['id'],COD,KEY );
                $mensaje.="Ok ID correcto".$ID."<br/>";
            
            }else{
                $mensaje.="Ups ID incorrecto"."<br/>";
            }

            if(is_string(openssl_decrypt($_POST['nombre'],COD,KEY))){
                $NOMBRE=openssl_decrypt($_POST['nombre'],COD,KEY);
                $mensaje.="Ok nombre".$NOMBRE."<br/";
            }else{ $mensaje.="Upps... algo pasa con el nombre"."<br/";  break;}

            if(is_string(openssl_decrypt($_POST['cantidad'],COD,KEY))){
                $CANTIDAD=openssl_decrypt($_POST['cantidad'],COD,KEY);
                $mensaje.="Ok cantidad".$CANTIDAD."<br/";
            }else{ $mensaje.="Upps... algo pasa con la cantidad"."<br/";  break;}

            if(is_string(openssl_decrypt($_POST['precio'],COD,KEY))){
                $PRECIO=openssl_decrypt($_POST['precio'],COD,KEY);
                $mensaje.="Ok precio".$PRECIO."<br/";
            }else{ $mensaje.="Upps... algo pasa con el precio"."<br/";  break;}

        if(!isset($_SESSION['CARRITO'])){

            $producto=array(
                'ID'=>$ID,
                'NOMBRE'=>$NOMBRE,
                'CANTIDAD'=>$CANTIDAD,
                'PRECIO'=>$PRECIO
            );
            $_SESSION['CARRITO'][0]=$producto;
            $mensaje= "Producto agregado al carrito";

        }else{
            $idProductos=array_column($_SESSION['CARRITO'],"ID");

            if(in_array($ID,$idProductos)){
                echo "<script>alert('El producto ya ha sido seleccionado...')</script>";
            }else{

            }
            $NumeroProductos=count($_SESSION['CARRITO']);
            $producto=array(
                'ID'=>$ID,
                'NOMBRE'=>$NOMBRE,
                'CANTIDAD'=>$CANTIDAD,
                'PRECIO'=>$PRECIO
            );
            $_SESSION['CARRITO'][$NumeroProductos]=$producto;
            $mensaje= "Producto agregado al carrito";
        }
        //$mensaje= print_r( $_SESSION,true);
         
        
        break;
        case "Eliminar":
            if(is_numeric( openssl_decrypt($_POST['id'],COD,KEY ))){
                $ID=openssl_decrypt($_POST['id'],COD,KEY );
                
                foreach($_SESSION['CARRITO'] as $indice=>$producto){
                    if($producto['ID']==$ID){
                        unset($_SESSION['CARRITO'][$indice]);
                        echo "<script>alert('Elemento borrado...');</script>";

                    }


                }
            
            }else{
                $mensaje.="Ups ID incorrecto"."<br/>";
            }

        break;

    

}
}

?>  
<?php 
include 'global/config.php';
include 'carrito.php';
include 'templates/cabecera.php';

?>

<br><br><br><br>
<h3>Lista del carrito</h3>
<?php if(!empty($_SESSION['CARRITO'])) { ?>
<table class="table table-light table-bordered">
    <tbody>
        <tr>
            <th width="40%">Descipcion> </th>
            <th width="15%" class="text-center">Cantidad</th>
            <th width="20%" class="text-center">Precio</th>
            <th width="20%" class="text-center">Total</th>
            <th width="5%" class="text-center">Eliminar</th>
        </tr>
        <?php $total=0;?>
        <?php foreach($_SESSION['CARRITO'] as $indice=>$producto){?>
        <tr>
            <td width="40%"><?php echo $producto['NOMBRE'] ?> </td>
            <td width="15%" class="text-center"><?php echo $producto['CANTIDAD'] ?></td>
            <td width="20%" class="text-center">$<?php echo $producto['PRECIO'] ?></td>
            <td width="20%" class="text-center">$<?php echo number_format($producto['PRECIO']*$producto['CANTIDAD'],2);?></td>
            <td width="5%">
                
            <form action="" method="post">

            <input type="hidden"
             name="id"
              id="id" 
              value="<?php echo openssl_encrypt($producto['ID'],COD,KEY);?>">

            <button
             class="btn btn-danger" 
             tipe="submit"
             name="btnAccion"
             Value="Eliminar"
             >Eliminar</button >
            </form>
            </td>
        </tr>
        <?php $total=$total+($producto['PRECIO']*$producto['CANTIDAD']);?>
        <?php } ?>
        <tr>
            <td colspan="3" align="right"><h3>Total</h3></td>
            <td align="right"><h3>$<?php echo number_format($total,2);?></h3></td>
            <td></td>
        </tr>
        <tr>
            <td colspan="5">
            <form action="pagar.php" method="post">
            <div class="alert alert-success">
                <div class="form-group">
                        <label for="my-input">Correo de contacto:</label>
                        <input id="email" neme="email"
                        class="form-control" 
                        type="email"
                        placeholter="Por favor escribe tu correo"
                        required>
                </div>
                <small id="emailHelp"
                class="form-text text-muted "
                >
                Los productos se enviaran a su direccion.
                </small>

                   </div> 

                   <button class="btn btn-primary btn-lg btn-block" type="submit"
                   name="btnAccion"
                   value="proceder">
                       Proceder a pagar >>
                   </button>
                
                </form>
            </td>
        </tr>

        </tbody>
</table>
<?PHP }else ?>

<div class="alert alert-success">
    No hay producto en el carrito...
</div>

<?php 
include 'templates/pie.php';
?>  

<?php 
include 'global/config.php';
include 'global/conexion.php';
include 'carrito.php';
include 'templates/cabecera.php';
?>

<?php
if($_POST){
    $total=0;
    $SID=session_id();
    $Correo="nio@gmail.com";

    foreach($_SESSION['CARRITO'] as $indice=>$producto){

        $total=$total+($producto['PRECIO']*$producto['CANTIDAD']);
    
    }
        $sentencia=$pdo->prepare("INSERT INTO `tblventas` 
                            (`ID`, `ClaveTransaccional`, `PlaypalDatos`, `Fecha`, `Correo`, `Total`, `Status`)
        VALUES (NULL,:ClaveTransaccional, '', NOW(),:Correo,:Total, 'pendiente');");
        
        $sentencia->bindParam("ClaveTransaccional",$SID);
        $sentencia->bindParam("Correo",$Correo);
        $sentencia->bindParam("Total",$total);
        $sentencia->execute();
        $idVenta=$pdo->lastInsertId();

        foreach($_SESSION['CARRITO'] as $indice=>$producto){
        
            $sentencia=$pdo->prepare("INSERT INTO `tbldetalleventa`
             (`ID`, `IDVENTA`, `IDPRODUCTO`, `PRECIOUNITARIO`, `CANTIDAD`, `DESCARGADO`)
             VALUES (NULL,:IDVENTA,:IDPRODUCTO,:PRECIOUNITARIO,:CANTIDAD, '0');");

            $sentencia->bindParam("IDVENTA",$idVenta);
            $sentencia->bindParam("IDPRODUCTO",$producto['ID']);
            $sentencia->bindParam("PRECIOUNITARIO",$producto['PRECIO']);
            $sentencia->bindParam("CANTIDAD",$producto['CANTIDAD']);
            $idVenta=$pdo->lastInsertId();

        }
    //echo "<h3>".$total."</h3";

}
?>
<div class="jumbotron text-center">
    <h1 class="display-4">!Gracias por la compra¡</h1>
    <hr class="my-4">
    <p class="lead">El valor de la compra es de:
        <h4>$<?php echo number_format($total,2); ?></h4>
    </p>
    
    <p>Los productos una vez se pagen se procedera al envio:<br>
    <strong>(Para aclaraciones :nicolas.munoz@aviatur.com)</strong>
    </p>

</div>



<?php include 'templates/pie.php';?>
