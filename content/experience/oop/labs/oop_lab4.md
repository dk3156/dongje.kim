---
title: "Lab4"
date: Homework
mainSectionTitle: "ptc"
---

## Answers
```
//
//  main.cpp
//  rec04
//
//  Created by DJ on 2/18/22.
//  Copyright © 2022 DJ. All rights reserved.
//


#include <iostream>
#include <vector>
using namespace std;

class Colour {
public:
    Colour(const string& name, unsigned r, unsigned g, unsigned b)
    : name(name), r(r), g(g), b(b) {}
    void display() const {
        cout << name << " (RBG: " << r << "," << g<< "," << b << ")";
    }
private:
    string name;      // what we call this colour
    unsigned r, g, b; // red/green/blue values for displaying
};


class SpeakerSystem {
public:
    void vibrateSpeakerCones(unsigned signal) const {
        
        cout << "Playing: " << signal << "Hz sound..." << endl;
        cout << "Buzz, buzzy, buzzer, bzap!!!\n";
    }
};

class Amplifier {
public:
    void attachSpeakers(const SpeakerSystem& spkrs)
    {
        if(attachedSpeakers)
            cout << "already have speakers attached!\n";
        else
            attachedSpeakers = &spkrs;
    }
    
    void detachSpeakers() { // should there be an "error" message if not attached?
        attachedSpeakers = nullptr;
    }
    
    void playMusic( ) const {
        if (attachedSpeakers)
            attachedSpeakers -> vibrateSpeakerCones(440);
        else
            cout << "No speakers attached\n";
    }
private:
    const SpeakerSystem* attachedSpeakers = nullptr;
};

class Person {
public:
    Person(const string& name) : name(name), roomie(nullptr) {}
    void movesInWith(Person& newRoomate) {
        if (!roomie && !newRoomate.roomie && this != &newRoomate){
        roomie = &newRoomate;
        newRoomate.roomie = this;
        }
    }
    const string& getName() const { return name; }
    // Don't need to use getName() below, just there for you to use in debugging.
    const string& getRoomiesName() const { return roomie->getName(); }
private:
    Person* roomie;
    string name;
};
class PlainOldClass {
public:
    PlainOldClass() : x(-72) {}
    int getX() const { return x; }
    void setX( int x )  { this->x = x; }
private:
    int x;
};

struct Complex {
    double real;
    double img;
};

int main(int argc, const char * argv[]) {
    int x;
    x = 10;
    cout << "x = " << x << endl;
    
    int* p;
    p = &x;
    cout << "p = " << p << endl;
    
    cout << "p points to where " << *p << " is stored\n";
    cout << "*p contains " << *p << endl;
    
    *p = -2763;
    cout << "p points to where " << *p << " is stored\n";
    cout << "*p now contains " << *p << endl;
    cout << "x now contains " << x << endl;
    
    int y(13);
    cout << "y contains " << y << endl;
    p = &y;
    cout << "p now points to where " << *p << " is stored\n";
    cout << "*p now contains " << *p << endl;
    *p = 980;
    cout << "p points to where " << *p << " is stored\n";
    cout << "*p now contains " << *p << endl;
    cout << "y now contains " << y << endl;
    
    int* q;
    q = p;
    cout << "q points to where " << *q << " is stored\n";
    cout << "*q contains " << *q << endl;
    
    double d(33.44);
    double* pD(&d);
    *pD = *p;
    *pD = 73.2343;
    *p  = *pD;
    *q  = *p;
    //pD  = p;
    //p   = pD;

    int joe = 24;
    const int sal = 19;
    int*  pI;
    pI = &joe;
    *pI = 234;
    //pI = &sal;
    //address that stores const variable cannot be assigned to another pointer.
    *pI = 7623;
    
    const int* pcI;
    //pointer to const int.
    pcI = &joe;
    //const pointer variable can be assgined another address
    //*pcI = 234;
    //dereferenced const pointer cannot be assigned
    pcI = &sal;
    //*pcI = 7623;
    
    //int* const cpI;
    //const pointer
    //int* const cpI(&joe);
    //int* const cpI(&sal);
    //cpI = &joe;
    //*cpI = 234;
    //cpI = &sal;
    //*cpI = 7623;
    
    //const int* const cpcI;
    //const int* const cpcI(&joe);
    //const int* const cpcI(&sal);
    //  cpcI = &joe;  // *cpcI = 234;
    //  cpcI = &sal;
    // *cpcI = 7623;
    
    //p = 0x7ffeefbff63c;
    
    Complex c = {11.23,45.67};
    Complex* pC(&c);
    
    //cout << "real: " << *pC.real << "\nimaginary: " << *pC.img << endl;
    
    cout << "real: " << (*pC).real << "\nimaginary: " << (*pC).img << endl;
    
    //"dereference operator for pointer to struct or class"
    //It must have a struct (or class) object as its left hand operand and the name of a member of that struct (or class) as its right hand operand
    cout << "real: " << pC->real << "\nimaginary: " << pC->img << endl;
    
    PlainOldClass poc;
    PlainOldClass* ppoc( &poc );
    cout << ppoc->getX() << endl;
    ppoc->setX( 2837 );
    cout << ppoc->getX() << endl;
    
    //In C++ the way to request more memory from the operating system is the operator: new (new is address).
    int* pDyn = new int(3); // p points to an int initialized to 3 on the heap
    *pDyn = 17;
    
    cout << "The " << *pDyn
    << " is stored at address " << pDyn
    << " which is in the heap\n";
    
    cout << pDyn << endl;
    delete pDyn;
    //pDyn = nullptr;
    cout << pDyn << endl;
    
    cout << "The 17 might STILL be stored at address " << pDyn<< " even though we deleted pDyn\n";
    //cout << "But can we dereference pDyn?  We can try.  This might crash... " << *pDyn << ".  Still here?\n";
    
    //We will often initialize a pointer variable to nullptr so that we are can more easily find illegal dereferencing errors later when used.
    double* pDouble = nullptr;
    
    //cout << "Can we dereference nullptr?  " << *pDyn << endl;
    //cout << "Can we dereference nullptr?  " << *pDouble << endl;
    
    double* pTest = new double(12);
    delete pTest;
    pTest = nullptr;
    delete pTest; // safe
    
    short* pShrt = new short(5);
    delete pShrt;
//No, delete can only be used with the heap (also known as dynamic memory or free store)
    
    long jumper = 12238743;
    //delete jumper;
    long* ptrTolong = &jumper;
    //delete ptrTolong;
    //Complex cmplx;
    //delete cmplx;
    
    // write code to model two people in this world
    Person joeBob("Joe Bob"), billyJane("Billy Jane");
    
    // now model these two becoming roommates
    joeBob.movesInWith(billyJane);
    
    // did this work out?
    cout << joeBob.getName() << " lives with " << joeBob.getRoomiesName() << endl;
    cout << billyJane.getName() << " lives with " << billyJane.getRoomiesName() << endl;
    
    vector<Colour*> colours;
    string inputName;
    unsigned inputR, inputG, inputB;
    
    while (cin >> inputName >> inputR >> inputG >> inputB) {
        colours.push_back(new Colour(inputName, inputR, inputG, inputB));
    }
    
    // display all the Colours in the vector:
    for (size_t ndx = 0; ndx < colours.size(); ++ndx) {
        colours[ndx]->display();
        cout << endl;
    }
}
```