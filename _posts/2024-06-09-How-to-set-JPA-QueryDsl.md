---
title: '스프링부트3.0 Querydsl 세팅과 실행'
excerpt: 'Querydsl 세팅과 gradle 빌드, test를 다룹니다.'

published: true

categories: jpa
tags: jpa

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: 'docs'
---

# Querydsl 란?

스프링 프로젝트에서 JPA의 동적 쿼리를 지원해주는 라이브러리 입니다.

# setting

STS 툴에서 세팅 하는데 도움이 많이 된 블로그입니다.  
[https://velog.io/@juhyeon1114/Spring-QueryDsl-gradle-%EC%84%A4%EC%A0%95-Spring-boot-3.0-%EC%9D%B4%EC%83%81#-%EC%B0%B8%EA%B3%A0](https://velog.io/@juhyeon1114/Spring-QueryDsl-gradle-%EC%84%A4%EC%A0%95-Spring-boot-3.0-%EC%9D%B4%EC%83%81#-%EC%B0%B8%EA%B3%A0)

## build.gradle

위의 블로그를 참고하여 작성한 정상 작동 되는 코드 입니다.

```gradle

plugins {
	id 'java'
	id 'org.springframework.boot' version '3.2.6'
	id 'io.spring.dependency-management' version '1.1.5'
}

group = 'org.zerock'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'

java {
	toolchain {
		languageVersion = JavaLanguageVersion.of(17)
	}
}

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	compileOnly 'org.projectlombok:lombok'
	developmentOnly 'org.springframework.boot:spring-boot-devtools'
	runtimeOnly 'org.mariadb.jdbc:mariadb-java-client'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
	implementation 'com.querydsl:querydsl-jpa:5.0.0:jakarta'

	annotationProcessor "com.querydsl:querydsl-apt:5.0.0:jakarta"
	annotationProcessor "jakarta.annotation:jakarta.annotation-api"
	annotationProcessor "jakarta.persistence:jakarta.persistence-api"
}

tasks.named('test') {
	useJUnitPlatform()
}

def querydslDir = "$buildDir/generated/querydsl"

sourceSets {
	main.java.srcDirs += [ querydslDir ]
}

tasks.withType(JavaCompile) {
	options.annotationProcessorGeneratedSourcesDirectory = file(querydslDir)
}

clean.doLast {
	file(querydslDir).deleteDir()
}
```

## gradle build

![](/images/2024-06-09/2024-06-09-17-09-17.png)  
_build.gradle 파일을 수정했다면, 항상 새로 빌드해 줍니다._

![](/images/2024-06-09/2024-06-09-17-12-51.png)  
_Gradle Tasks/build/Run Gradle Tasks를 클릭하여 빌드합니다._

![](/images/2024-06-09/2024-06-09-17-15-46.png)  
_정상적으로 'Q + 객체명' 의 java 파일이 만들어 졌습니다._

![](/images/2024-06-09/2024-06-09-17-17-50.png)  
_queryDsl을 사용하여 where 조건을 추가 해보았습니다._

---

여기까지 QueryDsl 세팅과 gradle build, 간단한 querydsql 실행까지 다뤄봤습니다.
