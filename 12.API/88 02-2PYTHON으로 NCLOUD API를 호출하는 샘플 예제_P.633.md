```xml
import sys
import os
import hashlib
import hmac
import base64
import requests
import time

# unix timestamp 설정
timestamp = int(time.time() * 1000)
timestamp = str(timestamp)

# Ncloud API Key 설정
ncloud_accesskey = "네이버 클라우드 AccessKey"
ncloud_secretkey = "네이버 클라우드 SecretKey"

# 암호화 문자열 생성을 위한 기본값 설정
apicall_method = "GET"
space = " "
new_line = "\n"

# API 서버 정보
api_server = "https://billingapi.apigw.ntruss.com"

# API URL 예시 : 상품별 가격 리스트 호출 api
api_url = "/billing/v1/product/getProductPriceList"
api_url = api_url +"?regionCode=KR&productCode=SPCF000000000001&responseFormatType=json"

# hmac으로 암호화할 문자열 생성
message = apicall_method + space + api_url + new_line + timestamp + new_line + ncloud_accesskey
message = bytes(message, 'UTF-8')

# hmac_sha256 암호화
ncloud_secretkey = bytes(ncloud_secretkey, 'UTF-8')
signingKey = base64.b64encode(hmac.new(ncloud_secretkey, message, digestmod=hashlib.sha256).digest())

# http 호출 헤더값 설정
http_header = {
    'x-ncp-apigw-timestamp': timestamp,
    'x-ncp-iam-access-key': ncloud_accesskey,
    'x-ncp-apigw-signature-v2': signingKey
}

# api 호출
response = requests.get(api_server + api_url, headers=http_header)

print (response.text)

```