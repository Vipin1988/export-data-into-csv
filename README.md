# export-data-into-csv
<?php 
 $dbHost     = "localhost"; 
 $dbUsername = "root"; 
 $dbPassword = "root"; 
 $dbName     = "database"; 
 // Create database connection 
 $db = new mysqli($dbHost, $dbUsername, $dbPassword, $dbName); 
 // Check connection 
 if ($db->connect_error) { 
     die("Connection failed: " . $db->connect_error); 
 }
 //global $wpdb;
 // Fetch records from database 
ob_start();
$query = $db->query("SELECT * FROM zipcodes ORDER BY id ASC");
if($query->num_rows > 0){ 
    $delimiter = ","; 
    $filename = "members-data_" . date('Y-m-d') . ".csv"; 
    // Create a file pointer 
    $f = fopen('php://memory', 'w'); 
    // Set column headers 
    $fields = array('Zipcode','Mobile','Scan','Partner Office','Impression Kit','Technician Name','Technician Email','Technician Email','Technician Picture','Partner Name','Partner Email','Partner Phone','Partner Street','Partner APT','Partner City','Partner Zipcode','Partner State'); 			
    fputcsv($f, $fields, $delimiter); 
    // Output each row of the data, format line as csv and write to file pointer 
    while($row = $query->fetch_assoc()){ 
        $lineData = array($row['zipcode'], $row['mobile'],$row['scan'],$row['partner_office'],$row['impression_kit'],$row['technician_name'],$row['technician_email'],$row['image'],$row['partner_name'],$row['partner_email'],$row['partner_name'],$row['partner_mobile'],$row['partner_street'],$row['partner_apt'],$row['partner_city'],$row['partner_zipcode'],$row['partner_state']); 
        fputcsv($f, $lineData, $delimiter); 
        ob_end_clean();
    } 
    // Move back to beginning of file 
    fseek($f, 0); 
    // Set headers to download file rather than displayed 
    header('Content-Type: text/csv'); 
    header('Content-Disposition: attachment; filename="' . $filename . '";'); 
    //output all remaining data on a file pointer 
    fpassthru($f); 
} 
exit; 
 
?>
