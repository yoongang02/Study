## 배열

- 같은 타입의 원소들을 효율적으로 관리할 수 있는 기본 자료형.
- C++에서는 STL의 벡터를 더 자주 사용함.

### 배열 선언

기본 선언문 외에도 아래와 같이 추가적인 선언문은 기억해 놓으면 편리함.

```cpp
// 0과 1 인덱스는 특정 값 초기화, 그 외의 인덱스는 0으로 초기화
int arr1[5] = {1,2};
// 모든 인덱스 값 0으로 초기화
int arr2[5] = {};
```

### 배열과 차원

2차원, 3차원 등 다차원 배열을 사용하는 경우가 많음.

하지만, 컴퓨터 메모리 구조는 1차원이기 때문에 차원과는 무관하게 메모리에 연속적으로 할당 됨.


>💡int 형은 크기가 4byte이기에, 주소가 4씩 증가
>
>double 형은 크기가 8byte이기에, 주소가 8씩 증가
>
>char 형은 크기가 1byte이기에, 주소가 1씩 증가
>
>이렇게, 메모리 주소가 연속적이기 때문에 인덱스를 통해 임의 접근할 수 있는 것임.


### 배열 연산의 시간복잡도

- 데이터 접근 시
    
    배열은 임의 접근이라는 방법으로 모든 위치의 데이터에 단 한 번에 접근 가능 → O(1)
    
- 데이터 삽입 시
    
    삽입 위치에 따라 달라짐.
    
    - 맨 뒤에 삽입 → O(1)
    - 맨 앞에 삽입 → 데이터들이 뒤로 밀려야 함. → O(N)
    - 중간에 삽입 → 일부 데이터들이 뒤로 밀려야 함 → O(N)

### 배열 선택 시 고려해야 할 점

- 할당할 수 있는 메모리 크기 확인! 보통 정수형 1차원 배열은 1,000만 개, 2차원 배열은 3000 * 3000 크기를 최대로 생각함.
- 중간에 데이터 삽입이 많은지를 확인! 중간 혹은 맨 처음에 데이터 삽입이 빈번하면 시간 복잡도가 높아져서 시간 초과 발생 가능.

→ C++에서는 STL에서 제공하는 벡터를 자주 사용함.

---

## 예제

### 1. 배열 정렬하기

>💡정수 배열 arr을 오름차순으로 정렬해서 반환하는 solution( ) 함수를 작성하기.
>
> < 제약 조건 >
>
>- arr의 길이는 2이상 10^5 이하
>- arr의 원소 값은 -100,000 이상 100,000 이하
>- 제한 시간 3초

여기서 주의해야 할 점은 arr의 최대 길이가 10^5 라는 것임. 

**일반적으로 알고리즘 문제에서 기준이 되는 연산 처리 속도는 1초당 약 10⁸번의 연산** (C++, Java 기준)

따라서, 일반적으로 이중 반복문을 통한(버블 정렬) 배열 정렬은 시간 초과를 야기함.

앞서 배운, sort( ) 함수를 활용하면 제한 시간 내에 정렬 가능

```cpp
// sort() 이용
vector<int> solution(vector<int> arr) {
	sort(arr.begin(), arr.end());
	return arr;
}

// 버블 정렬 이용
void bubbleSort(vector<int>& v) {
	for (size_t i = 0; i < v.size() - 1; ++i) {
		for (size_t j = 0; j < v.size() - i - 1; ++j) {
			if (v[j] > v[j + 1]) {
				swap(v[j], v[j + 1]);
			}
		}
	}
}
```

### 2. 배열 제어하기

>💡정수 배열 lst의 중복 값을 제거하고, 배열 데이터를 내림차순으로 정렬해서 반환하는 solution 함수 작성하기.
>
> <제약 조건>
>
>- lst의 길이는 2이상 1,000 이하
>- lst의 원소 값은 -100,000 이상 100,000 이하

```cpp
// 내가 작성한 코드
vector<int> solution(vector<int> lst) {
	// 내림차순으로 정렬
	sort(lst.rbegin(), lst.rend());
	// 중복 제거하기
	lst.erase(unique(lst.begin(), lst.end()), lst.end());

	return lst;
}

// 책에서의 정답 코드
bool compare(int a, int b) {
	return a > b; // b가 a 보다 더 클 경우 false를 출력하고, 순서를 바꾸게 하기 위해서 <- 내림차순 정렬
}

vector<int> solution2(vector<int> lst) {
	// 내림차순으로 정렬
	sort(lst.begin(), lst.end(),compare);
	// 중복 제거하기
	lst.erase(unique(lst.begin(), lst.end()), lst.end());

	return lst;
}
```

코드에서는 사용자 정의 함수를 이용해서 내림차순하는 방법을 알려주었는데, 앞에서 배운 것처럼 역방향 반복자를 이용해서 정렬하는 것이 더욱 간편해보이는 것 같음. 하지만, 조건이 복잡해지면 사용자 정의함수를 이용하여 정렬하는 방법은 매우 편리할 것 같음.

### 3. 두 수 뽑아서 더하기

>💡정수 배열 numbers에서 서로 다른 인덱스에 있는 2개의 수를 뽑아 더해 만들 수 있는 보든 수를 배열에 오름차순으로 담아 반환하는 solution 함수를 완성하기

```cpp
// 내가 쓴 코드
vector<int> solution(vector<int> numbers) {
	vector<int> temp;
	// 서로 다른 인덱스에 있는 2개의 수를 뽑아서 더해서 새로운 벡터에 insert하기
	for (int i = 0; i < numbers.size() - 1; ++i) {
		for (int j = i + 1; j < numbers.size(); ++j) {
			temp.push_back(numbers[i] + numbers[j]);
		}
	}
	// 오름차순으로 정렬
	sort(temp.begin(), temp.end());
	// 중복 제거하기
	temp.erase(unique(temp.begin(), temp.end()), temp.end());

	return temp;
}

// 책에서의 정답 코드
vector<int> solutionAnswer(vector<int> numbers) {
	set<int> temp;
	
	for (int i = 0; i < numbers.size() - 1; ++i) {
		for (int j = i + 1; j < numbers.size(); ++j) {
			temp.insert(numbers[i] + numbers[j]);
		}
	}

	vector<int> result(temp.begin(), temp.end());
	return result;
}
```

문제를 통해 중복 값이 존재해서는 안된다는 숨은 조건을 잘 파악하였으나, 내 코드에서 한가지 아쉬운 점이 있었음. 자동으로 오름차순 정렬과 중복 값을 제거해주는 set 컨테이너를 활용하는 것이 훨씬 간단한 방법이었음. 

→ STL에서 제공하는 컨테이너의 각 특징을 잘 기억하고 항상 문제에서 어떤 컨테이너가 가장 적합하고 효율적일지 고민하면서 풀어야 함.
