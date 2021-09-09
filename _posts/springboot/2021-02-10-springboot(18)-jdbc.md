---
title: Spring boot기반 Web Application 개발[18] - 순수 JDBC
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---

 이전 포스팅에서 `H2 데이터베이스` 를 설치했다. 이제 순수 `JDBC`를 활용해서 데이터베이스를 연동해보자. 참고로 `JDBC`는 20년전에나 활발하게 사용되던 기술이라고한다. 다음 포스팅의 통합 테스팅을 위한 사전작업 및 참고용으로만 확인하면 좋을 것 같다.

# 순수 JDBC

## 1. 환경설정

### build.gradle 설정

 ![image](https://user-images.githubusercontent.com/49560745/107455580-6f33e000-6b92-11eb-8975-df10c40afd28.png)

`build.gradle` 파일에 아래 코드를 추가한다.

```java
implementation 'org.springframework.boot:spring-boot-starter-jdbc'
runtimeOnly 'com.h2database:h2'
```

- `jdbc`, `h2` 데이터베이스 관련 라이브러리를 추가한다.
- `dependencies`에 설정해주자.

```
파일명 : build.gradle
```

```java
plugins {
	id 'org.springframework.boot' version '2.3.6.RELEASE'
	id 'io.spring.dependency-management' version '1.0.10.RELEASE'
	id 'java'
}

group = 'hello'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.springframework.boot:spring-boot-starter-jdbc'
	runtimeOnly 'com.h2database:h2'
	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
	}
}

test {
	useJUnitPlatform()
}
```

### application.properties 설정

![image](https://user-images.githubusercontent.com/49560745/107456106-f5e8bd00-6b92-11eb-96c2-d40b6b5cef5b.png)

`application.properties` 파일에 아래 코드를 추가하자.

```java
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
```

- 스프링부트 데이터베이스 연결 설정을 추가한다.
- 스프링부트 `2.4` 부터는 `spring.datasource.username=sa`를 꼭 추가해야한다.
  - 그렇지 않으면, `Wrong user name or password` 오류가 발생한다.

```
파일명 : application.properties
```

```java
server.port=8000
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
```

## 2.  JDBC 회원 리포지터리

 `JDBC`를 사용하기 위한 기본 설정을 마쳤다. 이제, 기존의 저장소로 활용하던 `HashMap` 리포지터리를 `JDBC` 리포지터리로 변경하는 작업을 진행해보자.

![image](https://user-images.githubusercontent.com/49560745/107458934-6d6d1b00-6b98-11eb-9bcc-b0bb5652e8f4.png)

```
파일명 : JdbcMemberRepository.java
위치 : /hello.hellospring/repository/JdbcMemberRepository.java
```

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.simple.SimpleJdbcInsert;
import org.springframework.jdbc.datasource.DataSourceUtils;

import javax.sql.DataSource;
import java.sql.*;
import java.util.*;

public class JdbcMemberRepository implements MemberRepository {

    private final DataSource dataSource;
    public JdbcTemplateMemberRepository(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @Override
    public Member save(Member member) {
        String sql = "insert into member(name) values(?)";
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql,
                    Statement.RETURN_GENERATED_KEYS);
            pstmt.setString(1, member.getName());
            pstmt.executeUpdate();
            rs = pstmt.getGeneratedKeys();
            if (rs.next()) {
                member.setId(rs.getLong(1));
            } else {
                throw new SQLException("id 조회 실패");
            }
            return member;
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }

    @Override
    public Optional<Member> findID(long id) {
        String sql = "select * from member where id = ?";
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql);
            pstmt.setLong(1, id);
            rs = pstmt.executeQuery();
            if(rs.next()) {
                Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                return Optional.of(member);
            } else {
                return Optional.empty();
            }
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }

    @Override
    public List<Member> findAll() {
        String sql = "select * from member";
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql);
            rs = pstmt.executeQuery();
            List<Member> members = new ArrayList<>();
            while(rs.next()) {
                Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                members.add(member);
            }
            return members;
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }

    @Override
    public Optional<Member> findName(String name) {
        String sql = "select * from member where name = ?";
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql);
            pstmt.setString(1, name);
            rs = pstmt.executeQuery();
            if(rs.next()) {
                Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                return Optional.of(member);
            }
            return Optional.empty();
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }
    private Connection getConnection() {
        return DataSourceUtils.getConnection(dataSource);
    }
    private void close(Connection conn, PreparedStatement pstmt, ResultSet rs)
    {
        try {
            if (rs != null) {
                rs.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if (pstmt != null) {
                pstmt.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if (conn != null) {
                close(conn);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    private void close(Connection conn) throws SQLException {
        DataSourceUtils.releaseConnection(conn, dataSource);
    }
}
```

- `conn = getConnection()` : `DB` 연동을 위한 커넥션 객체를 생성한다.
- `PreparedStatement pstmt` : `SQL` 구문을 실행하기 위한 객체이다.
- `ResultSet rs` : `SQL` 실행 결과를 받아오는 객체이다.
-  `pstmt.executeQuery()` : `SQL` 쿼리를 실행한다.

## 3. 스프링 설정 변경

`JDBC` 회원 리포지터리 구현이 완료되었으면, 스프링의 설정을 변경해줘야한다.

![image](https://user-images.githubusercontent.com/49560745/107458892-5af2e180-6b98-11eb-9e5b-8a9492b07047.png)

```
파일명 : SpringConfig.java
위치 : /hello.hellospring/service/SpringConfig.java
```

```java
package hello.hellospring;

import hello.hellospring.repository.JdbcMemberRepository;
import hello.hellospring.repository.MemberRepository;
import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

@Configuration
public class SpringConfig {

    private final DataSource dataSource;

    @Autowired
    public SpringConfig(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }
    @Bean
    public MemberRepository memberRepository() {
        // return new MemoryMemberRepository();
        return new JdbcMemberRepository(dataSource);
    }
}
```

- `DataSource`는 데이터베이스 커넥션을 얻기 위해 사용하는 객체다. 
- 스프링 부트에서는 데이터베이스 커넥션 정보를 바탕으로 `DataSource`를 생성하고, 스프링 빈으로 만들어둔다. 그래서 `DI`를 받을 수 있다.
- `memberRepository` 메소드의 주석문을 확인해보면, 저장소를 단순히 `MemoryMemberRepository()` 에서 `JdbcMemberRepository()` 로 변경해줬다.
  - 이게바로 객체지향의 장점이라고 할 수 있다. 기존의 코드 변경없이 부품만 갈아끼워주면 수정이 완료된다.

## 4. 테스트

이제, `JDBC`를 저장소로 애플리케이션에 연동이 끝났다.  `localhost:8000` 에 접속해서, 정상적으로 동작하는지 확인해보자. 

![image](https://user-images.githubusercontent.com/49560745/107457372-495c0a80-6b95-11eb-92ca-8fb85cd58d9d.png)

`회원가입` 을 누르고 이름을  `JDBC 완료!`로 설정해 등록해보자. 그리고 `회원 목록` 을 확인하면 정상적으로 회원목록이 조회된다.

![image](https://user-images.githubusercontent.com/49560745/107457498-80cab700-6b95-11eb-862b-5474f56d9f93.png)

## 5. 구현 원리

### 구조

![image](https://user-images.githubusercontent.com/49560745/107457984-85dc3600-6b96-11eb-9180-68e5a641ccf0.png)

- `MeberService`는 `MemberRepository`를 참조하고 있다.

- `MemoryMemberRepository` 와 `JdbcMemberRepository`는 인터페이스 `MemberRepository`의 구현체이다.

  

![image](https://user-images.githubusercontent.com/49560745/107458273-19ae0200-6b97-11eb-9092-0ddd260ac9d5.png)

- `Jdbc` 를 통해 `DB`를 연동한 스프링컨테이너의 상태는 위와 같다.
- `<memory>` 리파지토리를 단순히 `<jdbc>` 용 리파지토리로 변경해줬다.
  - 앞서 말했듯이 `SpringConfig`에서 설정만 변경해주면 자유롭게 객체를 갈아끼울 수 있다.
  - 즉, 스프링의 `DI (Dependencies Injection)`을 사용하면 기존 코드를 전혀 손대지 않고, 설정만으로 구현 클래스를 변경할 수 있다.

<br/>

 순수 `JDBC`를 활용해 `H2-Database`와 애플리케이션 연동을 마쳤다. 현 시점에서  `jdbc` 를 이용하는 것이 얼마나 비효율적인지 과거 개발자 선배들이 순수 `jdbc`로 개발할 때, 얼마나 고생했을지 정도만 생각하고, 넘어가도 충분할 것 같다. 다음 포스팅부터는 `DB` 까지 연동한 애플리케이션 통합테스팅을 진행하고, `JDBC Template` , `JPA` 를 연동해보자.

<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49577?tab=curriculum)

