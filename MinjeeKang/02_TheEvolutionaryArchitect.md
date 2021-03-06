# 2장 진화적 아키텍트
> 요약
>> __아키텍트__ <br/>
아키텍트는 통합된 기술적 비전을 확실히 확보하게 할 책임이 있으며 고객이 필요로 하는 시스템을 전달하도록 돕는 이들이다.<br/>
기술 리드의 역할을 병행하거나 전체 업무의 비전 정의 또는 전체 조직을 조율하는 등 그들의 역할을 한마디로 정의하기 쉽지 않다.<br/>
아키텍트를 건축가와 비교하기보다 아키텍트의 의미를 우리 입장에서 재정의하는 것이 최선의 방안이다.<br/><br/>
>> __아키텍트에 대한 진화적 관점__ <br/>
우리가 바라는 IT아키텍트 역할을 더 잘 요약하는데 적절한 비유로는 도시설계자가 있다. (심시티 게임을 해 본 사람이라면 누구나 알고 있는 역할, 도시를 구역화하고 사람이나 공공서립가 구역 간을 어떻게 이동할 것인가에 대해 고민)<br/>
시간이 흐르면서 도시가 변화하고 도시에서 발생하는 모든 측면을 직접 통제하려는 것이 헛수고임을 이미 아는 도시 설계자처럼 사용자가 소프트웨어를 사용하는 한 우리는 그에 대응하고 변화해야 한다.<br/>
도시 설계자로서 아키텍트의 방향자체는 개괄저긍로 설정하되, 특정 경우 세부 구현에 대해서만 매우 구체적으로 관여해야하며 시스템이 현재의 목적에 들어맞을 뿐만 아니라 미래의 플랫폼으로서 적합하다는 것을 보장하고 사용자와 개발자가 똑같이 행복해지도록 해야한다.(어렵다...)
<br/><br/>
>> __구역화__ <br/>
아키텍트의 구연은 서비스 경계 또는 서비스 대단위 그룹에 해당할 것이다. 따라서 아키텍트로서 우리는 구역 내의 일보다 __구역 사이__ 에서 발생하는 일을 더 걱정해야 한다. <br/>
넷플릭스는 데이터 저장 기술을 대부분 카산드라로 통일했다. 비록 모든 경우에 적합한 대응책은 아닐지라도, 카산트라와 관련된 도구 및 전문지식을 통해 얻는 가치야말로 몇몇 특정 태스크에 더 적합할 수 있는 다수으 ㅣ이종 플랫폼 확장을 지원하고 운영하는 것보다 더 중요하다고 생각했다.(이는 확장성을 가장 중요한 요소로 생각하는 극단적인 사례일 수 있다.)<br/><br/>
>> __원칙적인 접근법__ <br/>
마이크로서비스 아키텍처의 결정은 수많은 트레이드오플르 제공한다. 의사 결정을 프레이밍할 수 있는 가장 좋은 방법은 성취해야 할 목표에 기반하여 일련의 원칙과 실천 사항을 정의하는 것이다.
>>1. 전략적 목표 : 회사가 어디를 향해 나아가고 있는지, 고객을 행복하게 만들기 위해 어떻게 해야 할 지 언급해야 한다.
>>1. 원칙 : 더 큰 목표를 위해 해야 할 일을 정렬하는 규칙, 변경될 수 있으며 원칙의 수는 10개 미만인 것이 좋다. (예 클라우드 애플리케이션 플랫폼 헤로쿠의 12가지 요소 www.12factor.net
>>1. 실천 사항 : 원칙을 실행하는 방법, 업무 수행을 위한 자세 및 실질적인 지침이다. 대게 기술 명세적이며 구체적이어야 한다. 또한 조직의 제약을 반영하며 아키텍트의 원칙을 뒷받침 해야 한다.
>>1. 원칙과 실청 사항의 결합 : 누군가의 원칙이 다른 사람에게는 실천 사항이 될 수 있다. 작은 그룹에서는 원칙과 실천 사항을 결합하는 일은 문제가 되지 않는다. 그러나 큰 조직의 경우에는 공통된 원칙을 지향하면서도 기술과 실천 사항이 서로 다를 수 있으며 장소에 따라 다른 요소가 필요할 수 있다.

>> __필수 기준__ <br/>
우리는 거시적 시간을 잃지 않으면서도 개별 마이크로서비스 간의 자율성 최적화와 관련된 균형을 찾아야 한다. 이를 위한 확실한 방법 중 하나는 각 서비스가 가져야 할 명확한 속성을 정의하는 것이다.
>>1.모니터링 : 서비스 간 경계를 넘어 시스템 상태를 일관되게 살펴볼 수 있어야 한다. 서비스 지표를 얻기 위해서는 그래파이트를, 서비스 상태를 알아내기 위해서는 나기오스를 사용할 수 있다.
>>1.인터페이스 : 서비스 간 인터페이스 기술의 개수는 가능한 한 최소로 유지하는 편이 새로운 소비자(서비스)를 통합하는 데 도움이 된다(최소 1~2개, 스무 개는 매우 나쁨)
>>1.아키텍처의 안정성 : 아키텍트는 오동장하는 하나의 서비스가 전체를 망가뜨리게 해서는 안 되며, 서비스들이 비정상적인 하위 호출로부터 자신을 잘 보호하도록 해야 한다. 또한 응답코드가 규약대로 동작하도록 하는 것이 중요하다. 만약 시스템이 빠르게 동장하더라도 규칙에 대해 엄격하지 않다면 취약한 시스템으로 남게 된다.

>> __코드를 통한 통제__ <br/>
>>1.본보기 : 문서로 기록하는 것은 바람직하며 유용하다. 또한 시스템의 더 나은 부분을 모방하는 것만으로도 사람들은 큰 실수를 예방할 수 있다. 우리가 제공한 본보기를 실제로 적용하면서 우리는 모든 원칙의 타당성을 확신하게 된다.
>>1.맞춤형 서비스 템플릿 : 개발 실천 사항에 대한 서비스 템플릿을 맞춤화하면 팀은 더 빨라지고 개발자들도 서비스가 잘못된 행위에 빠지지 않게 방어할 수 있다. 서비스 템플릿을 만드는 일이 작업 방식을 지시하는 아키텍처 팀이나 중앙집권적 도구의 업무가 되지 않도록 주의해야 한다. 또한 맞춤형 서비스 템플릿을 사용하기로 했다면 매우 조심스럽게 다뤄라. 

>> __기술 부채__ <br/>
자신의 기술 비전을 충실히 이행하지 못하는 상황이 처하거나 긴급 기능 배포를 위해 절차를 무시한 선택을 하는데 이 것은 스스로 처리해야 할 또 하나의 트레이드 오프가 된다. 기술 비전에는 존재 이유가 있지만 그 이유에서 벗어날 경우 단기적으로는 이득일지라도 장기적으로는 비용이 발생할 수 있으며 이런 트레이드 오프의 이해를 돕는 개념이 바로 기술 부채다. 기술 부채는 단지 손쉬운 방법을 택했다는 이유만으로는 발생하지 않으며 변경된 시스템의 비전이 모든 시스템과 어울리지 않는다면 이 역시 새로운 기술 부채의 원인이 된다. 조직에 딷라 이키텍트가 관대한 지침을 제공하고 팀 스스로 부채를 챚아내서 상환하는 방법을 결정할 수 있다.<br/><br/>
>> __예외 처리__ <br/>
시스템이 원칙과 실천사항을 벗어나면 규칙에 대한 예외로 단정짓기도 하지만 나중에 참조할 수 있도록 기록할 수 있다. 충분히 많은 예외가 발견된다면 결과적으로 현실을 재인식시킬 수 있도록 원칙과 실천 사항을 변경하는 것은 타당할 수 있다.<br/>
문제를 직접 해결할 수 있도로 가능한 더 많은 자유를 팀에 부여하면서 팀의 자율성을 최적화하는 방법으로 마이크로서비스를 꽤 선호한다. 만약 개발자의 작업 방식에 많은 제약을 가하는 조직이라면 마이크로서비스가 적합하지 않을 수 있다.<br/><br/>
>> __중앙에서의 거버넌스와 지휘__ <br/>
아키텍트의 업무 중 하나가 기술 비전을 결정하는 것이라면 거버넌스는 우리가 구축하는 결과물이 해당 비전과 일치함을 보장하고 필요한 경우 비전을 진화시키는 것이다. 제 기능을 하는 거버넌스 그룹은 업무를 공유하고 비전을 형성하기 위해 협업할 수 있다. 거버넌스 그룹은 기술 전문가가 이끌어야 하고 대다수가 통제 업무를 실행하는 사람들로 구성되어 기술 위험도를 추적하고 관리할 책임이 있다.<br/><br/>
>> __팀 만들기__ <br/>
기술 리더의 역할은 사람들이 스스로 비전을 이해하도로 그들의 성장을 돕고 비전을 결정하고 구현하는데 적극적으로 참여할 수 있도록 만드는 것이다.<br/>
마이크로서비스에서 우리는 독립적 수명주기를 가진 수많은 자율적인 코드베이스를 가진다. 사람들이 더 많은 책무를 맡기 전에 개별 서비스의 소유권을 부여함으로써 그들의 ㄹ성장시키는 것은 그들 자신의 경력 목표를 성취하도록 돕는 훌륭한 방법일 뿐만 아니라 다른 담당자들의 짐도 덜어준다.<br/><br/>
>> __아키텍트의 핵심 책무__ <br/>
>>1.비전    > 명료하게 소통되고 시스템이 고객과 조직의 요구사항을 충족시키도록 돕는 기술 비전이 있는지 확인하라
>>1.공감    > 고객과 동료에 대한 여러분 결정의 파급력을 이해하라
>>1.협업    > 비전을 정의하고 다듬고 실행하기 위해 가능한 한 많은 동료와 협업하라
>>1.적응성   > 기술 버전이 고객과 조직이 요구하는 것을 반영하는지 확인하라 
>>1.자율성   > 여러분 팀의 표준화와 자율성 사이에서 올바른 균형을 찾아내라
>>1.거버넌스  > 시스템이 기술 비전에 맞게 구현되고 있는지 확인하라 

>> __진화적 아키텍트란__ <br/>
끊임없는 갈등 조정 과정을 통해 이러한 위업이 달성된다는 것을 이해하는 사람

> 책 내용 중
>> - p45</br>
아키텍트는 처음부터 완벽한 최종 상품을 만들기 보다 추가 학습을 통해 적합한 시스템이 생성되고 지속해서 성장할 수 있는 프레임워크를 만드는데 보탬이 되도록 생각을 전환해야 한다
>> - p48</br>
코딩하는 아키텍트(아키텍트가 팀과 함께 시간을 보내고, 이상적으로는 팀과 함께 실제로 코딩하는 것을 의미, 짝 프로그래밍)

> 느낀점
>> 아키텍트에 대해 이해가 어려웠는데 도시 설계자(특히 심시티!)와 비유해서 이해하기가 쉬웠다. 아무래도 아키텍트는 지금 내 역할과 거리가 멀지만 마이크로서비스를 적용하는 그룹에서 업무를 진행하게 된다면 지금 읽은 내용이 큰 도움이 될 것 같다.<br/>
내용이 너무 어렵다 T.T 

---
### 책 속 용어
- 프로그래밍 이디엄 (programming idiom) : 프로그래밍 언어에서 내장되어 있지 않은 간단한 작업, 알고리즘 또는 자료구조에 대한 표현 방식. 프로그래밍 문법, 스타일, 패턴 등 다양한 의미로 해석될 수 있다.
- 프로토콜 버퍼 : 구글에서 개발한 구조화된 데이터를 직렬화하는 방법으로, 네트워크 통신과 데이터 저장에 사용. 데이터 구조(메시지)와 서비스를 proto 정의 파일에 정의하고 다양한 언어로 인코딩/디코딩하여 이기종간의 호환이 가능. 유사한 기술로는 아파트 쓰리프트와이브로가 있음.
- 트레이드오프(trade-off) : 동시에 달성할 수 없어 한 목표를 택하면 다른 것을 포기해야 하는 상충 관계에 있는 상황 또는 그 상황 사이의 균형을 의미
- 비정상적인 하위 호출(downstream call) : 통신의 흐름에서 상위 통신 매체로부터 하위 통신 매체로 전해지는 데이터, 그 반대는 상향 호출(upstrean call) 이다. 일반적인 서버-클라이언트 전송 방식에서 서버는 상류, 클라이언트는 하위로 지정하며 서버에서 클라이언트로 가는 데이터를 지칭한다.
- 거버넌스 : 기업의 목적이 이해관계자의 요구, 조건, 선택을 평가함으로써 달성도리 수 있음을 보장한다. 우선순위 및 의사 결정을 통해 방향을 설정하고, 합의된 방향과 목표에 대한 성과, 준수 과정을 모니터링 한다.
