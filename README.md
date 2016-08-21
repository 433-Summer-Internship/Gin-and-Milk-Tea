# Gin-and-Milk-Tea

![Alt text](/logo.png?raw=true "logo")

Common Module : https://github.com/JinHyuk-Lee/ChatProtocol  
Client : https://github.com/kimsin3003/ChatClient  
Frontend server : https://github.com/kimsin3003/ChatServer/tree/Release  
Backend Server : https://github.com/lunker/lunkerRedis

---
- [Client](#client)
- [FrontEndServer](#fe)
- [BackendServer](#be)

---

#<div id ="client"> Client </div>

## 실행 환경
- .NET 4.5.2 이상

## Client : 채팅 클라이언트

### 실행
<pre>1. `Client.exe.config`파일에서 __App.config__ 의 <appSettings>에 `key =  "ip,port"`를 추가한다. </pre>
(ip와 port를 ,로 구별, 띄어쓰기 없음)

``` html 
  <appSettings>
    <add key="10.100.58.9,11000"/>
    <add key="10.100.58.9,12000"/>
    <add key="10.100.58.9,13000"/>
    <add key="10.100.58.9,14000"/>
  </appSettings>
```
<pre>2. client.exe 를 실행</pre>

### 기능
- Chatting
- Health Check
- Connect Passing

---

## Dummy : 더미 클라이언트

### 실행
<pre>1. __App.config__ 파일에서 <appSettings>에 `key =  "ip,port"`를 추가한다.</pre>
(ip와 port를 ,로 구별, 띄어쓰기 없음)

``` html 
  <appSettings>
    <add key="10.100.58.9,11000"/>
    <add key="10.100.58.9,12000"/>
    <add key="10.100.58.9,13000"/>
    <add key="10.100.58.9,14000"/>
  </appSettings>
```
<pre>2. Release 파일이 위피한 폴더에서 콘솔창을 열어 `dummy id`를 입력한다. </pre>
(단, id는 dummy로 시작하며 뒤에 숫자가 붙을 수 있다. 예시, dummy1, dummy2, ...)

### 기능
- 정해진 프로토콜에 따라 채팅 룸 입장
- 무작위 개수의 현재 시간 정보를 출력
- 5~10초 사이 기다린 후 로그아웃

### 로직
 <pre>            
      (성공)                                                        (RoomList에서 랜덤 Room No를 뽑아 입장)
Login ----------------------------------> Lobby ----> Room List 조회 ---------------------------------> Room ------> ...
      \                                /                            \                               / 
       ----> Signup -----> Login ---->                               ---------> Create Room ------->
      (실패)                                                         (RoomList가 비어잇으면)      
      
      
... ------> 무작위 개수의 현재 시간 정보를 출력 -----> 5~10초 사이 기다린 후 -----> LogOut -------> Exit
  </pre>     
  
---
## Monitoring : 현재 채팅 시스템 상황 보여주는 모니터링 클라이언트
### 실행
1. Release 파일이 위피한 폴더에서 콘솔창을 열어 `admin ip port`를 입력한다.
3. Admin.exe 를 실행
2. 아이디 : admin 비밀번호 : root

### 기능
- 채팅 룸 개수
-  FE 서버별 사용자 수(더미 클라이언트 포함)
-  채팅메시지 수 작성 TOP10을 실시간으로 보여줌(더미 제외) (6초마다 갱신)


---

# <div id="fe"> FrontEnd Server</div>

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

---
# <div id="be"> Backend Server</div>


# Chatting Project#1 Backend Server Application 
Team [혜진-태우-동규]  
4:33 intern Chatting Project-BackEnd Redis Server

## 개발 환경 
- Visual Studio 2015
- MySQL 5.7.13
- Redis 3.2.3
- log4net 1.2.15

## 실행 환경
- NET 4.5.2 이상  

## Redis 설계 문서 
- 'master'branch /design/  

## Dependencies
- log4net : logging
- StackExchange.Redis : redis client(https://github.com/StackExchange/StackExchange.Redis)   
- mysql connector (https://dev.mysql.com/downloads/connector/net/6.9.html)  

## 기능
- Frontend Server, Monitoring Client의 요청을 처리하기 위하여 각각의 Listener를 구현.
- Backend Server에 접속한 Frontend Server들의 정보들을 동적으로 관리합니다.
- Frontend Server가 이탈시, 해당 Frontend Server와 관련된 cache는 삭제됩니다.(채팅내역제외)
- User의 접속 여부, 채팅 횟수 등을 관리합니다.
- 채팅룸, 채팅 횟수, 채팅방의 입장인원 등의 정보 cache  
- Monitoring 기능을 제공합니다. 1) 전체 채팅방 수 조회 2) FE별 사용자수 조회 3) 채팅 랭킹 조회   
- Frontend Server와는 HealthCheck를 수행하여 disconnect를 체크하고 있습니다.
- Monitoring Client는 5초에 한번씩 조회 요청을 보내기에, 별도의 Heatlcheck를 구현하지 않았습니다.

## 실행방법
0) Release Binary 다운로드  
1) VM 환경 구성  
2) 환경설정  
3) 실행  


### 실행방법 - Release Binary 다운로드
1) 'release' branch에서 'release.7z'을 다운로드 

### 실행방법 - VM 환경 구성 

1) MySQL - 5.7.13 GA
- 가상 호스트 전용 어답터를 사용.  
- Release/sql/User.sql의 Query 실행하여 관련 Table Setup   

  
2) Redis
- 가상 호스트 전용 어답터를 사용.
- 별도의 설정없이 기본 설치 후 사용.

### 실행방법 - 환경설정 

1) MySQL config 설정
- path : config/MySQLConfig.xml
- database, connectiontimeout을 제외한 나머지 정보들을 알맞게 수정.


```
<MySQLConfig>
  <server>192.168.56.190</server>
  <uid>lunker</uid>
  <pwd>dongqlee</pwd>
  <database>chatting</database>
  <ConnectionTimeout>600</ConnectionTimeout>
</MySQLConfig>
```


2) Redis config 설정
- path : release/config/RedisConfig.xml    
- ip, port 수정
- ip : 가상 호스트 전용 ip (192.168.56.xxx) 
- port : port

```
<RedisConfig>
  <ip>192.168.56.102</ip>
  <port>6379</port>
</RedisConfig>
```


3) Application Config 설정
- path : release/config/AppConfig.xml  
- client, frontend Server의 listen port 지정


```
<AppConfig>
  <clientListenPort>20852</clientListenPort>
  <frontendListenPort>25389</frontendListenPort>
</AppConfig>
```

4) log4net Config 설정 
- path : release/config/LogConfig.xml
- 아래 항목 중 '<file>'을 수정하여 원하는 로그 파일 저장 경로 설정. 

```
<log4net>
  <appender name="exlog" type="log4net.Appender.RollingFileAppender">
    <file value="c:\log\" />
    <datePattern value="yyyy-MM-dd'_exlog.log'"/>
    <preserveLogFileNameExtension value="true" />
    <staticLogFileName value="false" />
    <appendToFile value="true" />
    <rollingStyle value="Composite" />
    <countDirection value="1"/>
    <maxSizeRollBackups value="100" />
     <maximumFileSize value="100MB" />
    <layout type="log4net.Layout.PatternLayout">
      <conversionPattern value = "[%-5level][%-5thread][%-25method] : %date{HH:mm:ss,fff}   %message%newline"/>
    </layout>
  </appender>
  <logger name = "Logger">
    <level value="Info" />
    <level value="Debug" />
    <appender-ref ref="exlog"/>
  </logger>
</log4net>
```

### 실행 
- MySQL VM 실행 및 mysqld 실행 
- Redis VM 실행 및 redis.service 실행
- release/LunkerRedis.exe 실행 

## 사용법 및 주의사항
- 실행파일을 실행 후, Console에서는 진행 사항이 출력되지 않습니다.
- 해당 진행 사항내역은 로그파일을 통해서 확인할 수 있습니다.
- 진행 내역을 확인하기 위하여, 어플리케이션이 종료되면 Redis의 cache내역을 모두 삭제합니다. 
- 단, FE의 접속이 끊길경우 해당 FE관련 정보들만 삭제되며 Backend Server는 정상 진행됩니다.
- 종료는 Console 창의 안내에 따라 종료해주십시오.


