#  Garbage Collector

자바에서는 메모리를 GC라는 알고리즘을 통하여 관리하기 때문에 개발자가 메모리를 처리하기 위한 로직을 만들 필요가 없고, 만들어서도 안된다.

### Minor GC

새로 생성된 대부분의 객체(Instance)는 Eden 영역에 위치한다. Eden 영역에서 GC가 한 번 발생한 후 살아남은 객체는 Survivor 영역 중 하나로 이동된다. 이 과정을 반복하다가 계속해서 살아남아 있는 객체는 일정시간 참조되고 있다는 뜻이므로 Old영역으로 이동시킨다.

### Major GC

Old 영역에 있는 모든 객체들은 검사하여 참조되지 않은 객체들을 한꺼번에 삭제한다. 시간이 오래 걸리고 실행 중 프로세스가 정지된다. 이것을 'Stop-the-world'라고 하는데 Major GC가 발생하면 GC를 실행하는 스레드를 제외한 나머지 스레드는 모두 작업을 멈춘다.  GC 작업을 완료한 이후에야 중단했던 작업을 다시 시작한다.

#### 가비지 컬렉션은 어떤 원리로 소멸시킬 대상을 선정하는가?

알고리즘에 따라 동작 방식이 매우 다양하지만 공통적인 원리가 있다. Garbage Collector는 힘 내의 객체 중에서 가비지(Garbage)를 찾아내고 찾아낸 가비지를 처리해서 힙의 메모리를 회수한다. 참조되고 있지 않은 객체(Instance)를 가비지라고 하며 객체가 가비지인지 아닌지 판단하기 위해서 reachability라는 개념을 사용한다. 어떤 힙 영역에 할당된 객체가 유효한 참조가 있으면 reachability, 없다면 unreachability로 판단한다. 하나의 객체는 다른 객체를 참조하고, 다른 객체는 또 다른 객체를 참조할 수 있기 때문에 참조 사슬이 형성이 되는데, 이 참조 사슬 중 최초에 참조한 것을 Root Set이라고 칭한다. 힙 영역에 있는 객체들은 총 4가지 결우에 대한 참조를 하게 된다.

1. 힙 내의 다른 객체에 의한 참조

2. Java 스택, 즉 Java 메서드 실행 시에 사용하는 지역변수와 파라미터들에 의한 참조

3. 네이티브 스택(JNI, Java Native Interface)에 의해 생성된 객체에 대한 참조

4. 메서드 영역의 정적 변수에 의한 참조

   2, 3, 4는 Root Set이다. 

인스턴스가 가비지 컬렉션의 대상이 되었다고 해서 바로 소멸이 되는 것은 아니다. 빈번한 가비지 컬렉션의 실행은 시스템에 부담이 될 수 있기 때문에 성능에 영향을 미치지 않도록 가비지 컬렉션 실행 타이밍은 별도의 알고리즘을 기반으로 계산이 되며, 이 계산결과를 기반으로 가비지 컬렉션이 수행된다.

### Serial GC

적은 메모리와 CPU 코어 개수가 적을 때 적절한 방식.

#### Parallel GC

기본적인 GC 알고리즘은 Serial GC와 동일하지만 Parallel GC는 GC를 처리하는 스레드가 여러 개라서 보다 빠른 GC를 수행할 수 있다. 메모리가 충분하고 코어의 개수가 많을 때 유리하다.

#### Parallel Old GC(Parallel Compacting GC)

JDK 5 update 6 부터 제공한 GC방식이다. 별도로 살아있는 객체를 식별한다는 부분에서 보다 복잡한 단계로 수행된다. 
+Councurrent Mark & Sweep GC(이하 CMS)
+G1(Garbage Frist) GC

## Garbage Collection Algorithms

Java 코드를 작성함에 있어서 JVM에 대해 이해를 하고 작성하는 것은 중요하다. 어떤 Object 를 Garbage로 볼 것이냐에 대한 이해가 글을 읽는데 도움이 된다. Garbage Collector는 객체를 Reachable과  Unreachable의 상태로 구분한다. 구분하는 방법은 **Root Set**과의 관계로 판단한다. Root Set으로부터 어떤 식으로든 Reference 관계가 있다면 Reachable Object라고 판단하고 Root Set으로부터 참조하는 Reference 관계가 없다면 Unreachable  Object라고 판단한다. Root Set을 간단하게 설명하면 객체들 간에 참조 사슬의 시작점이라고 보면 된다. 

Root Set은 아래의 세 가지 형태로 나뉜다.

1. JVM Stack 내의 Local Variable Section(??)과 Operand Stack에서의 참조(Java 메서드 내에서 실행하는 지역 변수 또는 파라미터에 의한 참조)
2. JVM Method Area의 Constant Pool에서 참조(정적 변수에 의한 참조)
3. 아직 메모리에 남아 있는 Native Method로 넘겨진 Object에서 참조(JNI에 의해 생성된 객체에 대한 참조)

위 세 가지 형태의 Root Set으로부터 이어진 참조 사슬에 포함되어 있으면 Reachable Object이고 그렇지 않은 것은 Unreachable Object이다. Reachability에 대한 자세한 내용은 Java Reference와 GC를 읽어보는걸 추천한다.

**Garbage 대상을 식별하고 -> 식별된 대상을 메모리에서 해제한다.**

1. Reference Counting Algorithm
2. Mark and Sweep Algorithm
3. Mark and Compact Algorithm
4. Copying Algorithm
5. Generational Algorithm

### Reference Counting Algorithm

Garbage의 Detection에 초점이 맞추어진 초기 Algorithm이다. 각 Object마다 Reference Count를 관리하여 Reference Count가 0이 되면 GC를 수행한다.

![스크린샷 2019-11-05 오후 9.20.00](/Users/user/Desktop/스크린샷 2019-11-05 오후 9.20.00.png)

위 그림에서 a와 b는 각각 100, 99의 값을 갖는 Integer Object로 선언되어있다. 각 Integer Object는 Reference Count가 1로 설정되어 있다.  이 상태에서 a=b로 변경하게 되면 a가 참조하고 있던 100의 값을 갖는 Integer Object는 참조가 없어지기 때문에 Reference Count가 0으로 감소하게 되고 GC가 수행하게 된다. 
이 방식은 Reference Count가 0이 될 때마다 GC가 발생하기 때문에 Pause Time이 분산되어 실시간 작업에도 거의 영향을 주지 않고 즉시 메모리에서 해제된다는 장점이 있으나 각 Object마다 Reference Count를 변경해 주어야 하고 참조를 많이 하고 있는 Object의 Reference Count가 0이 될 경우 연쇄적으로 GC가 발생할 수 있는 문제가 있고 Reference Count의 관리 비용도 크다. 또한 Linked List와 같은 순환 참조 구조에서 Memory Leak이 발생할 가능성도 크다.![스크린샷 2019-11-05 오후 10.25.49](/Users/user/Desktop/스크린샷 2019-11-05 오후 10.25.49.png)

위와 같은 순환 참조 구조에서 a로부터의 참조가 끊어졌다 해도 다른 노드로부터의 참조가 남아 있기 때문에 Reference Count가 0이 되지 않게 되고 그로 인해 Garbage 대상이 되지 않아 Memory Leak으로 이어지게 된다.

### Mark-and-Sweep Algorithm

Reference Counting Algorithm의 단점을 극복하기 위해 나온 Algorithm이다. 이 방식은 Garbage Detection은 Root Set에서 시작하는 Reference의 관계를 추적한다. 그래서 Tracing Algorithm이라고도 하며 이름에서 유추할 수 있듯이 Mark Phase와 Sweep Phase로 나뉘게 된다.
Mark Phase에서는 Garbage 대상이 아닌 살아남아야 할 Object에 Marking하는 방식으로 수행되며 Marking을 위해 각 Object의 Object Header에 Flag나 별도의 BitmapTable등을 사용한다. Mark Phase가 끝나면 곧바로 Sweep Phase로 진입하는데 이 단계에서는 Marking정보를 활용하여 Marking되지 않은 Object를 지우는 작업을 한다. 그리고 Sweep이 완료되면 살아남은 모든 Object의 Marking정보를 초기화 한다.![스크린샷 2019-11-05 오후 10.31.15](/Users/user/Desktop/스크린샷 2019-11-05 오후 10.31.15.png)

위 그림에서 Before는 GC가 일어나기 전의 상황이고 이후 Sweep Phase를 거치게 되면 Marking되지 않은 Object들이 지워지게 된다. Sweep이 완료되면 살아남은 모든 Object들의 Marking정보는 초기화된다. Mark-and-Sweep은 Reference관계를 정확하게 파악하고 Reference 관계의 변경 시에 별도의 Overhead가 없어 속도가 향상되기는 하나 GC가 수행되는 도중 Mark작업의 **정확성**과 **Memory Courrpution**을 방지하기 위해서 Heap의 사용이 제한되기 때문에 Suspend 현상이 발생한다.
그리고 그림에서 볼 수 있듯이 GC 이후에 제거된 Garbage가 존재하던 메모리 공간이 듬성듬성한 상태가 되어 Heap 메모리 전체로 봤을 때 충분한 공간이 있는 것처럼 보이지만 메모리 할당이 불가능한 상태가 되어 OutOfMemoryException이 발생할 수 있다. 위와 같이 메모리가 조각난 상황을 **Fragmentation**이라고 한다.
Fragmentation은 메모리의 총량은 충분함에도 불구하고 새로운 Object를 할당할 수 없는 상황을 유발하고 Object를 할당하기 위해 적절한 메모리 블록을 찾는 과정을 지연시키기 때문에 애플리케이션의 성능에도 영향을 끼친다. 우리가 컴퓨터의 성능을 개선하기 위해 디스크 조각모음을 했던 시절을 떠올려보면 이해가 쉬울것 같다.

### Mark-and-Compact Algorithm

Mark-and-Sweep Algorithm이 가지고 있는 Fragmentation이라는 약점을 극복하기 위해 나온 Algorithm이다. Sweep 대신 Compact이라는 용어를 사용하였지만 Sweep 이 사라진 것은 아니고 Compact Phase 안에 포함되어 있다. 아래 그림은 Mark, Sweep, Compact의 과정을 잘 나타내고 있다.![스크린샷 2019-11-05 오후 11.11.12](/Users/user/Desktop/스크린샷 2019-11-05 오후 11.11.12.png)

Mark와 Sweep의 단계는 이전 Mark-ans-Sweep Algorithm과 동일하나 Compact 과정을 한 단계 더 거치게 된다. 그림에서 볼 수 있듯이 Compaction 작업은 Sweep 이후 Garbage였던 Object들이 사라지고 남은 빈자리들을 살아남은 Object들로 연속된 메모리 공간에 차곡차곡 적재하는것을 의미한다. Compaction 작업을 통해 메모리 공간의 효율을 높일 수 있다. 하지만 Compaction 작업 이후 살아남은 모든 Object들의 Reference를 업데이트하는 작업이 필요하기 때문에 부가적인 Overhead가 수반된다.

### Copying Algorithm

Copying Algorithm도 Fragmentation의 문제를 해결하기 위해 제시된 또 다른 방법이다. 현대의 Garbage Collector가 차용하고 있는 Generational Algorithm이 Copying Algorithm을 발전시킨 형대이다. Copying Algorithm의 기본 아이디어는 Heap을 Active영역과 InActive영역으로 나누어 Active영역에만 Object를 할당할 수 있게 하고 Active영역이 꽉차게 되면 GC를 수행한다는 것이다. GC를 수행하면 Suspend 상태가 되고 살아남은 Live Object를 Inactive 영역으로 Copy하는 작업을 수행한다. Copy하는 동안 프로그램이 Suspend 상태가 되기 때문에 Stop-the-Copying이라고도 부른다.

Copy 작업을 완료하면 Active영역에는 Garbage Object를 제거하게 되면 Active영역은 Free Memory 상태가 되고 Active 영역과 Inactive 영역이 서로 바뀌게 된다. 이를 Scavenge이라고 하며 Active영역과 Inactive영역은 물리적인 구분이 아니라 논리적인 구분일 뿐이다. 

![스크린샷 2019-11-06 오후 8.30.00](/Users/user/Desktop/스크린샷 2019-11-06 오후 8.30.00.png)

그림을 보면 Region A, Region B이라고 표기하였는데, Active, Inactive는 개념적인 구분일 뿐이기 때문에 단순히 Heap 영역을 반으로 나누어 한쪽에만 Object를 할당한다고 이해하면 된다. 앞에서 Copying Algorithm도 Fragmentation문제를 해결하기 위해 고안되었다고 하였다. Fragmentation문제를 해결하기 위해 Copy할 때 Live Object를 옮기는 과정에서 Object들의 Reference를 업데이트하면서 연속된 메모리 공간에 차곡차곡 적재시킨다. Copying Algorithm은 Fragmentation 방지에는 효과적이지만 전체 Heap의 절반 정도밖에 사용하지 못한다는 공간 활용의 비효율성, Suspend 현상, Copy에 대한 Overhead가 존재한다는 단점이 있다. 

### Generational Algorithm

Copying Algorithm을 사용하면서 대부분의 프로그램에서 생성되는 대다수의 Object들이 생성된 지 얼마 되지 않아 Garbage가 되는 짧은 수명을 가지고 있다는 것과 수명이 긴 몇개의 Object는 반드시 가지고 있다는 경험적 체득을 통해 고안된 Algorithm이다. 이 Algorithm은 아래의 두 가지 가정을 전제로 만들어졌다.

1) 대부분의 할당된 객체는 오랫동안 참조되지 않으며 즉 금방 Garbage 대상이 된다.
2) 오래된 객체에서 젊은 객체로의 참조는 거의 없다.

위와 같은 가설을 Weak generational hypothesis라고 한다. 이 가설을 바탕으로 Young Generation과 Old Generation이라는 영역으로 Heap을 구분지었다. ![스크린샷 2019-11-06 오후 8.40.16](/Users/user/Desktop/스크린샷 2019-11-06 오후 8.40.16.png)

위의 그림은 Young Generation과 Old Generation의 두 개의 Sub Heap으로 나뉜 것을 표현한 것이다. Object는 Young Generation에 할당되고 GC가 수행될 때마다 살아남은 Object에 **Age**를 기록한다. HotSpotJVM에서 이 Age의 임곗값의 기본값은 31이다. 그냥 몇 번 살아남았는지를 기록하는 것이라고 보면 되며 특정 임곗값을 넘어서게 되면 Old Generation으로 Copy하는 작업을 수행한다. 이를 Promotion이라고 하며 대부분의 객체는 Young Generation에서 살다가 Garbage가 되기 때문에 Copy 작업의 횟수를 최소화시킬 수 있다. 또한 Old Generation으로 Copy하며 Compaction 작업이 이루어진다. 우리가 주로 사용하고 있는 HotSpot JVM이 Generational Algorithm을 바탕으로 다음과 같이 Generational Heap을 구성하고 있다.

HotSpot JVM에서 제공하는 Garbage Collector들을 보면 Young Generation과 Old Generation에서 어떠한 Algorithm으로 GC를 수행하는지 명시되어 있으므로 Garbage Collector 가 취하는 GC 전략에 대해 이해할 수 있을 것이다. 

## Java Reference와 GC

JDK 1.2부터 java.lang.ref 패키지를 추가해 제한저깅나마 사용자 코드와 GC가 상호작용할 수 있게 되었다.
 여기에는 String 래퍼런스 외에도 soft, weak, phantom 3가지의 새로운 참조 방식을 Reference 클래스로 제공한다. 이 3가지 Reference 클래스를 애플리케이션에 사용하면 앞서 설명하였듯이 GC에 일정 부분 관여할 수 있고, LRU 캐시 같이 특별한 작업을 하는 애플리케이션을 더 쉽게 작성할 수 있다. 

Strong References는 root set (of references)로 부터 참조를 말한다. 이것들은 java.lang.ref 패키지를 사용하지 않은 일반적인 참조이다.
Strong reachable : root set으로부터 시작해서 어떤 reference Object도 중간에 끼지 않은 상태도 참조 가능한 객체, 
Softly reachable : string reachable 객체가 아닌 객체중에 하위 래퍼런스 없이 soft reference만 통과하는 참조 사슬이 하나라도 있는 객체
Weakly reachable: soft 이하 동문
Phantomly reachble: 위 3 객체 모두 해당되지 않는 객체, 이 객체는 파이널라이즈 되었지만 아직 메모리 회수되지 않은 상태.
Unreachable : root set으로부터 시작되는 참조 사슬로 참조되지 않은 객체 

Softly Reachable : GC가 동작할 때마다 회수되지 않지만 사용 빈도에 따라 그 GC여부가 결정되는 참조. 다음 수식에 의해 그 시간은 결정된다. (마지막 String reference가 GC된 때로부터 지금까지의 지간) > (옵션 설정값 N) * (힙에 남아있는 메모리 크기)

Weakly Reachable : GC를 수행할 때마다 회수 대상이 된다. LRU 캐시와 같은 어플리케이션에서는 이것을 소프트보다 많이 사용하는데 이유는 softly reachable 객체는 힙에 남아 있는 메모리가 많을 수록 회수 가능성이 낮기 때문에 다른 비즈니스 로직 객체들을 위해 어느정도 비워두어야 할 힙 공간이 softly reachable 객체에 의해 일정 부분 점유된다.

ReferenceQueue: 객체가 GC 대상이 되면 SoftReference, WeakReference 객체 내의 참조는 null로 설정되고 SoftReference 객체, WeakReference 객체 자체는 ReferenceQueue에 enQueue 된다. WeakHashMap 클래스는 이 ReferenceQueue와 WeakReference를 사용하여 구현되어 있다. Soft, weak는 referenceQueue를 사용할 수도 있고, 않을 수 있다. 이들 클래스의 생성자 중에 referenceQueue를 인자로 받는 생성자를 사용하냐 아니냐로 결정한다. 하지만 PhantomReferece는 반드시 이를 사용한다. 팬텀의 생성자는 단 하나이며 항상 ReferenceQueue를 인자로 받는다. 

Phantomly Reachable :위의 것들가 달리 이것은 파이널라이즈와 메모리 회수 사이에 관여한다. 객체에 대한 참조가 PhantomReference만 남게 되면 해당 객체는 바로 파이널라이즈된다. 

CMS GC : GC과정에서 발생하는 STW 시간을 최소화 하는데 초점을 맞춘 GC방식
Initial Mark -> Concurrent Mark -> Remark -> Concurrent Sweep과정이 있다.
Initial Mark = GC과정에서 살아남은 객체를 탐색하는 시작 객체(GC Root)에서 참조 Tree상 가까운 객체만 1차적으로 찾아가며 객체가 GC대상인지를 판단. 이 때 STW현상이 발생하게되지만, 탐색 깊이가 얕기 때문에 발생기간이 매우 짧다. 
Concurrent Mark = STW 현상없이 진행되며, Initial Mark 단뎨에서 GC 대상으로 판별된 객체들이 참조하는 다른 객체들을 따라가며 GC대상인지를 추가적으로 확인한다. 
Remark = Concurrent Mark 단계의 결과를 검증한다. Concurrent Mark 단계에서 GC대산으로 추가 확인되거나 참조가 제거되었는지 등등의 확인을 한다. 이 검증과정은 STW를 유발하기 때문에 STW지속시간을 최대한 줄이기 위해 멀티스레드로 검증 작업을 수행한다.
Concurrent Sweep = STW없이 Remark 단계에서 검증 완료된  GC 객체들을 메모리에서 제거한다.

G1 GC : 큰 메모리에서 좋은 성능(짧은 STW)을 내는 GC, 큰 힙 메모리에서 짧은 GC시간을 보장한다. 이는 앞서 살펴본 GC와는 다른 방식으로 힙 메모리를 관리한다. Eden, Suvivor, Old 영역이 존재하지만 고정된 크기로 고정된 위치에 존재하는 것이 아니며, 전체 힙 메모리 영역을 Region이라는 특정한 크기로 나눠서 각 Region의 상태에 역할(Eden, Suvivior, Old)이 동적으로 부여되는 상태이다.

JVM 힙은 2048개의 Region으로 나뉠 수 있으며 각 Region의 크기는 1~32mb사이로 지정될 수 있다. (G1HeapRegionSize로 설정 ) 여기에 새로운 영역이 추가되는데 두가지는 다음과 같다
1) Humongous : Region 크기의 50%를 초과하는 큰 객체를 저장하기 위한 공간이며, 이 Region 에서는 GC 동작이 최적으로 동작하지 않는다.
2) Available/Unused: 아직 사용되지 않은 Region을 의미한다.

G1 GC에서 Young GC를 수행할 때는 STW현상이 발생, 이를 줄이기 위해 멀티 스레드로 GC수행. Young GC는 각 Region중 GC대상 객체가 가장 많은 Region(Eden 또는 Survivor 역할)에서 수행되며, 이 Region에서 살아남은 객체를 다른 Region(Survivor 역할)으로 옮긴후, 비워진 Region을 사용가능한 Region으로 돌리는 형태로 동작.

G1 GC에서 Full GC가 수행될 때는 Initial Mark -> Root Region Scan -> Concurrent Mark - > Remark -> Cleanup -> Copy 단계를 거치게 된다.
1)Initial Mark = Old Region에 존재하는 객체들이 참조하는 Survivor Region을 찾는다 이 과정에서 STW현상이 발생한다.
2)Root Region Scan = Initial Mark에서 찾은 Survivor Region에 대한 GC 대상 객체 스캔 작업을 진행한다. 
3) Concurrent Mark = 전체 힙의 Region에 대해 스캔 작업을 진행하며, GC 대상 객체가 발견되지 않은 Region은 이후 단계를 처리하는데 제외되도록 한다.
4)Remark = 애플리케이션을 멈추고 STW. 살아있는 객체가 가장 적은 Region에 대한 미사용 객체 제거 수행. 이후 STW를 끝내고 앞선 GC과정에서 완전히 비워진 Region을 Freelist에 추가하여 재사용될 수 있게 한다.
5)Cleanup = 애플리케이션을 멈추고 STW. 살아있는 객체가 가장 적은 Region에 대한 미사용 객체 제거 수행. 이후 STW를 끝내고 앞선 GC과정에서 완전히 비워진 Region을 Freelist에 추가하여 재사용될 수 있게 한다.
5)Copy = GC 대상 Region이었지만 Cleanup 과정에서 완전히 비워지지 않은 Region의 살아남은 객체들을 새로운 Available/Unused Region에 복사하며 Compaction작업을 수행한다.  