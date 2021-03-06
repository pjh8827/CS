# ACID

데이터베이스는 다음과 같은 특징을 가져야 한다.

## 1. Atomicity(원자성)

트랜잭션의 작업이 부분적으로 실행되거 중단되지 않는것을 보장
**롤백 세그먼트**를 사용한다.

## 2. Consistency(일관성)

트랜잭션이 성공적으로 완료되면 일관적인 DB상태를 유지
**트리거**를 사용하여 데이터베이스 시스템이 자동적으로 수행할 동작을 명시.(Create은 트리거 생성, After는 트리거가 실행되기 위한 Event)

## 3. Isolated(고립성)

트랜잭션 수행시 다른 트랜잭션 작업이 끼어들지 못하도록 한다.
문제점은 병행처리에서 온다. (갱신분실, 오손판독, 반복불가능, 팬텀문제) 이를 Lock & Excute unlock을 통해 고립성을 보장한다. 데이터를 읽을 때 또 여러 트랜잭션이 읽을 수 있도록 허용하는 Shared_lock을 하며

## 4. Durability (지속성)

성공적으로 수행된 트랜잭션은 영원히 반영되어야 한다.

