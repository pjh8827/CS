# TCP와 UDP

전송계층에서 사용되는 통신규약으로서 사용되는 환경에 따라 TCP와 UDP로 나뉘게 된다.

## UDP(User Datagram Protocol)

UDP 데이터 중심 프로토콜로서 주고받는 통신보다 데이터를 일방적으로 보내는 것을 중요시 한다. 따라서 데이터 전송의 신뢰성이 보장되지 않지만 그만큼 가볍고 단순한 구조이고 속도가 빨라 실시간으로 통신할 수 있는 장점이 있다. 보통 p2p나 스트리밍, 전화 같은 경우에 사용된다.

## TCP(Transmissor Control Protocol)

TCP 흐름 중심 프로토콜로서 서로가 통신을 주고 받는 것을 중요시한다. 따라서 데이터 전송의 안전을 신경쓰기 때문에 중간에 패킷이 손실되는 경우 재전송을 통해 (SYN-ACK handshaking) 신뢰성을 보장할 수 있다. 하지만 그만큼 전송속도가 느리다는 단점이 있다. TCP 프로토콜은 거의 대부분의 통신에서 사용되고 있으며, 특히 파일이나 데이터 전송시에 사용된다.

### TCP의 신뢰성 보장이 어떻게 이루어지는가

3way-handshaking과 혼잡제어, 흐름제어를 통해 신뢰성을 보장한다.

## 흐름제어(control flow)

TCP가 신뢰성 보장을 위해 사용하는 메커니즘 중 하나로서 송신측과 수신측의 속도 차이를 해결하기 위해 사용하는 메커니즘이다. 대표적으로 Stop and Wait ARQ(Automatic Repeat Request), Sliding Window 기법이 있다.

### Stop and Wait ARQ(정지대기방식)

 한번에 하나씩 긍정 응답(ACK)을 받고, 후속 데이터 전송. 가장 단순하나, 다소 비효율적이며 반이중 방식에서도 가능하다. 매번 패킷을 보내고 난 후 확인 응답을 받아야만 패킷을 전송하는 방식.

### Sliding Window

창의 크기를 조절하여 여러 패킷을 논리적인 하나의 패킷으로 묶어서 처리하는 방식. 데이터의 빠른 송수신을 위해서 사용한다. 수신측에서 설정한 윈도우 크기만큼 확인 없이 세그먼트를 전송하도록 하여 데이터 흐름을 동적으로 조절하는 방식

### Window

데이터를 보내기 전 3way-handshaking을 통해 수신측이 데이터를 받을 수 있는 버퍼양과 송신측이 데이터를 보낼 양을 맞추게 되는데 이 데이터 양을 Window size라고 한다.

### Go-Back-N ARQ

누적 응답을 사용한 방식으로 응답신호가 손실되더라도 이후 순서의 응답신호를 받으면 Window가 shift된다. timer가 하나이기때문에 순서가 낮은 프레임이 손실되면 이후의 모든 프레임을 제전송한다. 수신측의 window 사이즈가 1로 설정되어 있어 프레임을 순차적으로만 수신할 수 있다.

### Selective Repeat ARQ

선택 응답을 사용한 방식으로 window의 가장 처음 프레임의 응답신호를 받아야 window가 shift된다. 각 프레임당 timer가 동작하고 순서가 낮은 프레임이 손실되더라도 해당 프레임만 재전송한다. 송신측과 수신측의 Window size가 같기 때문에 수신측에서 프레임 순서와 상관없이 수신이 가능하다.

## 혼잡제어(congestion control)

TCP가 신뢰성 보장을 위해 사용하는 메커니즘 중 하나로서 네트워크의 혼잡을 피하기 위해 송신자의 전송속도를 줄이기 위해 사용하는 메커니즘이다. 대표적으로 AIMD, Slow Start, Fast Retransmit, Fast Recovery 기법이 있다.

### AIMD(Additive Increase/Multiplicative Decrease)

Window size를 1부터 시작하여 하나씩 증가시킨다. 혼잡상태가 감지되면 window size를 절반으로 감소시킨다.

### Slow Start

window size를 1부터 시작해서 매회 2배씩 증가시킨다. 혼잡이 발생하면 window size를 1로 감소시킨 후 지수적으로 증가시킨다. 혼잡이 발생했던 window size의 절반부터는 선형적으로 증가시켜 나간다.

### Fast Retransmit

수신자가 프레임을 순서대로 받지 못했을 경우 순서대로 받은 프레임 중 가장 최근 프레임에 대한 ACK 신호를 보내게 된다. 이때 송신자는 3개 이상의 중복된 ACK를 받을 경우 timeout까지 기다리지 않고 바로 패킷을 재전송함으로서 시간을 절약한다. 이런현상이 반복되면 혼잡 상태로 인지하여 window size를 감소시킨다.

### Fast Recovery

Slow Start처럼 window size를 증가시키다가 혼잡상태를 만나면 window size를 1이 아닌 절반으로 감소시킨후 선형적으로 증가시키는 방법이다.