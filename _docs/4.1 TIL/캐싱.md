---
title: Caching (Redis)
category: TIL
order: 8
---

### Caching이란 ?

<div class="content-box">
<li>Caching은 데이터나 계산 결과를 임시로 저장하여 이후 요청에서 빠르게 제공하는 기술</li>
</div>

###  Redis란?

<div class="content-box">
<li>Redis(Remote Dictionary Server)는 오픈 소스의 메모리 기반 데이터 저장소.</li>
<li>데이터를 메모리에 저장하므로 빠른 데이터 액세스를 제공하며, 주로 Key-Value 스토어로 사용</li>
<li>Redis는 데이터를 디스크에도 저장할 수 있으므로 데이터의 영속성을 보장.</li>
</div>

##### Redis를 Caching에 사용하는 이유

* **1. 빠른 속도**: <br>
Redis는 메모리에 데이터를 저장하고 빠르게 검색할 수 있으므로 데이터 액세스가 빠름<br>
* **2. 유연성**: <br>Redis는 다양한 데이터 구조를 지원하며, Key-Value, 리스트, 해시, 집합, 정렬 집합 등 다양한 데이터 형식을 처리할 수 있음<br>
* **3.데이터 영속성**:<br>
Redis는 데이터를 디스크에 저장할 수 있어 데이터 손실을 방지할 수 있음

##### Redis Caching의 작동 방식

* **1. 데이터 캐싱**: <br>Redis는 데이터베이스 또는 다른 소스로부터 데이터를 가져와 메모리에 캐싱
* **2. 캐싱된 데이터 사용**: <br>다음 요청이 들어올 때 Redis에서 데이터를 가져와 사용자에게 제공
* **3. 만료 시간 설정**: <br>캐시된 데이터에 만료 시간을 설정하여 일정 기간 후에 데이터가 자동으로 만료
* **4.캐시 삭제**: <br>데이터가 업데이트되거나 더 이상 필요하지 않을 때 Redis에서 캐시를 삭제

##### Redis Caching의 장점
* **1. 빠른 응답 시간**: <br>메모리에 데이터를 저장하므로 데이터 액세스가 빠름
* **2. 부하 감소**: <br>데이터베이스 또는 다른 시스템에 대한 부하를 줄일 수 있음
* **3. 데이터 일관성**: <br>데이터베이스와 동기화하여 데이터 일관성을 유지할 수 있음
* **4. 확장성**: <br>Redis 클러스터를 사용하여 데이터 용량을 확장할 수 있음.

##### Redis Caching 사용 사례

**1.** 웹 애플리케이션에서 페이지 렌더링 속도 향상<br>
**2.** 데이터베이스 쿼리 결과의 캐싱<br>
**3.** 세션 관리<br>
**4.** 실시간 알림 처리<br>
**5.** 빠른 카운팅 및 집계<br>

### 사용예시 (코드)

##### RedisConfig.java
```java
    @Bean
    public RedisCacheManager cacheManager(RedisConnectionFactory redisConnectionFactory) {
        // Redis 캐시 설정 구성
        RedisCacheConfiguration cacheConfiguration = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofDays(1)) // 캐시 항목의 만료 기간을 1일로 설정
                .serializeKeysWith(
                        RedisSerializationContext.SerializationPair
                                .fromSerializer(new StringRedisSerializer())) // 캐시 키를 문자열로 직렬화
                .serializeValuesWith(
                        RedisSerializationContext.SerializationPair
                                .fromSerializer(new GenericJackson2JsonRedisSerializer())); // 캐시 값을 JSON으로 직렬화

        // RedisCacheManager 빌더를 사용하여 Redis 캐시 매니저 생성
        return RedisCacheManager.builder(RedisCacheWriter.lockingRedisCacheWriter(redisConnectionFactory))
                .cacheDefaults(cacheConfiguration) // 기본 캐시 구성 설정
                .build(); // Redis 캐시 매니저 생성
    }
```

##### Cache 조회 후 없으면 저장
```java
    @Cacheable(value = "sggLatLonList", key = "'sggList'")
    public List<SggLatLonDTO> getSggLatLons() {
        return sggLatLonRepository.findAll().stream()
                .map(SggLatLonDTO::from)
                .collect(java.util.stream.Collectors.toList());
    }
```

`@Cacheable(value = "sggLatLonList", key = "'sggList'")`: 이 어노테이션은 메서드의 결과를 캐싱하는 역할<br>

`value = "sggLatLonList"`: 이 부분은 캐시의 이름 또는 식별자를 지정, 여러 개의 캐시를 사용할 때 구분하기 위한 역할<br>

`key = "'sggList'":` 이 부분은 캐시에 저장될 데이터의 키를 지정. 'sggList'는 문자열 리터럴로 지정된 키<br>

`@Cacheable` 어노테이션은 메서드가 호출될 때, 해당 메서드의 결과를 지정한 캐시에 저장. 만약 같은 메서드가 다시 호출되면, 캐시에서 데이터를 읽어와서 메서드의 실행을 스킵하고 캐시된 결과를 반환. 이렖게 함으로써 메서드의 실행을 생략하여 성능 향상.

##### Cache 삭제
```java
    @CacheEvict(value = "sggLatLonList", key = "'sggList'")
    public void updateCsv(MultipartFile csvFile) {
        try (BufferedReader br = new BufferedReader(new InputStreamReader(csvFile.getInputStream()))) {
            // 첫 번째 줄은 제목 행이므로 한 번 읽어서 무시.
            br.readLine();

            String line;
            while ((line = br.readLine()) != null) {
                String[] data = line.split(",");
                sggLatLonRepository.save(SggLatLon.from(data));
            }

        } catch (IOException e) {
            log.error("파일 읽기 실패 {}", e.getMessage());
        }
    }
```
`@CacheEvict(value = "sggLatLonList", key = "'sggList'")`: 이 어노테이션은 캐시를 비워주는 역할<br>

`value = "sggLatLonList"`: 이 부분은 `@Cacheable`에서와 동일하게 캐시의 이름 지정<br>

`key = "'sggList'"`: 이 부분은 비워야 할 캐시 데이터의 키를 지정. 'sggList'는 문자열 리터럴로 지정된 키.<br>

`@CacheEvict` 이 어노테이션을 사용하면 메서드 실행 결과에 대한 캐시를 비우고, 다음 요청부터는 다시 메서드가 실행되어 새로운 결과를 캐싱. 이렇게 함으로써 캐시된 데이터를 갱신할 수 있음.<br>

예를 들어, 위의 코드에서는 `"sggLatLonList"`라는 캐시에 `"sggList"`라는 키로 메서드의 실행 결과를 저장하고, `@CacheEvict` 어노테이션을 통해 해당 캐시의 `"sggList"` 키에 해당하는 데이터를 비워주는 역할





