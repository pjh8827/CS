# Big-Endian vs Little-Endian

## Endianness 정의

컴퓨터의 메모리와 같은 1차원의 공간에 여러개의 연속된 대상을 배열하는 방법

## Big-Endian

최상위 바이트(Most Significant Byte)부터 차례로 저장하는 방식

<-높은 주소 - - - - 낮은 주소 ->

사람이 읽고 쓰는 방법과 같아서 디버깅이 쉬우나, 수가 커질 경우 메모리에 저장된 데이터를 오른쪽으로 옮겨야하는 단점이 있다.

## Little-Endian

최하위 바이트(Least Significant Byte)부터 차례로 저장하는 방식

<-낮은 주소 - - - - 높은 주소 ->

디버깅은 어려울 수 있으나 수가 커지더라도 오버헤드가 발생하지 않는다.

EX) 0000ACED => ED부터 읽는것과 00부터 읽어서 불필요한 부분을 읽는것