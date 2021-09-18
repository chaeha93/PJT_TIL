## Maven
- Apache에서 만든 소프트웨어 프로젝트 관리 및 comprehension 툴
- POM 개념을 기반으로 프로젝트의 빌드, 보고 및 문서를 중앙 관리할 수 있음 -> 프로젝트의 빌드 Lifecycle 기반 프레임워크
- mvn 명령어로 관리
- XML 기반의 pom.xml 파일로 설정

## Gradle
- Maven을 대체할 수 있는 프로젝트 구성 관리 및 범용 빌드 툴
- gradle 명령어로 관리
- 스크립트 기반의 build.gradle 파일로 관리
- JVM의 스크립트 언어인 groovy로 만듦

### 공통점
- 두 빌드 툴 모두 라이브러리 의존성을 해결하고 프로젝트를 관리한다.

### 차이점
- 프로젝트 구성/빌드 툴로써 프로젝트 구성은 정적인 설정 정보이고 빌드는 동적인 행위이다.
그런데 이것을 정적인 데이터를 저장하느데 적합한 XML로 그 내용을 기술하게 함으로써 동적인 행위인 빌드에 큰 제약을 가한다.
게다가 XML은 너무 장황해서 실제 설정 내용보다 XML 뼈대가 더 많다.
- Maven의 설계 상의 문제. 바로 멀티 프로젝트 구성을 상속 구조로 한 점이다. 그에 반해 Gradle은 구성 주입 방식(Configuration Injection)을 사용한다.
이는 빌드 구성 정보에서 매우 큰 차이를 만든다. Gradle은 Groovy DSL로 작성하며, 설정 정보는 변수에 값을 넣는 형태로, 동적인 빌드는 Groovy 스크립트로 Gradle용 플러그인을 호출하거나 직접 코드를 짜면 된다.