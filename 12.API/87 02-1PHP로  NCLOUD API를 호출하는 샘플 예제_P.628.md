```php
<?php

	// 기본 데이터 설정
	$unixtimestamp =  round(microtime(true) * 1000);

	$ncp_accesskey = "네이버 클라우드 AccessKey";
	$ncp_secretkey = "네이버 클라우드 SecretKey";

	$api_server = "https://billingapi.apigw.ntruss.com";

	// API URL 예시 : 상품별 가격 리스트 호출 api
	$api_url = "/billing/v1/product/getProductPriceList";
	$api_url = $api_url."?regionCode=KR&productItemKindCode=VSVR";

	$apicall_method = "GET";
	$space = " ";
	$new_line = "\n";

	$is_post = false;

	// hmac으로 암호화할 문자열 설정
	$message =
		$apicall_method
		.$space
		.$api_url
		.$new_line
		.$unixtimestamp
		.$new_line
		.$ncp_accesskey;

	// hmac_sha256 암호화
	$msg_signature = hash_hmac("sha256", $message, $ncp_secretkey, true);
	$msg_signature = base64_encode($msg_signature);

	// http 호출 헤더값 설정
	$http_header = array();
	$http_header[0] = "x-ncp-apigw-timestamp:".$unixtimestamp."";
	$http_header[1] = "x-ncp-iam-access-key:".$ncp_accesskey."";
	$http_header[2] = "x-ncp-apigw-signature-v2:".$msg_signature."";

	// api 호출
	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, $api_server.$api_url);
	curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
	curl_setopt($ch, CURLOPT_POST, $is_post);
	curl_setopt($ch, CURLOPT_HTTPHEADER, $http_header);

	$response = curl_exec($ch);

	curl_close($ch);

?>
```