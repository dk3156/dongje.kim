---
title: "Lab12"
date: Homework
mainSectionTitle: "ptc"
---

## Answers

```
//
//  main.cpp
//  rec12
//
//  Created by DJ on 4/22/22.
//

#include <cstdlib>
#include <iostream>
using namespace std;

class List {

    friend ostream& operator<<(ostream& os, const List& rhs){
        os << "Outputting the list: " << endl;
        Node* temp = rhs.header->next;
        while(temp != rhs.trailer){
            os << temp->data << ' ';
            temp = temp->next;
        }
        return os;
    }
    
    struct Node{
        Node(int data = 0, Node* next = nullptr, Node* prior = nullptr): data(data), next(next), prior(prior){}
        int data;
        Node* next;
        Node* prior;
    };
public:
    
    class Iterator{
    public:
        friend bool operator==(const List::Iterator& lhs, const List::Iterator& rhs){
            return lhs.header == rhs.header;
        }

        friend bool operator!=(const List::Iterator& lhs, const List::Iterator& rhs){
            return !(lhs == rhs);
        }
        friend List;
    
        Iterator(Node* header): header(header){}
        
        Iterator& operator++(){
            this->header = header->next;
            return *this;
        }
        
        Iterator operator--(){
            this->header = header->prior;
            return *this;
        }
        
        int& operator*(){
            return header->data;
        }
        
    private:
        Node* header;
    };
    
    List(int size = 0): list_size(size), header(new Node()), trailer(new Node()){
        header->next = trailer;
        trailer->prior = header;
    }
    
    Iterator begin(){
        return Iterator(header->next);
    }
    Iterator end(){
        return Iterator(trailer);
    }
    
    Iterator insert(Iterator iter, int data){
        iter.header->prior = new Node(data, iter.header, iter.header->prior);
        iter.header->prior->prior->next = iter.header->prior;
        iter.header = iter.header->prior;
        ++list_size;
        return iter;
    }
    
    Iterator erase(Iterator iter){
        Node* p1 = iter.header->prior;
        Node* p2 = iter.header->next;
        p1->next = p2;
        p2->prior = p1;
        delete iter.header;
        --list_size;
        return Iterator(p2);
    }
    
    void push_back(int data){
//        if(list_size == 0){
//            //the new node should be attached right after the head
//            //the new node should be attached right before the trail
//            header->next = new Node(data);
//            header->next->next = trailer;
//            trailer->prior = header->next;
//            ++list_size;
//        } else {
            //if the list is not empty
            //the new node should be attached right before the trailer. and
            //right after trailer's prior node
        Node* sec = trailer->prior;
        Node* temp = new Node(data);
        trailer->prior->next = temp;
        temp->next = trailer;
        trailer->prior = temp;
        temp->prior = sec;
        ++list_size;
    }
    
     void pop_back(){
        //pop back should return the last node before the trailer node
        //and disconnect it and second last becomes the last.
        Node* temp = trailer->prior;
        temp->prior->next = trailer;
         trailer->prior = temp->prior;
         delete temp;
        --list_size;
    }
    
    int front() const {
        //front should return the first node's data. reference. so it can be changed
        return header->next->data;
    }

    int back() const {
        return trailer->prior->data;
    }
    int& front(){
        //front should return the first node's data. reference. so it can be changed
        return header->next->data;
    }
    
    int& back(){
        return trailer->prior->data;
    }
    
    size_t size() const {
//        Node* temp = header->next;
//        size_t list_size;
//        while(temp != trailer){
//            temp = temp->next;
//            ++list_size;
//        }
        return list_size;
    }

    void push_front(int data){
        //push front should add the node after the header node
        //head->next should be the next node from the current one
        //Node(data, next, prior)
        Node* newGuy = new Node(data);
        Node* temp = header->next;
        temp->prior = newGuy;
        newGuy->prior = header;
        newGuy->next = temp;
        header->next = newGuy;
        ++list_size;
    }
    
    void pop_front(){
        //pop front should delete the first element
        //the second node should now be the first node
        Node* temp = header->next;
        temp->next->prior = header;
        header->next = temp->next;
        delete temp;
        --list_size;
    }
    
    void clear(){
        Node* temp = header->next;
        while(temp != trailer){
            Node* toDelete;
            toDelete = temp;
            temp = temp->next;
            delete toDelete;
        }
        header->next = trailer;
        trailer->prior = header;
        list_size = 0;
    }
    
    int operator[](size_t index) const{
        Node* temp = header->next;
        while(index != 0){
            temp = temp->next;
            --index;
        }
        return temp->data;
    }
    
    int& operator[](size_t index){
        Node* temp = header->next;
        while(index != 0){
            temp = temp->next;
            --index;
        }
        return temp->data;
    }
    
private:
    int list_size;
    Node* header;
    Node* trailer;
};




// Task 1
void printListInfo(const List& myList) {
        cout << "size: " << myList.size()
             << ", front: " << myList.front()
             << ", back(): " << myList.back()
             << ", list: " << myList << endl;
}

// The following should not compile. Check that it does not.
// void changeFrontAndBackConst(const List& theList){
//     theList.front() = 17;
//     theList.back() = 42;
// }

void changeFrontAndBack(List& theList){
    theList.front() = 17;
    theList.back() = 42;
}

 //Task 4
void printListSlow(const List& myList) {
    for (size_t i = 0; i < myList.size(); ++i) {
        cout << myList[i] << ' ';
    }
    cout << endl;
}

// Task 8
//void doNothing(List aList) {
//    cout << "In doNothing\n";
//    printListInfo(aList);
//    cout << endl;
//    cout << "Leaving doNothing\n";
//}

int main() {

    // Task 1
    cout << "\n------Task One------\n";
    List myList;
    cout << "Fill empty list with push_back: i*i for i from 0 to 9\n";
    for (int i = 0; i < 10; ++i) {
        cout << "myList.push_back(" << i*i << ");\n";
        myList.push_back(i*i);
        printListInfo(myList);
    }
    cout << "===================\n";
    
    cout << "Modify the first and last items, and display the results\n";
    changeFrontAndBack(myList);
    printListInfo(myList);
    cout << "===================\n";

    cout << "Remove the items with pop_back\n";
    while (myList.size()) {
        printListInfo(myList);
        myList.pop_back();
    }
    cout << "===================\n";

     //Task 2
    cout << "\n------Task Two------\n";
    cout << "Fill empty list with push_front: i*i for i from 0 to 9\n";
    for (int i = 0; i < 10; ++i) {
        cout << "myList2.push_front(" << i*i << ");\n";
        myList.push_front(i*i);
        printListInfo(myList);
    }
    cout << "===================\n";
    cout << "Remove the items with pop_front\n";
    while (myList.size()) {
        printListInfo(myList);
        myList.pop_front();
    }
    printListInfo(myList);
    cout << "===================\n";

    // Task 3
    cout << "\n------Task Three------\n";
    cout << "Fill empty list with push_back: i*i for i from 0 to 9\n";
    for (int i = 0; i < 10; ++i) {
        myList.push_back(i*i);
    }
    printListInfo(myList);
    cout << "Now clear\n";
    myList.clear();
    cout << "Size: " << myList.size() << ", list: " << myList << endl;
    cout << "===================\n";

     //Task 4
    cout << "\n------Task Four------\n";
    cout << "Fill empty list with push_back: i*i for i from 0 to 9\n";
    for (int i = 0; i < 10; ++i)  myList.push_back(i*i);
    cout << "Display elements with op[]\n";
    for (size_t i = 0; i < myList.size(); ++i) cout << myList[i] << ' ';
    cout << endl;
    cout << "Add one to each element with op[]\n";
    for (size_t i = 0; i < myList.size(); ++i) myList[i] += 1;
    cout << "And print it out again with op[]\n";
    for (size_t i = 0; i < myList.size(); ++i) cout << myList[i] << ' ';
    cout << endl;
    cout << "Now calling a function, printListSlow, to do the same thing\n";
    printListSlow(myList);
    cout << "Finally, for this task, using the index operator to modify\n"
         << "the data in the third item in the list\n"
         << "and then using printListSlow to display it again\n";
    myList[2] = 42;
    printListSlow(myList);


//    // Task 5
    cout << "\n------Task Five------\n";
    cout << "Fill empty list with push_back: i*i for i from 0 to 9\n";
    myList.clear();
    for (int i = 0; i < 10; ++i)  myList.push_back(i*i);
    printListInfo(myList);
    cout << "Now display the elements in a ranged for\n";
    for (int x : myList) cout << x << ' ';
    cout << endl;
    cout << "And again using the iterator type directly:\n";
    // Note you can choose to nest the iterator class or not, your choice.
    //for (iterator iter = myList.begin(); iter != myList.end(); ++iter) {
    for (List::Iterator iter = myList.begin(); iter != myList.end(); ++iter) {
        cout << *iter << ' ';
    }
    cout << endl;
    cout << "WOW!!! (I thought it was cool.)\n";

//    // Task 6
    cout << "\n------Task Six------\n";
    cout << "Filling an empty list with insert at end: i*i for i from 0 to 9\n";
    myList.clear();
    for (int i = 0; i < 10; ++i) myList.insert(myList.end(), i*i);
    printListInfo(myList);
    cout << "Filling an empty list with insert at begin(): "
         << "i*i for i from 0 to 9\n";
    myList.clear();
    for (int i = 0; i < 10; ++i) myList.insert(myList.begin(), i*i);
    printListInfo(myList);
    // ***Need test for insert other than begin/end***
    cout << "===================\n";

    // Task 7
    cout << "\n------Task Seven------\n";
    cout << "Filling an empty list with insert at end: i*i for i from 0 to 9\n";
    myList.clear();
    for (int i = 0; i < 10; ++i) myList.insert(myList.end(), i*i);
    cout << "Erasing the elements in the list, starting from the beginning\n";
    while (myList.size()) {
        printListInfo(myList);
        myList.erase(myList.begin());
    }
    // ***Need test for erase other than begin/end***
    cout << "===================\n";

//    // Task 8
//    cout << "\n------Task Eight------\n";
//    cout << "Copy control\n";
//    cout << "Filling an empty list with insert at end: i*i for i from 0 to 9\n";
//    myList.clear();
//    for (int i = 0; i < 10; ++i) myList.insert(myList.end(), i*i);
//    printListInfo(myList);
//    cout << "Calling doNothing(myList)\n";
//    doNothing(myList);
//    cout << "Back from doNothing(myList)\n";
//    printListInfo(myList);
//
//    cout << "Filling listTwo with insert at begin: i*i for i from 0 to 9\n";
//    List listTwo;
//    for (int i = 0; i < 10; ++i) listTwo.insert(listTwo.begin(), i*i);
//    printListInfo(listTwo);
//    cout << "listTwo = myList\n";
//    listTwo = myList;
//    cout << "myList: ";
//    printListInfo(myList);
//    cout << "listTwo: ";
//    printListInfo(listTwo);
//    cout << "===================\n";
}

```