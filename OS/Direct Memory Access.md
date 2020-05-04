# DMA 모델

(사전지식)
I/O Structure
-I/O transaction은 Bus를 통해서 수행된다.
->Bus : 주소, 데이터, 제어신호 등을 옮기는 선들
->CPU에서 다른 Device에 접근할 수 있는 통로
-System Bus : CPU와 I/O Bridge를 연결해준다.
-Memory Bus : I/O Bridge와 Memory를 연결해준다.
-I/O Bus : 다양한 Device들 간의 연결해 줌.

## Direct Memory Access(직접 메모리 접근)

-CPU와 I/O Device들은 동시에 수행이 가능하다.
-하나 이상의 CPU와 Device Controller들은 공통된 버스를 통해서 공유 메모리와 연결된다. 
-Device Controller는 I/O operation이 완료되면 Interrupt를 통해서 작업이 끝남을 알려준다.

## Interrupt

DMA모델에서 I/O 연산을 끝내고 나면, Interrupt 신호를 통해서 CPU에 작업이 완료되었음을 알려준다. 이 때, CPU는 기존에 실행하던 작업을 멈추고, Interrupt Service Routine을 실행하게 된다. Interrupt가 수행되는 과정은 다음과 같다.

1. CPU는 Device controller로 부터 Interrupt 신호를 받음.
2. Interrupt 신호를 받게된 CPU는 기존에 하던 작업을 멈춤. 이 때, 기존의 수행되던 작업의 Program Counter의 주소가 메모리에 저장됨.
3. CPU는 Kernel Level로 들어가서 Interrupt Vector를 통해서 해당되는 Interrupt Service Routine을 찾음.
4. Interrupt Service Routine을 모두 수행한 CPU는 Interrupt로 부터 복귀하여 다시 원래 하던 작업을 수행함.(중단된 작업 재개)