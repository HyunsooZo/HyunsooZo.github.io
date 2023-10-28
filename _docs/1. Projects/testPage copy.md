---
title: Spring Transaction
category: TIL
order: 4
---

### Transaction이란?

<div class="content-box">
데이터베이스의 상태를 변환시키는 하나의 논리적 기능을 수행하기 위한 작업의 단위 또는 한꺼번에 모두 수행되어야 할 일련의 연산들을 의미한다.
</div>

Transaction의 기본 로직은 
2개 이상의 쿼리를 하나의 커넥션으로 묶어 DB에 전송하고, 이 과정에서 에러가 발생할 경우 자동으로 모든 과정을 원래대로 되돌려 놓는다. 
이러한 과정을 구현하기 위해 Transaction은 하나 이상의 쿼리를 처리할 때 동일한 Connection 객체를 공유한다.

### Spring Transaction
Spring은 코드 기반의 트랜잭션(Programmatic Transaction) 처리 뿐만 아니라 선언적 트랜잭션(Declarative Transaction)을 지원한다. 

Spring이 제공하는 트랜잭션 템플릿 클래스를 이용하거나 설정파일, 어노테이션을 이용해서 트랜잭션의 범위 및 규칙을 정의할 수 있다. 

Spring에서는 주로 선언적 트랜잭션을 이용하는데, <tx:advice>태그 또는 `@Transactional` 어노테이션을 이용하고 이는 Spring AOP로 손쉽게 구현되어 있다. 

퀴리문을 처리하는 과정에서 에러가 났을 경우 자동으로 Rollback 처리한다.



 |Spring Transaction의 세부설정||
 |--|--|
 |Isolation(격리수준)|트랜잭션에서 일관성이 없는 데이터를 허용하는수준<br>-DEFAULT<br>-READ_UNCOMMITTED(dirty read 발생)<br> -READ_COMMITTED (dirty read 방지)<br>-REPEATABLE_READ(Non-Repeatable Read 방지)<br>-SERIALIZABLE (Phantom Read 방지)<br>예시:`@Transactional(isolation=Isolation.DEFAULT)`|
 |Propagation(전파수준)|트랜잭션 동장 도중 다른트랜잭션을 호출하는 상황<br>트랜잭션을 시작하거나<br> 기존 트랜잭션에 참여하는 방법에 대해 결정하는 설정값.<br>-REQUIRED<br>-SUPPORTS(시작된TRX가 있으면 그안에서 자식TRX포함)<br>-REQUIRES_NEW(무조건 새로운 TRX생성)<br>-NESTED(중첩TRX 실행-부모TRX와 독립적으로..)|
 |ReadOnly|트랜잭션을 읽기 전용 속성으로 지정<br>예시:`Transactional(readOnly=true)`|
 |Rollback Exception|예외가 발생했을때 트랜잭션 롤백 시킬경우를 설정<br>`@Transactional(isolation=rollbackFor=Exception.class)`<br>`@Transactional(isolation=noRollbackFor=Exception.class)`|
 |Timeout|일정시간 내에 트랜잭션 끝내지 못하면 롤백<br>`@Transactional(timeout=10)`|


