---
title: "HW8"
date: Homework
mainSectionTitle: "ptc"
---

## Answers

## Polynomial.cpp
```
//
//  hw08.cpp
//  hw08
//
//  Created by DJ on 5/1/22.
//
#include "polynomial.h"
#include <iostream>
#include <vector>
using namespace std;
    
ostream& operator<<(ostream& os, const Polynomial& rhs){
    Polynomial::Node* rhs_ptr = rhs.head_ptr;
    size_t degree = rhs.degree;
    while(rhs_ptr != nullptr){
        int data = rhs_ptr->data;
        if(data != 0){
            if(degree == 0){
                os << data;
            } else if (degree == 1){
                if(data == 1){
                    os << "x + ";
                } else if (data == -1){
                    os << "-x + ";
                }
                else {
                    os << data << "x + ";
                }
            } else {
                if (data == 1){
                    os << "x^" << degree << " + ";
                } else if (data == -1){
                    os << "-x^" << degree << " + ";
                } else {
                    os << data << "x^" << degree << " + ";
                }
            }
        }
        --degree;
        rhs_ptr = rhs_ptr->next;
    }
    return os;
}

Polynomial::Polynomial(): degree(0){
    head_ptr = new Node();
}

Polynomial::Polynomial(vector<int> coeff){
    head_ptr = new Node(coeff[0]);
    Node* cur = head_ptr;
    for(size_t i = 1; i < coeff.size(); ++i){
        cur->next = new Node(coeff[i]);
        cur = cur->next;
    }
    degree = coeff.size() - 1;
    clean();
}

Polynomial::Polynomial(const Polynomial& rhs): degree(rhs.degree){
    head_ptr = new Node(rhs.head_ptr->data);
    Node* this_cur = head_ptr;
    Node* rhs_cur = rhs.head_ptr->next;
    while(rhs_cur != nullptr){
        this_cur->next = new Node(rhs_cur->data);
        this_cur = this_cur->next;
        rhs_cur = rhs_cur->next;
    }
}

Polynomial::~Polynomial(){
    Node* cur = head_ptr;
    while(cur != nullptr){
        Node* cur2 = cur->next;
        delete cur;
        cur = cur2;
    }
}

int Polynomial::evaluate(int value){}


Polynomial& Polynomial::operator+=(const Polynomial& rhs){
    size_t this_degree = degree;
    size_t rhs_degree = rhs.degree;
    Node* this_cur = head_ptr;
    Node* rhs_cur = rhs.head_ptr;
    if(degree > rhs.degree){
        while(this_degree != rhs_degree){
            this_cur = this_cur->next;
            --this_degree;
        }
    } else if (degree < rhs.degree){
        degree = rhs.degree;
        head_ptr = new Node(rhs.head_ptr->data);
        Node* this_cur2 = head_ptr;
        rhs_cur = rhs.head_ptr;
        while(this_degree != rhs_degree){
            this_cur2->next = new Node(rhs_cur->next->data);
            rhs_cur = rhs_cur->next;
            this_cur2 = this_cur2->next;
            --rhs_degree;
        }
    }
    while(this_cur != nullptr){
        this_cur->data += rhs_cur->data;
        this_cur = this_cur->next;
        rhs_cur = rhs_cur->next;
    }
    return *this;
}

bool operator==(const Polynomial& lhs, const Polynomial& rhs){
    if (lhs.degree != rhs.degree){
        return false;
    }
    Polynomial::Node* lhs_ptr = lhs.head_ptr;
    Polynomial::Node* rhs_ptr = rhs.head_ptr;
    while (lhs_ptr != nullptr){
        if(lhs_ptr->data != rhs_ptr->data){
            return false;
        }
        lhs_ptr = lhs_ptr->next;
        rhs_ptr = rhs_ptr->next;
    }
    return true;
}

Polynomial& Polynomial::operator=(const Polynomial& rhs){
    if (this != &rhs){
        Node* cur = head_ptr;
        while(cur != nullptr){
            Node* cur2 = cur->next;
            delete cur;
            cur = cur2;
        }
        degree = rhs.degree;
        head_ptr = rhs.head_ptr;
        Node* lhs_ptr = head_ptr;
        Node* rhs_ptr = rhs.head_ptr->next;
        while(rhs_ptr != nullptr){
            lhs_ptr->next = new Node(lhs_ptr->data);
            lhs_ptr = lhs_ptr->next;
            rhs_ptr = rhs_ptr->next;
        }
    }
    return *this;
}

void Polynomial::clean(){
    Node* cur = head_ptr;
    bool flag = true;
    while(cur->next != nullptr && flag){
        if (cur->data == 0){
            head_ptr = cur->next;
            delete cur;
            cur = head_ptr;
            --degree;
        } else {
            flag = false;
        }
    }
}


Polynomial operator+(const Polynomial& lhs, const Polynomial& rhs){
    Polynomial result = lhs;
    result += rhs;
    return result;
}


bool operator!=(const Polynomial& lhs, const Polynomial& rhs){
    return !(lhs == rhs);
}
```

## Polynomial.h
```

#ifndef polynomial_h
#define polynomial_h
#include <iostream>
#include <vector>
using namespace std;

class Polynomial{
    friend ostream& operator<<(ostream& os, const Polynomial& rhs);
    friend bool operator==(const Polynomial& lhs, const Polynomial& rhs);
    
public:
    struct Node{
        Node(int data = 0, Node* next = nullptr): data(data), next(next){}
        int data;
        Node* next;
    };
    Polynomial();
    Polynomial(vector<int> coeffs);
    Polynomial(const Polynomial& rhs);
    Polynomial& operator=(const Polynomial& rhs);
    ~Polynomial();
    Polynomial& operator+=(const Polynomial& rhs);
    void clean();
    int evaluate(int value);
    
private:
    Node* head_ptr;
    size_t degree;
};

Polynomial operator+(const Polynomial& lhs, const Polynomial& rhs);
bool operator!=(const Polynomial& lhs, const Polynomial& rhs);
    

#endif /* polynomial_h */
```

## testPolynomial.cpp
```
/*
  testPolynomial.cpp
  Test code for the Polynomial class
 */

#include "polynomial.h"
#include <iostream>
#include <vector>
using namespace std;

void doNothing(Polynomial temp) {}

int main() {
        
    //test constructor
    Polynomial p1({17});                 // 17
    Polynomial p2({1, 2});               // x + 2
    Polynomial p3({-1, 5});              // -x + 5
    Polynomial p4({5, 4, 3, 2, 1});      // 5x^4 + 4x^3 + 3x^2 + 2x + 1
    Polynomial has_a_zero({4, 0, 1, 7}); // 4x^3 + x + 7

    // Polynomial temp(p4);
    // cout << "temp: " << temp << endl;
        
    //    cerr << "displaying polynomials\n";
    cout << "p1: " << p1 << endl;
    cout << "p2: " << p2 << endl;
    cout << "p3: " << p3 << endl;
    cout << "p4: " << p4 << endl;
    cout << "has_a_zero: " << has_a_zero << endl;

    // Polynomial temp;
    // temp = p2 + p3;
    // cout << "temp: " << temp << endl;
    
    cout << "p2 + p3: " << (p2+p3) << endl; 
    cout << "p2 + p4: " << (p2+p4) << endl; 
    cout << "p4 + p2: " << (p4+p2) << endl;

    //test copy constructor - the statement below uses the copy constructor
    //to initialize poly3 with the same values as poly4
    Polynomial p5(p4);
    p5 += p3;
    cout << "Polynomial p5(p4);\n"
         << "p5 += p3;\n";

    cout << "p4: " << p4 << endl;  
    cout << "p5: " << p5 << endl;  


    cout << "Calling doNothing(p5)\n";
    doNothing(p5);
    cout << "p5: " << p5 << endl;

    //tests the assignment operator
    Polynomial p6;
    cout << "p6: " << p6 << endl;
    cout << boolalpha;  // Causes bools true and false to be printed that way.
    cout << "(p4 == p6) is " << (p4 == p6) << endl;
    p6 = p4;
    cout << "p6: " << p6 << endl;
    cout << boolalpha;
    cout << "(p4 == p6) is " << (p4 == p6) << endl;

    //test the evaluaton
    int x = 5;
//    cout << "Evaluating p1 at " << x << " yields: " << p1.evaluate(5) << endl;
//    cout << "Evaluating p2 at " << x << " yields: " << p2.evaluate(5) << endl;
//        
//    Polynomial p7({3, 2, 1});           // 3x^2 + 2x + 1
//    cout << "p7: " << p7 << endl;
//    cout << "Evaluating p7 at " << x << " yields: " << p7.evaluate(5) << endl;

    cout << boolalpha;
    cout << "(p1 == p2) is " << (p1 == p2) << endl;
    cout << "(p1 != p2) is " << (p1 != p2) << endl;

    Polynomial p8({ 1, 1 });
    Polynomial p9({ -1, 1 });
    Polynomial p10({ 0, 0, 2 });
    //p8 + p9 tests if += does the cleanup()
    //p10 tests if constructor does the cleanup()

    cout << "p8: " << p8 << endl
         << "p9: " << p9 << endl
         << "p10: " << p10 << endl;

    cout << "((p8 + p9) == p10) is " << ((p8 + p9) == p10) << endl;
}
```