Tietokannan suunnittelu.

Ajatuksena oli suunnittella haavahoidossa olevien asiakkaita ylläpitävä tietokanta, mikä helpottaisi hoitajia löytämään osaston haavahoitoasiakkaat yhdestä paikasta ilman, että niitä ja niihin liittyviä tietoja joutuu etsimään potilastietojärjestelmästä.

![image](https://user-images.githubusercontent.com/88820019/207504684-ecdd5804-e6ed-402a-af14-84b74e83d329.png)

Käytetyt ohjelmat:
+ Draw.io
+ Visual Studio Code
+ MySql Workbench
+ Azure


INDEX.PHP koodin alussa style.css on linkitetty html:llään. Luotu sivulle kirjautumislomake, käytetty post menetelmää lomaketietojen lähettämiseen palvelimelle.
Lomakkeen käsittely sivu session.php mihin lomaketiedot lähetetään käsiteltäväksi. 
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

 

