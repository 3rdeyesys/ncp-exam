```php
<?php
	$unixtimestamp =  round(microtime(true) * 1000);
	$ncloud_api_accesskey = {Ncloud API AccessKey};
	$ncloud_api_secretkey = {Ncloud API SecretKey};
	$ncloud_secret_id = {Ncloud Secret Manager Secret ID};
	$apicall_method = "GET";
	$api_server = "https://secretmanager.apigw.ntruss.com";
	$api_url = "/api/v1/secrets/".$ncloud_secret_id."/values";

	$msg_signature = "";
	$space = " ";
	$new_line = "\n";
	$message =
            $apicall_method
            .$space
            .$api_url
            .$new_line
            .$unixtimestamp
            .$new_line
            .$ncloud_api_accesskey;
	$msg_signature = base64_encode(hash_hmac('sha256', $message, $ncloud_api_secretkey, true));

	$http_header = array();
	$http_header[0] = "x-ncp-apigw-timestamp:".$unixtimestamp."";
	$http_header[1] = "x-ncp-iam-access-key:".$ncloud_api_accesskey."";
	$http_header[2] = "x-ncp-apigw-signature-v2:".$msg_signature."";

	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, $api_server.$api_url);
	curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
	curl_setopt($ch, CURLOPT_HTTPHEADER, $http_header);
	$json_response = curl_exec($ch);
	curl_close($ch);

	$rows_response = json_decode($json_response, JSON_OBJECT_AS_ARRAY);

	var_dump($rows_response);
?>
```
