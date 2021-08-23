## 블록체인 네트워크의 이해 : 프라이빗 네트워크 실습으로 이더리움 네트워크 시작하기

#### 이더리움(ethereum)이란...?
이더리움은 블록체인 기술을 기반으로 스마트 계약 기능을 구현하기 위한 분산 컴퓨팅 플랫폼이자 운영체제

#### 이더리움 블록체인 네트워크의 분류
- 프라이빗 네트워크 : 로컬 PC 또는 친구들끼리 이더리움을 구동하는 것을 의미 (마음껏 채굴, 배포 가능)  
- 퍼블릭 네트워크 : 전세계 사람들이 동일한 데이터를 유지하고 있는 네트워크 (데이터가 불가역적으로 저장)  
  - 메인넷  
  - 테스트넷 ex) _롭슨 Ropsten_, 코반 Kovan, 링키비 Rinkeby, 고얼리 Goerli

#### 이더리움 네트워크 개념도
![networkconcept](md-images/networkconcept.png)  
- 기본적으로 블록체인은 다수의 노드로 구성되는 것이 원칙 (로컬 PC는 노드가 한 개)  
- 노드는 다양한 데이터를 동기화시켜서 블록의 형태로 가지고 있음
- 이더리움에 참여하기 위해서는 이더리움 클라이언트를 활용해야함 -> geth를 많이 사용

#### 환경설정
1. Chocolatey 설치 with Windows PowerShell 관리자 권한
https://chocolatey.org/install
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

2. 사전 필요 요소 설치 with Windows PowerShell 관리자 권한
```
> choco install git -y
> choco install golang -y // geth가 go 언어로 되어있으므로 설치해야함
> choco install mingw -y
> choco install nodejs-lts // node.js 설치
```

3. Geth 설치 with 명령 프롬포트
```
> mkdir src\github.com\ethereum
> git clone https://github.com/ethereum/go-ethereum --branch v1.9.24 src\github.com\ethereum\go-ethereum // go-ethereum == geth
> cd src\github.com\ethereum\go-ethereum
> go get -u -v golang.org/x/net/context // 컴파일
> go install -v ./cmd/...
> geth version
```

4. 가나슈 설치 with 명령 프롬포트 
```
> npm install -g ganache-cli
> ganache-cli --version
```

5. 지갑 프로그램(Metamask) 설치 with 크롬 확장 프로그램
https://metamask.io/

#### 로컬 네트워크 활용 및 실습
1. 가나슈 구동 -> 가나슈를 사용하면 PC가 마치 이더리움 네트워크인 것처럼 개발 가능
###### 로컬 테스트넷 구동 -> 프라이빗 로컬 네트워크가 구성되었다.
```
ganache-cli -d -m -p 7545 -a 5
```
- -d -m (-deterministic --mnemonic) HD Wallet 생성시 니모닉 구문 사용
- -p(--port) 포트 지정 (default 8545)
- -a(--account) 구동 시 생성할 계정 수 (default 10)

######명령어 옵션 확인
```
ganache-cli --help
```

2. Geth로 네트워크에 접속
###### geth 명령어로 가나슈 테스트넷에 접속 with 새 명령 프롬포트
```
> cd src\github.com\ethereum\go-ethereum
```









