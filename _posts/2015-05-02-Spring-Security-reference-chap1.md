---
layout: post
title: Spring Security reference Part II. 시작하기
category: reference
tag: [spring_security, translation]
description: 스프링 시큐리티 레퍼런스 Part II Chap1 시작하기 부분입니다. 소제목 history와 realese numbering는 우선순위를 뒤로 미룹니다. 
---

## PART II. 시작하기

이 가이드의 후반부에서는 심도있는 사용자 정의를 하기 위해 이해해야하는 프레임워크 아키텍쳐와 클래스 구현에 대해 깊이있게 다룬다. 반면 이 파트에서는, Spring Security 3.0을 소개하고, 프로젝트 역사에 대해 간단하게 개괄한다. 또, 프레임워크 사용하는 방법에 대해 살짝 들여다본다. 특히, 개별 구현 클래스 전부를 직접 묶어야 했던 전통적인 Spring bean 접근법에 비해 훨씬 간단한 방법인 namespace 설정 방법으로 어플리케이션을 안전하게 하는 것에 대해 살펴본다. 

또, 예제 어플리케이션도 살펴본다. 예제로 실습을 하면, 후반부를 읽는데에 도움이 될 것이다 - 프레임워크에 대한 이해도가 높아진 후에 실습하는 것도 괜찮다. 다음의 웹사이트 [project website] (http://static.springsource.org/spring-security/site/index.html) 를 방문하면 프로젝트를 빌드하는데 유용한 정보와 글, 영상, 튜토리얼을 확인할 수 있다.


### 1. 도입


### 1.1 Spring Security란?

Spring Security는 Java EE 기반의 엔터프라이즈 소프트웨어를 위한 보안 서비스를 제공한다. 엔터프라이즈 어플리케이션 개발의 선두주자인 스프링 프레임워크를 사용하는 보조 프로젝트들이 강조되고 있다. 엔터프라이즈 어플리케이션을 사용하는데에 스프링을 사용하고 있지 않다면, 스프링을 한번 살짝 살펴보기를 추천하는 바다. 스프링 - 특히 의존성 주입 원칙 - 에 대한 익숙함이 Spring Security를 더 쉽게 사용하도록 해줄 것이다.

사람들은 많은 이유에서 Spring Security를 사용한다. 하지만 대부분의 경우, Java EE 서블릿 명세나 EJB 명세에서 전형적인 엔터프라이즈 어플리케이션 시나리오에 필요한 보안 피쳐의 깊이가 부족하다는 것을 깨달은 후에야 사용한다. 그러한 표준의 보안 설정은, WAR나 EAR 수준에서 이동이 불가능하다는 것이 핵심이다. 그렇기 때문에 서버 환경을 바꾸면 새로운 타겟 환경에 어플리케이션 보안을 재설정하기 위해 많은 작업이 필요하다. Spring Security는 이러한 문제를 해결해준다. 거기에 더해 유용한 사용자 정의 보안 피쳐도 여럿 제공된다.

아마 알고 있겠지만, 어플리케이션의 두가지 주요 분야는 "인증(authentication)"과 "권한부여(authorization)"(또는 "접근 제어(access-control)")이다. Spring Security의 주요 타겟인 두 분야이기도 하다. "인증"은 자신이 누구라고 주장하는 주체를 확인하는 프로세스다(일반적으로 "주체"는 사용자나 디바이스 혹은 어플리케이션에서 액션을 수행할 수 있는 시스템을 의미한다). "권한 부여"는 어플리케이션 내에서 주체가 액션을 수행하도록 허락하는 것을 결정하는 프로세스다. 권한 부여 결정이 필요한 포인트에 도달하기 위해서는, 주체가 이미 인증 프로세스를 통해 확인된 상태여야한다. 이 개념들은 Spring Security에만 해당하는 것이 아니라 일반적인 것이다.

인증 레벨에서 Spring Security는 넓은 범위의 인증 모델을 지원한다. 이 인증 모델의 대부분은 써드 파티에서 제공되거나 Internet Engineering Task Force같은 관련 표준 단체에서 개발된다. 추가적으로, Spring Security는 자체적인 인증 피쳐 셋을 제공한다. 구체적으로는 현재 아래와 같은 기술과 함께 통합 인증(authentication integration)을 지원한다:


* HTTP BASIC authentication headers (an IETF RFC-based standard)

* HTTP Digest authentication headers (an IETF RFC-based standard)

* HTTP X.509 client certificate exchange (an IETF RFC-based standard)

* LDAP (큰 환경에서 크로스-플랫폼 인증 니즈가 있을 때 아주 일반적인 접근 방식)

* Form-based authentication (간단한 사용자 인터페이스 니즈가 있을 때)

* OpenID authentication

* Authentication based on pre-established request headers (Computer Associates Siteminder와 같은)

* JA-SIG Central Authentication Service (CAS라고도 알려진 대중적인 오픈소스 통합인증(single sing-on) 시스템)

* Transparent authentication context propagation for Remote Method Invocation (RMI) and HttpInvoker (Spring remoting 프로토콜)

* Automatic "remember-me" authentication (그래서 예정된 기간동안 재인증하는 것을 피하기 위해 표시를 해둘 수 있다)

* Anonymous authentication (인증되지 않은 모든 요청에게 특정한 보안 신분을 허용하는 것)

* Run-as authentication (하나의 요청이 서로 다른 보안 신분으로 진행되어야하는 경우에 유용)

* Java Authentication and Authorization Service (JAAS)

* JEE container autentication (원한다면 여전히 Container Managed Authentication 을 사용할 수 있다)

* Kerberos

* Java Open Source Single Sign On (JOSSO) *

* OpenNMS Network Management Platform *

* AppFuse *

* AndroMDA *

* Mule ESB *

* Direct Web Request (DWR) *

* Grails *

* Tapestry *

* JTrac *

* Jasypt *

* Roller *

* Elastic Path *

* Atlassian Crowd *

* Your own authentication systems (아래를 확인)



(* 표시는 써드파티에 의해 제공되었음을 나타낸다)

많은 독립적인 소프트웨어 벤더들(ISVs)이 이렇게 많은 선택지 중에 유연하게 인증 모델들을 선택할 수 있기 때문에 Spring Security를 적용한다. 또한, 그렇게 하는 것이 엔지니어링에 많은 시간을 들이지 않고, 클라이언트의 환경을 바꿀 필요도 없이 그들의 솔루션을 빠르게 통합할 수 있도록 해준다. 만약 위의 인증 매커니즘 중 어느 것도 당신의 니즈를 충족하지 못한다면, Spring Security는 오픈 플랫폼이므로 당신만의 인증 매커니즘을 간단하게 작성하면 된다. Spring Security의 많은 기업 사용자들이 어떤 특정한 보안 표준도 따르지 않는 "레거시(legacy)" 시스템과 통합해야하는데, Spring Security 는 그러한 시스템과 기꺼이 "잘 융화된다(play nicely)".

심도있는 Spring Security는 인증(authentication) 매커니즘과는 상관없이, 심도있는 권한 부여(authorization) 기능 또한 제공한다. 세가지 주요 관심 분야가 있다 - 웹 리퀘스트 권한 부여, 메서드가 호출될 수 있는지에 대한 권한 부여, 개별 도메인 객체 인스턴스들에 접근할 수 있는 권한 부여. 차이점을 이해하는 데 도움이 되도록, 서블릿 명세 웹 패턴 보안, EJB Container Managed Security와 파일 시스템 보안에서 볼 수 있는 권한 부여 기능을 각각 생각해보라. 본 레퍼런스에서 살펴보겠지만, Spring Security는 이 모든 중요한 분야에서 심도있는 기능을 제공한다.


### 1.2 History
Spring Security began in late 2003 as "The Acegi Security System for Spring". A question was posed on the Spring Developers' mailing list asking whether there had been any consideration given to a Spring-based security implementation. At the time the Spring community was relatively small (especially compared with the size today!), and indeed Spring itself had only existed as a SourceForge project from early 2003. The response to the question was that it was a worthwhile area, although a lack of time currently prevented its exploration.

With that in mind, a simple security implementation was built and not released. A few weeks later another member of the Spring community inquired about security, and at the time this code was offered to them. Several other requests followed, and by January 2004 around twenty people were using the code. These pioneering users were joined by others who suggested a SourceForge project was in order, which was duly established in March 2004.

In those early days, the project didn't have any of its own authentication modules. Container Managed Security was relied upon for the authentication process, with Acegi Security instead focusing on authorization. This was suitable at first, but as more and more users requested additional container support, the fundamental limitation of container-specific authentication realm interfaces became clear. There was also a related issue of adding new JARs to the container's classpath, which was a common source of end user confusion and misconfiguration.

Acegi Security-specific authentication services were subsequently introduced. Around a year later, Acegi Security became an official Spring Framework subproject. The 1.0.0 final release was published in May 2006 - after more than two and a half years of active use in numerous production software projects and many hundreds of improvements and community contributions.

Acegi Security became an official Spring Portfolio project towards the end of 2007 and was rebranded as "Spring Security".

Today Spring Security enjoys a strong and active open source community. There are thousands of messages about Spring Security on the support forums. There is an active core of developers who work on the code itself and an active community which also regularly share patches and support their peers.



### 1.3 Release Numbering
It is useful to understand how Spring Security release numbers work, as it will help you identify the effort (or lack thereof) involved in migrating to future releases of the project. Each release uses a standard triplet of integers: MAJOR.MINOR.PATCH. The intent is that MAJOR versions are incompatible, large-scale upgrades of the API. MINOR versions should largely retain source and binary compatibility with older minor versions, thought there may be some design changes and incompatible updates. PATCH level should be perfectly compatible, forwards and backwards, with the possible exception of changes which are to fix bugs and defects.

The extent to which you are affected by changes will depend on how tightly integrated your code is. If you are doing a lot of customization you are more likely to be affected than if you are using a simple namespace configuration.

You should always test your application thoroughly before rolling out a new version.


### 1.4 Spring Security 불러오기

Spring Security를 불러오는 방법에는 몇가지가 있다. 메인 페이지 [Spring Security]( http://spring.io/spring-security) 에서 배포판 패키지를 다운로드받거나, Maven Central repository에서 개별 jars를(혹은 SpringSource Maven repository에서 snapshot과 milestone releases를)받거나, 소스에서 직접 빌드할 수 있다.

#### 1.4.1 maven을 이용한 방법

최소한의 메이븐용 코드는 다음과 같다:

<Strong>pom.xml</Strong>

```
<dependencies>
<!-- ... other dependency elements ... -->
<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-web</artifactId>
	<version>{spring-security-version}</version>
</dependency>
<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-config</artifactId>
	<version>{spring-security-version}</version>
</dependency>
</dependencies>
```

만약 LDAP, OpenID 등을 사용하고 있다면 적절한 [modules] 도 포함해야 한다.


###### Maven Repositories

모든 GA releases (.RELEASE로 끝나는 버전)는 Maven Central에 올라가 있다. 따라서, 다른 추가적인 메이븐 리파지토리가 pom에 선언될 필요는 없다.

SNAPSHOT 버전을 사용한다면, 아래와 같은 Spring Snapshot repository를 정의했는지 확인해야한다:

<Strong>pom.xml</Strong>

```
<repositories>
<!-- ... possibly other repository elements ... -->
<repository>
	<id>spring-snapshot</id>
	<name>Spring Snapshot Repository</name>
	<url>http://repo.springsource.org/snapshot</url>
</repository>
</repositories>
```

milestone 이나 release candidate 버전을 사용한다면, 아래와 같은 Spring Milestone repository를 정의했는지 확인해야한다:

<Strong>pom.xml</Strong>

```
<repositories>
<!-- ... possibly other repository elements ... -->
<repository>
	<id>spring-milestone</id>
	<name>Spring Milestone Repository</name>
	<url>http://repo.springsource.org/milestone</url>
</repository>
</repositories>
```


###### Spring Framework Bom

Spring Security는 스프링 프레임워크의 버전 4.1.6.RELEASE 와 달라도 빌드될 수 있다. 하지만 4.0.x 대의 버전이어야 한다. 많은 유저들이 이러한 Spring Security의 이행적 의존성(transitive dependencies)이 스프링 프레임워크 4.1.6.RELEASE에 녹아드는 것(resolve)으로 인해 이상한 클래스패스 문제에 빠진다.

이 문제를 피하는 하나의 (따분한) 방법은 pom에 모든 스프링 프레임워크 모듈 [[ dependencyManagement | http://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Dependency_Management]] 을 포함시키는 것이다. 다른 방법으로는 아래와 같이 `pom.xml`의 `<dependencyManagement>` 섹션에 `spring-framework-bom`를 포함시키는 것이다:

<Strong>pom.xml</Strong>

```
<dependencyManagement>
	<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-framework-bom</artifactId>
		<version>{spring-version}</version>
		<type>pom</type>
		<scope>import</scope>
	</dependency>
	</dependencies>
</dependencyManagement>
```

이 방법이 Spring Security의 모든 이행적 의존성이 Spring 4.1.6.RELEASE 모듈을 사용하도록 만들 것이다.

NOTE: 이 접근법은 메이븐의 "bill of materials" (BOM) 컨셉을 이용했고, 메이븐 2.0.9 이후 부터 가능한 방법이다. 의존성이 어떻게 녹아드는지에 대한 추가적인 정보는 [Maven's Introduction to the Dependency Mechanism documentation | http://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html ]를 참고하라.


#### 1.4.2 Gradle

최소한의 그래들용 의존성 코드는 다음과 같다:

<strong>build.gradle</strong>

```
dependencies {
	compile 'org.springframework.security:spring-security-web:{spring-security-version}'
	compile 'org.springframework.security:spring-security-config:{spring-security-version}'
}
```

만약 LDAP, OpenID 등을 사용한다면 적절한 [modules]도 포함시켜야한다.


###### Gradle Repositories

모든 GA releases (.RELEASE로 끝나는 버전)는 Maven Central에 올라가있으므로  mavenCentral() repository를 이용하는 것이면 충분하다.

<strong>.build.gradle</strong>

```
repositories {
	mavenCentral()
}
```

SNAPSHOT 버전을 사용한다면, 아래와 같이 정의된 Spring Snapshot repository가 있는지 확인해야한다:

<strong>.build.gradle</strong>

```
repositories {
	maven { url 'https://repo.spring.io/snapshot' }
}
```

milestone 이나 release candidate 버전을 사용한다면, 아래와 같은 Spring Milestone repository를 정의했는지 확인해야한다:

<strong>.build.gradle</strong>

```
repositories {
	maven { url 'https://repo.spring.io/milestone' }
}
```


###### Spring 4.0.x 과 Gradle

그래들은 transitive 버전을 변환할 때 디폴트로 최신의 버전을 사용한다. 즉, Spring Security 4.0.1.RELEASE 를 Spring Framework 4.1.6.RELEASE에서 사용할 때, 다른 추가적인 작업이 필요 없을 수도 있다는 뜻이다. 그러나 문제가 생길 수도 있으므로 아래와 같이 [[ Gradle's ResolutionStrategy | http://www.gradle.org/docs/current/dsl/org.gradle.api.artifacts.ResolutionStrategy.html ]]를 이용해서 사전에 방지하는 것이 좋겠다:

<strong>build.gradle</strong>

```
configurations.all {
	resolutionStrategy.eachDependency { DependencyResolveDetails details ->
		if (details.requested.group == 'org.springframework') {
			details.useVersion '{spring-version}'
		}
	}
}
```

이렇게 하면 Spring Security의 모든 transitive dependencies이 Spring 4.1.6.RELEASE 모듈을 사용하게 된다.

NOTE: 이 예제는 Gradle 1.9를 사용했는데, 이는 Gradle의 인큐베이팅 피쳐이므로 차후 버전에서는 수정이 필요할 수도 있다.

#### 1.4.3 Project Modules
Spring Security 3.0에서는 기능별로 분리하고 써드파티 의존성을 명확하게 구분하기 위해 코드베이스(codebase)가 별개의 jar파일로 나뉘어져있다. 만약, 당신의 프로젝트를 빌드할때 메이븐을 사용하고 있다면, 이것들이 `pom.xml`에 추가되어야 할 모듈이다. 메이븐을 사용하지 않고 있더라도 써드파티 의존성과 버전을 고려하여 `pom.xml`에 대한 조언을 구하기 바란다. 대안으로 예제 어플리케이션에 포함되어있는 라이브러리들을 살펴보는 것도 좋은 방법이다.


###### Core - spring-security-core.jar
인증, 접근제어를 위한 핵심 클래스와 인터페이스, remoting 지원, 기본 권한 설정 API(provisioning APIs)를 포함하고 있다. 이는 Spring Security를 사용하는 어떤 어플리케이션이든 필요한 것들이다. 독립적인 어플리케이션, 원격 클라이언트, 메서드(서비스 레이서) 보안 및 JDBC 유저 권한 설정도 지원한다. 최상위 패키지를 포함하고 있다:

* `org.springframework.security.core`

* `org.springframework.security.access`

* `org.springframework.security.authentication`

* `org.springframework.security.provisioning`





###### Remoting - spring-security-remoting.jar
Spring Remoting과 함께 통합(Integration)을 제공한다. Spring Remoting을 사용하는 원격 클라이언트를 작성하는 것이 아니라면 필요 없다. 메인 패키지는`org.springframework.security.remoting`.


###### Web - spring-security-web.jar
필터와 관련된 웹 보안 기반 코드가 포함되어있다. 서블릿 API 의존성과 관련된 것들이 들어있다. 웹 인증 서비스나 URL 베이스의 접근 제어가 필요하다면 이 jar를 써야한다. 메인 패키지는 `org.springframework.security.web`.


###### Config - spring-security-config.jar
보안 네임스페이스를 파싱하는 코드가 들어있다. 설정에 Spring Security XML 네임스페이스를 사용한다면 이 jar가 필요하다. 메인 패키지는 `org.springframework.security.config`. 어플리케이션에서 직접 사용하는 클래스는 없다.


###### LDAP - spring-security-ldap.jar
LDAP 인증과 권한 설정 코드를 포함하고 있다. LDAP 인증을 사용하거나 LDAP 유저 엔트리를 관리해야한다면 이 jar가 필요하다. 최상위 패키지는 `org.springframework.security.ldap`.


###### ACL - spring-security-acl.jar
전문화된 도메인 객체 ACL 구현체. 어플리케이션 내의 특정 도메인 객체 인스턴스에 보안을 적용하기 위해 사용된다. 최상위 패키지는 `org.springframework.security.acls`.


###### CAS - spring-security-cas.jar
Spring Security의 CAS 클라이언트 통합. 통합 인증 서버로 Spring Security 웹 인증을 한다면 필요하다. 최상위 패키지는 `org.springframework.security.cas`.


###### OpenID - spring-security-openid.jar
OpenID 웹 인증 지원. 외부 OpenId 서버에 있는 사용자들을 인증하기 위해 사용한다. `org.springframework.security.openid`. OpenID4Java가 필요하다.


#### 1.4.4 소스 체크하기
Spring Security는 오픈소스 프로젝트이기때문에, git을 이용하여 소스코드를 체크하기를 강력하게 권장한다. git을 이용하면 모든 예제 어플리케이션에 접근할 수 있고, 최신 버전의 프로젝트를 쉽게 빌드할 수 있다. 프로젝트의 소스를 가지고 있는 것은 디버깅에도 도움이 된다. Exception stack traces는 더이상 불투명한 블랙박스가 아니다. 문제를 일으키는 라인으로 직행하여 무슨 일이 일어난 것인지 확인할 수 있다. 소스는 프로젝트의 완전한 다큐먼트이고 실제로 어떻게 작동하는 지에 대해 가장 간단하게 알 수 있는 곳이 되어준다.

프로젝트의 소스를 다운받으려면 다음 git command를 이용하면된다:

```
git clone https://github.com/spring-projects/spring-security.git
```

이 명령어가 당신의 로컬 머신이 프로젝트 전체 히스토리(모든 배포 버전과 브랜치를 포함한)에 접근할 수 있게 해준다.

<sub> translated by 박소은(@skyhills13) </sub>