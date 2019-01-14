## Import from a Bank Austria CSV File into a mysql

Type of transaction:
0 = Minus
1 = Positive

```
<?php

$servername = "localhost:8889";
$username = "root";
$password = "root";
$dbname = "chatwallet";

$conn = new mysqli($servername, $username, $password, $dbname);


$filename = __DIR__ . '/imports/bankaustria2017.csv';
$file = fopen($filename, "r");

$account = 0;

         while (($emapData = fgetcsv($file, 10000, ";")) !== FALSE)
         { 
           $amount = str_replace(".", "", $emapData[5]);
           $amount = str_replace(",", ".", $amount);
           if($amount < 0){
             $type = 0;
             $amount = (float)$amount * -1;
           }else{
             $type = 1;
             $amount = (float)$amount;
           }
           
           $date_booking  = strtotime($emapData[0]);
           $date_booking =  date('Y-m-d', $date_booking);
           $date_valuta = strtotime($emapData[1]);
           $date_valuta =  date('Y-m-d', $date_valuta);
           
           echo $emapData[2] . " - " . $amount . " type: " . $type;
           echo "<br>";
           
           
           $sql = "INSERT into imports(
             account,
             date_buchung,
             date_valuta,
             buchungstext,
             currency,
             type,
             amount,    
             belegdaten, 
             belegnummer, 
             auftraggeber_name, 
             auftraggeber_konto, 	
             auftraggeber_blz,      
             empfaenger_name,
             empfaenger_konto,
             empfaenger_blz,
             transfersubject)

           values(
             '$account',
             '$date_booking',
             '$date_valuta',
             '$emapData[2]',
             '$emapData[4]', 
             '$type', 
             '$amount',
             '$emapData[6]',
             '$emapData[7]',
             '$emapData[8]', 
             '$emapData[9]', 
             '$emapData[10]',
             '$emapData[11]',
             '$emapData[12]',
             '$emapData[13]',
             '$emapData[14]'
             )";
             
             
             if ($conn->query($sql) === TRUE) {
                  echo "New record created successfully";
                  echo "<br>";
              } else {
                  echo "Error: " . $sql . "<br>" . $conn->error;
              }
          
            
         }
         fclose($file);
         echo "CSV File has been successfully Imported.";
?>
```
