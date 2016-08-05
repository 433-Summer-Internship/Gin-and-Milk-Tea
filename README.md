# Taewoo-Dongue-Hyejin

Common Module : https://github.com/lunker/ChatProtocol  
Client : https://github.com/Simhyejin/ChatClient  
Frontend server : https://github.com/kimsin3003/ChatServer  
Backend Server : https://github.com/lunker/lunkerRedis


#FrontEnd Server
##사용법
Release.zip파일을 압축을 풀고 콘솔창을 열어 ChatServer를 치면 사용법이 나옵니다.

##사용 환경
.Net 4.5.2 이상.
따로 사용한 라이브러리는 없습니다.

##기능
Client로부터 Request를 받아 BackEnd에 Request를 그대로 보내고, Response를 받으면 Response를 Client에도 보내줍니다. 
이때 SUCCESS를 받으면 그에 필요한 정보(Session, Room 등)를 저장합니다.

채팅의 경우에는 BackEnd로 내용은 보내지 않고 서버가 같은 Room에 있는 User들에게 BroadCasting을 해주며, BackEnd에는 채팅이 왔다는 정보만 줌으로서 채팅 카운드를 올릴수 있게 해줍니다.

Room에 있지 않은 경우 Room에 대한 요청을 하면 BackEnd로 보내지 않고 Fail을 바로 보내줍니다.
Chat상태가 아닌경우 Chat에 대한 요청을 하면 BackEnd로 보내지 않고 Fail을 바로 보내줍니다.

BackEnd가 접속이 끊기면 Back에 재접속함과 동시에 모든 방과 세션을 초기화 합니다.

Client, BackEnd 모두로부터 FIN이 오는지 항상 검사하며, HealthCheck도 하고 있습니다.
HealthCheck은 30초마다 보내며, 답이 오지 않은 경우 5초씩 기다려서 2번 더 보내보고 답이 오지 않으면 접속을 끊습니다. 



