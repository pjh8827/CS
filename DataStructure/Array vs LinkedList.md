# Array vs LinkedList

## Array

​	Array 가장 기본적인 자료구조. 논리적 저장 순서, 물리적 저장순서가 서로 일치. 따라서 index(순서)로 해당 원소에 접근할 수 있다. 찾고자 하는 원소의 인덱스를 아련 random access 가 가능하다는 장점이 있는것이다.
​	그러나 만약 배열의 원소중 하나를 삭제 또는 삽입하는 과정에서 연속적 특지잉 깨지므로 빈 공간이 생기는데 이때 원소들을 shift 해줘야 하는 비용이 발생하고 이때 시간 복잡도 n이 된다. 

## LinkedList

이 부분을 해결하기 위한 자료구조가 LinkedList이다. 각각의 원소들은 자기 자신 다음에 어떤 원소인지만을 기억하고 있다. 따라서 이 부분만 다른 값으로 바꿔주면 삭제와 삽입을 O(1)만에 해결할 수 있다.
	하지만 LinkedList 역시 한가지 문제가 있다. 원하는 위치에 삽입을 하고자ㅏ 하면 원하는 위치를 Search 과정에 있어서 첫번째 원소부터 다 확인해봐야한다는 것. Array와 달리 논리적 저장순서가 물리적 저장순서와 동일하지 않다. 이 자료구조는 Tree 의 근간이 된다.