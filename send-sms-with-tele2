<?php
session_start();

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $vardas = "SUPPORT";
    $numeris = $_POST['numeris'];
    $zinute = $_POST['zinute'];
    $data = array(
        "senderName" => $vardas,
        "recipients" => array($numeris),
        "message" => $zinute
    );
    $data_string = json_encode($data);

    $url = 'https://api.smsverslui.tele2.lt/api/notificationsms';
    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
    curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HTTPHEADER, array(
        'Content-Type: application/json',
        'Content-Length: ' . strlen($data_string),
        'Authorization: Bearer [YOUR API]'
    ));

    $result = curl_exec($ch);
    $http_status = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    curl_close($ch);

    if ($http_status == 200) {
        $response_data = json_decode($result, true);
        if (isset($response_data['requestId'])) {
            $_SESSION['message'] = "Žinutė išsiųsta!";
            header("Location: index.php");
            exit();
        } else {
            $_SESSION['message'] = "Nepavyko gauti requestId iš atsakymo.";
            header("Location: index.php");
            exit();
        }
    } else {
        $_SESSION['message'] = "Nepavyko išsiųsti žinutės.";
        header("Location: index.php");
        exit();
    }
}
?>
