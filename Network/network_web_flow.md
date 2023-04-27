# 웹 통신의 큰 흐름: https://www.naver.com을 접속할 때 일어나는 일
<aside>
💡 브라우저에 www.naver.com을 입력했을 때, 어떤 일이 일어나는지 설명해주세요.
+OSI 7 계층과 연관지어 설명해주세요.

</aside>

### IP 주소

- IP 주소란 많은 컴퓨터들이 인터넷 상에서 서로를 인식하기 위해 지정받은 식별용 번호라고 생각하면 된다.
- 현재는 IPv4 버전(32비트)로 구성되어 있으며, 한번씩은 들어봤을 법한 127.0.0.1 같은 주소를 말한다.
- 시간이 갈수록 IPv4 주소의 부족으로 IPv6가 생겼는데, 128비트로 구성되어 있기 때문에 IP 주소가 부족하지 않다는 특징이 있다.
- ex) 192.111.123.234

### 도메인 네임(Domain Name)

- IP 주소는 12자리의 숫자로 되어 있기 때문에 사람이 외우기 힘들다는 단점이 있다.
- 그렇기 때문에 12자리의 IP 주소를 문자로 표현한 주소를 **도메인 네임**이라고 한다.
- 다시 말해서, 도메인 네임은 'naver.com'처럼 몇 개의 의미있는 문자들과 .의 조합으로 구성된다.
- 도메인 네임은 사람의 편의성을 위해 만든 주소이므로 실제로는 컴퓨터가 이해할 수 있는 IP 주소로 변환하는 작업이 필요하다.
- 이때, 사용할 수 있도록 미리 도메인 네임과 함께 해당하는 IP 주소값을 한 쌍으로 저장하고 있는 데이터베이스를 **DNS(Domain Name System)** 이라고 부른다.
- **도메인 네임으로 입력하면 DNS를 이용해 컴퓨터는 IP 주소를 받아 찾아갈 수 있다.**

### DNS (Domain Name System Server)

- DNS는 도메인 이름과 IP 주소를 저장하고 있는 분산 데이터베이스로, **웹사이트를 위한 주소록**
- 분산형DB구조

![스크린샷 2023-04-25 오후 7.53.47.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/24756bd2-22c8-4e70-b921-42d7ced48702/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-04-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.53.47.png)

## https://www.naver.com을 접속할 때 일어나는 일

<aside>
💡

1. **브라우저 주소창**에 https://www.naver.com을 입력한다.
2. 브라우저가 naver.com의 **IP 주소**를 찾기 위해 **캐시**에서 **DNS 기록**을 확인한다.
3. 만약 요청한 URL(naver.com)이 캐시에 없다면, **ISP의 DNS 서버**가 **DNS 쿼리로** naver.com을 **호스팅**하는 서버의 IP 주소를 찾는다.
4. 브라우저가 해당 서버와 **TCP 연결**을 시작한다.
5. 브라우저가 웹서버에 **HTTP 요청**을 보낸다.
6. 서버가 요청을 처리하고 **응답**을 보낸다.
7. 브라우저가 **HTML** 컨텐츠를 보여준다.
</aside>

### 1. 브라우저에 주소(www.naver.com) 입력

- 주로 URL 형태로 주소 입력
- URL과 URI 차이

### 2. naver.com의 IP 주소 찾기 위해 캐시에서 DNS 기록 확인

- 4가지 캐시 확인
1. 브라우저 캐시
    1. 이전에 방문한 웹사이트의 DNS 기록 일정 기간 동안 저장
2. OS 캐시
    1. DNS cache 확인
3. 라우터 캐시 확인
    1. 공유기
4. ISP 캐시
    1. Internet Service Provider, 인터넷 서비스 제공자
    - KT, U+, SK 등등

### 3. 요청한 URL이 캐시에 없다면, ISP의 DNS 서버가 DNS 쿼리로 naver.com을 호스팅하는 서버의 IP 주소를 찾는다.

![스크린샷 2023-04-25 오후 7.53.47.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/24756bd2-22c8-4e70-b921-42d7ced48702/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-04-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.53.47.png)

<aside>
💡 DNS Lookup : DNS 서버에서 인터넷 도메인을 이용해 ip를 알아내는 과정

</aside>

- DNS 리커서(DNS Recursor) : ISP의 DNS 서버, 인터넷의 다른 DNS 서버에 답변을 요청하여 의도된 도메인 이름의 IP주소 찾는 일 담당
1. DNS 리커서가 루트 네임 서버에 연결한다.
2. 루트 네임 서버는 리커서를 .com DNS로 리디렉션
3. .com 서버는 [naver.com](http://naver.com) 서버로 리디렉션
4. [naver.com](http://naver.com) 서버는 일치하는 ip 주소 찾아 DNS 리커서로 반환, 브라우저로 다시 전송

### 4. 자신의 PC와 해당 서버가 TCP 연결

- IP주소 가지고 자신의 PC와 naver 서버와 tcp연결
- TCP/IP 3-way handshake 연결

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8068a1de-c892-4472-9631-6a8cb16728f2/Untitled.jpeg)

### 5. TCP 연결 성공하면 http request 전송

### 6.서버가 요청 처리하고 response 보냄

### 7. 브라우저가 HTML 컨텐츠를 보여준다.


## 예상 질문
1. 브라우저에 www.naver.com을 입력했을 때, 어떤 일이 일어나는지 설명해주세요.
2. URL과 URI의 차이점을 설명해주세요.
3. DNS 서버에 대해 설명해주세요.
4. DNS를 통한 도메인 이름 구조 기반의 검색과정을 설명해주세요.
5. 도메인과 URL의 차이를 설명해주세요.
6. 공인IP와 사설IP차이를 설명해주세요.




reference

- [웹 브라우저에 URL 입력하면 일어나는 일 - 인프라 위주](https://youtu.be/GAyZ_QgYYYo)

- [2023-CS-Study/network_dns_and_network_flow.md at main · Fancy96/2023-CS-Study](https://github.com/Fancy96/2023-CS-Study/blob/main/Network/network_dns_and_network_flow.md)

- [브라우저에 url을 입력하면 어떤일이 벌어질까?](https://velog.io/@khy226/브라우저에-url을-입력하면-어떤일이-벌어질까)


