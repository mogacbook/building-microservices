# 5장 모놀리스 분해하기 - 두번째

### 트랙잭션의 경계
>> 트랙잭션은 이벤트가 모두 발생하거나 전혀 발생하지 않게 해준다.

트랙잭션을 데이터베이스 분야에서 매우 자주 사용해왔지만 데이터베이스에만 적용되는 것은 아님 <br/>
 -> 예) 메시지 브로커는 오래 전부터 트랙잭션 내 메시지를 게시하고 수신해왔음  <br/>
모놀로식 스키마의 경우 모든 생성, 업데이트는 단일 트랙잭션 경계 내 수행될 것 <br/>
-> 데이터베이스 분리 시, 단일 트랙잭션에 의해 제공된 안정성을 잃는다 <br/>
>> 주문발주 프로세스가 두 개의 분리된 트랜잭션 경계를 포함할 때... <br/>
>> 주문 테이블 삽입에 실패하면 일관성을 유지한 상태에서 모든것을 확실히 중단 <br/>
>> 주문 테이블 삽입에는 성공했지만 수집 테이블 삽입에 실패할 경우에는???

1. 나중에 재시도하기
>> 트랜잭션이 완료되었을 때 시스템이 일관성을 유지하는 상태를 보장하기 위해 트랜잭션 경계를 사용하는 대신 향후 특정 시점에 시스템이 스스로 일관성을 유지하는 상태가 될 수 있음을 허용 (최종적 일관성의 또 다른 형태)
2. 전체 작업 중지하기
>> 전체 연산 작업을 중지하고 시스템을 다시 일관성이 유지된 상태로 복귀하는 것 <br/>
>> --> 보상 트랜잭션을 발행, 직전의 트랜잭션을 되돌릴 수 있는 새로운 트랜잭션을 발생<br/>
>> 보상트랜잭션이 실패하면...<br/>
>> 일광성 유지 작업이 여러개라면 각각의 실패모드에 대한 보상 트랜잭션 처리하는 것은 구현은 고사하고 이해하기도 어려움
3. 분산 트랜잭션
>> 보상 트랜잭션을 수동으로 통제하는 방식의 대안은 분산 트랜잭션의 사용<br/> 
>> 하부 시스템에서 수행되는 다양한 트랜잭션을 통제하기 위해 트랜잭션 관리자라는 전체적 통제 프로세스를 사용해서 트랜잭션 내부의 여러 트랜잭션을 확장하려 시도<br/>
>> 2단계 커밋(분산 트랜잭션 처리하는 가장 범용적 알고리즘) - 투표단계가 시작되며 각 참여자는 로컬트랜잭션의 진행 가능 여부를 트랜잭션 매니저에게 알리고 트랜잭션 매니저는 모든 참여자로부터 찬성표를 얻는다면 참여자에게 각각 커밋을 수행하라고 지시, 반대표 하나라도 받는다면 롤백 명령<br/>
>> --> 중앙의 조정 프로세스가 진행을 허락할 때까지 모든 참여자는 중지해야 하는데 이는 장애에 취약함
4. 그렇다면 무엇을 해야할까?
>> 정말로 일관성이 유지되어야 하는 상태가 필요하다면 처음부터 그 상태가 유지되도록 할 수 있는 모든 것을 하라<br/>
>> 기술적 관서메서 벗어나 트랜잭션 자체를 포함할 구체적 개념을 만들어라

### 리포팅
특정 서비스를 더 작은 부분으로 분리하는 데 있어 데이터의 저장 방법과 장소를 잠재적으로 분리할 필요가 있는데 이는 일반적이고 필수적인 사용 사례인 리포팅에 문제가 된다.<br/>
리포팅 영역이 장애를 만들 가능성이 있지만 우선 기존 프로세스와의 협업 방식을 정하는 것은 가치가 있다

### 리포팅 데이터베이스
리포팅은 유용한 결과 생성을 위해 조직 내 여러 부분의 데이터를 함께 모을 필요가 있다.<br/>
일반적으로 리포팅 질의가 메인 시스템 성능에 영향을 주는 것을 우려하여 리포트를 메인 데이터베이스에 실행하지 않고 읽기용 복제 데이터베이스에 연결된다.<br/>
이에 따른 단점은….<br/>
첫째, 데이터베이스 스키마는 실행 중인 모놀로식 서비스와 리포팅 사이에서 사실상 공유 API이기 떄문에 스키마의 변경은 조심스럽게 관리되어야 함<br/>
둘째, 이런 경우 데이터베이스를 최적화하는 방법이 제한적<br/>
셋째, 관계형 데이터베이스가 수많은 리포팅 도구와 호환되는 SQL 질의 인터페이스를 제공하더라도 동작 중인 서비스에 데이터를 저장하기 위한 최선의 방법은 아님<br/>

### 서비스 호출을 통한 데이터 추출
리포팅 시스템 역시 특정 방법으로 데이터를 추출하는 외부 도구에 자주 의존<br />
SQL 인터페이스를 제공하면 리포팅 도구 체인을 가능한 한 손쉽게 통합하기 위한 가장 신속한 방법이 됨<br />
>> 이 방법에 따른 몇가지 문제…<br />
>> 다양한 마이크로서비스에 의해 노출되는 API는 리포팅 용도로 설계된 것이 아닐 가능성<br />
>> —> 고객 서비스 ID 또는 다양한 고객 필드로 고객을 검색할 수 있게 허용하지만, 모든 고객을 추출하는 API를 반드시 외부에 공개하는 것은 아님 즉, 모든 데이터 추출을 위해 많은 API 호출을 해야할 수 있음<br />
>> —> 예를 들어 고객리스트를 얻기 위해 개별 호출을 반복해야 하는데 이는 리포팅 시스템이 비효율 적이며 부하가 될 수 있음<br /><br />
>> 리포팅을 쉽게 만들기 위해서는 배치 API를 제공, 이 문제를 해결할 수 있음<br />
>> —> 고객 서비스는 배치를 통해 고객 ID리스트를 전달받아 고객 정보 추출 또는 모든 고객 정보를 페이지 단위로 제공하는 인터페이스에 노출<br />
>> —> 더 극단적인 버전은 배치 요청을 자원으로 자체 모델링 하는 것<br />

### 데이터 펌프
리포팅 시스템이 데이터를 끌어오는 방식 대신 리포팅 시스템에 데이터를 밀어 넣는 방식을 시도할 수 있다<br />
—> 다만 다수의 호출을 할 때 발생하는 HTTP 부하 및 리포팅 목적으로만 사용될지 모르는 API를 만들어야 하는 부담<br />
—> 다른 대안 :  데이터 소스인 서비스의 데이터베이스에 직접 접근, 리포팅 데이터베이스로 밀어 넣는 독립 프로그램을 가지는 것<br />
>> 데이터 펌프는 해당 서비스를 관리하는 팀이 개발 및 관리<br />
>> 또한 서비스와 데이터 펌프 중 하나가 배포될 때 같이 배포하는 것을 가정해 버전을 관리하고 데이터 펌프를 서비스 자체 빌드의 일부로 추가해 부차 결과물 생성을 제안<br />
>> —> 함께 배포하고 서비스 팀 외부에 스키마 접근을 개방하지 않으면 전통적 DB통합 문제의 많은 부분이 완화<br />

1. 대체 종착지
>> 예시)<br />
>> AWS S3를 실제로 거대한 데이터 마트로 위장, JSON 파일을 S3에 저장하기 위해 일련의 데이터 펌프 사용<br />
>> 솔류션이 확장이 필요할 때까지 효과가 있었음<br />
>> 현재 표준 리포팅 도구(엑셀, 태블로 등) 와 통합할 수 있는 큐브를 채우도록 펌프 번경 검토 중<br />

### 이벤트 데이터 펌프
이벤트  데이터 펌프는 서비스 내부와 더 약하게 결함되므로 해당 마이크로서비스를 담당하는 팀이 아닌 다른 그룹이 이벤트 데이터 펌프를 관리하기 더 쉽다고 생각할 수 있음<br />
이벤트 흐름의 특성이 구독자와 서비스의 변경을 지나치게 결합하지 않는 한 이벤트 매퍼는 가입된 서비스와는 독립적으로 진화<br />
>> 주요단점<br />
>> 필요한 모든 정보를 이벤트로 확산해야 하는 것<br />
>> 그럼에도 불구하고 이미 적절한 이벤트를 공개하고 있다면 이 방법을 통한 더 느슨한 결합, 더 빠른 데이터 갱신을 고려할 필요 있음<br />

### 백업 데이터 펌프
>> 넥플릭스 사례<br />
>> 넷플릭스 SSTable에 백업된 것을 작업 원본으로 사용하는 하둡을 통해 방대한 양의 데이터를 처리할 수 있는 파이프라인을 구현<br />
>> 데이터 펌프처럼 이 백업 데이터 펌프 패턴 역시 리포팅 스키마와 결합<br />
>> 이는 추후 아이기스토스 프로제긑로 오픈 소스화 됨<br />

### 실시간을 향해
대시보드, 알림, 재무 리포트, 사용자 분석 등과 같은 사용 사례는 정확성과 타임라인에 대한 각기 다른 허용 오차를 가지며 다른 기술적인 선택을 하게 될 것이다.

### 변경 비용
코드를 코드베이스 내 이동시키는데 소요되는 비용은 낮지만 데이터베이스를 분리하는 것은 더 많은 작업을 필요하고 데잉터베이스 변경을 되돌리는 것은 복잡한 일이다<br/>
서비스들이 지나치게 결합된 통합을 분리하는 것 또한 다수의 소비자가 사용하는 API를 완전히 재작성하는 것은 상당히 큰 작업이다.<br/>
>> 이 위험을 관리하는 방법은 가장 영향도가 낮은 실수부터 시도하는 것!<br/>

### 원인 파악
시스템의 아키텍처가 시간이 지남에 따라 점진적인 방식으로 변화하는 것을 원한다.<br/>
따라서 변경 비용이 높아지게 전에 분리하는 작업이 필요하다는 사실을 인식하는 것이 핵심이다.<br/>

### 마치며
드러난 서비스 경계에 따라 접합부를 찾아서 시스템을 점진적인 방법으로 분해한다<br/>
접합부의 발견에 능숙해지고 처음부터 서비스 분리의 비용을 줄이도록 작업하면서 <br/>
어떠한 신규 요구 사항도 만족하도록 시스템을 계속 성장시키고 진화시킬 수 있다<br/>
