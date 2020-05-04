# 로드밸런싱(Load balancing)

가상 IP를 통하여 하나의 서비스를 여러대의 서버가 분산 처리하는 메커니즘을 말한다. 대표적으로 하나의 서버에 발생하는 트래픽이 많을 경우 서버의 부하량과 속도저하를 해소하거나 하나의 서버에서 장애가 발생하더라도 서비스가 중단되지 않고 지속하기 위해 사용됩니다.

## 분산처리를 어떻게 하는가

먼저 서버의 대표IP를 Virtual IP(가상 IP)로 설정한다. Virtual IP로 통해 들어오는 패킷들을 L4 또는 L7 스위치를 통해 분석한다. L4 스위치의 경우 포트를 분석하여 알맞는 서버를 찾아 보내고, L7 스위치의 경우 포트 뿐만 아니라 이메일 또는 파일 제목, url까지 분석하여 패킷을 분산처리한다. 이때의 각각의 서버에 트래픽을 균등하게 보내기 위해 Round Robin, Least Connection, Response Time, Hash 등의 기법으로 분산시킨다.

## Round Robin

각 서버에 session을 순차적으로 맺어주는 방식이다. 모든 클라이언트를 동일하게 취급하고, 각 서버별 처리량을 기억하고 있어야한다.

## Least Connection

클라이언트와 서버별 연결된 Connection 수를 고려하여 가장 적은 서버에 Connection을 맺는 방식이다.

## Weighted Least Connections

Least Connection 방식에서 서버에 가중치를 추가한 것으로 open된 Connection 수가 같을 경우 가중치가 높은 서버에게 우선 분배하는 방식이다.

## Response Time

서버의 응답시간에 대한 학습을 통하여 응답시간이 빠른 서버에 Connection을 우선 분배하고 응답이 느린 서버에는 Connection을 적게 분배하는 방식이다.

## Hash

Hash알고리즘을 적용하여 특정 서버에 Connection을 연결한 클라이언트는 다음 연결에도 같은 서버에 Connection을 맺는 방식이다. 한번 성립된 Session을 유지할 수 있는 장점이 있다.  