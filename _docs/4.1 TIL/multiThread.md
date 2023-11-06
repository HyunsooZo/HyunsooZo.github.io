---
title: 멀티쓰레드환경과 동시성 제어
category: TIL
order: 8
---
### Process란?
<div class="content-box">
<li>특정 작업을 수행하는 소프트웨어를 프로그램이라고 한다</li>
<li>이러한 프로그램이 실제로 실행되어, 메모리나 CPU와 같은 자원을 할당받게되면 이를 프로세스라고 한다.</li>
<li>프로세스는 독자적인 메모리를 할당받아 다른 프로세스끼리는 일반적으로 서로의 메모리 영역을 침범하지 못한다.</li>
</div>

### Thread란?

<div class="content-box">
<li>스레드는 프로세스를 구성하는 하나의 단위이다.</li>
<li>스레드는 서로 같은 프로세스 내부에 존재하므로 같은 자원을 공유하여 사용할 수 있다.</li>
<li>같은 자원을 공유하기 떄문에 동시성 이슈가 발생할 수 있고 이는 곳 병렬성의 향상으로 이어진다.</li>
</div>

### Thread Safe?

<div class="content-box">
<li><span class="emphasis">Thread safe</span> 는 멀티 스레드 환경에서 여러 스레드가 동시에 접근해도 안전하게 동작하는 소프트웨어 또는 코드를 의미. </li>
<li>주로 공유 데이터 및 리소스에 대한 안전성과 데이터 무결성을 보장하는 것을 말한다</li>
</div>

### Thread Safe가 깨지는 상황

- 예를 들어 특정 글의 좋아요기능을 여러 사용자가 동시에 접근하는 멀티스레드 환경이라고 가정해보면 아래와 같은 동시성 이슈가 발생한다.

![동시성이슈](https://drive.google.com/uc?id=15a1Ut3j47YUQr6Flks2LTJOVDKxgTfox)

### 동시성 제어 방법

- 그렇다면 아래와 같이 동시성을 제어하는 방법은 없을까?

![동시성제어](https://drive.google.com/uc?id=1KXKAUFhscteB42s33fzLAigA4giHB_JJ)

##### 암시적 Lock

- 가장 간단하면서 쉬운 방법은 Lock을 걸어 버리는 것이다.
- Lock을 적용하게 되면 하나의 스레드가 해당 메서드를 실행하고 있을 때 다른 메서드가 해당 메서드를 실행하지 못하고 대기하게 된다. 
- 즉 한 번에 하나의 스레드만 접근할 수 있게 되고 여러 스레드가 동시에 접근할 수 없게 만들어 동시성 이슈를 막을 수 있게 만들지만, 한 번에 하나의 스레드만 메서드를 실행시킬 수 있으므로 병렬성은 매우 낮아지게 된다.

- 문제가 된 `like` 메서드에 `synchronized` 키워드를 붙이면 암시적 락이 걸리게 된다.

- `lock`은 메서드, 변수에 각각 걸 수 있다. 메서드에 `lock`을 걸 경우 해당 메서드에 진입하는 스레드는 단 하나만 가능하다.<br> 변수에 `lock`을 걸 경우 해당 변수를 단 하나의 스레드만 참조할 수 있다. <br>
단, 변수에 `lock`을 걸기 위해선 해당 변수는 객체여야만 한다. `int`, `long`과 같은 기본형 타입에는 `lock`을 걸 수 없다.

**메서드 Lock**
```java
class Count {
    private int count;
    public synchronized int like() {return count++;}
}
```
 

**변수 Lock**
```java
class Count {
    private Integer count = 0;
    public int like() {
        synchronized (this.count) {
            return count++;
        }
    }
}
```

##### 명시적 Lock

`synchronized` 키워드 없이 명시적으로 `ReentrantLock`을 사용하는 `Lock`을 명시적 `Lock`이라고 한다.<br> 
해당 `Lock`의 범위를 메서드 내부에서 한정하기 어렵거나,<br> 
동시에 여러 `Lock`을 사용하고 싶을 때 사용한다.<br>
직접적으로 `Lock` 객체를 생성하여 사용할 수 있다. <br>
`lock()` 메서드를 사용할 경우 다른 스레드가 해당 `lock()` 메서드 시작점에 접근하지 못하고 대기하게 되고 `unlock()` 메서드를 실행해야 다른 메서드가 `lock`을 획득할 수 있다.

```java
public class CountingTest {
    public static void main(String[] args) {
        Count count = new Count();
        for (int i = 0; i < 100; i++) {
            new Thread(){
                public void run(){
                    for (int j = 0; j < 1000; j++) {
                        count.getLock().lock();
                        System.out.println(count.like());
                        count.getLock().unlock();
                    }
                }
            }.start();
        }
    }
}
class Count {
    private int count = 0;
    private Lock lock = new ReentrantLock();
    public int like() {
            return count++;
    }
    public Lock getLock(){
        return lock;
    };
}
```

##### volatile

`volatile` 키워드는 다중 스레드 환경에서 사용되는 변수에 대한 가시성과 순서성을 보장하는 데 사용된다. 이것은 특히 변수의 캐시 값을 사용하는 스레드 간 동기화와 관련이 있으며, 일부 상황에서 동시성 문제를 해결하는 데 도움이 될 수 있다. volatile 변수에 대한 주요 특징은 다음과 같다:

**가시성 (Visibility):**<br> volatile로 선언된 변수의 변경 내용은 다른 스레드에서 즉시 볼 수 있다.<br> 이것은 변수의 값을 읽거나 수정하는 스레드가 항상 가장 최신의 값을 보게 해준다.

**순서성 (Ordering):**<br> volatile 변수를 사용하면 해당 변수의 변경 내용이 메모리에서 읽기와 쓰기 작업을 수행하는 순서를 제어할 수 있다.<br> 이것은 변수를 변경한 스레드가 그 변경 사실이 다른 스레드에게 보이는 시점을 제어한다.

**원자성 (Atomicity):**<br> volatile 변수의 읽기 및 쓰기 작업은 원자적(atomic)으로 수행된다.<br> 다른 스레드가 변수를 동시에 읽거나 수정하는 경우, volatile 변수를 통해 원자적 작업이 보장된다.

`volatile` 키워드는 주로 다음과 같은 상황에서 사용된다:

**상태 플래그 (State Flag):**<br> 다중 스레드 간에 어떤 상태를 공유할 때, `volatile` 변수를 사용하여 모든 스레드가 상태 변경을 공유하고 즉시 볼 수 있도록 한다.
<br>
**무한 루프 종료 조건:**<br> `volatile`변수를 사용하여 무한 루프의 종료 조건을 제어하며, 한 스레드가 해당 변수를 수정하면 다른 스레드가 해당 루프를 종료한다.
<br>
**단순한 플래그 변수:**<br> 스레드 간 플래그 변수를 사용할 때 `volatile`을 활용하여 효율적으로 동기화할 수 있다.
<br>
`volatile`는 다중 스레드 환경에서 공유 데이터를 안전하게 다루는 데 도움을 주지만, 모든 종류의 동시성 문제에 적합하지는 않다.<br> 더 복잡한 동기화 작업이나 복합적인 상태 변경을 다룰 때에는 `synchronized` 블록이나 `java.util.concurrent` 패키지의 클래스들을 고려하는게 낫다.

##### 스레드 안전한 객체 사용

**Concurrent 패키지**
`AcomitInteger`과 같은 클래스는 `i++`과 같은 연산을 단일연산으로 만든 메서드를 제공해준다. 해당 클래스의 메서드는 내부적으로 `Thread-safe` 하게 구조화되어 있어서 스레드 안전한 프로그램을 만드는 것에 도움을 줄 수 있다.<br>
이외에도 `concurrent` 패키지는 각종 스레드 안전한 컬랙션을 제공한다. `ConcurrentHashMap`과 같은 컬랙션은 스레드 안전하게 사용할 수 있다.

**ConcurrentHashMap**
`concurrent`패키지에 존재하는 컬랙션들은 락을 사용할 때 발생하는 성능 저하를 최소한으로 한다. 락을 여러 개로 분할하여 사용하는 `Lock Striping` 기법을 사용하여 동시에 여러 스레드가 하나의 자원에 접근하더라도 동시성 이슈가 발생하지 않도록 도와준다. 
<br> `ConcurrentHashMap`은 내부적으로 여러개의 락을 가지고 해시값을 이용해 이러한 락을 분할하여 사용한다. <br>분할 락을 사용하여 병렬성과 성능이라는 두 마리의 토끼를 모두 잡은 컬랙션이라고 볼 수 있다.<br> 내부적으로 여러 락을 사용하지만 실제 구현이 어떻게 되어 있는지 자세히 알 필요는 없다. 일반적인 map을 사용할 때처럼 구현하면 내부적으로 알아서 락을 자동으로 사용해 줄 테니 편리하게 사용할 수 있다.

##### 불변 객체

스레드 안전한 프로그래밍을 하는 방법중 효과적인 방법은 불변 객체를 만드는 것이다.<br> `String` 객체처럼 한번 만들면 그 상태가 변하지 않는 객체를 불변객체라고 한다. <br>불변 객체는 락을 걸 필요가 없다.<br> 내부적인 상태가 변하지 않으니 여러 스레드에서 동시에 참조해도 동시성 이슈가 발생하지 않는다는 장점이 있다.<br> 즉, 불변 객체는 언제라도 스레드 안전하다.

불변 객체는 생성자로 모든 상태 값을 생성할 때 세팅하고, 객체의 상태를 변화시킬 수 있는 부분을 모두 제거해야 한다.<br> 가장 간단한 방법은 세터(`setter`)를 만들지 않는 것이다.<br> 또한 내부 상태가 변하지 않도록 모든 변수를 `final`로 선언하면 된다.

### 참조

docs.oracle.com/javase/specs/jls/se7/html/jls-17.html<br>
http://tutorials.jenkov.com/java-concurrency/volatile.html<br>
https://deveric.tistory.com/104