## Management > Certificate Manager > API v1.0 가이드

Certificate Manager는 인증서 업로드, 다운로드를 위한 API를 제공합니다. 클라이언트는 콘솔에서 인증서와 인증서 파일을 등록한 후 API를 통해 데이터를 사용할 수 있습니다.

| Method | URI | 설명 |
| ------ | --- | --- |
| POST | /certmanager/v1.0/appkeys/{appKey}/certificates/{certificateName}/files | 등록되어있는 인증서에 파일을 업로드합니다. 파일이 등록되어 있는 경우, 업로드하는 파일로 교체됩니다. |
| GET | /certmanager/v1.0/appkeys/{appKey}/certificates/{certificateName}/files | 등록되어있는 인증서 파일을 다운로드합니다. |

[API 요청의 경로 변수]

| 값 | 타입 | 설명 |
| --- | --- | --- |
| appKey | String | 사용하려는 데이터를 저장하고 있는 TOAST 프로젝트의 앱 키 |
| certificateName | String | 사용하려는 데이터(인증서)의 이름 |

[API 응답의 데이터 공통 헤더]

``` json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "body": {

    }
}
```

| 값 | 타입 | 설명 |
| --- | --- | --- |
| resultCode | Number | API 호출 결과 코드값 |
| resultMessage | String | API 호출 결과 메시지 |
| isSuccessful | Boolean | API 호출 성공 여부 |

### 인증서 파일 업로드

Certificate Manager에 등록한 인증서에 파일을 업로드 할 때 사용합니다. 파일이 등록되어 있다면, 새로 업로드하는 파일로 교체됩니다.
지원하는 인증서 파일(.pem) 형식은 '[문제 해결 가이드 > 인증서 파일 포맷 변환](http://alpha-docs.toast.com/ko/Management/Certificate%20Manager/ko/troubleshooting-guide/#_1)' 참고 부탁드립니다.

#### 요청

```
POST /certmanager/v1.0/appkeys/{appKey}/certificates/{certificateName}/files
```

[Request Header]

```
Content-Type:multipart/form-data
```

[Request Body]

```
file: {파일}
```

#### 응답

[Response Header]

```
Content-Type:application/json
```

[Response Body]

``` json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "body": null
}
```

### 인증서 파일 다운로드

Certificate Manager에 등록한 인증서 파일을 다운로드 할 때 사용합니다.

#### 요청

```
GET /certmanager/v1.0/appkeys/{appKey}/certificates/{certificateName}/files
```

#### 응답

[Response Header]

```
Content-Disposition:attachment; filename="{파일명}"
Content-Type:application/octet-stream
```

[Response Body]

```
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
...
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
```

### 응답 코드

| isSuccessful | resultCode | resultMessage | 설명 |
| ------------ | ---------- | ------------- | --- |
| true | 0 | SUCCESS | 성공 |
| false | 52000 | Certificate name does not exist. | 요청한 인증서 이름이 존재하지 않습니다. |
| false | 52001 | Certificate file does not exist. | 요청한 인증서 파일이 존재하지 않습니다. |
| false | 52002 | There are more than one certificate file. | 요청한 인증서에 등록된 파일이 두 개 이상입니다. |
| false | 52003 | The certificate file is not a pem file. | 요청한 인증서 파일이 pem 파일이 아닙니다. |
| false | 52004 | The certificate name in the file is different from the requested certificate name. | 요청한 인증서 이름과 인증서 파일에 등록된 이름이 다릅니다. |
| false | 52005 | Certificate file has expired | 요청한 인증서 파일이 만료된 파일입니다. |