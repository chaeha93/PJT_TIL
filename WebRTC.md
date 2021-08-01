## WebRTC API
## WebRTC(Web Real-Time Communication)은 웹 애플리케이션과 사이트가 중간자 없이 브라우저 간에 오디오나 영상 미디어를 포착하고 마음대로 스트림할 뿐 아니라, 임의의 데이터도 교환할 수 있도록 하는 기술

## WebRTC 통신 방식
- P2P : 두 단말이 서로 1:1 통신하는 방식
- MCU, SFU : 대규모 서비스를 구축할 때 사용하는 방식, 중앙 서버를 두어 트래픽 중계하도록 함
- 두 기기가 실시간 소통을 하기 위해선 다음과 같은 사항이 필요
1. 기기의 스트리밍 비디오/데이터 등을 가져올 수 있을 것
2. 소통하고자 하는 기기의 IP 주소와 포트 등 네트워크 데이터가 필요
3. 에러 보고, 세션 초기화를 위해 신호 통신을 관리
4. 서로 소통할 수 있는 해상도인지, 코덱은 맞는지 capability 정보 교환
5. 실제 연결 맺음
6. 이후 스트리밍 비디오/데이터 등을 주고 받을 수 있어야 함.

## WebRTC가 제공하는 API
- MediaStream : 사용자의 카메라 등 input 기기이 데이터 스트림에 접근
- RTCPeerConnection : 암호화/대역폭 관리 기능, 오디오/비디오 연결
- RTCDataChannel : 일반적인 데이터 P2P 통신
- 나머지는 Signaling이라는 과정으로 관리

## 시그널링(Signaling)이란..?
- WebRTC는 브라우저 간에 스트리밍 데이터를 교환하기 위해 RTCPeerConnection을 사용
- 그리고 통신 조정 및 메시지를 컨트롤 하는 메커니즘인 시그널링이 필요함. 즉, 시그널링은 통신을 조율할 메시지를 주고 받는 일련의 과정을 의미함. 
- 시그널링 표준은 WebRTC에 정의되어 있지 않으므로 개발자가 편한 방식을 선택

## 시그널링의 역할
- Session control messages : 통신의 초기화, 종료, 그리고 에러 리포트
- Network configuration (네트워크 정보 교환) : 외부에서 보는 내 컴퓨터의 IP 주소와 포트, ICE 프레임워크를 사용해서 서로의 IP와 포트를 찾는 과정
- Media capabilites (Media Capabilities) : 내 브라우저와 상대 브라우저가 사용 가능한 코덱, 그리고 해상도. 미디어 정보 교환은 offer와 answer 로직으로 진행, 형식은 SDP (Session Description Protocol)
이 작업은 스트리밍이 시작되기 전에 완료되어야 함.
시그널링을 성공적으로 마치면, 실제 데이터는 P2P 또는 중계서버를 거쳐서 통신하게 됨

## 서버의 역할
- 사용자 탐색과 통신 / Signaling
- NAT / 방화벽 탐색
- Peer to Peer 통신 서버 시 중계서버



