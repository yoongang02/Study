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

---

## STL 컨테이너(Container)

STL의 컨테이너는 데이터를 저장하는 객체를 의미합니다.

코딩 테스트 문제를 풀 때, 입력값을 가장 효율적으로 제어할 수 있는 컨테이너를 사용하면 효율적인 알고리즘을 구현할 수 있습니다.

### 1. 벡터(Vector)

벡터는 배열과 유사한 특징을 갖고 있습니다.

- 데이터를 순차적으로 저장함.
- 인덱스를 통해 특정 위치의 원소에 쉽게 접근할 수 있음.

백터는 ‘[ ]’ 연산자를 통해, 원소를 변경할 수 있습니다. → 시간복잡도 O(1)

벡터의 내부는 배열로 구성되어있기 때문에, 맨 뒤에 원소를 삽입/삭제 하는 것은 효율적이나, 맨 앞에 원소를 삽입/삭제하는 것은 비효율적입니다. (맨 앞에 원소를 삽입/삭제 하는 것은 덱 활용)

### 2. 셋(Set)

셋은 중복을 허용하지 않고, 저장된 데이터를 자동으로 정렬하는 컨테이너입니다.

집합이라고 표현할 수도 있습니다.

셋은 ‘균형 이진 트리’로 구성되어있기 때문에, 이 구조를 통해 항상 정렬 상태를 유지할 수 있습니다.

→ 이로 인해, 원소 탐색/삽입/삭제의 시간 복잡도는 O(logN)입니다.

<셋에서 원소 탐색>

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
	set<int> numbers = {1,2,3,4,5}
	int target = 3;
	
	auto it = numbers.find(target);
	if(it != numbers.end()) {
		cout << "원소를 찾았습니다" << endl;
	}
	else {
		cout << "원소를 찾지 못했습니다" << endl;
	}
	return 0;
}
```

셋에서 원소를 탐색 할 때에, 셋 내에 찾고자 하는 원소가 없다면 **end 반복자**를 반환합니다.

(참고) end 반복자는 컨테이너 마지막 원소의 다음 주소이며, 대부분의 STL은 탐색 함수 혹은 메소드를 찾지 못했을 경우, end 반복자를 반환합니다.

### 3. 맵(Map)

맵은 **키와 값**을 쌍으로 갖는 컨테이너입니다. 

키와 값의 쌍을 entry라고 부르며, STL에서는 **std::pair 타입**으로 표현합니다. pair 객체의 멤버 변수 first는 키 정보를 의미하며, second는 값 정보를 의미합니다.

**셋과 마찬가지로, 내부는 ‘균형 이진 트리’로 구성되어있기 때문에, 키 값을 기준으로 데이터가 자동으로 정렬됩니다. → 시간복잡도 O(logN)**

<맵에서 특정 키에 접근>

맵에서 특정 키에 접근하는 방식은 두 가지 방식이 존재하나, 두 방식에 차이가 있습니다.

- ‘[ ]’ 연산자 활용
- find( ) 메소드 활용

→ ‘[ ]’ 연산자를 통해 접근하는 키 값이 맵 내에 존재하지 않을 경우, 맵에 해당하는 키를 추가합니다. 만약, 키를 추가하고 싶지 않다면 ‘[ ]’ 연산자가 아닌 find( ) 메소드를 사용합니다.

### 4. 정렬되지 않은 셋과 맵

위에서 언급했듯이 셋과 맵은 ‘이진 탐색 트리’로 구성되어 있기 때문에, 항상 정렬 상태를 유지합니다.

하지만, 정렬이 필요하지 않다면 정렬 상태를 유지하는 것이 성능 저하를 일으킵니다.

따라서, C++에서는 **정렬되지 않은 셋(unordered_set)**과 **정렬되지 않은 맵(unordered_map)**을 제공합니다. 

→ 보통의 경우, 삽입/삭제/탐색의 시간 복잡도가 O(1)이나, 최악의 경우는 O(N)입니다. 하지만 최악의 경우는 거의 발생하지 않기에, 기존의 O(logN)에 비해 빠른 성능을 갖습니다.

```cpp
#include <iostream>
#include <unordered_set>
#include <unordered_map>
using namespace std;

int main() {
	unordered_set<int> set1;
	unordered_map<int,string> map1;
	
	// set1 원소 삽입
	set1.insert(2);
	set1.insert(5);
	set1.insert(9);
	set1.insert(1);
	// set2 원소 삽입
	map1[3] = "hamburger";
	map1[1] = "pizza";
	map1[4] = "coke";
	map1[8] = "icecream";
	
	// set1 원소 출력
	for(int num : set1){
		cout << num << endl;
	}
	
	// 출력 결과
	// 2
	// 5
	// 9
	// 1
	
	// set2 원소 출력
	for(const auto& pair : map1){
		cout << pair.first << " : " << pair.second << endl;
	}
	
	// 출력 결과
	// 3 : hamburger
	// 1 : pizza
	// 4 : coke
	// 8 : icecream
	return 0;
}
```

직접 빌드를 해서 실행해본 결과, 출력 값이 삽입한 순서대로 출력되었습니다.

이에, ***항상 삽입한 순서대로 출력이 되는건가? 하는 의문***이 들어 추가적으로 찾아보았습니다.

### ➕ unordered_set과 unordered_map의 내부 구조

책에서 unorderd_set과 unorderd_map에 대해 배우며, 궁금한 점이 3가지 생겼습니다.

1. 항상 삽입한 순서대로 출력이 되는 것인가?
    
    → No. 랜덤한 순서로 출력이 됩니다.
    
2. 이 컨테이너가 어떠한 경우에 필요한 것인가?
    
    → 삽입/삭제/탐색에서 시간복잡도 O(1)를 갖기에, 최적화가 필요할 때 사용 가능합니다.
    
3. 어떻게 시간복잡도가 O(1)를 가질 수 있는가?
    
    → 삽입/삭제/탐색의 과정에서 ‘해시 함수’를 사용하기 때문입니다.
    

![Image](https://github.com/user-attachments/assets/68352eaa-3bbd-4f73-8bb6-aeb3cf4f7a96)

- 같은 원소를 해시 함수에 전달하면 같은 값을 리턴하기에, 같은 곳에 위치함.
- 해시 함수 계산이 상수 시간 내에 처리하기에, 삽입/삭제/탐색의 시간복잡도가 O(1)이 가능한 것임.
- 해시 함수 계산으로 인해, 서로 다른 원소가 같은 값을 리턴할 경우, 즉 ‘해시 충돌’이 발생한다면 해당 위치의 원소들을 모두 탐색해야하기 때문에 시간복잡도가 O(N)이 됨. 여기서 N은 원소의 개수를 의미함. 이 경우가 최악의 경우이나, 최악의 경우는 흔히 발생하지 않음.
- 따라서, 보통 안전하게 set과 map을 사용하지만 최적화가 필수적으로 필요한 경우에는 해시 충돌이 발생하지 않도록 해시 함수를 잘 정의하여 사용할 수 있음.
