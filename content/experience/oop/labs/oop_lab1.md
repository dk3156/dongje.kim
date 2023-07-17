---
title: "Lab1"
date: Homework
mainSectionTitle: "ptc"
---

## Answers
```
/*
 rec01_start.cpp
 spring 2022
 jbs
 */
#include<iostream>
#include<fstream>
#include<vector>

// Task 15. Function to display a vector of ints
void printVector(std::vector<int> vec1){
    std::string display;
    for(int i: vec1){
        display += std::to_string(i) + " ";
    }
    std::cout<<display<<std::endl;
}


// Task 16. Function to modify the vector, using indices
void doubleVector(std::vector<int> vec1){
    std::string display;
    for(size_t i = 0; i < vec1.size(); ++i){
        vec1[i] = vec1[i]*2;
    }
    for(int i: vec1){
        display += std::to_string(i) + " ";
    }
    std::cout<<display<<std::endl;
}



// Task 17. Function to modify the vector, using a ranged for
void doubleVector2(std::vector<int> vec1){
    std::string display;
    std::vector<int> vec2;
    for(int i: vec1){
        vec2.push_back(i*2);
    }
    for(int i: vec2){
        display += std::to_string(i) + " ";
    }
    std::cout<<display<<std::endl;
}



//
// main
//
int main() { // Yes, it has to have an int for the return type
    //
    // Output
    //
    
    // Task 1. Printing hello ...  No use of "using namespace"
    std::cout<<"hello\n";
    
    
    // Task 2  Printing hello ...  Using "using namespace"
    using namespace std;
    cout<<"hello"<<endl;
    
    //
    // Types and variables
    //
    
    // Task 3  Displaying uninitialized variable
    int x;
    cout<<x<<endl;
    
    // Task 4 Use the sizeof function to see the size in bytes of
    //        different types on your machine.
    int y = 17;
    int z = 2000000017;
    double pie = 3.14159;
    
    cout<<sizeof(x)<< sizeof(y)<< sizeof(z)<< sizeof(pie)<<endl;
    
    // Task 5  Attempt to assign the wrong type of thing to a variable
    //x = "felix"
    
    
    //
    // Conditions
    //
    
    // Task 6  Testing for a range
    if (10<=y && y<=20){
        cout<<"y is between 10 and 20."<<endl;
    }
    else{
        cout<<"y is not between 10 and 20."<<endl;
    }
    //
    // Loops and files
    //
    
    // Task 7  Looping, printing values 10 to 20 inclusive
    
    // First with a for loop
    for(int i=10;i<=20;i++){
        cout<<i<<" ";
    }
    cout<<endl;
    // Then with a while loop
    int i = 10;
    while(i<=20){
        cout<<i<<" ";
        i++;
    }
    cout<<endl;
    // Finally with a do-while loop
    int j = 10;
    do {
        cout<<j<<" ";
        j++;
    } while(j<=20);
    cout<<endl;
    
    // Task 8  Looping to successfully open a file, asking user for the name
    ifstream myfile;
    do{
        string input;
        cout<<"Type the name of your file ";
        cin>>input;
        myfile.open(input);
    } while(!myfile.is_open());
    cout<<"file successfully opened"<<endl;
    myfile.close();
    
    // Task 9  Looping, reading file word by "word".
    ifstream myfile2("text.txt");
    string line;
    vector<string>lines;
    
    while(getline(myfile2, line)){
        lines.push_back(line);
    }
    myfile2.close();
    for (size_t i = 0; i < lines.size(); ++i) {
        cout << lines[i] << endl;
    }
    
    
    // Task 10 Open a file of integers, read it in, and display the sum.
    ifstream myfile3("integers.txt");
    int tokken =0;
    int sum=0;
    
    while(myfile3>>tokken){
        sum += tokken;
    }
    cout<<sum<<endl;
    myfile3.close();
    
    
    
    // Taks 11 Open and read a file of integers and words. Display the sum.
    ifstream myfile4("integers.txt");
    int tokken_2 =0;
    int sum_2=0;
    
    while(myfile4>>tokken_2){
        sum_2 += tokken_2;
    }
    cout<<sum_2<<endl;
    myfile4.close();
    
    //
    // Vectors
    //
    
    // Task 12 Filling a vector of ints
    vector<int>integers;
    for(int i=10; i<= 100; i+=2){
        integers.push_back(i);
    }
    
    
    // Task 13 Displaying the vector THREE times
    //         a) using the indices,
    for (size_t i = 0; i < integers.size(); ++i) {
        cout << integers[i] << endl;
    }
    
    //         b) using a "ranged for"
    for(int i: integers){
        cout << i << endl;
    }
    //         c) using indices again but backwards
    for (size_t i = integers.size(); i > 0; --i) {
        cout << integers[i-1] << endl;
    }
    
    // Task 14. Initialize a vector with the primes less than 20.
    vector<int>primes{2,3,5,7,11,13,17,19};
    for(int i: primes){
        cout << i << endl;
    }
    //
    // Functions
    //
    
    // Task 15  Function to print a vector
    printVector(primes);
    
    
    // Task 16  Function to double the values in a vector
    doubleVector(primes);
    
    // Task 17  Function to double the values in a vector, using a ranged for
    doubleVector2(primes);
    
}
```