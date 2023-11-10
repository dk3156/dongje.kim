---
title: "완전탐색"
date: 2023-11-05
mainSectionTitle: "hugo"
---
### Brute Force, 완전탐색
https://www.youtube.com/watch?v=9aHVA7mJcqQ 참고.
모든 경우의 수를 전부 탐색하는것.
세가지 종류가 있다 
* 포문 : for 문으로 모든 경우의 수를 본다. for 문의 개수는 임의로 정해도 되고 재귀를 서서 조건이 만족할 때까지 불러도 된다. 1초 제한 문제(코테) 에는 먹히지 않는다.
```
for(int i=0; i<n; i++)
	for(int j= i + 1; j < n; j++)
		sum = arr[i] + arr[j];
```
* 재귀 : 백트래킹이라고도 하며, 재귀가 끝나고 특정 조건을 다시 원위치시키는 것을 말한다. 다시 말하면 조건을 만족시키지 못한 경우의 수를 버리고 다시 parent recursion call 로 돌아간다는 것. (backtrack)

* 백트래킹 파라미터로 리스트가 있고, 귀납이 리턴되서 부모 귀납 단계로 다시 올라올때 변하지 않는 리스트를 사용하고 싶다면 리스트를 복사해서 자식 귀납 함수에 파라미터로 정의해 줘야한다.
리스트를 복사할때, List[:] 로 슬라이싱해도 되고, 간단하게 List.copy() 로 복사해도 된다.


* 순열 : 순열 조건을 만족하는 수를 구하시오 이런 식의 문제에서 next_permutation 알고리즘을 쓰면 된다. (c++ 의 경우)

```
<include algorithm>

do{

	for(int i=0; i<4; i++){
		cout << v[i] << " ";
	}

	cout << '\n';

}while(next_permutation(v.begin(),v.end()));
```

next_permutation 다음 순열이 없으면 false 를 리턴한다. 벡터의 수열을 in place 바꿔줌.

```
do{

	for(int i=0; i<4; i++){
		cout << v[i] << " ";
	}

	cout << '\n';

}while(prev_permutation(v.begin(),v.end()));
```

pre_permutation 은 전 수열로 돌아가는 방식으로 next_permutation 순서 꺼꾸로 바꾼다고 생각하면 된다.

### 완전탐색 재귀함수
숫자가 들어간 String 에서 꺼낼 수 있는 모든 조합의 숫자 - 예를들어, "17" 이 주어졌을때 만들 수 있는 수는 1, 7, 17, 71
를 위한 재귀함수. 방향을 보고 나중에 써먹을 수 있게 하기.
```
def recursive(combination, string):
	if combination != "":
		numberSet.add(int(combination))

	for i in range(0, len(string)):
		combination += string[i]
		recursive(combination, string[:i] + string[i+1:])
```

### 정규표현식

* []
or: 대괄호 안의 모든 문자를 말함
* [^]
not: 대괄호 안의 문자 외의 모든 문자를 말함
* ^[]
대괄호 안의 문자로 시작하는 문자열을 말함
* []$
대괄호 안의 문자로 끝나는 문자열을 말함
* /+
1개 이상의 문자를 말함