---
title: QueryDSL
category: TIL
order: 6
---
### QueryDSL 이란?

<div class="content-box">
<li>A쿼리 DSL은 데이터베이스 쿼리를 프로그래밍 언어의 문법을 사용하여 생성하는 도메인 특화 언어이다.</li>

<li>주로 ORM (Object-Relational Mapping) 프레임워크와 함께 사용되어 데이터베이스와 상호작용을 단순화하고 코드 안정성을 제공한다.</li>

<li>Java와 같은 프로그래밍 언어로 쿼리를 작성하며, 일반적으로 컴파일 시에 SQL 쿼리로 변환된다.</li>

<li>쿼리 DSL은 SQL 쿼리를 문자열로 작성하는 것보다 가독성이 높고 런타임 오류를 줄여준다.</li>

<li>대표적인 쿼리 DSL로 QueryDSL이 있으며, Java 개발자들이 주로 활용한다.</li>
</div>

### QueryDSL과 JPA

| 특성                     | JPA                        | 쿼리 DSL                   |
|------------------------|-----------------------------|---------------------------|
| 목적                     | 객체-관계 매핑 및 관리        | 데이터베이스 쿼리 작성 및 관리 |
| 쿼리 작성 방식           | 객체 지향 쿼리 (JPQL) 또는 Criteria API 사용 | 프로그래밍 언어 문법 사용하여 쿼리 작성 |
| 가독성 및 유지 보수성   | SQL과 유사한 JPQL 또는 Criteria API는 가독성이 떨어질 수 있음 | 프로그래밍 언어 문법을 사용하여 가독성이 좋음 |
| SQL 오류 검출           | 런타임 SQL 오류 가능성 있음    | 컴파일 시 SQL 오류 검출    |
| 유형 안정성              | 낮음 (문자열 기반 쿼리)      | 높음 (컴파일 시 타입 검출)    |
| 성능 최적화             | SQL을 최적화하기 어려울 수 있음 | 쿼리 작성 및 최적화 용이   |
| 프로그래밍 언어 지원    | Java, Kotlin, 등            | Java 등                   |

### QueryDSL 설정

##### build.gradle 설정
```
dependencies {
	... 
	implementation 'com.querydsl:querydsl-jpa'
	implementation 'com.querydsl:querydsl-apt'

	annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jpa"
	annotationProcessor 'jakarta.persistence:jakarta.persistence-api'
	annotationProcessor 'jakarta.annotation:jakarta.annotation-api'
}

def querydslSrcDir = 'src/main/generated'
sourceSets {
  main {
    java {
      srcDirs += [ querydslSrcDir ]
    }
  }
}

compileJava {
    options.compilerArgs << '-Aquerydsl.generatedAnnotationClass=javax.annotation.Generated'
}

tasks.withType(JavaCompile) {
	options.generatedSourceOutputDirectory = file(querydslSrcDir)
}

clean {
  delete file(querydslSrcDir)
}
```
##### 라이브러리 의존성 추가
`implementation 'com.querydsl:querydsl-jpa'` : QueryDSL 을 사용하기 위한 라이브러리<br>
`implementation 'com.querydsl:querydsl-apt'` : QClass 를 생성하기 위한 라이브러리<br>
`annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jpa"`: `@Entity` 어노테이션을 선언한 클래스를 탐색하고, `Q` 클래스를 생성<br>

##### Source Set

```
def querydslSrcDir = 'src/main/generated'
sourceSets {
  main {
    java {
      srcDirs += [ querydslSrcDir ]
    }
  }
}
```
gradle build 시 QClass 소스도 함께 build 하기 위해서 sourceSets 에 해당 위치를 추가

##### compileJava

```
compileJava {
    options.compilerArgs << '-Aquerydsl.generatedAnnotationClass=javax.annotation.Generated'
}
```
해당 내용을 명시해주지 않으면 Q 파일 내 Generated 를 import 할 때 자바 9 에만 있는 import javax.annotation.processing.Generated 로 import 해준다.

그렇기 때문에 다른 버전에서도 사용할 수 있도록 java.annotation.Generated 로 import 하도록 설정하는 코드

##### tasks.withType
```
tasks.withType(JavaCompile) {
	options.generatedSourceOutputDirectory = file(querydslSrcDir)
}
```
annotation processors 에서 생성한 소스 파일을 저장할 디렉토리를 지정 해준다. (Gradle 공식문서 → [CompileOptions - Gradle DSL Version 7.5.1](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.compile.CompileOptions.html#org.gradle.api.tasks.compile.CompileOptions:generatedSourceOutputDirectory) )

이 코드를 통해 위에서 선언한 querydslSrcDir 변수의 src/main/generated 에다가 annotation processors 가 만든 QClass 들을 저장

##### Clean
```
clean {
	// clean 실행 시 생성된 QClass 삭제
	delete file(querydslSrcDir)
}
```
build clean 시에 생성되었던 QClass 를 모두 삭제 (querydslSrcDir = src/main/generated)

##### Configuration
```java
@Configuration
public class QuerydslConfig {
    @PersistenceContext
    private EntityManager entityManager;

    @Bean
    public JPAQueryFactory jpaQueryFactory() {
        return new JPAQueryFactory(entityManager);
    }
}
```

##### Repository

```java
public interface RestaurantRepositoryCustom {
    Optional<Restaurant> findByUserSeq(Long userSeq);
}
```

```java
@Repository
@RequiredArgsConstructor
public class ContentRepositoryImpl implements ContentRepositoryCustom {
    private final JPAQueryFactory queryFactory;

    @Override
    public Content findByUserSeq(Long userSeq) {
        return queryFactory
                .selectFrom(content)
                .where(content.userSeq.eq(userSeq))
                .fetchFirst();
    }
}
```

```java
public interface ContentRepository extends JpaRepository<Content, Long>, ContentRepositoryCustom {
}
```

### QueryDSL 문법

| 문법 요소                   | QueryDSL 문법 예제                                             | SQL 쿼리 예제                                |
|-----------------------------|---------------------------------------------------------------|----------------------------------------------|
| 엔티티 및 Q 클래스 생성     | QPerson person = QPerson.person;                             | -                                            |
| from()                      | JPAQuery<Person> query = new JPAQuery<>(entityManager);      | SELECT * FROM person                        |
| select()                    | query.select(person.name, person.age);                       | SELECT name, age FROM person               |
| where()                     | query.where(person.age.gt(18).and(person.city.eq("New York"))); | WHERE age > 18 AND city = 'New York'        |
| orderBy()                   | query.orderBy(person.age.asc());                              | ORDER BY age ASC                            |
| join()                      | query.innerJoin(person.address, address).on(address.city.eq("New York")); | INNER JOIN address ON address.city = 'New York' |
| groupBy()                   | query.groupBy(person.gender);                                  | GROUP BY gender                             |
| having()                    | query.having(person.age.avg().gt(25));                       | HAVING AVG(age) > 25                        |
| limit()                     | query.limit(10);                                              | LIMIT 10                                    |
| offset()                    | query.offset(20);                                             | OFFSET 20                                   |
| fetch()                     | List<Person> result = query.fetch();                         | -                                            |
| fetchFirst()                | Person person = query.fetchFirst();                          | -                                            |
| fetchCount()                | long count = query.fetchCount();                              | -                                            |


출처 : [그래들 공식문서](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.compile.CompileOptions.html#org.gradle.api.tasks.compile.CompileOptions:generatedSourceOutputDirectory), [블로그](https://velog.io/@soyeon207/QueryDSL-Spring-Boot-%EC%97%90%EC%84%9C-QueryDSL-JPA-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)




