---
title: "2020-01-14 일과"
date: 2020-01-14 18:06:28 -0400---
categories:
  - Activity
---


| **Number** | **Problem** | **Solution** | **Algorithm_Site** |
| :---: | :---: | ------- | ------------------------------------------ |
| \- | **쇠막대기** | Stack | [쇠막대기 (Programmers)][쇠막대기] |
| \- | **위장** | Hash | [위장 (Programmers)][위장] |
| \- | **프린터** | Priority Queue | [프린터 (Programmers)][프린터] |
| \- | **괄호 짝짓기** | Stack | [괄호 짝짓기 (SWEA)][괄호 짝짓기] |

### 모든 경우의 수 구하기
\- (A + 1)(B + 1)(C + 1) - 1 = A + B + C + AB + AC + BC + ABC <br/>
<br/>
\- 영상처리 책을 새로 사야하는가? 공부는 해야함<br/>
\- 못 푼 문제 : https://www.acmicpc.net/problem/2565<br/>
\- 마크다운 쉽게 작성하는 사이트 : https://stackedit.io/editor <br/>

[쇠막대기]: https://programmers.co.kr/learn/courses/30/lessons/42585
[위장]: https://programmers.co.kr/learn/courses/30/lessons/42578
[프린터]: https://programmers.co.kr/learn/courses/30/lessons/42587
[괄호 짝짓기]: https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV14eWb6AAkCFAYD&categoryId=AV14eWb6AAkCFAYD&categoryType=CODE

<br/>

## 쇠막대기
```C++
#include <string>
#include <vector>

using namespace std;

int solution(string arrangement) {
	int answer = 0;

	bool Open = false;
	int Count = 0;
	int Max = 0;
	for (int i = 0; i < arrangement.size(); i++)
	{
		if (!Open)
		{
			if (arrangement[i] == '(')
			{
				Open = true;
				Count++;
				Max++;
			}
		}
		else
		{
			if (arrangement[i] == '(')
			{
				Count++;
				Max++;
			}
			else if (arrangement[i] == ')')
			{
				// Laser
				if (arrangement[i - 1] == '(')
				{
					if (Count == 1)
					{
						Count = 0;
						Max = 0;
						Open = false;
					}
					else
					{
						Max--;
						Count--;
						answer += Count;
					}
				}
				else
				{
					if (Count == 1)
					{
						Count = 0;
						answer += Max;
						Max = 0;
						Open = false;
					}
					else
						Count--;
				}
			}

		}
	}
	return answer;
}
```

## 위장
```C++

// 아무것도 안 입은 것만 제외하는 것이 핵심
// 각 의상마다 안 입었다는 의상 하나를 추가
// 각 의상 그룹을 곱하고 아무것도 안 입었을 때만 제외

#include <string>
#include <vector>

using namespace std;

int solution(vector<vector<string>> clothes)
{
	vector<vector<string>> Clothes_Hash;

	for (int i = 0; i < clothes.size(); i++)
	{
		bool IsBelong = false;

		for (int j = 0; j < Clothes_Hash.size(); j++)
		{
			if (clothes[i][1] == Clothes_Hash[j][0])
			{
				Clothes_Hash[j].push_back(clothes[i][0]);
				IsBelong = true;
				break;
			}
		}

		if (!IsBelong)
		{
			vector<string> Temp;
			Temp.push_back(clothes[i][1]);
			Temp.push_back(clothes[i][0]);
			Clothes_Hash.push_back(Temp);
		}
	}

	int Total_Cases = 1;

	for (int i = 0; i < Clothes_Hash.size(); i++)
		Total_Cases *= Clothes_Hash[i].size();

	return Total_Cases - 1;
}
```

## 프린터
```C++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<int> priorities, int location)
{
	int Print = 0;
	int Large_Num_Loc = 0;
	int Large_Num = *max_element(priorities.begin(), priorities.end());
    
	while (Print < priorities.size())
	{
		int Last_Large_Num_Loc = Large_Num_Loc;
		
		for (int i = 0; i < priorities.size(); i++)
		{
			int index = Last_Large_Num_Loc + i < priorities.size() ? Last_Large_Num_Loc + i : Last_Large_Num_Loc + i - priorities.size();
			if (priorities[index] == Large_Num)
			{
                priorities[index] = 0;
				Large_Num_Loc = index;
				Print += 1;
				if (location == index)
					return Print;
			}
		}
		Large_Num = *max_element(priorities.begin(), priorities.end());
	}
}
```

## 괄호 짝짓기
```C++
#include <iostream>
#include <string>
#include <stack>

using namespace std;

int main()
{
	for (int testcase = 1; testcase <= 10; testcase++)
	{
		stack<char> Open;
		string Brackets = "";

		int t;
		cin >> t;
		cin >> Brackets;

		bool IsOk = true;

		for (int i = 0; i < Brackets.size(); i++)
		{
			if (Brackets[i] == '(' || Brackets[i] == '[' || Brackets[i] == '{'|| Brackets[i] == '<')
				Open.push(Brackets[i]);
			else
			{
				if (Brackets[i] == ')')
				{
					if (Open.top() != '(')
					{
						IsOk = false;
						break;
					}
					Open.pop();
				}
				if (Brackets[i] == ']')
				{
					if (Open.top() != '[')
					{
						IsOk = false;
						break;
					}
					Open.pop();
				}
				if (Brackets[i] == '}')
				{
					if (Open.top() != '{')
					{
						IsOk = false;
						break;
					}
					Open.pop();
				}
				if (Brackets[i] == '>')
				{
					if (Open.top() != '<')
					{
						IsOk = false;
						break;
					}
					Open.pop();
				}
			}
		}
		
		if (IsOk)
			cout << "#" << testcase << " " << 1 << endl;
		else
			cout << "#" << testcase << " " << 0 << endl;
	}
}
```
