# expr & bc 명령어
쉘에서는 숫자와 문자를 구분하지 않기 때문에 여타 프로그래밍 언어 처럼 숫자 연산이 간단하지 않다.  
쉘에서 숫자를 연산하는 방법은 대표적으로 expr 와 bc 가 있다.

 * expr은 bash 내장 명령어로 정수만 연산 가능하다.
 * bc는 복잡한 공학용 산술과 실수 계산이 가능하다.

여기까지 보면, 차라리 bc만 쓰면 될것을 굳이 expr을 알아야 하는지 의문이겠지만,

정수 연산이 많은 프로그래밍을 할 경우 내장 명령어인 expr을 사용하는것이 속도면에서 효율적이기 때문이다.

# expr 명령어
expr 명령은 정수 계산을 하기 위해 사용되는 명령이다. 

계산식을 쓸 때 연산기호와 정수 사이 반드시 공백으로 띄어쓰기를 해야 한다는 주의 점이 있다.

```SHELL
$ expr 4 \* 3
12
$ expr 4 / 3
1
$ expr 4 % 3
1
```

```BASH
#!/usr/bin/bash
 
# 34*51의 결과를 result 변수에 저장
result=`expr 34 \* 51`
echo "$result"
```

> Tip  
> expr 명령어를 배쉬에서 사용할때 역따옴표로 묶는걸 잊지말자

# bc 명령어
bc 명령은 expr 명령처럼 수치 연산을 하기 위해 사용되는 명령이지만,  
차이점이 있다면 실수 연산과 사인, 코사인 등 공학용 계산기에 나오는 연산 기능을 사용할 수 있다.

여러 수식들을 괄호로 묶고 나누고 곱할때 expr을 그대로 사용하면  
계산이 제대로 안되기 때문에 bc 명령어를 사용하는 것이다.

문법도 역시 다른데, 산술식 뒤에 파이프로라인(|)으로  
계산식을 넘겨서 처리한다. ( | grep 과 같이 생각하면 된다.)

> Tip  
> bc는 bash calculator 약자이다.

```BASH
# 34.8 + 51.2를 더하고 이 값을 제곱하는 약간 복잡한 수식
echo "(34.8+51.1)^2" | bc
```

```BASH
# 단 나눗셈을 할때는 정수로 출력이 되어버린다.
$ echo "4 / 3" | bc
1
 
# -1 옵션을 줘서 계산결과를 실수로 출력
$ echo "4 / 3" | bc -l
1.33333333333333333333
 
# scale 옵션을 줘서 출력 소숫점을 고정
$ echo "scale=3; 4 / 3" | bc
1.333
```

```BASH
# 정확도가 높은 실수 계산을 bc -l을 사용한다.
# 여러 연산을 동시에 하려면 세미콜론(;)을 사용하고, 마지막 연산 값을 last 또는 점(.)으로 활용할 수 있다.
$ a=1.1
$ b=2.2
$ echo "$a/$b; ./3; .+1.11" | bc -l
.50000000000000000000
.16666666666666666666
1.27666666666666666666
 
 
# 지수도 함께 사용할 수 있다.
$ echo "e(1)" | bc -l
2.71828182845904523536
 
 
# 루트와 함께 사용 할 수 있다.
$ echo "sqrt(10^2)" | bc
10
```
 

또한 bc 명령어는 논리 조건문 에도 쓰이는 편이다.

```BASH
#!/usr/bin/bash
 
var=2
 
# echo 2 > 1.1 | bc 명령이 실행되어 참(1) 값을 반환
if [ `echo $var > 1.1 | bc` -eq 1 ] 
then
	echo "var > 1.1" # 1 -eq 1 은 참이니까 실행
else
	echo "var <= 1.1"
fi
 
 
# echo 2 == 2 && 2 > 1 | bc 명령이 실행되어 참(1) 값을 반환
if [ `$var == 2 && $var > 1 | bc` -eq 1 ]
then
	echo true
fi
```

# 참조
[inpa tistory](https://inpa.tistory.com/entry/LINUX-%F0%9F%93%9A-expr-bc-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%A0%95%EB%A6%AC-%EC%A0%95%EC%88%98-%EC%8B%A4%EC%88%98-%EC%97%B0%EC%82%B0)