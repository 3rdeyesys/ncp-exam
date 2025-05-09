```php
<?php
  $unixtimestamp =  round(microtime(true) * 1000);
  $ncloud_sub_account_accesskey = "{기간 제한 없는 Sub Account API Access Key}";
  $ncloud_sub_account_secretkey = "{기간 제한 없는 Sub Account API Secret Key}";
  $apicall_method = "POST";
  $api_server = "https://sts.apigw.ntruss.com";
  $api_url = "/api/v1/credentials";
  $msg_signature = "";

  $array_postvars = Array (
    "durationSec" => 900
  );
  $postvars = json_encode($array_postvars);

  $space = " ";
  $new_line = "\n";
  $message =
    $apicall_method
    .$space
    .$api_url
    .$new_line
    .$unixtimestamp
    .$new_line
    .$ncloud_sub_account_accesskey;
  $msg_signature = base64_encode(hash_hmac('sha256', $message, $ncloud_sub_account_secretkey, true));

  $http_header = array();
  $http_header[0] = "Content-Type:application/json; charset=utf-8";
  $http_header[1] = "x-ncp-apigw-timestamp:".$unixtimestamp."";
  $http_header[2] = "x-ncp-iam-access-key:".$ncloud_sub_account_accesskey."";
  $http_header[3] = "x-ncp-apigw-signature-v2:".$msg_signature."";

  $ch = curl_init();
  curl_setopt($ch, CURLOPT_URL, $api_server.$api_url);
  curl_setopt($ch, CURLOPT_HTTPHEADER, $http_header);
  curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
  curl_setopt($ch, CURLOPT_POST, TRUE);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $postvars);
  $json_response = curl_exec($ch);
  curl_close($ch);

  if ($json_response)
  {
    $obj_array = json_decode($json_response, 1);

    $sts_accesskey= $obj_array["accessKey"];
    $sts_secretkey= $obj_array["keySecret"];
    $sts_createtime= $obj_array["createTime"];
    $sts_expiretime= $obj_array["expireTime"];
    $sts_use_mfa= $obj_array["useMfa"];
  }
?>

```