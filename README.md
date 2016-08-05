# Taewoo-Dongue-Hyejin

Common Module : https://github.com/lunker/ChatProtocol  
Client : https://github.com/Simhyejin/ChatClient  
Frontend server : https://github.com/kimsin3003/ChatServer/tree/Release  
Backend Server : https://github.com/lunker/lunkerRedis

---
- [Client](#client)
- [FrontEndServer](#fe)

---

#<div id ="client"> Client </div>

## 실행 환경
- .NET 4.5.2 이상

## Client : 채팅 클라이언트

### 실행
1 __App.config__ 파일에서 <appSettings>에 `key =  "ip,port"`를 추가한다. (ip와 port를 ,로 구별, 띄어쓰기 없음)
``` html 
  <appSettings>
    <add key="10.100.58.9,11000"/>
    <add key="10.100.58.9,12000"/>
    <add key="10.100.58.9,13000"/>
    <add key="10.100.58.9,14000"/>
  </appSettings>
```
2  client.exe 를 실행

### 기능
- Chatting
- Health Check
- Connect Passing

## Dummy : 더미 클라이언트
- 정해진 프로토콜에 따라 채팅 룸까지 들어와서 무작위 개수의 현재 시간 정보를 출력하고 5~10초 사이 기다린 후 로그아웃

### 실행
1 __App.config__ 파일에서 <appSettings>에 `key =  "ip,port"`를 추가한다. (ip와 port를 ,로 구별, 띄어쓰기 없음)
``` html 
  <appSettings>
    <add key="10.100.58.9,11000"/>
    <add key="10.100.58.9,12000"/>
    <add key="10.100.58.9,13000"/>
    <add key="10.100.58.9,14000"/>
  </appSettings>
```
2 Release 파일이 위피한 폴더에서 콘솔창을 열어 `dummy id`를 입력한다.
(단, id는 dummy로 시작하며 뒤에 숫자가 붙을 수 있다. 예시, dummy1, dummy2, ...)

### 로직
 <pre>            
      (성공)                                                        (RoomList에서 랜덤 Room No를 뽑아 입장)
Login ----------------------------------> Loby ----> Room List 조회 ---------------------------------> Room ------> ...
      \                                /                            \                               / 
       ----> Signup -----> Login ---->                               ---------> Create Room ------->
      (실패)                                                         (RoomList가 비어잇으면)      
      
      
... ------> 무작위 개수의 현재 시간 정보를 출력 -----> 5~10초 사이 기다린 후 -----> LogOut -------> Exit
  </pre>     
  
## Monitoring : 현재 채팅 시스템 상황 보여주는 모니터링 클라이언트
- 채팅 룸 개수, FE 서버별 사용자 수(더미 클라이언트 포함), 채팅메시지 수 작성 TOP10을 실시간으로 보여줌(더미 제외)

### 실행
1. Release 파일이 위피한 폴더에서 콘솔창을 열어 `admin ip port`를 입력한다.
2. 아이디 : admin 비밀번호 : root

### 기능

---

# <div id="fe"> FrontEnd Server</div>
# ChatServer

##사용법
1. 반드시 release 브랜치의 파일을 다운받아주세요.
2. 폴더 안에 Release.zip파일이 있습니다.
3. Release.zip파일을 압축을 풀고 해당 폴더에서 콘솔창을 열어 
4. ChatServer listeningPort backEndIp backEndPort maxClientNum를 치면 시작 명령어 안내가 나옵니다.(maxClientNum: 한 서버에 접속가능한 최대 클라이언트 수)
5. 안전한 종료(FIN전송 포함)을 위해서는 ESCAPE 키를 눌러주세요..

##사용 환경
.Net FrameWork 4.5.2 이상.  
Visual Studio 2015.  
따로 사용한 라이브러리는 없습니다.

##기능
Client로부터 Request를 받아 BackEnd에 Request를 그대로 보내고, Response를 받으면 Response를 Client에도 보내줍니다.   
이때 SUCCESS를 받으면 그에 필요한 정보(Session, Room 등)를 저장합니다.  
  
채팅의 경우에는 BackEnd로 내용은 보내지 않고 서버가 같은 Room에 있는 User들에게 BroadCasting을 해주며, BackEnd에는 채팅이 왔다는   정보만 줌으로서 채팅 카운드를 올릴수 있게 해줍니다.  
  
Room에 있지 않은 경우 Room에 대한 요청을 하면 BackEnd로 보내지 않고 Fail을 바로 보내줍니다.  
Chat상태가 아닌경우 Chat에 대한 요청을 하면 BackEnd로 보내지 않고 Fail을 바로 보내줍니다.  
  
BackEnd가 접속이 끊기면 Back에 재접속함과 동시에 모든 방과 세션을 초기화 합니다.  
    
Client, BackEnd 모두로부터 FIN이 오는지 항상 검사하며, HealthCheck도 하고 있습니다.      
HealthCheck은 30초마다 보내며, 답이 오지 않은 경우 5초씩 기다려서 2번 더 보내보고 답이 오지 않으면 접속을 끊습니다.   

두개 이상의 서버를 띄우고 서로 다른 서버의 클라이언트가 같은방에 접속하려하면, 룸이 속해있는 서버의 IP와 Port를 BackEnd가 넘겨주며, 이를 Client에게 넘겨주면 Client가 해당 서버로 재접속합니다.


