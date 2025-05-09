
```php
<?php

  // 업로드한 파일을 바이너리로 읽어서 BASE64로 인코딩
  $ocr_upload_file = $_FILES["clova_ocr_upload_file"];
  $ocr_upload_file_tmp_name = $ocr_upload_file["tmp_name"];

  $ocr_upload_file_binary = fread(fopen($ocr_upload_file_tmp_name, "r"), filesize($ocr_upload_file_tmp_name));
  $ocr_upload_file_string = base64_encode($ocr_upload_file_binary);

  // Secret Key
  $ncloud_clova_ocr_secretkey = "Ijkouhuh89****중간 생략****hvhj&T*^VHJ&T*R^FVHJHJ";

  // API Gateway Invoke URL
  $api_url = "https://***.apigw.ntruss.com/custom/v1/28410/****중간 생략****/infer";

  // Template ID
  $ncloud_clova_ocr_template_ids = [28222];

  $unixtimestamp =  round(microtime(true) * 1000);

  // http 호출 헤더값 설정
  $http_header = array();
  $http_header[0] = "X-OCR-SECRET: ".$ncloud_clova_ocr_secretkey."";
  $http_header[1] = "Content-Type:application/json; charset=utf-8";

  $ocr_request_id = "ocr-test".$unixtimestamp;

  // 전송할 값들을 배열 형태로 저장
  $postvars = [
    "version"=> "V2",
    "requestId"=> $ocr_request_id,
    "timestamp"=> $unixtimestamp,
    "lang"=> "ko",
    "images"=> [
      [
        "format"=> "jpg",
        "name"=> "ocr-test",
        "data"=> $clova_ocr_upload_file_string,
        "url"=> null,
        "templateIds"=> $ncloud_clova_ocr_template_ids
      ]
    ]
  ];

  // 배열 형태로 저장한 값들을 json 형태로 변환
  $json_portvars = json_encode($postvars);

  // api 호출
  $ch = curl_init();
  curl_setopt($ch, CURLOPT_URL, $api_url);
  curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
  curl_setopt($ch, CURLOPT_POST, TRUE);
  curl_setopt($ch, CURLOPT_HTTPHEADER, $http_header);
  curl_setopt($ch,CURLOPT_POSTFIELDS, $json_portvars);

  $json_response = curl_exec($ch);
  $err = curl_error($ch);
  $status_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);

  curl_close($ch);


  if ($json_response && $status_code == "200")
  {
    $obj_response_array = json_decode($json_response, true);

    $obj_clova_ocr_data = $obj_response_array["images"][0];

    // 사업자 등록번호
    $business_number = $obj_clova_ocr_data["fields"][0];
    $business_number= $business_number["inferText"];

    //=============== 중간 생략 ================= //

    // 종목
    $business_item = $obj_clova_ocr_data["fields"][5];
    $business_item= $business_item["inferText"];

    echo("사업자 등록번호:".$business_number."<br />");
    echo("종목:".$business_item."<br />");
  }
  else
  {
    $obj_error_response_array = json_decode($json_response, true);
  }


?>
```
