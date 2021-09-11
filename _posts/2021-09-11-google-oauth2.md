---
title: Google OAuth2 인증 프로세스 이해하기(회원 인증)
categories:
- Project
---

SNS 인증 방식으로 OAuth가 개방형 표준으로 널리 사용되고 있습니다. 그 중 가장 대중화된 SNS인 구글 OAuth의 인증 프로세스에 대해 구현한 내용입니다.

아래는 그림으로 정리한 인증 프로세스 흐름입니다.
<br>
<br>
<br>
<br>
![oauth_인증](https://user-images.githubusercontent.com/72685070/132946844-4322f5a6-b6e7-4099-9a8f-f13fee132293.png)
<br>
<br>

가장 먼저, 아래 경로로 접속하여 OAuth 인증 사용을 위한 ClientID, ClientSecret를 발급받고, 허용가능한 RedirectURL을 입력합니다.

[https://console.cloud.google.com/apis/dashboard?hl=ko](http://)
<br>

1. 좌측 메뉴바에서 [사용자 인증정보]를 선택한 후, 상단에 [사용자 인증정보 만들기]를 선택하고 [OAuth 클라이언트 ID]를 선택합니다.  
![구글OAuth등록1](https://user-images.githubusercontent.com/72685070/132947037-3a9a4165-b392-4f44-8539-6990c3242752.png)



2. 제작하려는 서비스 타입을 선택합니다. 그리고 하단에 [만들기]가 활성화되면 Client ID, ClientSecret이 발급됩니다.  
![구글OAuth등록2](https://user-images.githubusercontent.com/72685070/132947045-8fbbf07f-4ce6-41e2-a1d9-94b6e4104957.JPG)
