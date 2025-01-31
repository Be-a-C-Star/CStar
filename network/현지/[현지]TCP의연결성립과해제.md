# TCP의 연결성립: 3-웨이 핸드셰이크

TCP 세션을 협상하고 시작하기 위해 TCP가 전송하는 세 가지 메시지는 각각 SYN(SYNchronize), SYN-ACK(SYNchronize-ACKnowledgement), ACK(ACKnowledge) 라는 이름을 가진다.
세 가지 메시지 메커니즘은 서로 정보를 주고 받기를 윈하는 두 컴퓨터가 HTTP 브라우저 요청과 같은 데이터를 전송하기 전에 연결 매개변수를 협상할 수 있도록 설계되었다.

1. 호스트(일반적으로 브라우저)는 TCP SYNchronize 패킷을 서버로 보낸다.
2. 서버는 SYN을 수신하고 SYNchronize-ACKnowledgement를 다시 보낸다.
3. 호스트는 서버의 SYN-ACK을 수신하고 ACKnowledge를 다시 보낸다.
4. 서버는 ACK를 수신하고 TCP 소켓 연결이 설정된다.

## 클라이언트와 서버의 상태

TCP연결을 하면서 클라이언트는 closed, syn-sent, established가 되며 server는 closed, listen, syn_received, established 상태가 된다.

## listen

서버는 클라이언트의 연락을 기다리는 상태, 이를 기반으로 서버 메서드의 이름이 결정된다.

# TCP의 연결해제: 4-웨이 핸드셰이크와 TIME_WAIT

1. 먼저 클라이언트가 연결을 닫으려고 할 때 FIN으로 설정된 세그먼트를 보낸다. 클라이언트는 FIN_WAIT_1 상태로 들어가고 서버의 응답을 기다린다.
2. 서버는 클라이언트로 ACK라는 승인 세그먼트를 보내고 CLOSE_WAIT 상태에 들어간다. 클라이언트가 세그먼트를 받으면 FIN_WAIT_2 상태에 들어간다.
3. 서버는 LAST_ACK 상태가 되며 일정 시간 이후에 클라이언트에 FIN이라는
   세그먼트를 보낸다.
4. 클라이언트는 TIME_WAIT 상태가 되고 다시 서버로 ACK를 보내서 서버는 CLOSED 상태가 되며 이후 클라이언트는 어느 정도의 시간(TIME_WAIT으로 설정된 시간)을 대기한 후 연결이 닫힘.

## TIME_WAIT

TIME_WAIT는 지연 패킷 등이 발생했을 때 데이터 무결성을 해결하기 위해 패킷을 기다리는 시간을 말하며, 연결이 올바르게 닫힌 상태로 만들기 위해 존재하기도 한다.
