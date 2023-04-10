# 3.1 Isolation & Lock
#### 1) NoSQL로 분류되는 모든 제품이 트랜잭션을 제어할 수 있고, 일관성과 공유 기능을 제공하는 것은 아니지만 Redis는 트랜잭션 제어가 가능함
#### 2) Redis는 Read Commited 타입의 트랜잭션 제어 타입도 제공하고 있음 
* 관계형 DB처럼 commit, rollback을 수행하게 되면 초당 100,000~200,000건 이상 빅데이터의 빠른 쓰기와 읽기 작업에 좋은 성능을 기대할 수 없게 되는 문제가 발생하게 되는데, 이를 보완가히 위해 Redis는 Read Commited 타입의 트랜잭션 제어 타입도 제공함
#### 3) Redis 4.0 버전은 Data Sets 락 매커니즘을 제공함
* 일반적으로 관계형 DB 또는 대부분의 NoSQL은 5가지 유형(Global Lock, Database Lock, Object Lock, Page Lock, Key/Value(Data Sets) Lock)의 락 매커니즘을 제공하는데, Redis 4.0 버전은 Data Sets 락 매커니즘을 제공함

#### ▶ Redis의 Isolation & Lock
* 읽기 일관성과 데이터 공유를 위해 Data Sets(Key/Value) Lock을 제공함
* 트랜잭션 제어를 위해 Read Uncommitted와 Read Committed 타입의 2가지 유형을 제공함

</br>

# 3.2 CAS(Check and Set)
#### 1) CAS(Check and Set)
* 데이터 일관성 공유(동일한 데이터를 동시 여러명의 사용자가 수정/삭제하는 경우 발생하는 충돌을 피하기 위한 기술)를 위해서 동시 처리가 발생할 때 먼저 작업을 요구한 사용자에게 우선권을 보장하고, 나중에 작업을 요구한 사용자의 세션에서는 해당 트랜잭션에 충돌이 발생했음을 인지할 수 있도록 하는 것을 의미함

#### 2) Wat 명령어
* 하나의 트랜잭션에 의해 데이터라 변경되는 시점에 다른 트랜잭션에 의해 동일한 데이터가 먼저 변경되는 경우, 일관성에 문제가 발생할 수 있는데, Redis에서는 Watch 명령어에 의해 트랜잭션을 취소할 수 있음

#### ▶ 실습
```redis
> WATCH                # 다중 트랜잭션이 발생하는지 여부를 모니터링을 시작함
> MULTI                # 트랜잭션의 시작
OK
> Set 1 rinjyu         # 임시 저장
QUEUED                 # 임시 저장
> Set 2 rachel
QUEUED                 # 임시 저장
>
> EXEC                 # 트랜잭션 종료(commit)
```

</br>

# 3.3 commit & rollback
#### 1) EXEC
* 변경한 데이터를 최종 저장 시 사용함

#### 2) DISCARD
* 변경한 데이터를 최종 저장하지 않고 취소할 때 사용함

#### ▶ 실습
```redis
>
> MULTI                # 트랜잭션의 시작
OK
> Set 1 rinjyu         # 임시 저장
QUEUED
> Set 2 joo            # 임시 저장
QUEUED
>
> EXEC                 # 트랜잭션 종료(commit)
1) OK
2) OK
>
> keys &
1) "2"
2) "1"
>
> FLUSHALL
OK
>
> MULTI
OK
> Set 3 juddy
QUEUED
> Set 4 jyu
QUEUED
>
> DISCARD              # 트랜잭션 종료(rollback)
OK
>
> keys *
(empty list or set)
>
```

</br>

# 3.4 Index 유형 및 생성
### 1. Index 유형
* Redis DB는 기본적으로 하나의 Key와 하나 이상의 Field/Element 값으로 구성되는데, 해당 Key에는 빠른 검색을 위해 기본적으로 인덱스가 생성됨
#### 1) Primary Key Index
* 기본적으로 생성되는 인덱스
#### 2) Secondary Key Index
* 사용자의 필요에 따라 추가적인 인덱스를 생성한 것

</br>

### 2. Index
#### 1) Exact Match By a Secondary Index
* 인덱스 키를 통해 검색할 때 유일한 값을 검색하는 경우
#### 2) Range By a Secondary Index
* 일정한 범위의 값을 검색 조건으로 부여한 경우

​

#### ▶ 실습
#### ① Sorted Sets 타입 입덱스
```redis
> zadd order.ship_date.index 2 '20220319:20220331'
(integer) 1
> zadd order.ship_date.index 1 '20220320:20220330'
(integer) 1                     # order 테이블, order_no:ship_date 필드에 인덱스 생성
>
> zrange order.ship_date.index 0 -1
1) "20220320:20220330"
2) "20220319:20220331"

> zscan order.ship_date.index 0
1) "0"
2) 1) "20220320:20220330"
   2) "1"
   3) "20220319:20220331"
   4) "2"
>
> zscan order.ship_date.index 0 match 20220319*
1) "0"
2) 1) "20220319:20220331"
   2) "2"
>
> zadd order.no.index 1 20220320    # order, ship_date:order_no 필드에 인덱스 생성
(integer) 1
> zadd order.no.index 2 20220319
(integer) 1
>
> zrange order.no.index 0 -1
1) "20220320"
2) "20220319"
```

</br>

#### ② LexicoGraphical Index
```redis
> zadd product.name.index 0 "Sky Pole"
(integer) 1
> zadd product.name.index 0 "Bunny Boots"
(interger) 1
> zadd product.name.index 0 "Ski Pants"
(integer) 1
> zadd product.name.index 0 "Mountain Pants"
(integer) 1
>
> zrangebylect product.name.index [S + 
1) "Ski Pants"
2) "Sky Pole"
>
> zrangebylext product.name.index [B [S
1) "Bunny Boots"
2) "Mountain Pants"
>
> zrangebylext product.name.index [B [T
1) "Bunny Boots"
2) "Mountain Pants"
3) "Ski Pants"
4) "Sky Pole"
>
> zrangebylext product.name.index [B (N
1) "Bunny Boots"
2) "Mountain Pants"
>
```

</br>

#### ③ Hash 타입 인덱스
```redis
> hmset order_index no:20220320 customer_name:"Woman & Sports" emp_name:"Magee" total:601100 payment_type:"Credit" order_filled:"Y"
OK
>
> hmset order_index no:20220319 customer_name:"PIT co." emp_name:"JM JOO" total:5000 payment_type:"Credit" order_filled:"Y"
OK
>
> hscan order_index 0
1) "0"
2) 1) "no:20220320"
   2) "customer_name:Woman & Sports"
   3) "emp_name:Magee"
   4) "total:601100"
   5) "payment_type:Credit"
   6) "order_filled:Y"
   7) "no:20220319"
   8) "customer_name:PIT co."
   9) "emp_name:JM JOO"
   10) "total:5000"
>
```

</br>

# 3.5 사용자 생성 및 인증/보안/Roles
Redis Server는 인가된 사용자만이 안전하게 데이터를 관리할 수 있도록 다양한 액세스 권한과 인증 방법들을 제공하고 있음

### 1. 액세스 컨트롤 권한(Access Control Privilege)
#### 1) 미리 DB 내에 사용자 계정과 비밀번호를 생성해두고, Redis Server에 접속하려는 사용자는 해당 계정과 비밀번호를 입력하여 허가 받는 방법
* 잘못된 계정과 비밀번호를 입력하게 되면 접속 에러가 발생함
#### 2) Standalone Redis Server 접속
* 사용자 계정과 비밀번호로 기본적인 액세스 컨트롤 권한을 부여할 수 있지만, 여러 대의 서버를 분산 및 복제 시스템으로 구축할 때는 추가로 Master, Slave, Sentinel, Partition, Replication 서버 간에 네트워크 액세스 컨트롤 권한이 필요하게 됨
#### 3) 액세스 컨트롤 기능 활성화
* conf 파일 내에 requirepass, masterauth 파라미터를 통해 환경설정해야 함
```redis
vi redis.conf
requirepass manager
masterauth redis 123
```

</br>

### 2. 인증 방법(Authorization Method)
Redis Server는 2가지 인증 방법을 제공함

#### 1) OS(운영체제) 인증 방법
* 시스템 환경 설정을 위한 conf 파일에 접속할 클라이언트의 IP 주소를 미리 지정하는 방법으로, 지정된 클라이언트는 Redis Server에 접속할 수 있는 권한을 부여 받음
#### 2) Internal(내부) 인증 방법
* Redis Server에 접속한 다음 auth 명령어로 미리 생성해둔 사용자 계정과 비밀번호를 입력하여 권한을 부여받는 방법을 말함

#### ① Community Edition에 Standalone 서버를 구축하는 경우
* 기본적으로 사용자 계정을 사용자가 직접 부여하는 이름으로 설계 및 생성할 수 없지만, conf 파일의 requirepass 파라미터를 통해 기본 계정에 비밀번호를 부여할 수 있음
#### ② Enterprisre Edition를 사용하는 경우
* 사용자를 직접 설계 및 생성할 수 있고, 사용자가 부여한 사용자명을 사용할 수 있으며 추가로 사용자 정의 롤을 생성하여 부여할 수도 있음
#### ⓐ 사용자 생성 및 Role 기능을 위한 모듈 설치 방법
```redis
curl -k -L -v -u ":" --location-trusted
     -H "Content-Type: application/json"
     -X POST http://:8080/v1/users
     -d "{\"auth_method\": \"external\", \"name\": \"\", \"role\": \"\"}"
```
#### ⓑ Enterprise Edition Server에서 제공되는 Role 유형과 작업 범위

|유형 | 설명
|:---:|:---:|
|Admin | 시스템에 전체 액세스할 수 있음
|DB Viewer | - cluster의 모든 데이터베이스에 대한 정보를 볼 수 있음 </br> - node 및 cluster에 대한 정보를 볼 수 없음 </br> - 로그를 볼 수 있음 </br> - 자체 비밀번호 변경 이외에는 cluster 설정을 볼 수 없음
|Cluster Viewer | - cluster, node, 데이터베이스에 대한 모든 정보를 볼 수 있음 </br> - 로그를 볼 수 있음 </br> - 자체 비밀번호 변경 이외에는 cluster 설정을 볼 수 없음
|DB Member | - 데이터베이스 생성이 가능함 </br> - DB Metric을 볼 수 있음 </br> - 데이터베이스 구성을 편집할 수 있음 </br> - slowlog를 클리어할 수 있음 </br> - 로그를 볼 수 있음 </br> - node 및 cluster에 대한 정보를 볼 수 있음 </br> - 자체 비밀번호 변경 이외에는 cluster 설정을 볼 수 없음
|Cluster Member | - node와 cluster 정보를 볼 수 있음 </br> - 데이터베이스 생성이 가능함 </br> - DB Metric을 볼 수 있음 </br> - 데이터베이스 구성을 편집할 수 있음 </br> - slowlog를 클리어할 수 있음 </br> - 로그를 볼 수 있음 </br> - 자체 비밀번호 변경 이외에는 cluster 설정을 볼 수 없음 </br> 

#### ▶ 실습
```redis
src/redis-cli -p 5000                       # 비밀번호 지정없이 로그인 시도
127.0.0.1:5000>
127.0.0.1:5000> shutdown
Not-connected> exir
$
$ vi redis.conf                             # Redis Server 환경설정 파일
requirepass       redis123                  # Redis 접속을 위한 비밀번호

$ src/redis-server /usr/lib/redis/redis.conf

$ src/redis-cli -p 5000                     # 비밀번호 지정없이 로그인 시도
127.0.0.1:5000>
127.0.0.1:5000> info
NOAUTH Authentication required.             # 인증 실패
127.0.0.1:5000>
127.0.0.1:5000> auth redis123
OK                                          # 인증 성공
127.0.0.1:5000> info
# Server
redis_version:6.2.6
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:d072f207cc4d0da3
redis_mode:standalone
...
127.0.0.1:5000> exit
$
$ src/redis-cli -p 5000 -a redis123         # 비밀번호를 지정하여 로그인 시도
127.0.0.1:5000> info
# Server
redis_version:6.2.6
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:d072f207cc4d0da3
redis_mode:standalone
...
127.0.0.1:5000>
127.0.0.1:5000> shutcdown
Not-connected> exit
$
$ vi redis.conf                             # Redis Server 환경설정 파일
#requirepass       redis123                 # 주석처리

$ src/redis-server /usr/lib/redis/reids.conf
```
