## 맵(Map)

a.k.a. 딕셔너리

`key-value` 쌍을 저장하는 자료구조

### 특징

- `Key`는 유일함(중복 불가)
- Key를 통해 Value에 빠르게 접근할 수 있음
- Key기준 자동 정렬 가능, 삽입 시 마다 자동 정렬
- 레드 - 블랙 트리로 구현
- 대괄호 연산자로 Key로 직접 참조 가능

### 시간복잡도

- 참조 : $O(logN)$
- 탐색 : $O(logN)$
- 삽입 / 삭제 : $O(logN)$

### 활용

- 빈도 카운팅
  ```cpp
  // 문자열에서 각 문자의 개수 세기
  string str = "hello world";
  map<char, int> freq;

  for(char c : str) {
      freq[c]++;
  }

  // 결과
  // ' ': 1
  // 'd': 1
  // 'e': 1
  // 'h': 1
  // 'l': 3
  // 'o': 2
  // 'r': 1
  // 'w': 1
  ```

## 셋(Set)

중복을 허용하지 않는 값들의 모음

### 특징

- 중복 값 자동 제거
- 자동 정렬, 삽입순서 유지 가능
- 값의 존재 여부를 빠르게 확인할 수 있음

### 시간복잡도

- 참조 : $O(logN)$
- 탐색 : $O(logN)$
- 삽입 / 삭제 : $O(logN)$

### 활용

```cpp
// O(N²) 방식
vector<int> v = {1, 2, 3, 4, 5};
for(int i = 0; i < v.size(); i++) {
    for(int j = 0; j < v.size(); j++) {
        if(v[i] == target) { /* ... */ }
    }
}

// O(N log N) 방식 - set 사용
set<int> s(v.begin(), v.end());
if(s.find(target) != s.end()) {
    // 존재함
}
```
