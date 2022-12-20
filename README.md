Tietokannan suunnittelu.

Ajatuksena oli suunnittella haavahoidossa olevien asiakkaita ylläpitävä tietokanta, mikä helpottaisi hoitajia löytämään osaston haavahoitoasiakkaat yhdestä paikasta ilman, että niitä ja niihin liittyviä tietoja joutuu etsimään potilastietojärjestelmästä.

![image](https://user-images.githubusercontent.com/88820019/207504684-ecdd5804-e6ed-402a-af14-84b74e83d329.png)

Käytetyt ohjelmat:
+ Draw.io
+ Visual Studio Code
+ MySql Workbench
+ Azure


MySql Workbench:illä taulukoiden luominen sql käskyillä

![tietokanta1](https://user-images.githubusercontent.com/88820019/208717161-fc5051ab-2e47-488e-a04f-3dee89a59706.png)


![tietokanta2](https://user-images.githubusercontent.com/88820019/208717290-9da000ab-e2ae-4f11-97b8-35c8d3316655.png)


![tietokanta3](https://user-images.githubusercontent.com/88820019/208717423-9748d55d-3b36-4490-958a-b2d57e332182.png)


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
Koodi palvelimella:


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


$servername = "hyvis.mysql.database.azure.com";
$username = "db_projekti";
$password = "Sivyh2022";
$dbname = "Marika_db";

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


