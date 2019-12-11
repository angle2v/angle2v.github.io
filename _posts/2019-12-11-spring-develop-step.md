---
layout: post
title: SpringBoot 환경 셋팅
description: 
tags: [ Spring, SpringBoot, 스프링부트]
image:
background: triangular.png
---
<br>
<br>
<br>
# Spring Boot 소개

# 0.SpringBoot 란?
> Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run". We take an opinionated view of the Spring platform and third-party libraries so you can get started with minimum fuss. Most Spring Boot applications need very little Spring configuration.

* 스프링 부트는 단독실행되는, 실행하기만 하면 되는 상용화 가능한 수준의, 스프링 기반 애플리케이션을 쉽게 만들어낼 수 있다.
* 최소한의 설정으로 스프링 플랫폼과 서드파티 라이브러리들을 사용할 수 있도록 하고 있다.

> 스프링 기반의 애플리케이션을 개발하기 쉽도록 기본설정되어 있는 설정을 기반으로 해서 빠르게 개발할 수 있도록 해주는 개발플랫폼이랄까?

# 0.1. Spring Boot 기능
* Create stand-alone Spring applications
> 단독실행가능한 스프링애플리케이션을 생성한다.,

* Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)
> 내장형 톰캣, 제티 혹은 언더토우를 내장(```WAR``` 파일로 배포할 경우에는 필요없음)

* Provide opinionated 'starter' component to simplify your build configuration
> 기본설정되어 있는 'starter' 컴포넌트들을 쉽게 추가

* Automatically configure Spring whenever possible
> 가능한 자동설정되어 있음

* Provide production-ready features such as metrics, health checks and externalized configuration
> 상용화에 필요한 통계, 상태 점검 및 외부설정을 제공

* Absolutely no code generation and no requirement for XML configuration
> 설정을 위한 XML 코드를 생성하거나 요구하지 않음

***

# 1. Spring Boot 시작하기

## 1.1. Spring Boot 프로젝트 생성
* 주의사항
	- **네트워크**가 연결되어 있어야 한다.
	> 그렇다면, 네트워크가 연결되지 않은 인트라넷 환경에서는 어떻게 해야할까?
		= [넥서스Nexus 에 Spring Boot 설정](http://stackoverflow.com/questions/29627505/artifact-not-found-during-install-task-with-gradle-spring-boot-plugin-and-nexus)을 해야겠지요? 사실, 안해봐서 모르겠음. @_@);;

	- **Maven** 혹은 **Gradle** 플러그인이 IDE에 설치되어 있어야 한다.

## 1.2. <http://start.spring.io/> 에서 생성하기
### 1.2.1. 프로젝트 메타데이터를 등록
* Maven 보다는 Gradle
	- Maven 예제가 많은 편이지만, Maven의 골, 페이즈만으로는 프로젝트의 필요한 기능을 모두 지원하지 못할 수도 있음
	- Gradle은 Groovy DSL로 구성되어 있어서 그루비를 익혀야하지만, 지원되는 기능을 익히고 나면 훨씬 강력해짐
* 배포형태에 따라서 war 또는 jar
	- 기본적으로는 단독실행가능지만, 프로젝트 환경에 따라 배포할 수도 있으니 war도 가능

### 1.2.2. [Generate Project] 버튼클릭
* 'artifact' 이름으로 된 zip 파일 다운로드

### 1.2.3. IDE에서 Import Project
* Maven 혹은 Gradle 플러그인 설치 되어 있어야 함

## 1.3. STS에서 생성하기
### 1.3.1. ```[File]-[New]-[Spring Starter project]``` 선택
### 1.3.2. 사용하려는 스프링 starter 선택
* 최초에는 필요한 라이브러리들을 다운로드 받는데 상단한 시간이 소요된다.

## 1.4. Spring Boot 프로젝트 실행
* ```[Run as]``` - ```[Spring Boot App]``` 선택
* ```[gradle] - [Tasks quick launcher]``` 을 이용해서 실행
	- 프로젝트 지정된 JDK 버전과 IDE 실행 JDK 버전이 다르면...
* 프로젝트 생성 위치에서 ```$ gradle bootRun``` 명령 실행

***

# 2. Spring Boot 구성 살펴보기
## 2.1. 최초 생성된 Spring Boot 프로젝트 살펴보기
### 2.1.1. jar 프로젝트
* ```build.gradle```

```
//코드 생략
apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'idea'
apply plugin: 'spring-boot'
apply plugin: 'io.spring.dependency-management'

jar {
    baseName = 'demo'
    version = '0.0.1-SNAPSHOT'
}
//코드생략
dependencies {
    compile("org.springframework.boot:spring-boot-starter-web")
    testCompile("org.springframework.boot:spring-boot-starter-test")
}
```

* ```{artifact-name}Application.java```
* IDE에서 Java Application project로 인식
* ```build``` 태스크 실행시 ```demo.jar``` 생성
* 실행

```
$ java -jar demo.jar
```

### 2.1.2. war 프로젝트
* ```build.gradle```

```
//코드생략
apply plugin: 'java'
apply plugin: 'eclipse-wtp'
apply plugin: 'idea'
apply plugin: 'spring-boot'
apply plugin: 'io.spring.dependency-management'
apply plugin: 'war'

war {
    baseName = 'demowar'
    version = '0.0.1-SNAPSHOT'
}
//코드생략
configurations {
    providedRuntime
}

dependencies {
    compile("org.springframework.boot:spring-boot-starter-web")
    providedRuntime("org.springframework.boot:spring-boot-starter-tomcat")
    testCompile("org.springframework.boot:spring-boot-starter-test")
}
```

* ```public class ServletInitializer extends SpringBootServletInitializer``` 클래스가 있음
* IDE에서 웹프로젝트로 인식
* ```build``` 태스크 실행시 ```demowar.war``` 생성
* 실행
	- <https://spring.io/guides/gs/convert-jar-to-war/>
```
$ java -jar demowar.war
```

### 2.1.3. Excutable JAR
* 자세히 알고 싶다면 <http://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#executable-jar> 를 보고 알려주세요...!

## 2.2. ```@SpringBootApplication```
* 코드

```
/**
 * Indicates a {@link Configuration configuration} class that declares one or more
 * {@link Bean @Bean} methods and also triggers {@link EnableAutoConfiguration
 * auto-configuration} and {@link ComponentScan component scanning}. This is a convenience
 * annotation that is equivalent to declaring {@code @Configuration},
 * {@code @EnableAutoConfiguration} and {@code @ComponentScan}.
 *
 * @author Phillip Webb
 * @since 1.2.0
 */
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@Configuration
@EnableAutoConfiguration
@ComponentScan
public @interface SpringBootApplication {

	/**
	 * Exclude specific auto-configuration classes such that they will never be applied.
	 * @return the classes to exclude
	 */
	Class<?>[] exclude() default {};

}
```

* ```@SpringBootApplication``` 애노테이션이 선언된 애플리케이션 클래스는 패키지 루트에 위치하는 것이 좋다.
	- 물론 ```@ComponentScan```에 별도의 패키지를 지정할 수 있지만 굳이~ 그렇게 하지 맙시다.
	- [Locating the main application class](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-structuring-your-code) 의 기본적인 구조를 따르는 걸 권장하고 싶음
	```
	com
	 +- example
		   +- myproject
		       +- Application.java
		       |
		       +- domain
		       |   +- Customer.java
		       |   +- CustomerRepository.java
		       |
		       +- service
		       |   +- CustomerService.java
		       |
		       +- web
		           +- CustomerController.java
	```

***

# 3. Spring Boot 설정
## 3.1. Spring Boot 자동설정AutoConfig
### 3.1.1. autoconfigure 패키지 확인
* [spring-boot-autoconfigure](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-project/spring-boot-autoconfigure/src/main/java/org/springframework/boot/autoconfigure) 살펴보기

### 3.1.2. ```@Conditional, @ConditionalOnBean, @ConditionalOnMissingBean, @ConditionalOnClass``` 조건에 따라 스프링 빈Bean 으로 등록
* ```@ConditionalOnBean```
* ```@ConditionalOnMissingBean```
* ```@ConditionalOnClass```

## 3.2. 외부설정 하기
* [Common Application properties](http://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#common-application-properties)
* 스프링부트의 프로퍼티스 확인순서에 따라서 외부요인들을 읽어오게 됨
* Environment

### 3.2.1. ```application.properties```
* PropertySource

### 3.2.2. ```application.yml```
* YamlPropertySourceLoader

## 3.3. 프로파일즈 활용하기
* 로컬, 개발, 테스트, 운영 설정을 각각 관리 및 적용
* [Profiles](http://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-profiles) - springframework
* 환경별 설정요소를 한곳에서 집중하여 관리할 수 있음

### 3.3.1. 기존 방식
* 설정디렉토리를 분리
	- config/local
	- config/development
	- config/test
	- config/production
* 빌드시 프로파일을 지정하여 지정한 설정디렉토리의 설정파일을 적용하는 형식을 취함

### 3.3.2. application.properties
* application-{profiles}.properties
	- application-local.properties
	- application-development.properties
	- application-test.properties
	- application-production.properties

### 3.3.3. application.yml
* application.yml

```
# 공통설정부분 지정가능
---
spring:
	profiles: local
---
spring:
  profiles: development
---
spring:
	profiles: test
---
spring:
  profiles: production
```

***

# 4. Spring Boot 확장하기
## 4.1. starter POMs 추가하기
* [Starer POMs](http://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#using-boot-starter-poms)
* 추가시 spring.provides 에 연관된 스프링 라이브러리들이 정의되어 있다.
* 관련 의존성 라이브러리에 대해서는 starter 프로젝트 내부에 있는 pom.xml 에 정의되어 있음

## 4.2. 의존성 라이브러리 추가하기
### 4.2.1. 의존성 버전
* [dependency-versions](http://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#appendix-dependency-versions)
	- 목록에 있는 의존성 라이브러리들은 버전을 정의하지 않으면, 기본지정된 버전이 적용됨
	- 별도의 버전을 지정할 수 있음

### 4.2.2. 의존성 라이브러리 검색
* <http://search.maven.org/>
* <http://mvnrepository.com/>

***

# ● 개인적 목표
* JDK 8 이상 사용
* Gradle 사용
* SpringBoot 적용
	- 스프링 프레임워크 4.0 이상 적용
	- Spring Data JPA 사용
