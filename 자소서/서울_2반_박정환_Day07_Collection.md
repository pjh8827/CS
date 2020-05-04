**public** **class** pjh5929 {
	**static** String *date* = "2020-3";
	**static** **int** *year* = 2020;
	**static** **int** *month* = 2;
	**static** **int** *day* = 0;
	**static** **int**[] *cal* = {0,31,28,31,30,31,30,31,31,30,31,30,31};	

​	**public** **static** **void** main(String[] args) {
​		System.***out\***.println(*getDay*(*year*, *month*)+" days for "+*year*+"-"+*month*);
​	}
​	
​	**public** **static** **int** getDay(**int** year, **int** month) {
​		System.***out\***.println(year+" "+month);
​		**if**(month!=2) **return** *cal*[month];
​		**else** **return** *getYun*(year, month);
​	}
​	**public** **static** **int** getYun(**int** year, **int** month) {
​		**if**((year%4==0)&&(year % 100 != 0) || (year % 400)==0) **return** 29;
​		**else** **return** 28;
​	}
}

2월 29, 28을 구별해 주기위한 함수하나를 추가해주었다. pjh5929 과제 끝