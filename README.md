Tietokannan suunnittelu.

Ajatuksena oli suunnittella haavahoidossa olevien asiakkaita ylläpitävä tietokanta, mikä helpottaisi hoitajia löytämään osaston haavahoitoasiakkaat yhdestä paikasta ilman, että niitä ja niihin liittyviä tietoja joutuu etsimään potilastietojärjestelmästä.

Drow.io:lla piirretty tietokantasuunitelma

![tietokantakuva3](https://user-images.githubusercontent.com/88820019/208730592-7989a1ab-c05a-403c-bf22-239e623da800.png)





Käytetyt ohjelmat:
+ Draw.io
+ Visual Studio Code
+ MySql Workbench
+ Azure

-------------------------------

+ Tietokannan ja taulukoiden luominen MySql Worbechillä

+ MySql Workbench:illä taulukoiden luominen sql kielellä



![tietokanta1](https://user-images.githubusercontent.com/88820019/208717161-fc5051ab-2e47-488e-a04f-3dee89a59706.png)


![tietokanta2](https://user-images.githubusercontent.com/88820019/208717290-9da000ab-e2ae-4f11-97b8-35c8d3316655.png)


![tietokanta3](https://user-images.githubusercontent.com/88820019/208717423-9748d55d-3b36-4490-958a-b2d57e332182.png)


+ php sivujen luominen Visual Studio Code:ssa
+ Palvelimen käynnistäminen localhostissa 



index.php

koodin alussa style.css on linkitetty html:llään. Luotu sivulle kirjautumislomake, käytetty post menetelmää lomaketietojen lähettämiseen palvelimelle.
Lomakkeen käsittely sivu session.php mihin lomaketiedot lähetetään käsiteltäväksi. Lähetä painike lähettää tiedot käsittely sivulle.  
```
<html>
    <head>
    <link rel="stylesheet" href="style.css">
    </head>
 <body>
    <div class="login">
     <form name="kirjautumislomake" method="post" action="session.php">
           <h3>Kirjaudu sisään</h3>
           
        <table>
          <tr>
            <td><label for="k_nimi">Nimi:</label></td>
            <td><input type="text" name="nimi" id="k_nimi" size="20"/> </td>
            </tr>
            <tr>
            <td><label for="s_sana">Salasana:</label></td>
            <td><input type="password" name="salasana" id="s_sana" size="20"/></td>
          </tr>
        </table>
    <p>
    <input type="submit" name="laheta" value="Lähetä"\>
    </p>
    
    </form>
   </div>   
  </body>
</html>   
```
Koodi selaimella:


![kirjaudusisaan](https://user-images.githubusercontent.com/88820019/208705497-e9213042-5983-4fb5-857c-3abacce536fd.png)

```
session.php 
Aluksi määritellään muuttujat $_POST kerää lomaketiedot methon = "post" kanssa


<?php

$nimi = $_POST['nimi'];  
$salasana = $_POST['salasana'];


```
Yhteyden luominen tietokantaan;

```


$servername = "xxxxxxx";
$username = "xxxxx";
$password = "xx";
$dbname = "xxxx";

mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);
$conn = new mysqli($servername, $username, $password, $dbname);
if ($conn->connect_error) {
   die("Connection failed: " . $conn->connect_error);

}
```
Sql käskyllä haetaan tietokannasta tiedot nimi ja salasana
```
$sql = "SELECT*FROM Osasto WHERE nimi = '$nimi' and salasana = '$salasana'";  
$result = mysqli_query($conn, $sql);  
$row = mysqli_fetch_array($result, MYSQLI_ASSOC);  
$count = mysqli_num_rows($result);  

```
jos tiedot täsmää tietokannan kanssa, siirrytään asiaks.php sivulle
```

if($count== 1){  
  $_SESSION['nimi'] = $nimi;
  $_SESSION['salasana'] = $salasana;
  
  header('Location:asiakas.php');

```
jos tiedot ei täsmää tietokannan kanssa, siirrytään index.php sivulle
```
  
}else{  
  $_SESSION['nimi'] = $nimi;
  $_SESSION['salasana'] = $salasana;
  header('Location:index.php'); 
   
} 

$conn->close();
?>

```
Asiakas ja tuote.php sivu. 
```

<!DOCTYPE HTML>  
<html>
    <head>
    <link rel="stylesheet" href="style.css">
    </head>
<body> 
<div class ="asiakastuote">
     

<center><h3>Lisää asiakas:</h3> 
<form name= "asiakas" method="post" action="insert.php?source=1">
     
     <label for = "nimi"> nimi: </label>
     <input type="text" name="nimi"> 
     
<br><br>

<button type="submit" name= "asiakasnappi" value="lisää">lisää</button> <button type="reset" value="Reset">tyhjennä</button> 

</form>


<div class="etusivu">      
<form name= "asiakas" method="post" action="insert.php?source=3">

    <p>
    <input type="submit" name="laheta" value="hae"\>
    </p>

</form>
</div> 



<h3>Lisää tuote:</h3> <center>
<form name= "tuote" method="post" action="insert.php?source=2">

     <label for = "kuvaus"> tuote: </label>
     <input type="text" name="kuvaus"> 

    <label for = "asiakasid"> asiakasid</label>
     <input type="text" name="asiakasid"> 


<br><br>
<button type="submit" name="tuotenappi" value="lisää">lisää</button> <button type="reset" value="Reset">tyhjennä</button> 

</form>



<form method="POST" action="insert.php?source=4"> <center>

 <p> 
<input type="submit" name= "laheta" value="Hae"\>
</p>  

</form>    


 </body>
</html>

```


Koodi selaimella

![tietokantakuva1](https://user-images.githubusercontent.com/88820019/208727534-6dd42542-9929-43e3-8a73-006ad73927fe.png)


```

```
insert.php asiakas ja tuote sivun käsittelysivu.

aluksi yhteys tietokantaan
```

<?php

 $servername = "xxx";
 $username = "xxx";
 $password = "xxx";
 $dbname = "xxx";
 
 mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);
 $conn = new mysqli($servername, $username, $password, $dbname);
 if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
 
}

```
Asiakkaan lisääminen tietokantaan 
```

if($_GET['source'] ==1){
  $user_id = $_POST['user_id'];
  $asiakasid = $_POST['asiakasid'];
  $nimi =  $_POST['nimi'];
  
  $sql = "INSERT INTO Asiakas VALUES ('1',0,'$nimi')"; 
  $result = mysqli_query($conn, $sql);
  header('location:asiakas.php'); 
}

```
Tuotteen lisääminen tietokantaan 
```

if($_GET['source'] ==2){

  $id = $_POST['id'];
  $kuvaus = $_POST['kuvaus'];
  $asiakasid = $_POST['asiakasid'];

  $sql = "INSERT INTO Tuote VALUES (0,'$kuvaus','$asiakasid')";
  $result = mysqli_query($conn, $sql);
  header('Location:asiakas.php');

}

```
Asiakkaan ja asiakasid:n haku ja tulostaminen tietokannasta 
```

if($_GET['source'] ==3){

  $sql = "SELECT nimi,asiakasid FROM Asiakas";
  $result = mysqli_query($conn, $sql);


   if($result->num_rows > 0)
      while($row = $result->fetch_assoc())
       echo "asiakasid: " . $row["asiakasid"]. " - asiakas: " . $row["nimi"]. "<br>";
      
else
      echo "0 results";
    }
    
    
```
Asiakkaan ja tuotteiden haku sekä tulostaminen tietokannasta. 
```

if($_GET['source'] ==4){
 
 $sql=" SELECT Asiakas.nimi AS Asiakas, Tuote.kuvaus AS Tuote FROM Asiakas, Tuote WHERE Asiakas.asiakasid = Tuote.asiakasid"; 
 $result = mysqli_query($conn, $sql); 
    

   if($result->num_rows > 0)
     while($row = $result->fetch_assoc())  
      echo " asiakas: " .  $row["Asiakas"]. " tuote: " .  $row["Tuote"]. "<br>";  


else
      echo "0 results";
}



$conn->close();


?>

```
style.css sivujen tyyli ja asettelu
```

body{   
    background: rgb(132, 167, 152);  
}  

.login{
    width: 400px;
    margin: 100px auto;
}

.asiakastuote{
    width: 800px;
    margin: 100px auto;

}

.tuotteet{
    width: 400px;
    margin: 100px auto;

}

.etusivu{
    background: rgb(132, 167, 152);
    
    
}








