```xml
<?php

  // 전송해야 할 설정값
  $ncloud_accesskey = "네이버 클라우드 AccessKey";
  $ncloud_secretkey = "네이버 클라우드 SecretKey";
  $ncloud_domain_id = "Archive Storage 도메인 ID";
  $ncloud_project_id = "Archive Storage 프로젝트 ID";

  $api_server = "https://kr.archive.ncloudstorage.com:5000";
  $api_url = "/v3/auth/tokens";

  // http 호출 헤더값 설정
  $http_header = array();
  $http_header[0] = "Content-Type: application/json";

  // 전송할 값들을 배열 형태로 저장한다
  $postvars = [
  	"auth"=> [
  		"identity"=> [
  			"methods"=> [
  				"password"
  			],
  			"password"=> [
  				"user"=> [
  					"name"=> $ncloud_accesskey,
  					"password"=> $ncloud_secretkey,
  					"domain"=> [
  						"id"=> $ncloud_domain_id
  					]
  				]
  			]
  		],
  		"scope"=> [
  			"project"=> [
  				"id"=> $ncloud_project_id
  			]
  		]
  	]
  ];

  // 배열 형태로 저장한 값들을 json 형태로 변환해서 전송
  $json_portvars = json_encode($postvars);

  // api 호출
  $ch = curl_init();
  curl_setopt($ch, CURLOPT_URL, $api_server.$api_url);
  curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
  curl_setopt($ch, CURLOPT_POST, TRUE); //POST 방식으로 호출
  curl_setopt($ch, CURLOPT_HTTPHEADER, $http_header);
  curl_setopt($ch, CURLOPT_HEADER, TRUE); //response에 header 값도 수신
  curl_setopt($ch,CURLOPT_POSTFIELDS, $json_portvars);

  $response = curl_exec($ch);

  curl_close($ch);

  if ($response)
  {
  	// X-Subject-Token 토큰 값은 request body가 아닌 header로 전달되므로
  	// header를 분리해서 배열에 저장한다
  	$headers = array();
  	$header_text = substr($response, 0, strpos($response, "\r\n\r\n"));

  	foreach (explode("\r\n", $header_text) as $i => $line)
  	{
  		if ($i === 0)
  		{
  		   $headers["http_code"] = $line;
  		}
  		else
  		{
  		   list ($key, $value) = explode(": ", $line);
  		   $headers[$key] = $value;
  		}
  	}

	// 인증 토큰 확인
  	$x_auth_token = $headers["X-Subject-Token"];
  	echo($x_auth_token);

  	//var_dump($headers);
  	//echo("<hr>");
  	//var_dump($response);
  }
  else
  {
  	echo "Curl error: " . curl_error($ch);
  }
?>
```