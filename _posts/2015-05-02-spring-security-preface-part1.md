---
layout: post
title: Spring Security reference Part 1. 서문
description: 스프링 시큐리티 레퍼런스 파트1입니다.
---

Spring Security 레퍼런스
----------------------
*Authors*
 Ben Alex; Luke Taylor; Rob Winch; Gunnar Hillert

Spring Security는 강력한 사용자 정의 인증 프레임워크이자 접근 제어 프레임 워크이다. Spring 기반의 응용 프로그램 보안을 위한 실질적인 표준이다. 


### 서문

Spring Security는 Java EE 기반의 엔터프라이즈 소프트웨어를 위한 통합 보안 솔루션을 제공한다. 본 레퍼런스 가이드를 읽으며 발견하겠지만, 유용한 보안 시스템을 제공하기 위해 노력했다.

보안에 대해서는 포괄적이고 시스템 전반에 걸친 접근이 중요하다. Security Circles에서는 “계층적 보안(layers of security)”을 적용함으로써 각각의 계층이 보안을 책임질 뿐 만 아니라, 연속적인 계층들이 추가적으로 보안 하도록 하는 방법을 권고한다. 각 계층의 보안이 “더 촘촘”할수록, 당신의 어플리케이션은 더 견고하고 안전할 것이다. 하위 레벨에서는 중간자 공격을 방지하기 위해 transport security와 system identification과 같은 이슈를 다뤄야한다. 그 다음으로는 VPN이나 IP security와 함께 방화벽을 이용하여 권한이 있는 시스템만 연결을 시도할 수 있도록 만들어야 한다. 기업 환경에서는 공용 서버를 뒷단의 데이터베이스와 어플리케이션 서버로부터 분리하기 위해 DMZ를 배치하기도 한다. 운영체제 역시 중요한 역할을 수행하는데, 파일 시스템 보안을 최대화하고 권한 없는 사용자를 위한 별도 프로세스를 실행하는 등의 이슈를 처리한다. 일반적으로 운영 체제는 자체적인 방화벽을 구성하고 있다. 서비스 거부 공격(DoS)이나 시스템에 대한 brute force 공격을 막아야 할 일이 생길 수도 있다. 침입 감지 시스템은 이러한 공격을 모니터링하는데에 유용하다. 실시간으로 수상한 TCP/IP 주소를 막는 등의 방어 액션을 취할 수 있기 때문이다. 이제 상위 레벨을 보면, 당신의 JVM은 상이한 Java 타입에 대해서 최소한의 권한을 주도록 설정되어있고, 당신의 어플리케이션 역시 domain-specific 보안 설정을 각각 가지고 있다. Spring Security는 이 후자 부분 -어플리케이션 보안- 을 훨씬 쉽게 만들어준다. 

물론 위에서 언급한 보안 계층들은 모든 계층을 아우를수 있는 요소들과 함께 적절히 다뤄야 한다. 그러한 요소에는 security bulletin monitoring, patching, personnel vetting, audits, change control, engineering management systems, 데이터 백업, disaster recovery, performance benchmarking, 로드 모니터링, centralised logging, incident response procedures 등이 있다.

Spring Security로 다룰 수 있는 요구사항들은 비지니스 영역이 다양한 것만큼이나 많다. 뱅킹 어플리케이션에서 필요한 요구사항과 이커머스 어플리케이션의 요구사항이 다르고, 이커머스 어플리케이션에서 필요한 요구사항과 기업용 세일즈 포스 자동화 툴에서 필요한 것과는 또 다르다. 이러한 다양한 요구사항이 어플리케이션 보안을 흥미롭고, 도전적이고, 보람있게 만든다. 

[Part2. getting-started]에는 빠르게 시작할 수 있도록 프레임워크와 네임스페이스(namespace) 기반의 설정 시스템에 대하여 설명한다. Spring Security의 작동 방식과 사용하는 클래스들을  더 잘 이해하고 싶다면, [Part3. 아키텍쳐와 구현]을 읽으면 된다. 이 가이드의 나머지 파트는 필요한 부분을 선택적으로 읽을 수 있도록 전통적인 방식으로 구성되어있다. 어플리케이션 보안 문제와 관련된 전반을 가능한 한 많이 읽는 것 또한 추천하는 바이다. Spring Security가 모든 보안 문제를 해결할 수 있는 만병 통치약은 아니다. 중요한 것은, 어플리케이션 설계부터 보안 문제를 생각하고 있는 것이다. 나중에 보안을 끼워넣으려고 하는 것은 좋지 않은 생각이다. 특히 웹 어플리케이션을 만들고 있다면, cross-site scripting, request-forgery, session-hijacking같은 잠재적인 취약성에 대해 처음부터 고려해야한다. OWASP 웹사이트(http://www.owasp.org/) 는 유용한 레퍼런스 정보와 함께 웹어플리케이션 취약점 상위 10개의 리스트를 지속적으로 갱신해서 보여준다.

이 가이드가 유용하길 바라며, 언제나 피드백에 열려있다 [제안].

이제, Spring Security의 세계에 온 것을 환영한다 [커뮤니티].


<sub> translated by 박소은(@skyhills13) </sub>