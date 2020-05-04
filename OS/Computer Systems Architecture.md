# Computer Systems Architecture

## Multiprocessors

Parallel System 이라고도 부르며, CPU를 여러 개 둔 구조이다.
장점
1 성능 향상 : CPU가 많으니까, 더 많은 일을 할 수 있음.
2 경제성 : 각 프로세서 들은 하나의 메모리를 공유하기 때문에 경제적이다.
3 신뢰성 : 하나의 프로세서가 고장나더라도 다른 프로세서가 있기 때문에 신뢰성이 있다. 

## Asymmetric Multiprocessing(비대칭 멀티 프로세싱)

-하나의 프로세서(Master Processor)가 다른 프로세서(Slave Processor)들을 관리해준다.
-Master Processor는 메모리의 전체적인 부분에 접근할 수 있으며, Slave Processor는 각각의 메모리를 갖는다.
-Processor가 각각의 메모리를 갖기 때문에, Private memory를 갖게 된다.
-Master Processor는 시스템의 전반적인 부분을 제어하며, 나머지 프로세서들은 Master processor의 명령을 따른다.

## Symmetric Multiprocessing(대칭 멀티 프로세싱)

-프로세서 간의 Master-Slave 관계가 없음.
-각각의 프로세서들은 메모리 영역을 공유하고, 서로 간의 직접적인 상호작용을 한다.
-Non-private Memory

## Multiprocessor Memory Model

-Uniform Memory Access(UMA)
-> CPU들 간 Memory에 Access하는 시간이 같다.
-Non-uniform Memory Access(NUMA)
-> 프로세서와 관련된 메모리의 Location에 따라서 시간이 결정된다.

## Multi-core System

-하나의 칩에 프로그램을 수행할 수 있는 코어가 여러 개 있음
-새로운 패러다임을 가져야 한다. 병렬적으로 수행될 수 있는 프로그램을 만들어야 한다.

## Clustered System

-System을 여러개 두어서 서로를 연결해준다.
-Storage Area Network가 존재한다.