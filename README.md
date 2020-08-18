# Day19project

#pragma once

#ifndef stdafx_h
#define stdafx_h

#include<iostream>
#include<string>
#include<stdlib.h>
#include<time.h>
#include<stdio.h>
#include<vector>
#include<Windows.h>
#include<assert.h>
#include<stack>//STL스택

using namespace std;

#endif



#pragma once
#include"stdafx.h"
// Array_Stack

#define MAX_STACK_COUNT 5

//val을 이용한 stack을 구현하는 법

template<typename T>
class Array_Stack
{
public:
	Array_Stack()
	{
		//memset(values, 0, sizeof(T) * MAX_STACK_COUNT);//배열을 한꺼번에 초기화를 할 수 있다.
		ZeroMemory(values, sizeof(T) * MAX_STACK_COUNT);//배열의 값을 한꺼번에 0으로 clear해준다.


	}
	//집어 넣을때 사용
	void Push(T val)
	{
		//top+1dl max_stack_count 보다 작은지 물어봄..(스택은 쌓아올리는데 들어오는 최댓 값보다 하나가 작아야????한칸을 집어 넣을 수 있음
		//top이 맥스보다 작아야한다.
		//참일때는 아무 문제가 없는데 false일때는 값이 뻑난다.
		assert(top + 1 < MAX_STACK_COUNT);//assert주장하다
		values[++top] = val;//배열이 -부터 시작해서 선행증가를 사용한다.

	}

	//맨 위의 값을 가져올 때 사용
	T Top()//둘다 T를 return한다.
	{
		assert(Empty() == false);//비어있냐? 아니요(결론: 비어있지 않아야 문제가 없다는 것.)
		return values[top];
	}

	//꺼낼때 사용
	T Pop()
	{
		// 비어있으면 못 꺼낸다.
		assert(Empty() == false);

		T val = values[top];
		
		memset(&values[top--], 0, sizeof(T));


		return val;
	}

	//값이 비어있는지 검사할 때 사용
	bool Empty()
	{
		//bool result = top < 0 ? true : false;(이렇게 쓰일 수 있다)
		//뭐 ? 첫번째 : 두번째 (뭐?는 조건식이다 조건식이 true이면 첫번째것을 return)
		return top < 0 ? true : false;
	}


private:
	int top = -1;
	T values[MAX_STACK_COUNT];


};




#pragma once
#include"stdafx.h"

//List_Stack
template<typename T>
class Link_Stack
{
public:
	struct Node
	{
		Node* next = nullptr;
		T data;

	};

	static Node* Create(T data)
	{
		Node* node = new Node();
		node->data = data;
		node->next = nullptr;

		return node;
	}

	static void Destroy(Node* node)
	{
		delete node;
		node = nullptr;

	}

public:
	void Push(Node* node)
	{
		if (head != nullptr)
		{
			Node* tail = head;
			while(tail->next != nullptr)
				tail = tail->next;

			tail->next = node;
		}
		else
		{
			head = node;

		}
		top = node;
	}

	Node* Top()
	{
		return top;
	}
	Node* Pop()
	{
		Node* temp = top;
		if (head == top)
		{
			head = nullptr;
			top = nullptr;

		}
		else
		{
			Node* tail = head;
			while (tail->next != nullptr && tail->next != top)
				tail = tail->next;

			top = tail;
			if(tail->next != nullptr)
				temp = tail->next;
			tail->next = nullptr;

		}
		return temp;

	}

	bool Empyt() { return head == nullptr; }

	Node* Bottom() { return head; }


private:
	Node* head = nullptr;
	Node* top = nullptr;

};

#include"stdafx.h"
#include"Array_Stack.h"
#include"Link_Stack.h"
#include<iostream>

using namespace std;

typedef Link_Stack<int> L_Stack;


int main()
{
	stack<int> s;
	s.push(10);
	s.push(20);
	s.push(30);
	cout << s.top() << endl;
	//s.pop();//이녀석은 top을 return하지 않음.(빼기만함.)
	//제일 위에 있는 원소 삭제
	
	// s.empty();

	//while (s.empty() == false)
	//{
	//	int top = s.top();
	//	cout << top << endl;
	//	s.pop();
	//}//요런식으로 삭제를 해야 한다.


	s.size();
	cout << s.size() << endl;

	//Array_Stack<int> s2;
	//s2.Push(10);
	//s2.Push(20);
	//s2.Push(30);
	//s2.Push(40);
	//s2.Push(50);
	//
	//cout << s2.Pop() << endl;
	//cout << s2.Top() << endl;//왜?40인가
	////pop에서 하나를 빼가지고 현재 제일 높은 40을
	////가져온다.

	//stack은 넣을 때 순서대로 나오고 꺼낼때는 거꾸로 나온다.
	L_Stack s3;
	for (int i = 0; i < 10; i++)
	{
		L_Stack::Node* node = L_Stack::Create(i);
		s3.Push(node);

	}

	while (s3.Empyt() == false)
	{
		L_Stack::Node * node = s3.Pop();
		cout << node->data << endl;
		L_Stack::Destroy(node);

	}


	return 0;
}

#pragma once

#ifndef stdafx_h
#define stdafx_h

#include<iostream>
#include<string>
#include<stdlib.h>
#include<time.h>
#include<stdio.h>
#include<vector>
#include<Windows.h>
#include<assert.h>
#include<stack>//STL스택
#include<queue>//STL스택

using namespace std;

#endif




#pragma once

//List_Queue

#include"stdafx.h"

template<typename T>
class List_Queue
{
public:
	struct Node
	{
		Node* next = nullptr;
		T data;

	};

	static Node* Create(T data)
	{
		Node* node = new Node();
		node->data = data;
		node->next = nullptr;

		return node;
	}

	static void Destroy(Node* node)
	{
		delete node;
		node = nullptr;

	}


public:
	//큐 마지막에 집어 넣자
	void Enqueue(Node* node)
	{
		if (first != nullptr)//first가 nullptr이면 첫 값이 들어간 놈이 없는 것이다.
		{
			//뭔가 값이 있다.
			//마지막을 찾는다.

			if (first != nullptr)
			{
				Node* tail = first;
				while (tail->next != nullptr)
					tail = tail->next;

				tail->next = node;
			}
		}
		else
		{
			first = node;

		}
		last = node;//last는 새로운 놈인 node가 온다.
	}

	Node* Last() { return last; }

	//큐의 맨 앞의 원소를 꺼낸다.
	Node* Dequeue()//Equeue의 반대
	{
		Node* nextfirst = first->next;
		first->next = nullptr;

		Node* result = first;//result임시변수 
		first = nextfirst;
		return result;
	}

	bool Empty()
	{
		return first == nullptr;
	}

	Node* First() { return first; }
private:
	Node* first = nullptr;
	Node* last = nullptr;


};





#include"stdafx.h"
#include"List_Queue.h"

////Queue는 어캐 써먹을까? ?????
//클라이언트에서 일이 바쁜데 서버에서 메세지를 막 쏴 (그 메세지를 처리를 해야함. massageQueue를 만듦)
//컴퓨터가 바쁘면 멈췄다가 나옴.(순서대로 처리?)
//데미지가 들어왔는데 그 데미지 순서대로 처리를 해야 할 때 막타처리(들어온 순서대로 처리를 해야되는데 뭔가 쌓아야 할 때)
//면접에서 나옴........


typedef List_Queue<int>L_Queue;
int main()
{
	L_Queue datas1;
	for (int i = 0; i < 10; i++)
	{
		L_Queue::Node* node = L_Queue::Create(i);
		datas1.Enqueue(node);

	}

	while (datas1.Empty() == false)
	{
		L_Queue::Node * node = datas1.Dequeue();
		cout << node->data << endl;

		L_Queue::Destroy(node);//파괴~


	}

	queue<int>datas2;
	for (int i = 10; i < 20; i++)
	{
		datas2.push(i);

	}

	int data = 0;
	while (datas2.empty() == false)
	{
		data = datas2.front();//꺼내기 전에 소스를 받아와야 한다.
		cout << data << endl;
		datas2.pop();//return을 하지 않음.
	}



	return 0;
}




//Day23
//#숙제
//3겹의 아머를 입은 경우 데미지 메소드를 구현하라
//- 중갑/경갑/옷/맨살
//- 각 80%/50%/20%/0% 의 데미지 경감
//- 각 10%/20%/25% 의 내구도가 깍인다.




