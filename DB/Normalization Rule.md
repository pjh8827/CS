# 데이터베이스 정규화

관계형 데이터베이스에서 **데이터 중복**을 최소화하고 데이터를 **구조화**하는 것을 정규화라고 한다. 대표적으로는 제 1정규형, 제 2정규형, 제 3정규형이 있다. 함수적 종속성을 기반으로 유사한 속성들은 모으고 종속성이 없는 독립적인 속성들은 분리하는 것 또한 정규화라고 할 수 있다.

## 제 1 정규형

테이블의 한 셀이 복합적이고 중복된 값을 가지지 않는 상태를 제 1정규화라고 한다. 여기서 셀은 하나의 튜플이 가지는 속성값을 말하며 복합적인 값은 배열 값을 의미한다.

## 제 2 정규형

테이블에서 부분함수 종속이 없는 상태를 제 2정규형이라고 한다. 여기서 함수 종속은 기본키라는 입력에 유일한 속성이 출력되는 관계가 성립되면 마치 함수와 같다하여 함수 종속이라 말하는 것이며 여기서 기본키를 결정자, 속성을 종속자라 자칭한다. 따라서 부분함수 종속은 기본키를 구성하는 속성 일부에만 함수 종속이 존재하는 것을 말한다. 그렇기 때문에 기본키가 하나뿐인 테이블은 자동적으로 제 2 정규형을 만족하게 된다.

### 부분함수 종속이 안 좋은 이유 && 제 2정규형이 필요한 이유

첫 번째로 테이블이 가지는 개념의 혼란을 일으킨다. 부분함수 종속이 존재한다는 의미는 한 테이블 안에 카테고리가 다른 집합들이 혼재되어 있다는 뜻이 되기에 테이블을 나누어야 한다. 두 번째로 갱신 이상을 야기한다. 테이블이 올바른 집합 단위로 규정되어 있지 않기 때문에 각 속성의 값을 어느정도 알고 있어야만 등록이 가능할뿐더러, 기본키를 구성하는 속성이 각 행에 대하여 불필요하게 반복됨으로써 일부가 잘못 등록될 위험이 있다.

## 제 3 정규형

제 2정규형에서 추이 함수(이행 함수) 종속을 제거한 상태를 제 3 정규형이라고 한다. 추이 함수(이행 함수)란 기본키이외의 속성과 다른 속성간 함수 종속이 존재하는 것을 말한다.

### 추이함수 종속이 안 좋은 이유 && 제 3정규형이 필요한 이유

추이함수로 인한 갱신이상 때문이다. 추이함수 종속에서 종속자에 해당하는 속성값은 마음대로 갱신이 안되고 결정자인 속성 값에 따라 값이 좌우되기 때문에 이를 무시하고 데이터를 갱신한다면 이상이 발생한다. 결국 부분함수 종속과 마찬가지로 계층이 다른 집합들을 하나의 테이블로 관리하려는 설계 문제를 내포하기에 테이블을 나누어야 문제를 해결할 수 있습니다.

## 역정규화

역정규화는 데이터의 정합서이 보증되지 않더라도 속도의 향상을 위해 정규화를 위배하는 행위를 말한다. 정규화 된 테이블은 빈번한 Join을 당하게 되는데 이것은 성능을 크게 저하시키는 요소이다. 역정규화를 하면 중복된 데이터가 발생하고 데이터의 정합성을 헤치게 되더라도 그만큼 성능을 높을 수 있는 이점이 있다. 허나 품질저하, 테이블 성격 불명확, 용량 증가등의 단점이 있다.