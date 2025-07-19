## 스택(Stack)

### 정의

스택이란?

- 먼저 입력한 데이터를 제일 나중에 꺼낼 수 있는 자료구조
- 후입선출, LIFO(Last In First Out)의 특징을 갖고 있음

### ADT(Abstract Data Type)


>💡추상 자료형(ADT)란?
>
>- 인터페이스만 있고 실제로 구현되지 않은 자료형
>- 자료형의 설계도와 같아서, 자료형의 동작 원리를 이해할 때 유용함


| 구분 | 정의 | 설명 |
| --- | --- | --- |
| 연산 | boolean isFull( ) | 스택 내의 데이터 개수가 maxSize와 같은지 확인.<br>같으면 true, 같지 않으면 false를 반환함. |
|  | boolean isEmpty( ) | 스택 내의 데이터 개수가 하나 이상 있는지 확인.<br>비어있으면 true, 하나라도 있으면 false를 반환함. |
|  | void push(ItemType item) | 스택에 데이터를 넣음. |
|  | ItemType pop( ) | 스택에서 가장 위에 있는 아이템을 빼서, 반환함. |
| 상태 | int top( ) | 스택에서 가장 최근에 넣은 데이터 위치를 기록함. |
|  | ItemType data[maxsize] | 스택의 데이터를 관리하는 배열.<br>최대 maxSize 개의 데이터를 관리함. |

### 세부 동작 이해하기

1. Push(3 )
    
    ![Image](https://github.com/user-attachments/assets/52fead7d-9716-4661-b15a-74f807c1b631)
    
    - IsFull( )을 통해 배열이 가득 차있는지 확인.
    - 가득 차있지 않다면 top을 1 증가
    - top이 가리키는 위치에 입력 값을 넣음.
2. Pop( )
    
    ![Image](https://github.com/user-attachments/assets/dad3b034-7490-4adc-a9c2-c4c8d5be3397)
    
    - IsEmpty( )를 통해 배열이 비어 있는지 확인.
    - 비어있지 않다면 top을 1 감소
    - 감소 전 위치에 있는 값을 반환


>💡데이터를 그냥 저장하고, 순서와 상관없이 임의 접근만 한다면 배열을 사용
>
>최근에 삽입한 데이터를 대상으로 연산이 필요하다면 스택을 이용
>
>C++에서는 pop( ) 을 실행할 경우, 최근에 넣은 원소를 삭제한 후 아무것도 반환하지 않음을 명심



---

## 예제

### 1. 괄호 짝 맞추기


>💡열린 괄호 ‘(’ 와 닫힌 괄호 ‘)’가 무작위로 뒤섞인 문자열 s가 있을 때,
>
>괄호가 정상적으로 열고 닫혔는지 판별하는 solution 함수를 구현하기
>
>정상적으로 열고 닫혔다면 true를, 그렇지 않다면 false를 반환



< 내 풀이 >

```cpp
#include <iostream>
#include <stack>
#include <string.h>

using namespace std;

bool solution(string s) {
	stack<char> stack1;
	stack<char> stack2;
	// 입력받은 문자열의 각 char을 stack에 삽입
	for (char c : s) {
		stack1.push(c);
	}

	while (!stack1.empty()) {
		if (stack2.empty()) {
			stack2.push(stack1.top());
			stack1.pop();
		}
		else {
			char c1 = stack1.top();
			char c2 = stack2.top();
			if (c1 == '(' && c2 == ')') {
				stack1.pop();
				stack2.pop();
			}
			else {
				stack2.push(stack1.top());
				stack1.pop();
			}
		}
	}

	return stack2.empty();
}
```

<img width="776" height="160" alt="Image" src="https://github.com/user-attachments/assets/27e05584-e0cf-414e-9aa7-de10ac84a43f" />

올바르게 출력 됨을 확인함.

< 모범 답안 >

```cpp
#include<stack>
#include<string>

using namespace std;

bool solution(string s){
	stack<char> stack;
	
	for(char c : s){
		if(c == '(')){
			stack.push(c);
		}
		else if(c == ')'){
			if(stack.empty()){
				return false;
			}
			else{
				stack.pop();
			}
		}
	}
	return stack.empty();
}
```

< 풀이 비교>

<img width="2314" height="1031" alt="Image" src="https://github.com/user-attachments/assets/676c39dd-9f87-4993-a436-bc9461bb2eb7" />

1. 내 풀이
    - 스택1에 입력 string을 char 형태로 모두 푸시함.
    - 스택1이 모두 빌 때까지 아래의 과정을 반복함.
        - 스택1이 비어있지 않고, 스택2가 비어있으면 스택1 데이터를 pop하고 스택2에 push함.
        - 스택2가 비어있지 않으면 비교 시작, 스택1의 top이 ‘(’ 이고 스택2의 top이 ‘)’ 이면 각각 pop.
            
            그 외의 경우라면 스택1의 top을 pop하여 스택2에 push
            
    - 스택1이 모두 비었을 때, 스택2도 비어있다면 true 리턴, 아니라면 false 리턴
2. 모범 답안 풀이
    - string을 char에 따라 순환.
    - 현재 인덱스의 데이터가 ‘(’ 면 스택에 푸시
    - 현재 인덱스의 데이터가 ‘)’ 인데, 스택이 비어있으면 false 리턴.
        
        스택이 비어있지 않으면 스택의 데이터를 pop
        
    - string을 모두 순환하였을 때 stack이 비어있다면 true, 비어있지 않다면 false 리턴
3. 풀이 비교
    - 스택을 이용하였다는 점은 같으나, 스택 개수 사용에 차이가 있음. 스택을 하나만 사용한 것이 더욱 효율적임.
    - 모범 답안이 괄호의 특성을 더욱 잘 이용한 풀이라고 생각 됨.
        
        특히, 스택이 비어있을 때 ‘ ) ‘ 가 먼저 나오면 절대 괄호가 닫힐 수 없기 때문에 바로 false를 리턴함으로써 조기 반환이 되도록 코드를 구성한 것이 배워야 할 점이라고 생각 됨.
