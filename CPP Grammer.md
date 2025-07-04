# 빌트인 데이터 타입

빌트인 타입은 헤더 파일을 포함하지 않아도 C++에서 사용할 수 있는 기본 내장 데이터 타입입니다.

여러 빌트인 데이터 타입 중, 정수형 비트 연산은 특정 경우에 효율적임에도 익숙하게 사용하지 못하는 것 같아 개념을 다시한번 복습했습니다.

## 정수형 비트 연산

```jsx
int a = 13;
int b = 4;

cout << a & b << endl; // 4
cout << a || b << endl; // 13
```

AND(&) 연산자는 동일 위치의 bit가 둘 다 1인 경우에만 1, 그 외의 경우는 0으로 표시합니다.

OR(||) 연산자는 동일 위치의 bit가 둘 중 하나라도 1이면 1, 그렇지 않으면 0으로 표시합니다.

![Image](https://github.com/user-attachments/assets/86beabb5-3a56-4030-a1a6-7d853618db0b)

참고. Byte 크기를 반환하는 메소드 sizeof(…)

## 형 변환

### 암시적 형변환(Implict Conversion)

타입이 서로 다른 변수 간 연산을 할 때 발생하는 형 변환입니다.

보통 변수의 메모리 크기가 큰 쪽으로 타입을 변환합니다. → 작은 쪽으로 변환하면 잘리는 데이터가 많아서 오차가 커짐.

### 명시적 형변환(Explict Conversion)

사용자가 임의로 변경하는 형 변환입니다.

C++에서는 static_cast<타입>( ) 메소드를 통해 형 변환이 가능합니다.

```jsx
static_cast<int>(var);
```

## 문자열

### 초기화

문자열을 초기화하는 방법

- 대입 연산자 활용
- 생성자로 다른 문자열 복사(부분 복사 가능)
- 특정 문자를 반복하는 패턴의 문자열 생성 가능

```jsx
string str1 = "Hello"; // Hello
string str2(str1,0,3); // Hell
strign str3(3,'*'); // ***
```

### 문자열 찾기

find() 메소드 활용 → 시간 복잡도 O(N)

- find(찾으려는 문자열)
- find(찾으려는 문자열, 탐색 시작 위치)

⇒ 문자열을 찾으면 문자열의 시작 인덱스 반환

⇒ 찾지 못하면, string::npos 반환

size_t : 컨테이너나 문자열의 크기 및 인덱스를 표현할 때 사용하는 타입

### 문자열 추가

- ‘+’ 연산자 ,  ‘+=’ 연산자 사용

### 문자열 수정

- '[ ]' 연산자를 통해 임의 접근 후 수정
- replace( ) 메소드를 활용
    - 세 개의 인수 필요 → 시작 위치, 시작 위치부터 대체할 문자열 개수, 대체할 문자열
    - 시간복잡도 O(N) (N은 대체할 문자열의 길이)

---

# STL(Standard Template Library)

STL은 C++에서 제공하는 템플릿 기반의 표준 라이브러리입니다.

헤더 파일로 라이브러리를 불러오면 사용할 수 있으며, 어떤 타입에서도 동작합니다.

컨테이너, 알고리즘, 반복자로 이루어져 있습니다.

## 상수 레퍼런스

C++에서는 함수의 인수로 값을 전달할 때 값을 복사하기 때문에, 아래 코드처럼 main()의 value 값을 modify()의 인자로 넘겨 값을 수정하여도, main()의 value 값은 수정되지 않습니다.

```jsx
#include <iostream>
using namespace std;

void modify(int value) {
	value =  10;
	cout << "주소 : " << &value << endl;
	cout << "값 : " << value << endl;
}

int main()
{
	int value = 5;

	cout << "주소 : " << &value << endl;
	cout << "값 : " << value << endl;

	modify(value);
	cout << "값 : " << value << endl;
}
```

그 이유는, modify( )가 호출되면 value라는 변수(main의 value와는 다름)가 생성되고, 인자로 넘어 온 값이 복사됩니다. 

그렇기 때문에, modify( )에서 value 값을 수정하여도 modify( )가 종료되면 새로 생성된 value 변수가 사라지기에 main( )의 value에 영향을 미치지 못합니다.

![Image](https://github.com/user-attachments/assets/28397644-55ab-41ff-af77-1f13a364fe8b)

실제 값이 바뀌게 하려면 두가지 방식을 활용할 수 있습니다.

- 레퍼런스 연산자 & 활용
- 포인터 활용

### 레퍼런스 연산자 &

```jsx
#include <iostream>
using namespace std;

void modify(int& value) {
	value = 10;
	cout << "주소 : " << &value << endl;
	cout << "값 : " << value << endl;
}

int main()
{
	int value = 5;

	cout << "주소 : " << &value << endl;
	cout << "값 : " << value << endl;

	modify(value);
	cout << "값 : " << value << endl;
}
```

연산자 & 는 두가지 기능이 있습니다.

- 주소를 구하는 연산자의 기능
- 레퍼런스 연산자의 기능

위의 코드에서는 변수 선언에 & 연산자가 사용되었기 때문에, 레퍼런스 연산자의 기능으로 사용되었습니다.

레퍼런스 연산자를 활용해 참조자를 선언하였기 때문에, modify( )에서 main( )의 value에 접근하여 수정할 수 있습니다.

### 포인터 활용

```jsx
#include <iostream>
using namespace std;

void modify(int* value) {
	*value =  10;
	cout << "주소 : " << value << endl;
	cout << "값 : " << *value << endl;
}

int main()
{
	int value = 5;

	cout << "주소 : " << &value << endl;
	cout << "값 : " << value << endl;

	modify(&value);
	cout << "값 : " << value << endl;
}
```

포인터 문법을 활용하면 마찬가지로 main( )의 value를 modify( )에서 수정할 수 있습니다.

그러나, 레퍼런스 연산자 &를 활용하는 방식이 좀 더 효율적입니다.

### 레퍼런스 연산자 VS 포인터 차이점
<img width="5016" height="1924" alt="Image" src="https://github.com/user-attachments/assets/36896a3f-0196-4495-8d8c-07bc0c7dae33" />

- 레퍼런스 연산자를 활용하면 참조 변수와 참조 대상 변수의 주소값이 일치하기에, 추가적으로 메모리 값을 읽고 쓰는 추가 문법 필요 없음.
- 포인터를 활용할 경우, 주소값을 전달받은 변수의 주소와 실제 변수의 주소값이 다르기에, 포인터 문법을 통해 주소값을 받아와야 함.

=> 포인터를 사용하면 의도하지 않은 예외 발생 가능. 잘못된 주소에 접근할 수도 있음.

=> 포인터 문법은 간접 참조를 하므로, 주소를 얻을 때와 값을 얻을 때의 문법이 달라서 상대적으로 문법이 좀 더 복잡함.
