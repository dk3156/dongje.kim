---
title: "Lab13"
date: Homework
mainSectionTitle: "ptc"
---

## Answers
```
// rec13_test.cpp

#include <iostream>
#include <fstream>
#include <vector>      // task 1
#include <list>        // task 2
#include <algorithm>   // task 3
#include <string>      // task 21
#include <set>         // task 22
#include <map>         // task 23
using namespace std;

list<int>::iterator ourFind(list<int>::iterator start, list<int>::iterator end, int target){
    while (start != end){
        if(*iter == target){
            return start;
        }
        ++start;
    }
    return end;
}

template <typename Iter, typename val> /// two typenames
Iter ourFind(Iter start, Iter end, val target) {
    while (start != end) {
        if (*start == target) { return start; }
        ++start;
    } /// write regular function first, then switch to template
    return end;
}


void printList(const list<int>& lis){
    list<int>::const_iterator conIter;
    for(conIter = lis.begin(); conIter != lis.end(); ++conIter){
        cout << *conIter << " ";
        cout << endl;
    }
}

void printListRanged(const list<int>& lis){
    for (int elem : lis){
        cout << elem << " ";
    }
    cout << endl;
}

list<int>::const_iterator findList(const list<int>& lis, int tokken){
    for (list<int>::const_iterator iter = lis.begin(); iter != lis.end(); ++iter){
        if(*iter == tokken){
            return iter;
        }
    }
    return lis.end();
}

auto findListAuto(const list<int>& lis, int tokken){
    for (auto iter = lis.begin(); iter != lis.end(); ++iter){
        if(*iter == tokken){
            return iter;
        }
    }
    return lis.end();
}

void alternateList(list<int>& lis){
    for (auto iter = lis.begin(); iter != lis.end(); ++ ++iter){
        cout << *iter << " ";
    }
    cout << endl;
}

bool isEven(int tokken){
    return tokken % 2 == 0;
}

bool findIf(const list<int>& lis){
    auto found = find_if(lis.begin(), lis.end(), isEven);
    if (found == lis.end()){
        return false;
    }
    return true;
}

class MyFunctor{
public:
    bool operator()(int x){
        return x % 2 == 0;
    }
};

bool findIf(const vector<int>& vec){
    MyFunctor func;
    auto found = find_if(vec.begin(), vec.end(), func);
    if (found == vec.end()){
        return false;
    }
    return true;
}

bool findIf1(const vector<int>& vec){
    auto found = find_if(vec.begin(), vec.end(), [](int a) {return a % 2 == 0;});
    if (found == vec.end()){
        return false;
    }
    return true;
}

bool findIf2(const vector<int>& vec){
    auto found = find_if(vec.begin(), vec.end(), MyFunctor());
    if (found == vec.end()){
        return false;
    }
    return true;
}


int main() {
    // 1. Create a vector with some values and display using ranged for
    cout << "Task 1:\n";
    vector<int> vec = {1,4,3,10,5,6};
    for (int elem : vec){
        cout << elem << " ";
    }

    cout << "\n=======\n";

    // 2. Initalize a list as a copy of values from the vector
    cout << "Task 2:\n";
    list<int> lis(vec.begin(), vec.end());
    for (int elem : lis){
        cout << elem << " ";
    }
    cout << "\n=======\n";

    // 3. Sort the original vector.  Display both the vector and the list
    cout << "Task 3:\n";
    sort(vec.begin(), vec.end());
    for(int elem : vec){
        cout << elem << " ";
    }
    cout << endl;
    for (int elem : lis){
        cout << elem << " ";
    }
    cout << "\n=======\n";

    // 4. print every other element of the vector. ASSUMING the length is odd.
    cout << "Task 4:\n";
    
    cout << "\n=======\n";
    for(size_t i = 0; i < vec.size(); i += 2){
        cout << vec[i] << " ";
    }
    
    // 5. Attempt to print every other element of the list using the
    //    same technique.
    cout << "Task 5:\n";
    
//    list<int>::iterator iter;
//    for(size_t i = 0; i < lis.size(); i += 2){
//        cout << lis[i] << endl;
//    }

    cout << "\n=======\n";

    //
    // Iterators
    //

    // 6. Repeat task 4 using iterators.  Do not use auto;
    cout << "Task 6:\n";
    vector<int>::iterator iter1;
    list<int>::iterator iter2;
    
    for(iter1 = vec.begin(); iter1 != vec.end(); iter1 += 2){
        cout << *iter1 << " ";
    }
    
    cout << "\n=======\n";

    // 7. Repeat the previous task using the list.  Again, do not use auto.
    //    Note that you cannot use the same simple mechanism to bump
    //    the iterator as in task 6.
    cout << "Task 7:\n";
    int counter = 0;
    for(iter2 = lis.begin(); iter2 != lis.end(); ++iter2){
        if (counter % 2 == 0){
            cout << *iter2 << " ";
        }
        ++counter;
        //or ++iter2 in the body / ++++iter2
    }

    cout << "\n=======\n";

    // 8. Sorting a list
    cout << "Task 8:\n";
    
    lis.sort();
    for (int elem : lis){
        cout << elem << " ";
    }
    cout << "\n=======\n";

    // 9. Calling the function to print the list
    cout << "Task 9:\n";
    
    printList(lis);

    cout << "=======\n";

    // 10. Calling the function that prints the list, using ranged-for
    cout << "Task 10:\n";
    
    printListRanged(lis);

    cout << "=======\n";

    //
    // Auto
    //

    // 11. Calling the function that, using auto, prints alterate
    // items in the list
    cout << "Task 11:\n";
    
    alternateList(lis);

    cout << "=======\n";

    
    // 12.  Write a function find that takes a list and value to search for.
    //      What should we return if not found
    cout << "Task 12:\n";
    
    if(findList(lis, 5) == lis.end()){
        cout << "int not found." << endl;
    } else {
        cout << "int found: " << *findList(lis, 5) << endl;
    }

    
    cout << "=======\n";

    // 13.  Write a function find that takes a list and value to search for.
    //      What should we return if not found
    cout << "Task 13:\n";
    
    if(findListAuto(lis, 5) == lis.end()){
        cout << "int not found." << endl;
    } else {
        cout << "int found: " << *findListAuto(lis, 5) << endl;
    }
   
    cout << "=======\n";

    //
    // Generic Algorithms
    //
    // 14. Generic algorithms: find
    cout << "Task 14:\n";
    list<int>::iterator found = find(lis.begin(), lis.end(), 5);

    if (find(lis.begin(), lis.end(), 5) == lis.end()){
        cout << "int not found with find()" << endl;
    } else {
        cout << "int found: " << *found << endl;
    }
    
    cout << "=======\n";

    // 15. Generic algorithms: find_if
    cout << "Task 15:\n";
    
    if (findIf(vec)){
        cout << "Even number found in the vector." << endl;
    } else {
        cout << "Even number not found in the vector." << endl;
    }
    if (findIf(lis)){
        cout << "Even number found in the list." << endl;
    } else {
        cout << "Even number not found in the list." << endl;
    }
    cout << "=======\n";

    // 16. Functor
    cout << "Task 16:\n";
    MyFunctor Even;
    bool twoIsEven = Even(2);
    if (twoIsEven){
        cout << "2 is even." << endl;
    } else {
        cout << "2 is not even." << endl;
    }
    cout << "=======\n";
    
    // 17. Lambda
    cout << "Task 17:\n";
    bool result = [](int a)->bool {return a % 2 == 0;}(4);
    if (result){
        cout << "4 is even" << endl;
    } else {
        cout << "4 is not even" << endl;
    }
    cout << "=======\n";

    // 18. Generic algorithms: copy to an array
    cout << "Task 18:\n";
    int* array = new int[6];
    copy(vec.begin(), vec.end(), array);
    copy(lis.begin(), lis.begin(), array);
    

    cout << "=======\n";

    //
    // Templated Functions
    //

    // 19. Implement find as a function for lists
    cout << "Task 19:\n";
    
    cout << *ourFind(l1.begin(), l1.end(), 3);

    cout << "=======\n";
    
    // 20. Implement find as a templated function
    cout << "Task 20:\n";
    
    cout << *ourFind(v1.begin(), v1.end(), 3);
    
    cout << "=======\n";

    //
    // Associative collections
    //

    // 21. Using a vector of strings, print a line showing the number
    //     of distinct words and the words themselves.
    cout << "Task 21:\n";
    ifstream file("pooh-nopunc.txt");
    if (!file) {
        cerr << "failed to open the file" << endl;
        exit(1);
    }
    vector<string> vec1;
    string token;
    
    while(file >> token){
        vector<string>::iterator found = find(vec1.begin(), vec1.end(), token);
        if(found == vec1.end()){
            vec1.push_back(token);
        }
    }
    
    for(string elem : vec1){
        cout << elem << " ";
    }
    cout << vec1.size() << "words";
    file.close();
    cout << "\n=======\n";

    // 22. Repeating previous step, but using the set
    cout << "Task 22:\n";
    ifstream file2("pooh-nopunc.txt");
    set<string> setString;
    string token2;
    
    while(file2 >> token2){
        setString.insert(token2);
    }
    file2.close();
    
    for(string elem : setString){
        cout << elem << " ";
    }
    cout << endl;
    
    cout << vec1.size() << "words";
    cout << endl;

    cout << "=======\n";

    // 23. Word co-occurence using map
    cout << "Task 23:\n";
    ifstream file3("pooh-nopunc.txt");
    string token3;
    map<string, vector<int>> wordMap;
    int num = 1;
    
    while(file3 >> token3){
        wordMap[token3].push_back(num);
        ++num;
    }
    file2.close();
    
    for(auto pair : wordMap){
        cout << pair.first << " ";
        for (int num : pair.second){
            cout << num << " ";
        }
        cout << endl;
    }
    
    cout << "=======\n";
}
```