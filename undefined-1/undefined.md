# 우리가 웹을 보는 과정

우리가 웹을 보는 과정을 아시나요?

도메인을 입력하는 지점부터, 실제 웹을 보는 과정까지 어떤 순서로 진행될까요?



크게 4가 과정을 진행됩니다.

(시작) 도메인 접속 🏁

1. IP 주소 접속
2. 신뢰 확인
3. 요청 받기
4. 렌더링

(끝)  사용자가 웹 페이지 확인 🥳



### 1. IP 주소 접속

인터넷 상의 주소는 IP 주소입니다. 컴퓨터가 읽을 수 있는 숫자로 이뤄진 주소죠

허나 우리가 사이트에 접속할 때, 숫자로 이뤄진 이 복잡한 주소를 외운다면 너무 불편하겠죠?



www.github.com와 같이 우리가 읽을 수 있는 주소 (= 도메인)가 등장했습니다.

여전히 사이트의 주소는 IP 주소이기에 도메인 주소를 IP로 변환해주는 중간 다리가 필요합니다.



이것이 DNS (Domain Name Service)입니다.

DNS는 도메인 이름과 IP 주소를 서로 변환하는 역할을 합니다.&#x20;



참고로 이 DNS가 올라간 서버는 네임 서버라고 부릅니다.&#x20;

흔히 배포 후 도메인 커스텀 등의 작업을 할 때 보게 되는 바로 그 네임 서버입니다.





### 2. 신뢰 확보

웹에 접속하기 위해서는 서버와 클라이언트 모두 데이터를 전송할 준비가 되어야 합니다.

이 과정에서 TCP의 3-Way Handshake를 진행하게 됩니다.

<details>

<summary>"TCP/IP 프로토콜"에 대해서</summary>

TCP와 IP는 서로 정보를 주고 받을 때 사용되는 대표적인 프로토콜입니다.

각각 OSI 7 계층의 통신 계층 (TCP)과 네트워크 계층 (IP)입니다.

과정 1에서 접속하고자 하는 IP를 확인했다면, 이제 데이터 송수신을 위한 TCP를 확인할 차례입니다.

계층에 대한 자세한 설명은 [OSI 7 계층](https://namu.wiki/w/OSI%20%EB%AA%A8%ED%98%95?from=OSI)을 확인하실 수 있습니다.

</details>



<figure><img src="../.gitbook/assets/스크린샷 2024-05-13 오후 5.09.59.png" alt=""><figcaption><p>출처: <a href="https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake">https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake</a></p></figcaption></figure>



1. SYN 패킷을 보낸다. 이때 A클라이언트는 SYN 을 보내고 SYN/ACK 응답을 기다리는SYN\_SENT 상태가 된다.\

2. B서버는 SYN요청을 받고 A클라이언트에게 요청을 수락한다는 ACK 와 SYN가 설정된 패킷을 발송하고 A가 다시 ACK으로 응답하기를 기다린다. 이때 B서버는 SYN\_RECEIVED 상태가 된다.\

3. A클라이언트는 B서버에게 ACK을 보내고 이후로부터는 연결이 이루어지고 데이터가 오가게 되는것이다. 이때의 B서버 상태가 ESTABLISHED 이다.



위와 같은 방식으로 서버와 클라이언트 간의 신뢰성 있는 연결을 맺습니다.



### 3. HTTP req / res

이제 연결이 잘 되었으니, 클라이언트가 서버에게 필요한 리소스 요청을 날립니다. (request)

요청에 문제가 없으면, 정상적으로 서버는 클라이언트에게 응답을 보내줍니다. (response)



github.com에 접속하는 과정을 크롬 개발자 도구를 확인해보면, 이런 식으로 됩니다.

![](<../.gitbook/assets/스크린샷 2024-05-13 오후 5.29.34.png>)

<figure><img src="../.gitbook/assets/스크린샷 2024-05-13 오후 5.34.48.png" alt=""><figcaption></figcaption></figure>

GET 요청이 잘 수행되어, html로 이뤄진 응답을 잘 받았습니다.

html이 외에도 css, js 파일, 이미지 파일 또한 응답으로 받습니다.



### 4. 렌더링

이제 받아온 데이터를 바탕으로 사용에게 렌더링을 시켜줄 차례입니다.



1. html 파싱
2. css 파싱 cssom 트리 구축
3. js 실행
4. DOM과 CSSOM 합쳐서 렌더트리 구축 (공간 차지 없거나 안보는 건 제외)
5. layout / reflow 뷰포트 기반으로 계산
6. 계산을 한 위치와 트리를 통해 화면에 paint

