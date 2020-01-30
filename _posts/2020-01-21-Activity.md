---
title: "2020-01-21 일과"
date: 2020-01-21 16:06:28 -0400
categories:
  - Activity
---

| **Number** | **Problem** | **Solution** | **Algorithm_Site** |
| :---: | :---: | ------- | ------------------------------------------ |
| 2565 | **전깃줄** | DP | [2565 : 전깃줄 (baekjoon)][전깃줄] |
| 12865 | **평범한 배낭** | DP | [12865 : 평범한 배낭 (baekjoon)][배낭] |
| 1219 | **길찾기** | BFS | [길찾기 (SWEA)][길찾기] |
| \- | **주식 가격** | Stack | [주식 가격 (Programmers)][주식가격] |

\- DP 문제에 대한 아이디어가 안 떠오르는 것이 문제!<br/>
\- 못 푼 문제 : https://programmers.co.kr/learn/courses/30/lessons/42747 <- 문제가 상당히 난잡함<br/>
\- 마크다운 쉽게 작성하는 사이트 : https://stackedit.io/editor <br/>

[전깃줄]: https://www.acmicpc.net/problem/2565
[배낭]: https://www.acmicpc.net/problem/12865
[길찾기]: https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV14geLqABQCFAYD
[주식가격]: https://programmers.co.kr/learn/courses/30/lessons/42584

<br/>

## 전깃줄
```C++
// 전깃줄을 끊는 것보다
// 전깃줄을 최대한 많이 잇는다.

// 앞 전봇대를 기준으로 정렬하고
// 뒷 전봇대의 LIS를 구하는 것이 풀이법

#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main()
{
	vector<pair<int, int>> Line;

	int N;
	cin >> N;

	for (int i = 0; i < N; i++)
	{
		pair<int, int> Temp;
		cin >> Temp.first;
		cin >> Temp.second;

		Line.push_back(Temp);
	}

	sort(Line.begin(), Line.end());

	// LIS
	int DP[100] = { 0, };
	int Max = 0;
	for (int i = 1; i < N; i++)
	{
		int Last = i;
		for (int j = i - 1; j >= 0; j--)
		{
			if (DP[Last] <= DP[j] && Line[j].second < Line[i].second)
				Last = j;
		}
		if (Last != i)
			DP[i] = DP[Last] + 1;

		if (DP[i] > Max)
			Max = DP[i];
	}

	cout << N - (Max + 1) << endl;
}

```

## 배낭문제
```C++

#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int DP[101][100001] = { 0, };

int main()
{
	int N, K;
	cin >> N >> K;

	vector<pair<int, int>> Stuffs;

	for (int i = 0; i < N; i++)
	{
		pair<int, int> Temp;
		cin >> Temp.first;
		cin >> Temp.second;

		Stuffs.push_back(Temp);
	}

	for (int i = 1; i <= N; i++)
	{
		for (int j = 1; j <= K; j++)
		{
			if (Stuffs[i - 1].first <= j)
				DP[i][j] = max(Stuffs[i - 1].second + DP[i - 1][j - Stuffs[i - 1].first], DP[i - 1][j]);
			else
				DP[i][j] = DP[i - 1][j];
		}
	}

	cout << DP[N][K] << endl;
}
```

## 길찾기
```C++
#include <iostream>
#include <vector>
#include <queue>

using namespace std;


int main()
{
	for (int t = 0; t < 10; t++)
	{
		int testcase;
		cin >> testcase;

		int N;
		cin >> N;

		bool Graph[100][100] = { false, };

		for (int i = 0; i < N; i++)
		{
			int x, y;
			cin >> x >> y;

			Graph[x][y] = true;
		}

		queue<int> BFS;

		// BFS
		bool IsOk = false;
		BFS.push(0);
		do
		{
			int target = BFS.front();

			if (target == 99)
			{
				IsOk = true;
				break;
			}
			for (int i = 0; i < 100; i++)
			{
				if (Graph[target][i])
				{
					Graph[target][i] = false;
					BFS.push(i);
				}
			}
			BFS.pop();

		} while (BFS.size());

		if (IsOk)
			cout << "#" << testcase << " " << 1 << endl;
		else
			cout << "#" << testcase << " " << 0 << endl;
	}
}
```

## 주식 가격
```C++

// 스택으로 풀지 못해 다른 사람의 소스를 참고하였다(주석처리).
#include <string>
#include <vector>

using namespace std;

vector<int> solution(vector<int> prices) {
    vector<int> answer;
    
    for(int i = 0; i < prices.size(); i++)
    {
        bool Great = true;
        for(int j = i+1;j < prices.size();j++)
        {
            if(prices[i] > prices[j])
            {
                Great = false;
                answer.push_back(j - i);
                break;
            }
        }
        if(Great)
            answer.push_back(prices.size() - i - 1);
    }
    
    return answer;
}

/*
#include <string>
#include <vector>
#include <stack>

using namespace std;

vector<int> solution(vector<int> prices) {
	vector<int> answer(prices.size());
	stack<int> s;
	int size = prices.size();
	for (int i = 0; i < size; i++) {
		while (!s.empty() && prices[s.top()] > prices[i]) {
			answer[s.top()] = i - s.top();
			s.pop();
		}
		s.push(i);
	}
	while (!s.empty()) {
		answer[s.top()] = size - s.top() - 1;
		s.pop();
	}
	return answer;

}
*/
```
