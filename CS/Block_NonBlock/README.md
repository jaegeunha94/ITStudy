# 블록 논블록
행위자가 취한 행위 자체가,  
또는 그 행위로 인해 다른 무엇이 막혀버린, 제한된, 대기하는 상태.

대개의 경우에는 나 이외의 대상으로 하여금 내가 Block 당하겠지만(Blocked),  
어찌 되었든 문자 자체로는 나라는 단일 개체 스스로의 상태를 나타낸다.

호출된 함수가 자신이 할 일을 모두 마칠 때까지  
제어권을 계속 가지고서 호출한 함수에게 바로 돌려주지 않으면 Block

호출된 함수가 자신이 할 일을 채 마치지 않았더라도  
바로 제어권을 건네주어(return) 호출한 함수가 다른 일을 진행할 수 있도록 해주면 Non-block



# 싱크 어싱크
동시에 발생하는 것들(always plural, can never be singular).

동시라는 것은 즉, 시(time)라는 단일계(system)에서 같이,  
함께 무언가가 이루어지는 두 개 이상의 개체 혹은 이벤트를 의미한다고 볼 수 있겠다.

호출된 함수의 수행 결과 및 종료를 호출한 함수가  
(호출된 함수뿐 아니라 호출한 함수도 함께) 신경 쓰면 Synchronous

호출된 함수의 수행 결과 및 종료를  
호출된 함수 혼자 직접 신경 쓰고 처리한다면(as a callback fn.) Asynchronous


## 🔖Non-blocking & Synchronous
호출된 함수가 호출한 함수에게 제어권을 바로 건네주어(return)  
호출한 함수가 다른 업무를 볼 수 있었음(Non-blocked)에도 불구하고  
여전히 호출된 함수의 업무 결과에만 줄곧 함께(synchronously) 신경쓰느라  
제 할 일을 못 하게 되는 상태


## 🔖블럭/동기 
A가 실행되다가 B라는 일을 수행하는 함수를 호출해서 B를 시작한다. 
B라는 일이 끝나면 함수를 리턴한다.  

A와 B는 순차적으로 진행되기 때문에 동기이며,  
B라는 일을 하는 함수를 호출하고 그 일이 끝나고 나서야 리턴되므로 블럭된 것이다.


## 🔖블럭/비동기 
일단 A는 B라는 일을 시킨다. 그리고 바로 리턴하고  
(여기서는 논블럭)  B는 일을 시작하고, A도 자신의 일을 한다.  

A는 중간에 B라는 일이 하는 중간 결과를 보고 받아서 처리해야한다.  
A는 B에게 요청을 해서 중간결과를 기다린다(블록),   
요청의 결과를 받고 나서 그 결과를 이용해서 A는 자신의 일을 처리한다.  
동시에 B 는 또 자신의 일을 동시에 한다. 

(비동기) A는 다시 B에게 중간결과를 요청해서 기다린다 (블록) ,  
요청의 결과를 받고 A는 자신의 일을 , B는 자신의 일을 한다. 반복된다.  

이 글을 읽고, 사실 갸우뚱 해야한다. 중간에 블록되는 동안에는 "동기" 라고 말 할 수 있기 때문이다.    
즉 어느 한 순간에 대해 해석하자면 틀릴 수도 있는것이다.   
즉 처음부터 말해왔듯이 "정답"이 존재하지 않는다.   
다만 이런 패턴들이 분명히 사용되고 있구나라고 감을 잡는게 목적이다.



## 🔖논블럭/동기 
이것도 역시 A는 B라는 일을 시킨다. 바로 리턴한다. (논블럭)   
B는 일을 시작하는데, A는 자신의 일을 하지 않는다.   

A의 하는 일이란 그저 B가 하는일을 확인하는 것이다.  
B가 결과 보고(중간 보고가 아니다) 를 했는지를 확인하는 함수를 호출하고,   
바로 리턴한다 (논블럭) 즉 결과 보고를 받을 때 까지 기다리는게 아니라,  
결과 보고가 나왔는지 확인하고 바로 리턴하는 것이다.   

이 짓을 계속한다. 즉 함수를 계속 논블럭으로 호출되긴 하나,  
A는 그저 B를 염탐할 뿐이다.  

이 상태를 말한다.  
이후에 B가 결과보고를 하면,B는 자신의 일이 끝난 것이고 A는 이제서야 자신의 일을 처리하게 된다.

## 🔖논블럭/비동기 
간단하다. A는 B의 일을 시작시키고 바로 리턴한다 (논블럭)  
그리고 A와B는 각자 자신의 일을 한다 (비동기) 



# 참조
[hamait tistory](https://hamait.tistory.com/930)
