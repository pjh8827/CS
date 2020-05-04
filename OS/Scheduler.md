# 스케줄러

프로세스를 스케쥴링하기 위한 Queue에는 세가지 종류가 존재한다.
Job Queue, 현재 시스템 내에 있는 모든 프로세스의 집합
Ready Queue, 현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는 프로세스의 집합
Device Queue, Device I/O 작업을 대기하고 있는 프로세스의 집합
각각의 Queue에 프로세스들을 넣고 빼주는 스케줄러에도 크게 세가지 종류가 존재한다.

## 장기스케줄러(Long-term scheduler or job scheduler)

메모리는 한정되어 있는데 많은 프로세스들이 한꺼번에 메모리에 올라올 경우, 대용량 메모리(일반적으로 디스크)에 임시로 저장된다. 이 Pool에 저장되어 있는 프로세스 중 어떤 프로세스에 메모리를 할당하여 Ready Queue로 보낼지 결정하는 역할을 한다.

1)메모리와 디스크 사이의 스케줄링을 담당.
2)프로세스에 Memory(및 각종 리소스)를 할당(admit)
3)degree of Multiprogramming 제어(메모리에 여러 프로그램이 올라가는 것) 몇 개의 프로그램이 올라갈 것이지를 제어
4)프로세스의 상태 new-> ready(in memory)

## 단기스케줄러(Short-term scheduler or CPU scheduler)

1)CPU와 메모리 사이의 스케줄링을 담당.
2)Ready Queue에 존재하는 프로세스 중 어떤 프로세스를 Running시킬지 결정.
3) 프로세스에 CPU를 할당(scheduler dispatch)
4) 프로세스의 상태  ready-> running->waiting->ready

## 중기스케줄러(Medium-term scheduler or Swapper)

1)여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫒아냄(Swapping)
2) 프로세스에게서 Memory를 Deallocate
3) degree of Multiprogramming 제어
4) 현 시스템에서 메모리에 너무 많은 프로그램이 동시에 올라가는 것을 조절하는 스케쥴러
5)ready->suspended

Process state-suspended
Suspended(stopped): 외부적인 이유로 프로세스의 수행이 정지된 상태 메모리에서 내려간 상태를 의미한다. 프로세스 전부 디스크로 Swap out 된다. Blocked 상태는 다른 I/O 작업을 기다리는 상태이기 때문에 ready state로돌아갈 수 있지만 이 상태는 외부적인 이유로 suspending되었기 때문에 스스로 돌아갈 수 없다.

