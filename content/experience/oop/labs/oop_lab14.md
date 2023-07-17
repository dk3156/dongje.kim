---
title: "Lab14"
date: Homework
mainSectionTitle: "ptc"
---

## Answers

## tree.cpp
```
#include <iostream>
using namespace std;

struct TNode {
  TNode(int data = 0, TNode *left = nullptr, TNode *right = nullptr)
    : data(data), left(left), right(right) {}
  int data;
  TNode *left, *right;
};

struct Node{
    Node(int data = 0, Node* next = nullptr): data(data), next(next){}
    int data;
    Node* next;
};

//int max(TNode* root) {
//  // Provide this code
//}

bool recursive_even(int num){
    if (num < 2) return num == 0;
    if (num % 2 == 0) return recursive_even(num / 2);
    return !recursive_even(num / 2);
}

// count every digit of the binary number. if the number of digit is even, return true
// else, return false
// 1. computer remainder of the the integer, and if the remainder is 1, increase the counter
// 2. if the counter is even, return true. if it is not even, return false.
Node* add_list(Node*& head, int data){
    Node* front = new Node(data);
    front->next = head;
    return front;
}


Node* recursive_linked(const Node* head1, const Node* head2){

    //base case
    if(head1 == nullptr && head2 == nullptr){
        return nullptr;
    }
    //if head2 is longer
    else if(head1 == nullptr){
        return new Node(head2->data, recursive_linked(head1, head2->next));
    }
    //if head1 is longer
    else if (head2 == nullptr){
        return new Node(head1->data, recursive_linked(head1->next, head2));
    }
    
    else {
        return new Node(head1->data + head2->data, recursive_linked(head1->next, head2->next));
    }
}

// set max = root.data
// if the right root.data is bigger than max, then max = right root.data
// if the left root.data is bigger, max = left root.data
//

int find_max(TNode* root){
    if (!root)
        throw exception();
    
    int cur_max = root->data;
    if (root->right){
        int right_max = find_max(root->right);
        if (cur_max < right_max){
            cur_max = right_max;
        }
    }
    if (root->left){
        int left_max = find_max(root->left);
        if (cur_max < left_max){
           cur_max = left_max;
       }
    }
    return cur_max;
}

//palindrome: compare the first, last chr and second, second to last...compare if they are equal

bool palindrome(char* chars, int length){
    if (length == 0 || length == 1) return true;
    if (*chars != *(chars + length - 1)) return false;
    return palindrome(chars + 1, length - 2);
    }

int towers(int num, char start, char target, char spare){
    if(num == 0) return 0;
    int step1 = towers(num-1, start, spare, target);
    int step2 = towers(num-1, spare, start, target);
    return step1 + step2 + 1;
}

void mystery(int n) {
   if (n > 1) {
      cout << 'a';
      mystery(n/2);
      cout << 'b';
      mystery(n/2);
   }
   cout << 'c';
}

int main() {

  TNode a(1), b(2), c(4), d(8, &a, &b), e(16, &c), f(32, &d, &e);
    //task1
    cout << "number of ones is even in 4 : " << boolalpha << recursive_even(4) << endl;
    cout << "number of ones  is in 5 : " << boolalpha << recursive_even(5) << endl;
  //cout << max(&f) << endl;
    
    
    //task2
    Node* n1 = new Node(1);
    Node* n2 = new Node(2, n1);
    Node* n3 = new Node(3, n2);
    Node* n4 = new Node(4);
    Node* n5 = new Node(5, n4);
    Node* sum = recursive_linked(n3, n5);
    while(sum != nullptr){
        cout << sum->data << " ";
        sum = sum->next;
    }
    cout << endl;
    
    //task3
    TNode* tn1 = new TNode(5);
    TNode* tn2 = new TNode(6);
    TNode* tn3 = new TNode(4, tn1, tn2);
    TNode* tn4 = new TNode(7);
    TNode* tn5 = new TNode(8);
    TNode* tn6 = new TNode(3, tn4, tn5);
    TNode* tn7 = new TNode(2, tn3, tn6);
    cout << "max is: " << find_max(tn7) << endl;
    
    //task6
    for (int i = 0; i < 11; ++i){
        cout << i << ": ";
        mystery(i);
        cout << endl;
    }
    
}
```