---
title: "완전탐색"
date: 2023-11-05
mainSectionTitle: "hugo"
---
# Brute Force, 완전탐색
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