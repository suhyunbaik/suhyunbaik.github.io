---
layout: post
title: 대기업에 익스트림 프로그래밍 적용하기 
tags: [Extreme programming, Agile, study]
categories: [Agile]
---

익스트림 프로그램은 애자일 개발 방법론의 한 방법이다. 최근 핫해진 스크럼과는 달리 많이 사용되지 않는다. 한번은 내가 어느 자리에서 방법론에 대한 발표를 할 기회가 있었다. 내가 주제를 XP(익스트림 프로그래밍) 선정했다고 하자 처음 들어본다는 말도 들었다. 
내가 XP에 관심을 둔 이유는 현재 소속된 조직에서 XP 실천방법 중 여러 방법을 사용하고 있기 때문이다. XP를 실천하려는 의도가 있는 건 아니다. 프로젝트 개발을 도와줄 요소를 리서치하고 채택한 결과가 XP의 실천방법 중 몇가지를 적용하는 것이였다. 밑에서 12가지 방법을 모두 소개할건데, 현재 조직에서는 페어 프로그래밍, 단위테스트, 코드 공동 소유권등을 채택해 적용하고 있다.
XP의 목표는 ‘고객이 원하는 양질의 소프트웨어를 빠르게 전달' 하는 것이고, 비즈니스 상의 요구 변동이 심할 때 적합한 방법으로 알려져 있다. 그래서 XP의 장점은 고객의 요구사항이 개발 단계와 상관없이 계속 변경될 때 드러난다. 전통적인 워터폴에서는 소프트웨어가 개발이 진행되면서 변경비용이 증가한다.  XP 방법을 사용하면 소프트웨어 개발 진행 여부와 상관없이 변경 비용이 적게 든다. 예를 들어 전통적인 워터폴 방법 조직에서 어떤 프로젝트가 기획 단계를 통과하고 디자인이 이미 끝났다고 하자. 기획이 다시 변경된다면, 변경사항이 반영된 기획이 기획 단계를 다시 통과해야 하고 디자이너들이 다시 디자인을 변경해야 한다. XP 방법 조직에서는 일이 다르게 흘러간다. 프로젝트는 작은 단위로 쪼개지고, 하루에도 매번 지속적인 통합 작업이 이뤄진다. 그래서 기획이 변경되어도 변경 비용이 적게 된다. 
지속적인 통합, 작은 단위의 배포, 현장에서 팀원과 소통하는 고객 등 12개의 XP 실천요소가 이를 가능하게 한다. 이런 강력한 장점이 있지만 많이 사용되지 않는 이유가 있다. XP는 대규모 프로젝트에 적용하기 어렵다.  

켄트 벡(Kent Beck) 은 이렇게 말했다.

>“You probably couldn’t run an XP project with a hundred programmers. Nor fifty. Nor twenty, probably. Ten is definitely doable.”   
“프로그래머 100으로는 XP 프로젝트 실행이 아마 힘들겁니다. 50명도, 아마 20명도 안될겁니다. 그러나 10명은 확실히 가능합니다.”

또다른 예를 들자면, 마틴 파울러(Fowler)는 25명의 개발자로 구성된 한 팀을 XP를 적용하려고 2팀으로 나눈 적이 있다. 

XP가 이런 단점을 갖고 있는 이유는 그 태생 때문이다. XP는 단일 고객이 사용할 커스터마이즈된 소프트웨어를 빠르게 개발하기 위해 태어난 방법론이다. 켄트 벡은 다임러 크라이슬러 회사에서 프로젝트를 진행할 때 XP 를 탄생시켰다. 그리고 그 프로젝트는 크라이슬러 사내에서 사용할 투표 소프트웨어였다. 사내 소프트웨어를 사용할 고객은 단일 고객이라고 보아도 무방하다. 항상 현장에 있으면서 실시간으로 피드백을 줄것이고, 개발된 소프트웨어의 많은 부분을 검증하고 싶어 할 것이다. 그래서 XP 개발 방법론이 적합하다. 
XP의 실천 방법중에는 어느것도 대규모 조직 적용을 제한하는 기술적 한계는 없다. 그러나 팀원의 수, 지리적인 위치등 비기술적인 이유들이 XP를 대규모 조직에 적용하기 어렵게 만든다. 그런데 이런 XP를 대규모의 프로젝트에 적용한 연구가 있었다. 

### A Study of Extreme Programming in a Large Company 
### 대기업의 익스트림 프로그래밍에 대한 연구

이 논문은 대기업의 프로젝트에 XP를 적용하고, XP를 경험한 개발자들을 인터뷰 했다. 

>ABSTRACT  
Agile software development is an approach to software that focuses on lightweight processes and adaptability to change. The best-known agile methodology is called Extreme Programming. It suggests twelve practices that include iterative development practices, automated unit testing, and pair programming. Extreme Programming is designed for small projects, but has been picked up through grassroots efforts in some large projects in large companies, including Avaya. We studied six such projects in Avaya. We were interested to learn how projects adapted Extreme Programming to their needs, and which of the twelve practices they used. Projects adopted iterative development practices, along with dynamic prioritization of work. Although Extreme Programming downplays architecture, every project retained focus on software architecture. They had mixed success moving to automated unit testing, and most used some form of pair programming. These practices appear to be both beneficial and practical for projects of various sizes in large companies, and we recommend them for those wishing to use agile development practices in large companies.

초록  
애자일 소프트웨어 개발은 경량 프로세스와 변화 적응성에 중점을 둔 소프트웨어 접근 방식이다. 가장 잘 알려진 애자일 방법론은 익스트림 프로그래밍(Extreme Programming, 이하 XP)이다. 익스트림 프로그래밍은 반복적인 개발, 자동화된 단위 테스트 및 페어 프로그래밍을 포함한 12가지 실천방법을 제안한다. 익스트림 프로그래밍은 소규모 프로젝트를 위해 설계됐다. 그러나 Avaya(Avaya)를 포함한 대기업의 일부 대규모 프로젝트에서 프로젝트 소속원들이  XP를 프로젝트 개발 방법으로 선택했다. 우리는 Avaya에서 6개의 프로젝트를 연구했다. 프로젝트가 익스트림 프로그래밍을 그들의 요구에 맞게 어떻게 조정하는지, 12 가지 실천방법 중 어떤 것을 사용하는지에 대해 관심이 있었다. 프로젝트는 역동적인 작업 우선 순위와 반복적인 개발 방법을 채택했다. 익스트림 프로그래밍은 아키텍처를 과소평가 하는 경향이 있지만, Avaya의 각 프로젝트는 소프트웨어 아키텍처에 주의 집중했다. 그들은 자동화 된 단위 테스트로 성공적으로 전환했고, 대부분이 어떤 형태이든 페어 프로그래밍을 사용했다. 이러한 실천방법은 대기업 내 다양한 규모의 프로젝트에 유리하고 실용적이다. 우리는 대기업에서 애자일 개발 방법을 실천하려는 사람들에게 이러한 실천방법들을 추천한다.

참고로 [Avaya](https://g.co/kgs/Jhanen)는 미국에 본사를 둔 다국적 통신기업이다. 

다음은 익스트림 프로그래밍을 프로젝트에 적용할 때 실천해야 할 사항들이다. 

>• The Planning Game – Quickly determine the scope of the next release by combining business priorities and technical estimates. As reality overtakes the plan, update the plan.  
• Small Releases – Put a simple system into production quickly, then release new versions on a very short cycle.  
• Metaphor – guide all development with a simple shared story of how the whole system works.   
• Simple design – The system should be designed as simply as possible at any given moment. Extra complexity is removed as soon as it is discovered.  
• Testing – Programmers continually write unit tests, which must run flawlessly for development to continue. Customers write tests demonstrating that features are finished.   
• Refactoring – Programmers restructure the system without changing its behavior to remove duplication, improve communication, simplify, or add flexibility.   
• Pair programming – All production code is written with two programmers at one machine.   
• Collective ownership – Anyone can change any code anywhere in the system at any time.   
• Continuous integration – Integrate and build the system many times a day, every time a task in completed.   
• 40-hour week – Work no more than 40 hours a week as a rule. Never work overtime a second week in a row.   
• On-site customer – Include a real, live user on the team, available full-time to answer questions.  
• Coding standards – Programmers write all code in accordance with rules emphasizing communication through the code.   

• 계획 게임 – 비즈니스 우선 순위와 기술 견적을 결합해 다음 배포 범위를 신속하게 결정한다. 개발 상황이 계획보다 앞서가면 계획을 업데이트한다.  
• 작은 배포 – 간단한 시스템을 빠르게 생산 한 다음 새 버전을 매우 짧은 주기로 배포한다.  
• 메타포(은유) – 전체 시스템 작동 방식에 대한 간단한 공유 스토리를 통해 모든 개발을 안내한다.  
• 단순한 설계 – 시스템은 주어진 순간에 가능한 한 간단하게 설계한다. 추가적인 복잡도는 발견 즉시 없앤다.  
• 테스트 – 프로그래머는 지속적으로 단위 테스트를 작성하며, 개발을 계속하려면 테스트가 완벽하게 실행돼야 한다. 고객은 기능이 개발  완료되었음을 나타내는 테스트를 작성한다.  
• 리팩토링 – 프로그래머는 동작을 변경하지 않고 시스템을 재구성해 반복을 제거하고, 의사 소통을 개선하거나, 유연성을 추가한다.  
• 페어 프로그래밍 – 모든 생산 코드는 한 머신에서 두 명의 프로그래머로 작성한다.  
• 공동 소유권 – 누구나 언제 어디서나 시스템의 모든 코드를 변경할 수 있다.  
• 지속적인 통합 – 작업이 완료 될 때마다 하루에 여러 번 시스템을 통합하고 구축한다.   
• 주당 40 시간 – 일반적으로 주당 40 시간을 넘기면 안된다.  
• 현장 고객 – 팀에 실제 사용자를 포함시켜 실시간 질문에 답변 할 수 있다.  
• 코딩 표준 – 프로그래머는 코드를 통한 소통을 중요시 여기는 규칙에 따라 모든 코드를 작성한다.  

>FINDINGS OF THE STUDY   
There was fairly strong agreement among the projects on which practices they followed, and which they felt were important. The following is a list of the findings from the study.
As expected, no project followed XP exactly. A major reason for this was that most of the projects studied were small parts of larger projects. Therefore, they could not implement all the practices of XP; they did what they could in their own situation. It was not clear from the study which other XP practices they would have implemented if they had had the chance. However, since a large part what we wanted to learn was which practices work in our culture, we were interested in the empirical rather than the hypothetical, anyway. 
Because of the position in the schedule, as well as the fact that they were in larger projects, it was impossible to collect reliable quality and productivity metrics. However, it appeared that their productivity compared favorably to standard development methodologies in the company. It was significant to note that every person interviewed was enthusiastic about the methodology. All intended to continue their practices, and expand them where possible. 
The following list describes the teams followed each of the twelve XP practices: 
• The Planning Game – No project implemented the Planning Game fully. While planning was flexible, it was required to be within the boundaries of the larger product plan. This had two major ramifications. First, an overall project plan was established. This was necessary not only for the project as a whole, but also for other teams, in particular documentation, sales, marketing, and product support. Second, others’ needs often constrained prioritization somewhat. 
• Small Releases – All projects used small releases in some form. The size of the development intervals was from two to four weeks. Where the XP development was part of a larger project, the development intervals culminated in deliveries into the official code base of the project. 
One team punctuated each interval with a demo to themselves and others on the project. They reported that this was a strong morale booster on the team. 
One project’s data showed that they had not made substantial improvement in the accuracy of their development estimates over several iterations. They admitted, however, that they had not re-estimated after each iteration, but plan to do so in the future. 
• Metaphor – No project had a metaphor. This is consistent with reports from Kent Beck , who stated that people tell him that they do XP, “..except metaphor, of course.”[18]. Others have also noted that people have difficulty understanding Metaphor [19]. This is certainly one major reason that no project used metaphor. 
• Simple design – Projects did not highlight simple design as an important part of their XP process. This should not be construed to imply that they did not have simple designs, but rather that it was not an important difference from their traditional processes. 
• Testing – All projects intended to require that tests be submitted along with code, creating a body of automated regression tests. One project followed this rigorously. Other projects followed it partially. The major reason for this was schedule pressure with focus on delivered functionality. A related reason was the time and effort were not available to set up an automated regression testing system, particularly where the XP project was part of a larger project. All projects were for software to be sold to multiple customers, so it was not realistic to have a customer write and execute acceptance tests. However, every project did have an extensive system verification program. In this model, the system testers functioned as “surrogate customers”, writing and executing acceptance tests.
• Refactoring – Refactoring did not figure prominently in the projects. The only project to refactor frequently was a forward looking work project with three people on the team. The other projects indicated that they were not opposed to refactoring, but there really hadn’t been a need to do so. This may be a reflection of more up-front design than is typically done in an XP project
• Pair programming – All projects did some form of pair programming, but each did it a little differently. No project required it for every line of code; in fact in every project, pair programming was voluntary. In one project, developers began by doing all programming as pairs, but found it was too inefficient. So they programmed the simple code individually, but paired up on the difficult code. Another project had two developers, located 2000 miles apart. They tended to write code individually, but debugged their code together, using a shared desktop. One developer pointed out that this was actually more convenient, because they weren’t crowded against each other. 
 • Code inspections are standard practice in Avaya. In one instance, pair programming was allowed to replace code inspections. The project did not have data to indicate whether one was more effective than the other in finding errors. 
 • Collective ownership – Code ownership practices varied from project to project, due in part to constraints of the surrounding projects. Where collective ownership was practiced, there was a practice of de facto code ownership: people gained natural expertise in certain parts of the system, and made the bulk of changes there. One project codified this practice into a 4 “lightweight ownership” policy: one could change any of the code, but needed to check with the owner of the code for advice. 
 • Continuous integration – In most cases, projects worked within the larger project methodology of integration. This was generally weekly integrations into the main software base, although the XP sub-projects were able to integrate more often. In one standalone case, the team members integrated continuously. 
 • 40-hour week – One or two projects made this a policy. However, no project appeared to have suffered extended periods of long hours. 
 • On-site customer – No project had an on-site customer. As stated above, this model is not practical where one has many customers or potential customers. In addition, it is usually not desirable; the project gets only a single view among many customers. Projects continued to use a surrogate customer model, where an aggregate view of customer needs is created. 
 • Coding standards – Avaya has had a tradition of coding standards. The XP projects followed their pre-existing coding standards
In brief summary, projects followed XP most closely in the coding activities. As expected, where the projects touched other organizations in the company, XP practices were weak, or not followed at all. 

연구 결과
그들이 실천한 실천사항과, 그들이 중요하다고 느낀 실천사항 사이에는 상당히 강력한 합의가 있었다. 연구를 통해 알게 된 사실은 다음과 같다.
우리의 예상대로 XP를 정확히 따르는 프로젝트는 없었다. 이러한 현상의 발생한 주요한 이유는 연구한 대부분의 프로젝트가 더 큰 프로젝트의 작은 일부분이였기 때문이다. 따라서 XP의 모든 관행을 적용할 수 없었다. 대신, 프로젝트 참여자들은 자신의 상황에서 행할 수 있는 일을 했다. 그들에게 기회가 있었다면 어떤 XP 실천사항을 구현했는지는 명확하게 알 수 없다. 그러나 우리가 배우고 싶었던 부분은 바로 어떤 실행 요소가 우리 문화에서 작동하는 지에 관한 것이였고, 우리는 가설보다 경험에 더 관심이 많았기 때문에 상관이 없었다.
근로자의 근무시간이 및 근무요일이 고정이였고(scheduled position), 그들이 큰 프로젝트에 포함돼 있었기 때문에, 신뢰할 수있는 품질 및 생산성 지표를 수집하는 게 불가능했다. 하지만 그들의 생산성을 회사의 기본 개발 방법에 비교하는 일은 순조로웠다. 인터뷰를 했던 모두가 개발 방법에 열정적이였다는 점을 잊지말자. XP의 12개 실천사항과, 해당 실천사항을 실천했던 팀의 모습을 아래에 정리했다.

• 계획 게임 - 계획 게임을 완전히 구현한 프로젝트가 없었다. 계획은 융통성이 있었지만
더 큰 제품 계획의 경계 내에서만 변경할 수 있었다. 이것은 두 가지 큰 파급효과가 있었다. 첫째, 전체 프로젝트 계획이 설립됐다. 이것은 프로젝트 전체뿐만 아니라 다른 팀, 특히 문서, 영업, 마케팅 및 제품 지원등을 위해서라도 필요하다. 둘째, 다른 사람들의 요구는 종종 우선 순위가 다소 제한됐다.

• 작은 배포 - 모든 프로젝트가 어떤 형태로든 작은 단위의 배포를 했다. 개발 주기는 2주에서 4주였다. XP개발이 큰 프로젝의 일부분일 경우, 개발 단위는 프로젝트의 공식적인 코드 베이스로 전달하는데 절정을 이뤘다.
한 팀은 프로젝트 개발 주기마다 해당 팀원과 다른 사람들에게 데모를 제공했다. 그들은 이것이 팀의 사기를 강하게 증진했다고 보고했다.
• 메타포(은유) - 메타포가 있는 프로젝트는 없었다. 이러한 현상은 사람들이 켄트 벡(Kent Beck)의 보고서와 일치한다. 사람들은 그에게 XP를 한다고 할때 이렇게 말했다. “..당연히 메타포 빼고요.”[18]. 다른 이들은 사람들이 메타포를 이해하는 걸 어려워 했다고 보고했다[19] 그 어려움이 메타포를 적용한 프로젝트가 없는 주요 이유 중 하나다.

• 단순한 디자인 - 프로젝트는 XP를 적용해 개발하는 과정에서 단순한 디자인을 중요한 요소로 여기지 않았다. 단순한 디자인이 없었다는 뜻이 아니다. 그들이 하던 전통적인 개발 과정에서 크게 다른 요소가 아니었기 때문이었다. 

• 테스트 - 모든 프로젝트는 코드와 함께 테스트를 제출하여 일련의 자동화 된 회귀 테스트를 작성해야 했다. 한 프로젝트는 이 규칙을 엄격하게 따랐다. 다른 프로젝트는 부분적으로 적용했다. 주요 이유는 제공되는 기능에 중점을 둔 일정 압력 때문이었다. 연관된 이유는 자동화된 회귀 테스트를 작성할 수 있는 시간과 노력 때문이었다. 특별하게는 XP 프로젝트가 더 큰 프로젝트의 부분이였기 때문이다. 모든 프로젝트가 다수의 고객에게 판매될 소프트웨어였다. 그래서 고객이 승인 테스트를 작성하고 실행하도록 하는 건 현실성이 없었다. 그러나, 개별 프로젝트는 프로그램을 검증할 확장된 시스템이 없었다. 이 모델에서, 시스템 테스터들은 “대리 고객"으로 일했다. “대리고객”들은 승인 테스트를 작성하고 실행했다. 

• 리팩토링 - 이 프로젝트에서는 리팩토링이 눈에띄게 나타나지 않았다. 3 명의 팀원으로 구성된 미래 지향적인 프로젝트만이 유일하게 리팩토링을 자주 했다. 다른 프로젝트는 리팩토링에 반대하지 않았지만 실제로는 할 필요가 없었다. 이것은 XP 프로젝트에서 보통 작업하는 것보다 더 미래적인 디자인의 반영일 수 있다.

 • 페어 프로그래밍 - 모든 프로젝트가 페어 프로그래밍을 어떤 형태로든 수행했는데, 각각 조금씩 달랐다. 모든 코드의 라인에 페어 프로그래밍이 필요한 건 아니였다. 사실 프로젝트마다 페어 프로그래밍은 자발적인 참여로 이뤄졌다. 한 프로젝트에서는, 개발자들이 모든 프로그래밍을 페어 프로그래밍으로 진행했다. 하지만 그게 비효율적이라는 걸 알았다. 그래서 간단한 코드는 각자 프로그래밍 하는 대신, 어려운 코드는 페어 프로그래밍을 했다. 또 다른 프로젝트는 2명의 개발자가 있었다. 그들 사이에는 2000 마일이라는 물리적 거리가 있었다. 그들은 주로 각자 코딩을 했는데, 디버깅을 할때는 한 데스크탑을 두고 함께 했다. 개발자 한명이 이것이 더 편리하다고 했다. 왜냐면 서로를 번잡스럽게 하지 않았기 때문이다. 
코드 검사는 Avaya의 표준 관행 이었다. 페어 프로그래밍은 코드 검사를 대체 할 수 있었다. 이 프로젝트에서는 오류를 찾는 데 다른 것보다 더 효과적인지 여부를 나타내는 데이터가 없었다. 

 • 공동 소유권 – 코드 소유권은 주변 프로젝트의 제약으로 인해 프로젝트마다 달랐다. 공동 소유권이 실행된 곳에서는 사실 코드 소유의 관행이 있었다. 사람들은 시스템의 특정 부분에 대해 자연스런 전문 지식을 얻었고, 거기서 많은 부분을 변경했다. 한 프로젝트는 이 관행을 4 가지“경량 소유”정책으로 체계화했다. 개인은 어떤 코드든지 변경할 수 있지만, 코드 소유자에게 조언을 구해야 했다.

 • 지속적인 통합 – 대부분의 경우 프로젝트는 더 큰 통합 프로젝트의 통합 방법 안에서 통합됐다. XP 하위 프로젝트가 더 자주 통합 될 수 있었지만, 일반적으로는 메인 소프트웨어를 기준으로 매주 통합했다. 한 독립적인 사례에서는 팀 구성원들이 지속적으로 시스템을 통합했다.

 • 주 40 시간 – 한두 개의 프로젝트가 이것을 정책으로 만들었다. 그러나 장시간 추가 근무때문에 고통받은 프로젝트는 없었다.

 • 현장의 고객 – 현장에 고객이 있었던 프로젝트는 없었다. 위에서 언급했듯이, 이 모델은 많은 고객 또는 잠재 고객을 가진 곳에서는 실용적이지도, 바람직하지도 않다. 이 프로젝트는 많은 고객들에게서 한가지 관점만 얻기 때문이였다. 프로젝트는 대리 고객 모델을 계속 활용해 고객의 요구에 대한 종합적인 관점을 작성했다.
 
 • 코딩 표준 – Avaya는 전통적으로 코딩 표준을 가지고 있었다. XP 프로젝트는 그들이 기존에 갖고있던 코딩 표준을 따랐다.

간략하게 요약하자면, 프로젝트가 코딩 활동을 할때 XP의 실천요소들을 가장 원래 형태와 근접하게 따랐다. 예상한대로 프로젝트가 회사의 다른 조직과 연관되면 XP가 약하거나 실천요소들을 아예 따르지 않았다.


>CONCLUSIONS  
Much of XP is proving to be useful to projects within Avaya. No project follows all of XP. There were two major reasons for deviance from all the XP practices. The first was that certain XP practices were not appropriate for large software projects. The second was that some practices did not fit with other practices in the existing projects. For the most part, the projects benefited
from the practices they did adopt.
Large projects in other companies may benefit from adapting the practices that the Avaya projects found useful.
Those projects may wish to explore agile methodologies in general, rather than focus on XP, which is most appropriate for small projects. 

결론  
XP의 많은 부분이 Avaya 사내 프로젝트에 유용한 것으로 입증했다. XP의 모든 실천사항을 따르는 프로젝트는 없었다. XP의 모든 실천방법이 실천되지 않은 데에는 두 가지 중요한 이유가 있었다.
첫 번째는 특정 XP 실천방법이 대규모 소프트웨어 프로젝트에 적합하지 않다는 것이다. 
두 번째는 일부 실천방법이 기존 프로젝트의 다른 실천방법과 안맞다는 것이다. 대부분 프로젝트는 그들이 채택한 실천방법의 혜택을 봤다. 
다른 회사의 대규모 프로젝트는 Avaya의 프로젝트가 유용하다고 생각한 관행을 채택함으로써 이익을 얻을 수 있습니다. 이러한 프로젝트는 소규모 프로젝트에 적합한  XP에 중점을 두는 것 보다, 일반적인 애자일 방법론을 탐색하는 것을 원할 수도 있다.

우선 해당 논문을 통해 알 수 있었던 것은 다음과 같다. XP를 대규모 프로젝트 전체에 적용하는 건 어렵다. 대규모 프로젝트의 작은 부분은 XP를 적용하기도 쉽고 혜택도 볼 수 있다. XP의 12가지 실천방법은 규모와 상관없이 현실에 전부 적용하기 힘들다. 현장 고객(on-site customer)은 소규모 프로젝트이더라도 실현하기 어렵다. 그리고 아이템에 따라 필요없을 수도 있다. 
반면에 지속적인 통합, 테스트, 배포, 코딩 표준, 페어프로그래밍, 코드 공동소유는 프로젝트의 규모나 소프트웨어의 특징과 상관없이 유용하다. XP의 모든 실천방법을 실천하려고 하기 보다, 상황과 소프트웨어의 특징에 맞게 취사선택해 적용하는 방법이 프로젝트에 도움이 될 것이다.

