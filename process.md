Virtual Waiting Room Server Settings.
==============================
아래의 문서는 Virtual Waitng Room xQueue를 세팅하기위한 과정이다.
> - 설치 환경   
   RockyLinux 9을 기준으로 작성하였음.

1.서버 세팅
-----------
1. rpm파일을 해제하기 위해 gcc패키지가 설치 되어있어야 함 (glibc 6 이상).
2. DB관리를 위한 sqlite3이 설치 되어있어야 함.
3. xQueue rpm 패키지를 /usr/local/ 아래에 푼다.   
```bash
:$ rpm -ivh coat9vwr-x.x.x.rpm  --prefix /usr/local
```
4. /usr/local/coat9vwr/bin 디렉토리로 이동한다.
5. coat9_ts(Transaction Server)를 실행한다. 이 떄 root로 사용자 변경 또는 sudo를 이용하여 실행시켜야 한다.
```bash
:$ bin/coat9_ts start
```   
6. 서버가 실행되고 나면 초기화 도구를 실행한다.
```bash
:$ bin/tool_init
```  

```bash
- 초기화 도구 설정 예시 -

set the service initial `domain`, `sysadmin` id or password.
- caution: use only when servicing in `master` mode.

#도메인 이름 설정.
input domain [length:5~30](ex coat9.com)? coat9.com

#관리자 id 설정.
input system admin id [length:5~20]? sysadmin

#관리자 pw 설정.
input system admin password [length:5~15]? sysadmin
```

* ### admin페이지 접속은 반드시 도메인으로 접속해야 정상적으로 로그인이 가능하다.

2.License 설정
--------------   
- admin페이지에 정상적으로 로그인이 되는 것을 확인하면 License를 생성한다.
1. License 생성을 위한 JSON 파일을 작성한다.
```json
{
	"domain":"coat9.com",           //도메인 이름.
	"class":"vwr.room",             //class는 "vwr.com"으로 설정.
	"serialno":20231211003,         //관리를 위한 시리얼 넘버.
	"expire":"2024-12-31 23:59:59", //해당 라이센스의 만료기간.
	"limit":10                      //생성할 room의 갯수.
}
```
2. License를 생성하기 위해 xQueue가 설치된 서버의 /usr/local/coat9vwr/bin 디렉토리로 이동한다.
3. bin 디렉토리 안의 tool_makekey를 실행하여 값을 입력한다. 
```bash
:$ bin/tool_makekey

# 초기 License Key 생성을 위해서는 c를 입력한다(create).
input command[c:create, i:checkinfo, a:append, q:quit]? c

# License 생성키 입력.
input cryptkey? xxxxx crypt key xxxxx

# License 설정 JSON 입력. string 입력 후 Enter키를 두 번 누른다.
input string(multi line)? {
        "domain":"coat9.com",
        "class":"vwr.room",
        "serialno":20231211002,
        "expire":"2024-12-31 23:59:59",
        "limit":10
}

--------------------------------
xxxxx Generated License Key xxxxx

```
4. 생성된 License Key를 복사하여 admin 페이지의 settings - license 페이지로 이동하여 추가한다.