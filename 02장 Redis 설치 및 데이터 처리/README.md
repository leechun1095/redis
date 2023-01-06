# 2.1 주요 특징

## 1. Redis의 주요 특징

#### 1) Redis는 Key-Value 데이터베이스로 분류되는 NoSQL

​

#### 2) Key-Value DB면서 대표적인 In Memory 기반의 데이터 처리 및 저장 기술을 제공함

- 다른 NoSQL에 비해 상대적으로 빠른 Read/Write가 가능함

​

#### 3) String, Set, Sorted Set, Hash, List, HyperLogLogs 유형의 데이터를 저장할 수 있음

​

#### 4) Dump 파일과 AOF(Append Of File) 방식으로 메모리 상의 데이터를 디스크에 저장할 수 있음

​

#### 5) Master/Slave Replication 기능을 통해 데이터의 분산, 복제 기능을 제공함

- Query Off Loading 기능을 통해 Master는 Read/Write를 수행하고, Slave는 Read만 수행할 수 있음

​

#### 6) 파티셔닝을 통해 동적인 스케일 아웃인 수평 확장이 가능함

​

#### 7) Expriation 기능 지원

- Expiration 기능은 일정 시간이 지났을 때 메모리 상의 데이터를 자동 삭제할 수 있는 기능으로, 이를 지원함

​

## 2. Redis의 주요 업무 영역

#### 1) In Memory DB가 제공하는 최대 장점인 빠른 쓰기와 읽기 작업은 가능하지만 모든 데이터를 저장할 수 없음

- 지속적인 관리가 요구되는 비즈니스 영역에 사용하는 것은 제한적일 수 밖에 없는데, 기업의 비즈니스 영역과 데이터 성격에 따라 다르겠지만 전형적인 기업 데이터를 저장하기 위한 용도보다 덜 위험한 업무 영역에서 Secondary DB로 사용하는 것이 보편적인 경우임

​

#### 2) 주된 비즈니스 영역

- 데이터 캐싱을 통한 빠른 쓰기/읽기 작업이 요구되는 업무 영역, IoT 디바이스를 활용한 데이터 수집 및 처리 영역, 실시간 분석 및 통계 분석 영역 등이 있음

​

#### 3) 메시지 큐, 머신 러닝, 애플리케이션 잡 매니지먼트, 검색 엔진 업무 영역

- 기존의 관계형 DB와 타 NoSQL에 비해 효율적으로 사용할 수 있음

​

# 2.2 제품 유형

Redis는 대부분의 NoSQL 제품과 같이 듀얼 라이선스를 제공하는데, 사용자의 개발 용도, 개발 환경, 요구되는 기술, 향후 기술 지원과 유지보수 등을 고려하여 선택할 수 있음

​

## 1. 커뮤니티 에디션

- 일반적으로 NoSQL은 오픈소스 라이선스를 기반으로 개발과 지원되는 기술로, 해당 SW를 다운로드하여 사용하더라도 별도의 비용이 발생하지 않고, 사용자는 SW 구입 및 설치 비용을 지불하지 않아도 됨

- 해당 개발사는 SW를 무상 제공하지만 사용자가 이를 이용하여 SW를 개발하면서 발생하는 어떠한 문제에 대해서도 개발사는 책임지지 않고, 모든 사용자가 지게 됨

- 오픈소스 SW를 사용하기 위해 사용자가 따라야 하는 의미 사항이 존재하는데, 반드시 내용을 파악 후 사용해야 함

​

## 2. 엔터프라이즈 에디션

- 사용자가 해당 SW를 사용하면서 발생하게 되는 기술적 문제에 대해 개발사가 책임지고 기술 지원 및 유지보수 작업을 수행하며 긴급 패치가 필요한 경우, 개발사는 우선 제공하게 됨

- 개발사와 사용자는 연간 단위의 유지보수 계약을 체결하게 되며 사용자는 그에 상응하는 비용을 개발사에 지불해야 함

- 커뮤니티 에디션에는 없는 다양한 옵션 기능들이 제공되기 때문에 상황에 따라 사용해야 할 필요가 있음

​

## 3. Redis 시스템의 아키텍처 구성

#### 1) Redis 시스템을 구성하고 있는 Sub 시스템

- 데이터 저장 엔진

- 분산 시스템

- 복제 시스템

- Index Support

- 관리 툴(redis-server, redis-cli, Redis-benchmark 등) 

​

#### 2) 3'rd Party에서 제공하는 다양한 Redis 확장 Module을 함께 연동해서 사용하면 강력한 솔루션으로 활용 가능

- Redis Search Engine : 검색 엔진

- RedisSQL : Redis와 SQLite DB 연동 솔루션

- RedisGraph : Redis와 Graph DB 연동 솔루션

- Redis sPiped : Redis 암호화 솔루션

​

# 2.3 다운로드 및 설치

## 2.3.1 Redis 설치 on Linux

## 1. Redis 제품 다운로드

#### 1) Redis Community Edition

https://redis.io/download

​

# 2.5 데이터 처리

## 2.5.1 용어 설명

#### ▶ Key-Value DB의 논리적 구조
* Table : 하나의 DB에서 데이터를 저장하는 논리적 구조 (관계형 DB에서 표현하는 논리적 개념인 테이블과 동일)
* Data Sets
  * 테이블을 구성하는 논리적 단위
  * 하나의 Data Sets은 하나의 Key와 한 개 이상의 Field/Element로 구성됨
* Key
  * 하나의 Key는 하나 이상의 조합된 값으로 표현이 가능함
  * 예) 주문번호 또는 주문번호+순번, 년월일+순번 등
* Values
  * 해당 Key에 대한 구체적인 데이터 값을 표현함
  * Values는 하나 이상의 Field 또는 Element로 구성됨

​

## 2.5.2 데이터 입력/수정/삭제/조회
Redis DB에 데이터를 입력/수정/삭제/조회하기 위해서는 반드시 Redis 서버에서 제공하는 명령어를 사용해야 하고, 데이터를 조작할 때는 하나의 Key에 대해 하나 이상의 Field 또는 Element로 표현해야 함

* set :데이터를 저장할 때(key, value)
* get : 저장된 데이터를 검색할 때
* rename : 저장된 데이터 값을 변경할 때
* randomkey : 저장된 key 중에 하나의 key를 랜덤하게 검색할 때
* keys : 저장된 모든 key를 검색할 때
* exits : 검색 대상 key가 존재하는지 여부를 확인할 때
* mset/mget : 여러 개의 key와 value를 한 번 저장하고 검색할 때

#### ▶ 실습
```redis
$ src/redis-server /usr/lib/redis/redis.conf
$
$
keys cd /usr/lib/redis/src
$ ./redis-cli -p 5000
127.0.0.1:5000>
127.0.0.1:5000> set 1111 "rinjyu"         # key: 1111, value: rinjyu 데이터 저장(set)
OK
127.0.0.1:5000> set 1112 "rachel"
OK
127.0.0.1:5000> set 1113 "joo"
OK
127.0.0.1:5000> set 1114 "jyuuuu"
OK
127.0.0.1:5000>
127.0.0.1:5000> get 1111                  # key: 1111에 대한 데이터 검색(get)
"rinjyu"
127.0.0.1:5000> get 1112
"rachel"
127.0.0.1:5000>
127.0.0.1:5000> keys *                    # 현재 저장되어 있는 모든 key 출력
1) "1111"
2) "1112"
3) "foo"
4) "1113"
5) "1114"
127.0.0.1:5000>
127.0.0.1:5000> keys *2            # 현재 저장되어 있는 key 중에 2로 끝나는 key만 검색 "1112"
127.0.0.1:5000>
127.0.0.1:5000> del 1112                   # key: 1112 삭제
(Integer) 1
127.0.0.1:5000>
127.0.0.1:5000> keys *
1) "1111"
2) "foo"
3) "1113"
4) "1114"
127.0.0.1:5000>
127.0.0.1:5000> rename 1113 1116           # key: 1113을 key: 1116으로 변경
OK
127.0.0.1:5000> keys *
1) "1111"
2) "1116"
3) "foo"
4) "1114"
127.0.0.1:5000> randomkey                  # 현재 key 중에 랜덤으로 key 선택
"1111"
127.0.0.1:5000>
127.0.0.1:5000> exists 1116                # key: 1116의 존재여부 검색(존재 : 1)
(integer) 1
127.0.0.1:5000>
127.0.0.1:5000> exists 1112                # key: 1112는 존재하지 않음(integer: 0)
(integer) 0
127.0.0.1:5000>
127.0.0.1:5000> strlen 1111                # key: 1111의 value 길이(rinjyu)
(integer) 6
127.0.0.1:5000>
127.0.0.1:5000> flushall                   # 현재 저장되어 있는 모든 key 삭제
OK
127.0.0.1:5000> keys *
(empty list or set)
127.0.0.1:5000>
127.0.0.1:5000> setex 1111 30 "rinjyu"     # ex는 expire(일정 기간 동안만 저장)
OK
127.0.0.1:5000> ttl 1111                   # 현재 남은 시간 확인, 30초 중 20초 남은 상태
(integer) 20
127.0.0.1:5000> ttl 1111
(integer) 16
127.0.0.1:5000> ttl 1111 
(integer) 8
127.0.0.1:5000> ttl 1111
(integer) -2
127.0.0.1:5000>
127.0.0.1:5000> mset 1113 "NoSQL User Group" 1115 "PIT"    # 여러 개 필드를 한번에 저장
OK
127.0.0.1:5000> get 1113
"NoSQL User Group"
127.0.0.1:5000> get 1115
"PIT"
127.0.0.1:5000>
127.0.0.1:5000> mget 1113 1115              # mset에 저장된 값을 한번에 다중 검색
1) "NoSQL User Group"
2) "PIT"
127.0.0.1:5000>
127.0.0.1:5000> mget 1113 1115 nonexisting  # nonexisting은 존재하지 않은 경우 nil로 출력
1) "NoSQL User Group"
2) "PIT"
3) (nil)
127.0.0.1:5000>
127.0.0.1:5000> mset seq_no 20220318        # 연속번호 발행을 위한 key/value 저장
OK
127.0.0.1:5000> incr seq_no                 # Incremental 증가값 +1
(integer) 20220319
127.0.0.1:5000>
127.0.0.1:5000> decr seq_no                 # Decremental 감소값 -1
(integer) 20220318
127.0.0.1:5000>
127.0.0.1:5000> incrby seq_no 2             # Incremental 증가값 +2
(integer) 20220320
127.0.0.1:5000>
127.0.0.1:5000> descrby seq_no 10           # Decremental 감소값 -10
(integer) 20220310
127.0.0.1:5000>
127.0.0.1:5000> get 1115
"PIT"
127.0.0.1:5000> append 1115 " co."          # 현재 value에 value 추가
(integer) 6
127.0.0.1:5000> get 1115
"PIT co."                                   # 최초값 PIT에 co.가 추가됨
127.0.0.1:5000>
127.0.0.1:5000> keys *
1) "1115"
2) "seq_no"
3) "1113"
127.0.0.1:5000>
127.0.0.1:5000> save                        # 현재 입력한 key/value 값을 파일로 저장
OK
127.0.0.1:5000> flushall                    # 모든 key/values 값 제거
OK
127.0.0.1:5000> keys *
(empty list or set)
127.0.0.1:5000>
127.0.0.1:5000> clear                       # 화면 clear
127.0.0.1:5000> time
1)                                          # 데이터 저장 시간(unix time in secods) microseconds
2)
127.0.0.1:5000>
127.0.0.1:5000> info                        # Redis Server 설정 상태 조회
# Server
redis_version:6.2.6
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:d072f207cc4d0da3
redis_mode:standalone
...
127.0.0.1:5000> exit
$
$ cd /usr/lib/redis/src
$ ls dump.rdb
dump rdb                                    # SAVE 명령어에 의해 OS 상에 생성된 RDB 파일
$ 
```

​

## 2.5.3 데이터 타입

* Strings : 문자(text), Binary 유형 데이터를 저장
* List : 하나의 Key에 여러 개의 배열 값을 지정
* Hash : 하나의 Key에 여러 개의 Fields와 Value로 구성된 테이블을 저장
* Set, Sorted set : 정렬되지 않은 String 타입, Set과 Hash를 결합한 타입
* Bitmaps : 0 & 1로 표현하는 데이터 타입
* HyperLogLogs : Element 중에서 Unique한 개수의 Element만 계산
* Geospatial : 좌표 데이터를 저장 및 관리하는 데이터 타입

​

## 1. Hash 타입
#### 1) Redis에서 데이터를 표현할 때 기본 타입은 하나의 Key와 하나 이상의 Field/Element 값으로 저장함
- Key : 아스키 값을 저장할 수 있음
- Value : String 데이터를 저장할 수 있고, 추가로 컨테이너(Container) 타입의 데이터들을 저장할 수 있음
- Container 타입 : Hash, List, Set/Sorted Set이 있음

​

#### 2) Hash 타입의 주요 특징
① 기존 관계형 DB에서 PK와 하나 이상의 컬럼으로 구성된 테이블 구조와 매우 흡사한 데이터 유형  
② 하나의 Key는 오브젝트명과 하나 이상의 필드 값을 콜론(:) 기호로 결합하여 표현할 수 있음  
예) order:20220318, order_detail:20220318:01  
③ 문자값을 저장할 때 인용부호("")를 사용하고, 숫자값을 저장할 때는 인용부호("")가 필요하지 않음  
④ 기본적으로 필드 개수는 제한이 없음  
⑤ 사용 명령어 : hmset, hget, hgetall, hkey, hlen  

​

## 2. List 타입

#### ▶ List 타입의 주요 특징

① 기존의 관계형 테이블에는 존재하지 않는 데이터 유형으로, 일반적인 프로그래밍 언어에서 데이터를 처리할 때 사용되는 배열 변수와 유사한 데이터 구조임  
② 기본적으로 String 타입의 경우, 배열에 저장할 수 있는 데이터 크기 512MB  
③ 사용 명령어 : lpush, prange, rpush, rpop, llen, lindex   
​

## 3. Set 타입

#### ▶ Set 타입의 주요 특징
① List 타입은 하나의 필드에 여러 개의 배열 값을 저장할 수 있는 데이터 구조라면, Set 타입은 배열 구조가 아닌 여러 개의 Element로 데이터값을 표현하는 구조임  
② 사용 명령어 : sadd, smembers, scard, sdiff, sunion  

​

## 4. Sorted Set 타입

#### ▶ Sorted Set 타입의 주요 특징
① Set 타입과 동일한 데이터 구조로, 차이점은 저장된 데이터 값이 분류(sorting)된 상태라면 Sorted Set 타입이고 분류되지 않은 상태이면 Set 타입인 것을 알 수 있음  
② 사용 명령어 : zadd, zrange, zcard, zcount, zrank, zrevrank  

​

## 5. Bit 타입

#### ▶ Bit 타입의 주요 특징

① 일반적으로 사용자가 표현하는 데이터는 문자, 숫자, 날짜인데 이를 ASCII 값이라고 표현하는데 컴퓨터는 이를 최종적으로 0, 1로 표현되는 Bit 값으로 변환하여 저장함  
* Redis에서 제공되는 Bit 타입은 사용자의 데이터를 0과 1로 표현하고, 컴퓨터가 가장 빠르게 저장할 수 있고 해석할 수 있도록 표현하는 구조임
② 사용 명령어 : setbit, getbit, bitcount  

​

## 6. Geo 타입

① Redis DB의 Geo 타입은 위치정보(경도, 위도) 데이터를 효율적으로 저장, 관리할 수 있고 이를 활용한 위치 정보 데이터의 분석 및 검색에 사용할 수 있음  
② 사용 명령어 : geoadd, geopos, geodist, georadius, geohash

​

## 7. HyperLogLogs 타입
① 관계형 DB의 테이블 구조에서 Check 제약 조건과 유사한 개념의 데이터 구조  
* Redis DB에서도 관계형 DB에서와 같이 동일하게 특정 Field 또는 Element에 저장되어야 할 데이터 값을 미리 생성하여 저장한 후에 필요에 따라 연결(Link)하여 사용할 수 있는 데이터 타입을 말함  
② 사용 명령어 : pfadd, pfcount, pfmerge

​

## 2.6 Redis 확장 Module

#### 1) Redis Server
빅데이터 저장 및 관리 기술을 제공하는 소프트웨어

​

#### 2) Redis Extent Module
Redis Server의 소스를 이용한 다양한 기능들을 개발하여 배포하는 것을 말함

* REJSON : JSON 데이터 타입을 이용하여 데이터를 처리할 수 있는 모듈
* REDISQL : Redis Server에서 관계형 DB인 SQLite로 데이터를 저장할 수 있는 모듈
* RedisSearch : Redis DB 내에 저장된 데이터에 대한 검색엔진을 사용할 수 있는 모듈
* Redis-ML : Machine Learning Model Server를 Redis 서버에서 사용할 수 있는 모듈
* Redis-sPiped : Redis Server로 전송되는 데이터를 암호화할 수 있는 모듈

​

## 2.6.1 REJSON
Redis Server 내에서도 JSON 데이터 타입을 저장할 수 있게 해주는 확장 모듈을 말함

​

## 2.6.2 REDISQL
NoSQL 기술을 도입하면 기존에 구축된 관계형 데이터베이스를 활용할 수 밖에 없는 경우가 발생하는 데, 이 같은 경우에는 확장 모듈 REDISQL을 사용하면 Redis 서버와 관계형 DB인 SQLite를 연동해서 사용할 수 있음

​

## 2.7 Lua Function & Script
#### ▶ Lua 스크립트의 주요 특징
① Lua는 매우 가볍과 내장 가능한 Sript Programming Language 중에 하나임  
* 절차향 프로그래밍과 객체지향 프로그래밍이 가능하고 데이터베이스를 기반으로 하는 프로그래밍이 가능함  
② 간단한 절차형 프로시저와 배열로 데이터를 저장하고 결합할 수 있으며 동적으로 코딩할 수 있을 뿐만 아니라 가상머신 기반에서 바이트 코드를 해석하여 실행할 수 있고, 증분 가비지 컬렉션으로 메모리를 자동 관리할 수 있도록 프로그래밍 할 수 있음  
③ Lua는 브라질 리오네자네이로에 있는 교황청 카톨릭 대학교의 PUC-Rio 팀에서 개발 및 유지 관리되고 있음  
④ Redis Server는 아키텍처 내부에 Lua Interpreter를 내장해 두었으며, 사용자는 미리 작성해둔 Lua 스크립트 또는 Lua 함수를 Redis 서버 내에서 직접 실행할 수 있음  
⑤ Redis Server는 다양한 Lua 함수를 제공하며 사용자는 이를 통해 데이터를 검색, 수정, 삭제할 수 있음  
* 대표적으로 eval, evalsha 함수가 있음
