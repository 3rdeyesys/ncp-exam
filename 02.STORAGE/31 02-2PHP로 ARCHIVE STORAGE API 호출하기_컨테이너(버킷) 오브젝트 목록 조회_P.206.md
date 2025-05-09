```xml
<?php

  $x_auth_token = "Archive Storage API 인증 토큰";

  $ncloud_project_id = "Archive Storage 프로젝트 ID";
  $ncloud_container = "Archive Storage 컨테이너(버킷) 이름";

  $api_server = "https://kr.archive.ncloudstorage.com";
  $api_url = "/v1/AUTH_".$ncloud_project_id."/".$ncloud_container."?format=json";

  // http 호출 헤더값 설정
  $http_header = array();
  $http_header[0] = "X-Auth-Token: ".$x_auth_token;
  $http_header[1] = "charset=UTF-8";

  // api 호출
  $ch = curl_init();
  curl_setopt($ch, CURLOPT_URL, $api_server.$api_url);
  curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
  curl_setopt($ch, CURLOPT_HTTPHEADER, $http_header);
  curl_setopt($ch, CURLOPT_HEADER, TRUE); //request에 header 값도 수신
  curl_setopt($ch, CURLOPT_POST, FALSE); //GET 방식으로 호출

  $response = curl_exec($ch);
  $http_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
  curl_close($ch);

  if ($response)
  {
  	if ($http_code == 200)
	{
		// response에서 header 값 분리
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

		// response에서 json형태의 body 값 분리
		$json_response = substr($response, strpos($response, "\r\n\r\n"));
		$rows_response = json_decode($json_response, JSON_OBJECT_AS_ARRAY);

		$object_count = $headers["X-Container-Object-Count"];
		$used_bytes = $headers["X-Container-Bytes-Used"];
		$used_k_bytes = $used_bytes / 1024;
		$used_m_bytes = $used_k_bytes / 1024;
		$used_g_bytes = $used_m_bytes / 1024;
	}
	else if ($http_code == 404)
	{
		echo("존재하지 않는 컨테이너(버킷)입니다");

		$rows_response = [];

		$object_count = 0;
		$used_bytes = 0;
		$used_k_bytes = 0;
		$used_m_bytes = 0;
		$used_g_bytes = 0;
	}
	else
	{
		echo($response);
	}

  }
  else
  {
  	echo("Error");
  }

?>


<?php  {linenos=true,linenostart=1}
  // 오브젝트 목록 출력
  $cnt = 0;
  foreach ($rows_response as $row)
  {
  	$archive_object_name = $row["name"];
  	$archive_object_hash = $row["hash"];
  	$archive_object_content_type = $row["content_type"];
  	$archive_object_last_modified = $row["last_modified"];
  	$archive_object_bytes = $row["bytes"];

  	$cnt++;
  ?>
	<tr>
	  <td><?php echo($cnt);?></td>
	  <td><?php echo($archive_object_name);?></td>
	  <td><?php echo($archive_object_content_type);?></td>
	  <td><?php echo($archive_object_hash);?></td>
	  <td><?php echo($archive_object_last_modified);?></td>
	  <td><?php echo(number_format($archive_object_bytes));?></td>
	</tr>

<?php
  }
?>
```