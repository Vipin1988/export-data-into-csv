# export-data-into-csv
<?php <br/>
 $dbHost     = "localhost"; <br/>
 $dbUsername = "root"; <br/>
 $dbPassword = "root"; <br/>
 $dbName     = "database"; <br/>
 // Create database connection <br/>
 $db = new mysqli($dbHost, $dbUsername, $dbPassword, $dbName); <br/>
 // Check connection <br/>
 if ($db->connect_error) { <br/>
     die("Connection failed: " . $db->connect_error); <br/>
 }
 //global $wpdb;<br/>
 // Fetch records from database <br/>
ob_start();<br/>
$query = $db->query("SELECT * FROM zipcodes ORDER BY id ASC");<br/>
if($query->num_rows > 0){ <br/>
    $delimiter = ","; <br/>
    $filename = "members-data_" . date('Y-m-d') . ".csv"; <br/>
    // Create a file pointer <br/>
    $f = fopen('php://memory', 'w'); <br/>
    // Set column headers <br/>
    $fields = array('Zipcode','Mobile','Scan','Partner Office','Impression Kit','Technician Name','Technician Email','Technician Email','Technician Picture','Partner Name','Partner Email','Partner Phone','Partner Street','Partner APT','Partner City','Partner Zipcode','Partner State'); <br/>			
    fputcsv($f, $fields, $delimiter); <br/>
    // Output each row of the data, format line as csv and write to file pointer <br/>
    while($row = $query->fetch_assoc()){ <br/>
        $lineData = array($row['zipcode'], <br/>$row['mobile'],$row['scan'],$row['partner_office'],$row['impression_kit'],$row['technician_name'],$row['technician_email'],$row['image'],$row['partner_name'],$row['partner_email'],$row['partner_name'],$row['partner_mobile'],$row['partner_street'],$row['partner_apt'],$row['partner_city'],$row['partner_zipcode'],$row['partner_state']); <br/>
        fputcsv($f, $lineData, $delimiter); <br/>
        ob_end_clean();<br/>
    } 
    // Move back to beginning of file <br/>
    fseek($f, 0); <br/>
    // Set headers to download file rather than displayed <br/>
    header('Content-Type: text/csv'); <br/>
    header('Content-Disposition: attachment; filename="' . $filename . '";'); <br/>
    //output all remaining data on a file pointer <br/>
    fpassthru($f); <br/>
} <br/>
exit; <br/>
 
?>
