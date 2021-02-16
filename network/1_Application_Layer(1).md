# Application Layer(1)

## Application Layer란

네트워크에서 어플리케이션 레이어란 우리가 가장 많이 접하게 되는 부분이다. 대표적으로는 많이들 사용하는 인스타그램, 유튜브, 페이스북, 트위치 등등이 있다. 브라우저(HTTP통신)도 이 부분에 속하며 이러한 계층화를 통해서 하위의 계층을 자세히 몰라도 어플리케이션을 사용하는데에 큰 문제가 없게 된다.

대표적인 구조로 Server Client 구조이다. 우리가 보통 사용하는 모든 어플리케이션이 C-S구조를 가지고 있다.

Application Layer는 결론적으로 프로세스 간의 커뮤니테이션이다.

OS에서 소켓이라는 인터페이스를 제공해주어서 이 통로를 이용해서 네트워크를 통해서 프로세스간의 통신을 하게된다. 3,4계층에서 언급하게 되겠지만 이 소켓을 IP와 Port를 이용해서 구분하게 된다. 브라우저를 통해 통신할때에는 도메인 주소를 이용하여 통신을 하게 되는데 이때 DNS라는 프로토콜을 추가적으로 사용하여 통신한다.



### HTTP

Hypertext Transfer Protocol 의 약자이며 여기서 Hyper Text는 Text인데 중간 중간에 링크들이 있는 Text이며 주로 HTML을 의미한다. Request와 Response로 이루어져있다. 보통 브라우저를 통해서 통신을 할 때 이 프로토콜을 사용하며 TCP프로토콜 기반으로 통작한다.(최근 HTTP 3.0이 개발되며 UDP를 사용하기도 한다.)

TCP를 사용하기 때문에 UDP프로토콜을 사용할때에 비해 당연히 비용이 추가적으로 발생한다. UDP에 비해 추가적으로 필요한 작업들이 있고 제공하는 기능들이 있기 때문에 UDP에 비해 비용이 더 든다.

(가끔 블로그들에 HTTP와 소켓의 차이점을 작성할때 Websocket인지 4 Layer Socket인지 명확히 안적은 글들이 보이는데 HTTP도 OS에서 제공하는 네트워크 인터페이스인 TCP Socket 위에서 작동한다. HTTP도 여러개로 나뉜 패킷이 여러개의 통로를 통해서 보내기 때문에 4 Way Handshake를 해서 연결을 끊기 전까지는 연결 유지가 필요하다.)

또한 HTTP는 Stateless 프로토콜이기 때문에 한번 통신하고 나면 그 상태를 기억하지 않는다.

#### Non-persistent HTTP(HTTP 1.0) VS Persistent HTTP(HTTP 1.1)

두가지에 가장 큰 차이점은 한번 연결한 TCP 연결을 재사용할 것인지 아니면 매 요청마다 TCP 커넥션을 다시 맺을것 인가의 차이이다. 이전 연결을 재사용 하지 않는다고 가정해보자. TCP는 3 Way Handshake를 통해서(자세한 내용은 4계층에서 설명한다.) 연결을 맺는다. 따라서 만약 처음 요청한 HTML 파일에 사진파일이 10개가 포함되어서 사진 파일을 10번 요청을 다시 해줘야 한다고 하면 10번의 TCP 연결을 다시 해야한는 문제점이 발생한다. 따라서 현재 웹브라우저는 Persistent HTTP로 동작하며 HTTP1.0과 HTTP 1.1의 가장 큰 차이점이다.

#### HTTP Request Message

HTTP Request Message와 Response Message의 모습은 아래와 같다.

![Requests and responses share a common structure in HTTP](https://mdn.mozillademos.org/files/13827/HTTPMsgStructure2.png)

HTTP Method 정보와 버전, 보내는 대상, 유저의 정보등을 포함하여 보낸다. 그러면 서버가 해당 정보를 받아서 그에 대한 리퀘스트로 성공여부와 실패했다면 어떤 문제로 실패했는지 등의 정보를 헤더에 보내고 Body에 정보들을 포함하여 보낸다.

#### HTTP 상태 유지(Cookies)

Http는 기본적으로 Stateless인 프로토콜이기 떄문에 특정 유저에 대한 상태를 유지시킬 수 없다. 이것을 보완하기 위해 Cookie를 이용해서 상태를 서버에  남긴다. 원리는 간단하다. 처음 접속하는 유저에 대해서 랜덤한 값을 부여하고 그 값을 서버 내부에 저장한다. 나중에 유저가 헤더에 쿠키를 포함시켜서 전송하면 그때에 쿠키 내부에 있는 값을 통해서 서버에 저장된 유저의 정보를 찾아서 필요한 용도대로 사용한다. 로그인도 Session과 Cookie 방식을 사용하여 구현하는 방법도 있다.