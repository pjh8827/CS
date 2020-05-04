## Call By Value

ex) int x = 10; 
	modify(x);
	public void modify(int data){ data=20;}

선언한 변수(x)는 스택메모리에 쌓인다.
호출된 함수 내의 파라미터(data)도 스택메모리에 쌓인다.
호출함수 내에서 data 값이 변하면 그 값은 스택메모리에 쌓이고 실제 바꾸고싶은 변수는 바뀌지 않는다.



## Call By Reference

ex) Rectangle r1 = new rectangle();
	r1.length = 10;
	modify(r1);	  
	public void modify(Rectangle r2){ r2.length=20;}

선언한 객체의 변수(r1)는 힙영역에 있는 인스턴스를 레퍼런스하고 있습니다.
호출된 함수 내의 파라미터(r2) 역시 힙영역에 r1이 가리키는 인스턴스를 레퍼런스합니다. 따라서 함수에서 값을 변경시 실제로 변경됩니다.

